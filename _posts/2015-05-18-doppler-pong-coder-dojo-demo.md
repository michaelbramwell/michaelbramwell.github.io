---
layout: post
title: 'Doppler Pong' Demo for the Mt Lawely CoderDojo
tags:
  - Coderdojo
  - Doppler
  - Pong
  - HTML5
  - Javascript
---

{{ page.title }}
================

Draft - In progress
-------------------

Abstract
--------
todo...

Anyone can do HTML5 Audio
-------------------------
Put this in your console

{% highlight JS %}
var msg = new SpeechSynthesisUtterance("Oh, so they have the Internet on computers now!");			
speechSynthesis.speak(msg);
{% endhighlight %}

You can also change the voice, the pitch, speed and culture.

{% highlight JS %}
var msg = new SpeechSynthesisUtterance("This perpetual motion machine Lisa made today is a joke. It just keeps going faster and faster");			
msg.voice = speechSynthesis.getVoices().filter(function(voice) { return voice.name == 'Zosia'; })[0];
msg.rate = 1
msg.pitch = 0.3
speechSynthesis.speak(msg);
{% endhighlight %}

Notice how we filter on getVoices() array. There are plenty to choose from.

Most importantly you can make noise. Here we create a really annoying siren sound using the AudioContext to create two oscillators that change frequency according to the mouse x and y coordinates on mousemove.

{% highlight JS %}
(function(){
	var context = new AudioContext(),
    osc = context.createOscillator(),
    osc2 = context.createOscillator(),
    gain = context.createGain(),
    w = window.innerWidth,
    h = window.innerHeight;
  
  osc.frequency = 400;
  osc.type = 'sawtooth';
  osc.connect(context.destination);
  osc.start(0);
  
  gain.gain.value = 100;
  gain.connect(osc.frequency);
  
  osc2.frequency.value = 1; 
  osc2.type = 'square';
  osc2.connect(gain);
  osc2.start(0);
  
  document.addEventListener("mousemove", function(e) {
    osc.frequency.value = e.clientY / h * 1000 + 200;
    osc2.frequency.value = e.clientX / w * 30 + 1;
  });
})();
{% endhighlight %}

See the function with extra parentheses wrapped around the opening and closing of the code block

{% highlight JS %}
(function(){
  // codes
}();
{% endhighlight %}

That is called a self execution function which is why the code, when pasted into your browser console, will be immediately executed.

Doppler Pong
------------

Every heard of something called the Doppler effect? Its the change of frequency in audio when a sound is compressed as an object moves closer to the source of the sound, or as the source of the sound gets closer to the object hearing the source. For exampler the sound of an Ambulance siren as it gets closer. Here is a good [example](https://www.youtube.com/watch?v=imoxDcn2Sgo) in glorious 144p.

As we know HTML5 has an Audio Context that can output a sound at a constant frequency and it can also analyse sound coming back through the microphone. If we can measure the difference between the frequency emitted and recieved due to the compression of the sound wave resulting from say a hand gesture we could do something pretty neat, like say for instance 'Doppler Pong'.

Fortunately we live in the age of open source code where alot of amazing people make their projects freely available on sites like github for anyone to view and reuse.   

Lets take this [pong game](https://github.com/dasmikko/dasmikko.github.io/tree/master/public/games/pong-game) and this bit of [doppler code](http://danielrapp.github.io/doppler/), mash it together and see what we get.

First we need to run the Dopple Pong game website, which you can git clone from [here](https://github.com/michaelbramwell/dopplerPong.git) so start up a webserver, any will do.

{% highlight Py %}
python -m SimpleHTTPServer
{% endhighlight %}

