---
title: "Brick Layer Post-Processor, Promising Stronger 3D Prints, Now Available"
date: 2025-01-23
---

![](https://hackaday.com/wp-content/uploads/2025/01/brickslice_feat.jpg?w=800)

Back in November we first brought you word of a slicing technique by which the final strength of 3D printed parts could be considerably improved by adjusting the first layer height of each wall so that subsequent layers would interlock like bricks. It was relatively easy to implement, didn’t require anything special on the printer to accomplish, and testing showed it was effective enough to pursue further. Unfortunately, there was some patent concerns, and it seemed like nobody wanted to be the first to step up and actually implement the feature.

Well, as of today, \[Roman Tenger\] has decided to answer the call. As explained in the announcement video below, the company that currently holds the US patent for this tech hasn’t filed a European counterpart, so he feels he’s in a fairly safe spot compared to other creators in the community. We salute his bravery, and wish him nothing but the best of luck should any lawyer come knocking.

So how does it work? Right now the script supports PrusaSlicer and OrcaSlicer, and the installation is the same in both cases — just download the Python file, and go into your slicer’s settings under “Post-Processing Scripts” and enter in its path. As of right now you’ll have to provide the target layer height as an option to the script, but we’re willing to bet that’s going to be one of the first things that gets improved as the community starts sending in pull requests for the GPL v3 licensed script.

There was a lot of interest in this technique when we covered it last, and we’re very excited to see an open source implementation break cover. Now that it’s out in the wild, we’d love to hear about it in the comments if you try it out.

<iframe loading="lazy" title="Bricklayers for Stronger 3D-Prints now Opensource!" width="800" height="450" src="https://www.youtube.com/embed/EqRdQOoK5hc?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Thanks to \[greg\_bear\] in the Hackaday Discord for the tip.

Go to Source
