---
title: "Challenging devChallenges.io with SvelteKit"
date: 2025-02-06
---

Before we get into it, I would like to thank whatever powers that be who selected my previous post as top 7 featured of the week. I'm quite proud of that, and now I'm motivated to do more writing, so let's get into it!

After seeing that a certain company in my area (70-ish miles away, close enough) was looking for experience in SvelteKit, I decided to try it out. Most of my experience is in PHP frameworks utilizing Timber/Twig, but I want to get more familiar with JavaScript frameworks. What better way to learn than diving in? What I've chosen to do is hop on devChallenges, pick a project which comes fully loaded with a design, and go to town. I chose Simple Coffee Listing as my first since it seemed simple enough.

## Challenges overcame while Challenging devChallenges

It took me a few days to complete this one page project, and most of that time was spent setting up a development environment on Windows 10, and figuring out how to build a toggle component. I would like to state that ever since my first internship position, I learned that documentation is my best friend. Svelte's documentation and tutorials are excellent, and without them I'd still be figuring out how to build a toggle component.

Looking at the design I could tell I was going to need a Card component and a Toggle component. The Toggle was built with radio buttons. To have an option checked by default I had to set initial state for the bind:value to the first radio option value. Let's look at the code!

## +page.svelte

```
<script>
    import Card from '$lib/components/Card/Card.svelte';
    import Toggle from '$lib/components/Toggle/Toggle.svelte';

    let {data} = $props(); //data is coming from +page.server.js

    let fullCatalog = Array.from(data.item); //Array.from since we are receiving an object full of objects.
    let filteredCatalog = fullCatalog.filter(coffee => coffee.id !== 5); 

    const options = [{
        value: 'all-products',
        label: 'All Products',
    }, {
        value: 'available-now',
        label: 'Available Now',
    }, ]

    let radioValue = $state(options[0].value); //default radio choice

</script>
```

The main page where I import the UI components Card and Toggle, pulling in data from +page.server.js, and then form 2 separate arrays from that data. Initially I attempted to loop through the data array and use skip or continue when faced with the item that was supposed to be filtered, but apparently there is no equivalent to those keywords when looping with svelte's {#each}. That is why I opted to create two arrays, though I would have preferred having only one.

The next relevant piece of code is  

```
 <Toggle {options} bind:userChoice={radioValue}/>
```

We're using options to help build the radio buttons inside of Toggle, and bind is used to pass data from the child () to the parent (+page.svelte).

## Toggle.svelte

```
<script>
    let { options, userChoice = $bindable(options[0].value) } = $props();

    const slugify = (str = "") => str.toLowerCase().replace(/ /g, "-").replace(/./g, "");

    const uniqueID = Math.floor(Math.random() * 100)

    console.log(options[0])
</script>

<div role="radiogroup" aria-labelledby={`label-${uniqueID}`} class="productFilterWrapper mx-auto max-w-96 flex justify-center gap-6 pb-8" >

    {#each options as { value, label }}

        <input bind:group={userChoice} value={value} type="radio" id={slugify(label)} class="hidden-toggles__input hidden">
        <label for={slugify(label)} class="hidden-toggles__label p-4 rounded-xl">{label}</label>

     {/each}

</div>
```

From Svelte:

> If you have multiple type="radio" or type="checkbox" inputs relating to the same value, you can use bind:group along with the value attribute. Radio inputs in the same group are mutually exclusive; checkbox inputs in the same group form an array of selected values.

While there are only two options to iterate through with each, I want to leave the possibility for more options to be included, and would work just as well with 2 or 200 (please do not have 200 radio buttons for any reason ever). Now that I have the functionality to filter our results from our api call based on which radio button is selected, HOW are we going to present this information? Glad you asked. Back to +page.svelte!

Below toggle, we have this logic  

```
<div class="CardGrid w-full max-md:mt-6 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {#if radioValue === "" || radioValue === 'all-products'}
            {#each fullCatalog
                as { id, name, image, popular, price, rating, votes }}

                <Card
                    coffeeId={id}
                    coffeeName={name}
                    coffeeImg={image}
                    coffeePopular={popular}
                    coffeePrice={price}
                    coffeeRating={rating}
                    coffeeVotes={votes}

                />  
            {/each}
        {:else}
            {#each filteredCatalog
                as { id, name, image, popular, price, rating, votes }}

                <Card
                    coffeeId={id}
                    coffeeName={name}
                    coffeeImg={image}
                    coffeePopular={popular}
                    coffeePrice={price}
                    coffeeRating={rating}
                    coffeeVotes={votes}

                />  
            {/each}    
        {/if}
    </div>
```

We set up a container Grid and based on the value passed in by Toggle, we iterate one of our arrays. Each array item has an object which gets destructured into a list corresponding with their properties.

## Card.svelte

```
<script>
    let {coffeeId, coffeeName, coffeeImg,  coffeePopular, coffeePrice, coffeeRating, coffeeVotes } = $props();

</script>

<div class="cardWrapper">
    <div class="cardImgWrapper">
        {#if coffeePopular === true}
            <p class="absolute mx-2 my-2 px-2 py-1 bg-yellow-400 rounded-xl text-black text-xs font-medium">Popular</p>
        {/if}
        <img class="rounded-xl w-full" alt="" src="{coffeeImg}" />
    </div>
    <div class="cardPriceWrapper flex justify-between pt-2">
        <p class="text-white font-medium">{coffeeName}</p>
        <span class="text-black bg-[#FEF7EE] rounded-md px-2 py-1 font-medium text-sm h-min">{coffeePrice}</span>
    </div>
    <div class="cardRatingWrapper flex items-center justify-between">
        <div class="score-votes flex items-center">
            {#if coffeeVotes}
                <img alt="" src="/img/Star_fill.svg" />
                <p class="text-md font-medium">{coffeeRating} <span class="text-[#4d5562] pl-1">({coffeeVotes} votes)</span></p>
            {:else if !coffeeVotes}
                <img alt="" src="/img/Star.svg" />
                <p class="text-[#4d5562] text-small">No Ratings</p>
            {:else}
                <p>No Data</p>
            {/if}
        </div>

        {#if coffeeId===5}
            <p id="sold-out" class="text-[#ED735D] font-medium text-sm">Sold Out</p>
        {/if}
    </div>
</div>
```

All we have to do is place the data to their assigned seats, and make it look nice with TailwindCSS.

## Room for Improvement

I'm not certain what $bindable does in Toggle, as +page.svelte is already setting that value to the same thing. Last time I tried to remove it, things stopped working so I may be causing an error somewhere else.  
While there was no private API key for this project, I still would like to practice with keeping one hidden and I think I know just the project to work on that!  
I also want to take options out of the script inside of the main page, but since it's only two items it's fine as is. Though I feel I would prefer it in its own file if it were any larger.  
Next Project, I'm going to try and set up a basic layout so that there are better and defined boundaries. Some animations to create visual interest wouldn't hurt either.

I believe that is all for this particular project, and there will definitely be another as I would like to do more with Svelte. I found it to be pleasant, daresay fun to work with. If there is anything you've seen that can be improved or corrected, please do tell. Thank you for reading and I hope you had something to take away!

### Helpful Links / References

Radio Button Playground 1  
Radio Button Playground 2  
My devChallenge solution hosted by Netlify  
My GitHub Repo for Coffee Listing  
Reddit: The differences between +page.js, +page.server.js, and +server.js

Go to Source
