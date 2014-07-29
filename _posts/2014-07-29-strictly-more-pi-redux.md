---
layout: post
title: "Strictly more Pi : part 1, redux"
description: "The backgound or how I finally accepted my inner mad professor"
tags: [raspberry pi, images, google glass, wearable-tech]
share: true
---

## The Raspberry Pi Head Cam

<figure>
	<a href="/images/2014-07-28-113754.jpg"><img src="/images/2014-07-28-113754_small.jpg" alt=""></a>
	<figcaption>does my hair look big in this ?</figcaption>
</figure>

While you ponder the magnificance of the image above, let me explain how it was that I came to be sitting in my office wearing a Raspberry Pi camera sewn onto one of Claire's Accessories finest headbands.  That I'm posting it at all should answer the question that first popped into your head *"Has he no shame?"* to which the answer is a resounding **no**

It all started in Starbucks on Street Lane, as all things <a href="https://twitter.com/cgarside">Clare Garside</a> are wont to do. I can't remember exactly how the conversation went, probably as I was still hyperventilating having, to paraphrase Witnall, "gone cycling by mistake".  Anyway by the end of it I'd left with one of these

## XLoBorg

![XLoBorg](/images/XLoBorg-top-690.png) 


<a href="https://www.piborg.org/xloborg">XLoBorg</a> is a motion and direction sensor for the  Raspberry Pi.  The plan was that we, and I say <em>we</em> with the hyperventaliting caveat still fresh in your minds, would learn to dance using some of the ideas from Tim Ferriss' <a href="http://fourhourchef.com/">The 4-Hour Chef</a> in particluar exploring his ideas around Meta-Learning.

<figure class="third">
	<a href="/images/meta_learning.jpg"><img src="/images/meta_learning_small.jpg" alt="meta-learning"></a>
	<a href="/images/DSSS.jpg"><img src="/images/DSSS_small.jpg" alt="Deconstruction, Selection, Sequencing and Stakes"></a>
	<a href="/images/CaFE.jpg"><img src="/images/CaFE_small.jpg" alt="Compression, Frequency and Encoding"></a>
	<figcaption><a href="http://fourhourchef.com/">&copy; Tim Ferriss.</a></figcaption>
</figure>

So how did I end up with the rather fetching headband? I hear you ask.  It all started when Claire mentioned that the Ten Centre had a Google Glass, well at that moment the project just expanded to incorporate Glass.

## First Principles


<figure class="half">
	<a href="/images/dance-definition.png"><img src="/images/dance-definition.png" alt="Dance as defined by a simple Google Search"></a>
	<a href="/images/algorithm-definition.png"><img src="/images/algorithm-definition.png" alt="Algorithm as defined by a simple Google Search"></a>
	<figcaption>dance and algorithms.</figcaption>
</figure>

Nothing new there, indeed Mark Dorling has an excellent resource <a href="http://www.digitalschoolhouse.org.uk/algrithms/294-get-with-the-algor-rhytm">Get with the 'algor-rhytm'</a> on his <a href="http://www.digitalschoolhouse.org.uk/">digitalschoolhouse.org.uk</a> site.

We wanted to find out 

1. what a dance looked like from the dancers point of view. 
2. what a dance felt like from a dancers point of view.

The Google Glass would give us one <abbr title="Point of View">POV<abbr>, a Raspberry Pi headcam could give us the other, the same Raspberry Pi with the XLoBorg would give us a record of the motion and direction, the G force exerted on the dancer and thus from humble beginnings I give you the **"Dance, sensor, camera thingymebob Pi&#8482;"**

<figure>
	<a href="/images/IMG_20140728_113051.jpg"><img src="/images/IMG_20140728_113051_small.jpg" alt=""></a>
	<figcaption>Dance, sensor, camera thingymebob Pi</figcaption>
</figure>


<figure class="half">
	<a href="/images/IMG_20140728_110950_small.jpg"><img src="/images/IMG_20140728_110950.jpg" alt="Needles, thread, a Pi Camera and a headband"></a>
	<a href="/images/IMG_20140728_115043~2.jpg"><img src="/images/IMG_20140728_115043~2_small.jpg" alt="The finished headband"></a>
	<figcaption>My sewing skills haven't improved.</figcaption>
</figure>

## Getting the thingymebob&#8482; working

### Pi Camera

