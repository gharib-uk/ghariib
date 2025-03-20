---
title: "Preventing AI Plagiarism With .ASS Subtitling"
date: 2025-01-26
---

![](https://hackaday.com/wp-content/uploads/2025/01/subtitle-poison-main.jpg?w=800)

Around two years ago, the world was inundated with news about how generative AI or large language models would revolutionize the world. At the time it was easy to get caught up in the hype, but in the intervening months these tools have done little in the way of productive work outside of a few edge cases, and mostly serve to burn tons of cash while turning the Internet into even more of a desolate wasteland than it was before. They do this largely by regurgitating human creations like text, audio, and video into inferior simulacrums and, if you still want to exist on the Internet, there’s basically nothing you can do to prevent this sort of plagiarism. Except feed the AI models garbage data like this YouTuber has started doing.

At least as far as YouTube is concerned, the worst offenders of AI plagiarism work by downloading the video’s subtitles, passing them through some sort of AI model, and then generating another YouTube video based off of the original creator’s work. Most subtitle files are the fairly straightfoward `.srt` filetype which only allows for timing and text information. But a more obscure subtitle filetype known as Advanced SubStation Alpha, or `.ass`, allows for all kinds of subtitle customization like orientation, formatting, font types, colors, shadowing, and many others. YouTuber \[f4mi\] realized that using this subtitle system, extra garbage text could be placed in the subtitle filetype but set out of view of the video itself, either by placing the text outside the viewable area or increasing its transparency. So now when an AI crawler downloads the subtitle file it can’t distinguish real subtitles from the garbage placed into it.

\[f4mi\] created a few scripts to do this automatically so that it doesn’t have to be done by hand for each one. It also doesn’t impact the actual subtitles on the screen for people who need them for accessibility reasons. It’s a great way to “poison” AI models and make it at least harder for them to rip off the creations of original artists, and \[f4mi\]’s tests show that it does work. We’ve actually seen a similar method for poisoning data sets used for emails long ago, back when we were all collectively much more concerned about groups like the NSA using automated snooping tools in our emails than we were that machines were going to steal our creative endeavors.

Thanks to \[www2\] for the tip!

<iframe loading="lazy" title="Poisoning AI with &quot;.аss&quot; subtitles" width="640" height="480" src="https://www.youtube.com/embed/NEDFUjqA1s8?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Go to Source
