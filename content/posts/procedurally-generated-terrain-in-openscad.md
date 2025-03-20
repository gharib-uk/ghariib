---
title: "Procedurally Generated Terrain in OpenSCAD"
date: 2025-01-15
---

![](https://hackaday.com/wp-content/uploads/2025/01/scadterrain_feat.jpg?w=800)

We’re big fans of OpenSCAD here at Hackaday — it’s free and open source software, runs on pretty much anything, and the idea of describing objects via code seems like a natural fit for producing functional parts. Rather than clicking and dragging elements on the screen, you can knock out a quick bracket or other simple component with just a few lines of code. But one of the things we don’t often get a chance to showcase is the incredible potential of generating 2D and 3D objects algorithmically.

In a recent Reddit post, \[ardvarkmadman\] dropped an extremely impressive snippet of OpenSCAD code that he calls TerrainGen. In fewer than fifteen lines of code, it’s able to create randomized “islands” which range from simple plateaus to craggy mountain ranges. After dropping the code in the OpenSCAD editor, you can just keep hitting F5 until you get a result that catches your eye. This seems like an excellent way to generate printable terrain elements for gaming purposes, but that’s just one possibility.

```
r1=rands(0,1,1)[.1];
r2=rands(0,1,1)[.2];

for (j=[1:.25:10])
    color(c=[j/10,r2,r1,1])
    linear_extrude(j/r2)

offset( -j*2)
for(i=[1:.25:20]){
random_vect=rands(0,50,2,i/r2);
   translate(random_vect*2)
    offset(i/j)
     square(j*1.5+i/1.5,true);
 }

```

So what’s happening here? The code generates several random numbers and uses those to define the height and position of an array of points that are used to make the final piece of terrain. When creating functional parts in OpenSCAD, we’re almost elusively dealing with very specific parameters, so it’s interesting to see how easily you can tweak objects just by sprinkling in some random values.

Inspired by the positive response to TerrainGen received, another user by the name of \[amatulic\] chimed in to share a similar project they’ve been working on. The code is able to generate blocks of terrain based on the dimensions and seed value provided by the user, and even simulates realistic weathering and erosion. This approach is far more computationally intensive, and requires a few hundred lines of code, but the results are undeniably more realistic. There’s a blog post that deep-dives into the math behind it all, if you’re looking for some light reading.

![](https://hackaday.com/wp-content/uploads/2025/01/scadterrain_detail.jpg)

Although it’s probably not something we’d personally get much use out of, we think the ability to randomly generate 3D models like this is absolutely fascinating. We’d love to hear what readers think about these techniques, especially in regards to potential applications for them.

Go to Source
