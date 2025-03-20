---
title: "Un experimento rápido: translating Cloudflare Stream captions with Workers AI"
date: 2025-01-02
categories: 
  - "general"
---

Cloudflare Stream launched AI-powered automated captions to transcribe English in on-demand videos in March 2024. Customers' immediate next questions were about other languages — both _transcribing_ audio from other languages, and _translating_ captions to make subtitles for other languages. As the Stream Product Manager, I've thought a lot about how we might tackle these, but I wondered…

**What if I just translated a generated** **VTT** **(caption file)? Can we do that?** I hoped to use Workers AI to conduct a quick experiment to learn more about the problem space, challenges we may find, and what platform capabilities we can leverage.

There is a sample translator demo in Workers documentation that uses the “m2m100-1.2b” Many-to-Many multilingual translation model to translate short input strings. I decided to start there and try using it to translate some of the English captions in my Stream library into Spanish.

## Selecting test content

I started with my short demo video announcing the transcription feature. I wanted a Worker that could read the VTT captions file from Stream, isolate the text content, and run it through the model as-is.

The first step was parsing the input. A VTT file is a text file that contains a sequence of numbered “cues,” each with a number, a start and end time, and text content. 

```
WEBVTT
X-TIMESTAMP-MAP=LOCAL:00:00:00.000,MPEGTS:900000
 
1
00:00:00.000 --> 00:00:02.580
Good morning, I'm Taylor Smith,
 
2
00:00:02.580 --> 00:00:03.520
the Product Manager for Cloudflare
 
3
00:00:03.520 --> 00:00:04.460
Stream. This is a quick
 
4
00:00:04.460 --> 00:00:06.040
demo of our AI-powered automatic
 
5
00:00:06.040 --> 00:00:07.580
subtitles feature. These subtitles
 
6
00:00:07.580 --> 00:00:09.420
were generated with Cloudflare WorkersAI
 
7
00:00:09.420 --> 00:00:10.860
and the Whisper Model,
 
8
00:00:10.860 --> 00:00:12.020
not handwritten, and it took
 
9
00:00:12.020 --> 00:00:13.940
just a few seconds.
```

## Parsing the input

I started with a simple Worker that would fetch the VTT from Stream directly, run it through a function I wrote to deconstruct the cues, and return the timestamps and original text in an easier to review format.

