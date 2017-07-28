---
layout: post
title:  "Lesson Learned, scale THEN reset"
date:   2017-07-25 18:22:48 -0700
categories: update
project: thebutton
---

So I just spent about two days trying to figure out this issue.  
TLDR; Reset a UI element's scale BEFORE resetting it's frame or else it will scale/position incorrectly
  
For a little background and visualization, here's what a snippet of my storyboard looks like at the moment:

![My helpful screenshot]({{ site.url }}/assets/storyboard.png)

So when the user plays, they rotate through emoji. When they match an emoji, it gets scaled down and moved (by updating its frame) to the profile button. When the user wins the game they are sent to the win screen, and then sent back to the game screen. Here's what I was trying to do to reset all of the emoji:

{% highlight swift %}
var i = 0
for emoji in self.currentEmojiLabels! {
    emoji.frame = self.currentEmojiLabelInitFrames[i]
    emoji.transform = CGAffineTransform(scaleX: 1, y: 1) //scale was previously (0.1, 0.1)
    emoji.isHidden = false
    i+=1
}
{% endhighlight %}

Such a small issue for how much time I spent on it, but turns out you need to set an element's scale BEFORE setting it's scale if you want it to work correctly. I changed it to this and now it works perfectly. It should NOT have taken me this long to figure that out hahaha 

{% highlight swift %}
var i = 0
for emoji in self.currentEmojiLabels! {
    emoji.transform = CGAffineTransform(scaleX: 1, y: 1) //this has to be first
    emoji.frame = self.currentEmojiLabelInitFrames[i] //and then this
    emoji.isHidden = false
    i+=1
}
{% endhighlight %}