As I'd done some timelapse work with the Pi Camera before the initial plan was to capture a series of pictures from the camera on the headband and make a timelapse video out of resulting stills.  So testing was required, hence the image at the start of this post.  Testing pointed out the first real problem with the endeavour, the shutter speed was too slow and the images where blurry, oh and they were 90&ordm; off.  

So first solution involved changing the mode of the camera to **sports** which would force a faster shutter speed and adding **rot 90** to rotate the resulting image.

modififications made I ended up with `dance_capture.sh`

{% highlight bash %}
#!/bin/bash
# script to take timelaspe images of a dance using a head mounted raspberry pi camera (don't ask) rotated at 90 degrees
# verion 1.2 RBM 28/07/14 amended for dance capture, head band

# This script will run at startup and run for a set time taking images every x seconds
# it is based on the scripts found at 
# https://github.com/raspberrypi/userland/issues/84 
# and http://www.stuffaboutcode.com/2013/05/time-lapse-video-with-raspberry-pi.html


# for the format of the final images we will use  date
# the output directory will be /home/pi/dance_capture

filename=$(date +"%d%m_%H%M%S")_%04d.jpg
outdir=/home/pi/dance_pics/
length=1800000 # 1/2 hours 
rate=1000 # 1000 miliseconds = 1 second  



/opt/vc/bin/raspistill -ex sports -rot 90 -o  $outdir/$filename -tl $rate -t $length &
{% endhighlight %}


Thanks to Martin O'Hanlon's excellent <a href="http://www.stuffaboutcode.com/">&lt;Stuff   about="code" &#47;&gt;</a> blog for the scripts.

The images where still too blurred, so a Plan B was required.  

## Raspivid

Enter the 90 <abbr title="Frames Per Second">fps</abbr> mode for the Raspberry Pi camera, you can read about it on the <a href="http://www.raspberrypi.org/new-camera-mode-released/">Raspberry Pi Foundation blog</a> 
 
`dance_capture.sh` became the slightly less documented `90frames.sh` with a sleep to give the dancer time to get into position before recording.

{% highlight bash %}
#! /bin/sh
# script to capture vga video at 90fps for dance project
sleep 30
raspivid -rot 90 -w 640 -h 480 -fps 90 -t 900000 -o /home/pi/dance_pics/ninetyfps_dance.h264 &
{% endhighlight %}

As the blog explains the 90fps mode is limited to 640x480 which is more than enough for our lttle experiment.


### The resulting 1.2gb file is still being editied but it'll go here soon.

## XLoBorg

**hack** is the only word I can use to describe what I did to `xloborg.py` which came from the <a href="https://www.piborg.org/xloborg/examples">PiBorg examples</a> this snippet is my only alteration, and proper programmers will be able to spot why it took me an age to find `xloborg_results` 

{% highlight python %}
# Auto-run code if this script is loaded directly
if __name__ == '__main__':
    # Load additional libraries
    import time
    # open a file to write the results to
    f = open('xloborg_results', 'w') 
    # Start the XLoBorg module (sets up devices)
    Init()
    try:
        # Loop indefinitely
        while True:
            # Read the 
            x, y, z = ReadAccelerometer()
            mx, my, mz = ReadCompassRaw()
            temp = ReadTemperature()
            everything = (x, y, z, mx, my, mz, temp)
            everythingString = str(everything)
            # print 'X = %+01.4f G, Y = %+01.4f G, Z = %+01.4f G, mX = %+06d, mY = %+06d, mZ = %+06d, T = %+03d°C' % (x, y, z, mx, my, mz, temp)
            f.write (everythingString)
            time.sleep(0.1)
    except KeyboardInterrupt:
        # User aborted
        pass
{% endhighlight %}

`xloborg_results` did give me a 1.1MB file full of readings.

~~~
(-1.0625, -0.421875, 0.15625, 936, 85, -85, 3)(-0.828125, -0.265625, 0.265625, 930, 86, -84, 3)(-0.96875, -0.078125, 0.328125, 931, 67, -68, 3)(-1.109375, -0.046875, 0.484375, 950, 35, -61, 4)(-0.859375, -0.15625, -0.171875, 948, 23, -50, 4)(-1.046875, -0.390625, 0.4375, 942, 23, -42, 3) and so on....
~~~

which correspond to:

X, Y, Z, mX, mY, Mz, T in the python snippet above, so for the moment I'm happy that it worked.  The next challenge is to represent this data in a meaningful way, so I'll be looking at <a href="http://www.gnuplot.info/">gnuplot</a> to do that.

tbc.