```
export default {
  async fetch(request: Request, env: Env, ctx): Promise<Response> {
    // Step One: Get our input.
    const input = await fetch(PLACEHOLDER_VTT_URL)
      .then(res => res.text());
 
    // Step Two: Parse the VTT file and get the text
    const captions = vttToCues(input);
 
    // Done: Return what we have.
    return new Response(captions.map(c =>
      (`#${c.number}: ${c.start} --> ${c.end}: ${c.content.toString()}`)
    ).join('n'));
  },
};
```

That returned this text:

```
#1: 0 --> 2.58: Good morning, I'm Taylor Smith,
#2: 2.58 --> 3.52: the Product Manager for Cloudflare
#3: 3.52 --> 4.46: Stream. This is a quick
#4: 4.46 --> 6.04: demo of our AI-powered automatic
#5: 6.04 --> 7.58: subtitles feature. These subtitles
#6: 7.58 --> 9.42: were generated with Cloudflare WorkersAI
#7: 9.42 --> 10.86: and the Whisper Model,
#8: 10.86 --> 12.02: not handwritten, and it took
#9: 12.02 --> 13.94: just a few seconds.
```

## AI-ify

As a proof of concept, I adapted a snippet from the demo into my Worker. In the example, the target language and input text are extracted from the user’s request. In my experiment, I decided to hardcode the languages. Also, I had an array of input objects, one for each cue, not just a string. After interpreting the caption input _but before returning a response_, I used a map callback to parallelize all the AI.run() calls to translate each cue, so they could execute asynchronously and in-place, then awaited them all to resolve. Ultimately, the AI inference call itself is the simplest part of the script.

```
await Promise.all(captions.map(async (q) => {
  const translation = await env.AI.run(
    "@cf/meta/m2m100-1.2b",
    {
      text: q.content,
      source_lang: "en",
      target_lang: "es",
    }
  );
 
  q.content = translation?.translated_text ?? q.content;
}));
```

Then the script returns the translated output in the format from before.

Of course, this is not a scalable or error-tolerant approach for production use because it doesn’t make affordances for rate limiting, failures, or processing bigger throughput. But for a few minutes of tinkering, it taught me a lot.

```
#1: 0 --> 2.58: Buen día, soy Taylor Smith.
#2: 2.58 --> 3.52: El gerente de producto de Cloudflare
#3: 3.52 --> 4.46: Rápido, esto es rápido
#4: 4.46 --> 6.04: La demostración de nuestro automático AI-powered
#5: 6.04 --> 7.58: Los subtítulos, estos subtítulos
#6: 7.58 --> 9.42: Generado con Cloudflare WorkersAI
#7: 9.42 --> 10.86: y el modelo de susurro,
#8: 10.86 --> 12.02: No se escribió, y se tomó
#9: 12.02 --> 13.94: Sólo unos segundos.
```

A few immediate observations: first, these results came back surprisingly quickly and the Workers AI code worked on the first try! Second, evaluating the quality of translation results is going to depend on having team members with expertise in those languages. Because — third, as a novice Spanish speaker, I can tell this output has some issues.

Cues 1 and 2 are okay, but 3 is not (“Fast, this is fast” from “\[Cloudflare\] Stream. This is a quick…”). Cues 5 through 9 had several idiomatic and grammatical issues, too. I theorized that this is because Stream splits the English captions into groups of 4 or 5 words to make them easy to _read_ quickly in the overlay. But that also means sentences and grammatical constructs are interrupted. When those fragments go to the translation model, there isn’t enough context.

## Consolidating sentences

I speculated that reconstructing sentences would be the most effective way to improve translation quality, so I made that the one problem I attempted to solve within this exploration. I added a rough pre-processor in the Worker that tries to merge caption cues together and then splits them at sentence boundaries instead. In the process, it also adjusts the timing of the resulting cues to cover the same approximate timeframe.

Looking at each cue in order:

```
// Break this cue up by sentence-ending punctuation.
const sentences = thisCue.content.split(/(?<=[.?!]+)/g);

// Cut here? We have one fragment and it has a sentence terminator.
const cut = sentences.length === 1 && thisCue.content.match(/[.?!]/);
```

But if there’s a cue that splits into multiple sentences, cut it up and split the timing. Leave the final fragment to roll into the next cue:

```
else if (sentences.length > 1) {
  // Save the last fragment for later
  const nextContent = sentences.pop();

  // Put holdover content and all-but-last fragment into the content
  newContent += ' ' + sentences.join(' ');

  const thisLength = (thisCue.end - thisCue.start) / 2;

    result.push({
      number: newNumber,
      start: newStart,
      end: thisCue.start + (thisLength / 2), // End this cue early
      content: newContent,
    });

    // … then treat the next cue as a holdover
    cueLength = 1;
    newContent = nextContent;
    // Start the next consolidated cue halfway into this cue's original duration
    newStart = thisCue.start + (thisLength / 2) + 0.001;
    // Set the next consolidated cue's number to this cue's number
    newNumber = thisCue.number;
  }
}
```

Applying that to the input, it generates sentence-grouped output, visualized here in green:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1MzmQ0KAJBntBrqgwGAqTd/035d044fc9e70c9933c1406074de52b9/image2.png)

There are only 3 “new” cues, each starts at the beginning of a sentence. The consolidated cues are longer and might be harder to read when overlaid on a video, but they are complete grammatical units:

```
#1: 0 --> 3.755:  Good morning, I'm Taylor Smith, the Product Manager for Cloudflare Stream.
#3: 3.756 --> 6.425:  This is a quick demo of our AI-powered automatic subtitles feature.
#5: 6.426 --> 12.5:  These subtitles were generated with Cloudflare Workers AI and the Whisper Model, not handwritten, and it took just a few seconds.
```

Translating this “prepared” input the same way as before:

```
#1: 0 --> 3.755: Buen día, soy Taylor Smith, el gerente de producto de Cloudflare Stream.
#3: 3.756 --> 6.425: Esta es una demostración rápida de nuestra función de subtítulos automáticos alimentados por IA.
#5: 6.426 --> 12.5: Estos subtítulos fueron generados con Cloudflare WorkersAI y el Modelo Whisper, no escritos a mano, y solo tomó unos segundos.
```

¡Mucho mejor! \[Much better!\]

## Re-exporting to VTT

To use these translated captions on a video, they need to be formatted back into a VTT with renumbered cues and properly formatted timestamps. Ultimately, the solution should automatically upload them back to Stream, too, but that is an established process, so I set it aside as out of scope. The final VTT result from my Worker is this:

```
WEBVTT
 
