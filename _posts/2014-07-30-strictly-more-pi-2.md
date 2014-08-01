---
layout: post
title: "Strictly more Pi : part 2"
description: "The completed videos"
tags: [raspberry pi, images, google glass, wearable-tech, dance, code]
image:
  feature: aoife_dance_banner.jpg
  credit: Aoife Martin
  creditlink: /images/aoifes_dance_small.png
share: true
---

Mark and Ashleigh danced 3 times for us; a Viennese Waltz, a Tango and something Richard Claydermanesque. I only know the first from the movies, the second from The Gotan Projects’ *“La Revancha Del Tango”* while the third seems inexplicably and inescapably linked to the 80's melodrama "The Thorn Birds" staring with Richard Chamberlain, go figure, and here you thought I was going all "high brow."


The Pi **“Dance, sensor, camera thingymebob Pi™“** was a one shot deal, the scripts started on boot and ran until I manually stopped them by connecting an ethernet cable and `ssh`’ing back into the Pi and running `sudo /etc/init.d/dance90fps stop` and `sudo /etc/init.d/xloborgdata stop`.  Not very elegant at all, but it would do for a first run.

## The Viennese Waltz 

<iframe width="560" height="315" src="//www.youtube.com/embed/RhPLQcg4-ro" frameborder="0" allowfullscreen></iframe>
As seen from Claire's iPad

<iframe width="560" height="315" src="//www.youtube.com/embed/g8en82i9y2o?start=20&end=78" frameborder="0" allowfullscreen></iframe>
As seen from Mark's Google Glass

## The Tango 

<iframe width="560" height="315" src="//www.youtube.com/embed/FDhmOY-LR18" frameborder="0" allowfullscreen></iframe>
As seen from Claire's iPad

<iframe width="420" height="315" src="//www.youtube.com/embed/HrVtRT04x18" frameborder="0" allowfullscreen></iframe>
As seen from Ashleigh's **“Dance, sensor, camera thingymebob Pi™“**


## The Final Dance 

<iframe width="560" height="315" src="//www.youtube.com/embed/Adf8hiVkpLk" frameborder="0" allowfullscreen></iframe>
The Final Dance as seen from with Claire's iPad

<iframe width="560" height="315" src="//www.youtube.com/embed/cgfP9Vx8tmY?start=21&end=170" frameborder="0" allowfullscreen></iframe>
The Final Dance as from with Mark's Google Glass

<iframe width="420" height="315" src="//www.youtube.com/embed/XbNy87KABt8" frameborder="0" allowfullscreen></iframe>
The Final Dance as seen from with Ashleigh's **“Dance, sensor, camera thingymebob Pi™“**

The thing to remember is that Ashleigh's headcam was recording at 90fps and even then it stuggled to capture the speed of her motion as they performed. In the next post (i.e. when I work out how to do it) I'll post the data captured from the XLoBorg and complete the triptych.

### Reflections aka Things I wish I'd thought of at the start 

My kit bag would have included the following:

1. a pen
2. a notepad
3. a stopwatch

It turned out to be rather more time consuming than I thought finding the start and end points of each performance in the 1.2GB 90fps file, and a basic note of start/stop times would have made the last few days a lot easier. 

It would also be *very* useful to have a working video-editing software, because Linux. Pitivi did it's usual trick of pretending to start and atempting to import the raw footage only to vanish with a *"What'd you expect?"* Kdenlive, pulled in a monster truck of dependancies and took an absolute age to render. These are both really cool open source projects but they're not there yet. 

So in the end my editing workflow looked something like this.

<figure>
	<a href="/images/how-not-to-edit.jpg"><img src="/images/how-not-to-edit.jpg" alt="Handwritten scrawl of timing details"></a>
	<figcaption>How not to edit.</figcaption>
</figure>

followed by commands like 

{% highlight bash %}
ffmpeg -ss 0 -t 00:04:32 -i ashleigh_clayderman.mp4 ashleigh_clayderman_2.mp4
{% endhighlight %}

Which is the polar opposite of non-destructive editing, but "you live and learn"
