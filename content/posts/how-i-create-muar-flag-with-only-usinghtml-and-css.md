---
title: "How I Create Muar Flag with only usingHTML and CSS"
date: 2025-01-19
tags: 
  - "ce1126"
---

![Muar city](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fz0pesyxnmwrooftj7uxq.jpg)

Muar also known as Bandar Maharani is a small town in Johor and is one of the most popular spots to visit in Malaysia.

![This is My top hidden gem to breakfast](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fjfcu4dlzcd082drpv1mk.jpg)

This is My top hidden gem to breakfast

In order To honor my hometown, I’ve decided to create the Muar flag using only HTML and CSS (doing my best here). I’ll avoid using design tools, SVG files, and definitely not any images.

![muar-flag](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5f6goobatkkazcodnwf0.png)

The flag of Muar consists of an orthogonally quartered design.

![first flag quater](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdbfo93jd4zjcfwtf0oo4.png)

The first quarter is red with a white five-pointed star and crescent.

![Second flag quater](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fn14hfrkcie43kks2zovv.png)

The second and third quarters are black, while the fourth quarter is yellow with a red five-pointed star and crescent.

### Step 1: Building the HTML Framework for a Flag Design

First, I need to create the HTML framework for the flag design. It’s pretty simple: I’ll use a `<div class="flag">`to create a main container that holds the flag structure.

Next, inside the “flag” container, I’ll divide it into two rows using `<div class="row">`

For each row, I’ll further split it into two columns using  
  
`<div class="column">`  

```
  <div class="flag"> 

    <div class="row">
      <div class="column">
  <div class="red-1-left"></div> 
  <div class="cresent-1"></div>
    <div class="star-1"></div>
  </div>

  <div class="column">

    </div>
  </div>

 <div class="row">
  <div class="column">
  </div>

  <div class="column">
  <div class="yellow-2-right"></div>
  <div class="cresent-2"></div>
  <div class="star-2"></div>
  </div>
</div>
</div>
</div>
```

This ensures we get a clean layout with two sets of rows and columns.

![image layout](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyadl6ltg0wd8p2ja1m7w.jpg)

Now that we have the columns and rows set up, I’ll add another div for each combination of rows and columns and assign specific class names:

Set 1: red-1-left `<div class="red-1-left">`  
Set 2: yellow-2-right `<div class="cresent-1">`

### Step 2: Create the Layout System

This is where the magic happens. For the layout system, I’ll be using Flexbox instead of CSS Grid since it’s a straightforward layout approach.

Next, I’ll apply the Flexbox system and arrange the items in a horizontal direction within the “row” class. To achieve this, I’ll use:  

```
.row {
  display: flex;
  flex-direction: row;
}
```

\-For the column system, where the parent is the “row” class, I need to ensure that the columns stretch evenly and that the shapes inside each column are properly positioned. To accomplish this, I’ll use:  

```
.column {
  display: flex;
  flex-direction: column;
  flex: 1;
}
```

### Step 3: Defining sections

![Flag sections](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fvhfjwyc3acozez95023p.jpg)

I split into four sections

To make the design process more understandable for everyone, I will first define variables for each color by assigning them their respective hex code values. Here is the list of variables:  

```
root {
  --red : #ce1126;
  --white : #ffffff;
  --yellow : #ffff00;
  --black : #000000;
```

This is where the magic happens. I’ll begin by defining the parent class for the entire flag using the following:

I need to set a background color of black to the entire flag.

It would be more easier because we don’t have to make another 2 `<div>`

Then, set the width to 800px and the height to 400px to create the base rectangle according to the flag standard size  

```
.flag {
  background-color: var(--black);
  width: 800px;
  height: 400px;
}
```

Now we have our black rectangle. All I need to do is to define the other 2 background colors of yellow and red.

And I set the height to 200px where it was the half of the full height.  

```
.red-1-left{
  background-color: var(--red);
  height: 200px;
}

.yellow-2-right {
  background-color: var(--yellow);
  height: 200px;
}

```

### Step 4: Crescent

How do I made the crescent? Well, first of all, I need to create a perfect round of circle. to achieve this, I need to make the border radius to 50%

Now It’s time to define the height to 130px and width of 150px.

\-set the margin left to 120px  
\-Set the margin top to 35px

for final touch of the crescent, I'm using box shadow property with adjusting the width and height value until the shape circular by rounding its edges.

Box shadow property also allowed us to define the background color.

`box-shadow: -15.5px 0px 0px 6px var(--white);`

\-15.5px (horizontal offset): The shadow is moved slightly left to create a crescent-like curve.  
\-0px (vertical offset): The shadow is aligned vertically with the element.

\-0px (blur radius): No blur is applied,

\-6px (spread radius): Expands the size of the shadow outward, further defining the crescent shape.  

```
.crescent-1 {
  position: absolute;
  width: 150px;
  height: 130px;
  margin-top: 35px;
  margin-left: 120px;
  border-radius: 50%;
  box-shadow: -15.5px 0px 0px 6px var(--white);
}
```

### Step 5: Star

This is the trickiest part. The star rotation is little bit slanted. for this one, I'll be creating using clip-path property with polygon function.

.star-1 CSS rule creates a star shape using the clip-path property with the polygon() function. Here's a breakdown of the key components:

\-set the size to 114X120px.  
\-aspect-ratio: select to 1. To ensures the star is roughly proportional, maintaining a uniform shape regardless of container size.  
Positioning:  
\-Adding the position to absolute to places the star in a fixed position relative to its nearest positioned parent.  
margin-top: 42px; margin-left: 170px; defines its offset.

For Styling, I add on:

\-background-color: var(--white); makes the star white or whatever color is assigned to the --white CSS variable.

\-transform: rotate(-13deg); rotates the star to make it slightly slanted.

The trickiest part is to define the percentages that represent x and y positions relative to the element's size.  

```
.star-1 {
  width: 114px;
  height: 120px;
  aspect-ratio: 1;
  position:absolute;
  margin-top: 42px;
  margin-left: 170px;
  background-color: var(--white);
  transform: rotate(-13deg);
  clip-path: polygon(79.39% 90.45%,50.00%
  69.00%,20.61% 90.45%,31.93% 55.87%,2.45%
  34.55%, 38.83% 34.63%,50.00% 0.00%,61.17%
  34.63%,97.55% 34.55%,68.07% 55.87%);
}
```

### Result

<iframe height="600" src="https://codepen.io/21leo4l/embed/qEWyKmr?height=600&amp;default-tab=result&amp;embed-version=2"></iframe>

There we go! I have successfully created the Muar flag using only HTML and CSS.

Anyway, it's time to get some Mee Bandung! See you in the next post!

Go to Source