1
00:00:00.000 --> 00:00:03.754
Buen día, soy Taylor Smith, el gerente de producto de Cloudflare Stream.
 
2
00:00:03.755 --> 00:00:06.424
Esta es una demostración rápida de nuestra función de subtítulos automáticos alimentados por IA.
 
3
00:00:06.426 --> 00:00:12.500
Estos subtítulos fueron generados con Cloudflare WorkersAI y el Modelo Whisper, no escritos a mano, y solo tomó unos segundos.
```

I saved it to a file locally and, using the Cloudflare Dashboard, I added it to the video which you may have noticed embedded at the top of this post! Captions can also be uploaded via the API.

## More testing and what I learned

I tested this script on a variety of videos from many sources, including short social media clips, 30-minute video diaries, and even a few clips with some specialized vocabulary. Ultimately, I was surprised at the level of prototype I was able to build on my first afternoon with Workers AI. The translation results were very promising! In the process, I learned a few key things that I will be bringing back to product planning for Stream:

**We have the tools.** Workers AI has a model called "m2m100-1.2b" from Hugging Face that can do text translations between many languages. We can use it to translate the plain text cues from VTT files — whether we generate them or they are user-supplied. We’ll keep an eye out for new models as they are added, too.

**Quality is prone to "copy-of-a-copy" effect.** When auto-translating captions that were auto-transcribed, issues that impact the English transcription have a huge downstream impact on the translation. Editing the source transcription improves quality _a lot_.

**Good grammar and punctuation counts.** Translations are significantly improved if the source content is grammatically correct and punctuated properly. Punctuation is often missing when captions are auto-generated, but not always  — I would like to learn more about how to predict that and if there are ways we can increase punctuation in the output of transcription jobs. My cue consolidator experiment returns giant walls of text if there’s no punctuation on the input.

**Translate full sentences when possible.** We split our transcriptions into cues of about 5 words for several reasons. However, this produces lower quality output when translated because it breaks grammatical constructs. Translation results are better with full sentences or at least complete fragments. This is doable, but easier said than done, particularly as we look toward support for additional input languages that use punctuation differently.

**We will have blind spots when evaluating quality.** Everyone on our team was able to adequately evaluate English _transcriptions_. Sanity-checking the quality of _translations_ will require team members who are familiar with those languages. We state disclaimers about transcription quality and offer tips to improve it, but at least we know what we're looking at. For translations, we may not know how far off we are in many cases. How many readers of this article objected to the first translation sample above?

**Clear UI and API design will be important for these related but distinct workflows.** There are two different flows being requested by Stream customers: "My audio is in English, please make translated subtitles" alongside "My audio is in another language, please transcribe captions as-is." We will need to carefully consider how we shape user-facing interactions to make it really clear to a user what they are asking us to do.

**Workers AI is really easy to use.** Sheepishly, I will admit: although I read Stream's code for the transcription feature, this was the first time I've ever used Workers AI on my own, and it was definitely the easiest part of this experiment!

Finally, as a product manager, it is important I remain focused on the outcome. From a certain point of view, this experiment is a bit of an XY Problem. The _need_ is "I have audio in one language and I want subtitles in another." Are there other avenues worth looking into besides "transcribe to captions, then restructure and translate those captions?" Quite possibly. But this experiment with Workers AI helped me identify some potential challenges to plan for and opportunities to get excited about!

I’ve cleaned up and shared the sample code I used in this experiment at https://github.com/tsmith512/vtt-translate/. Try it out and share your experience!

Go to Source
