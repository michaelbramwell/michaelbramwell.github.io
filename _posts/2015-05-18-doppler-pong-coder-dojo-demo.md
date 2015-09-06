Post - Doppler Pong Coder Dojo Demo
============================

Draft - In progress
-------------------

Abstract
--------
todo...

Anyone can do HTML5 Audio
-------------------------
Put this in your console

<pre>
var msg = new SpeechSynthesisUtterance("Oh, so they have the Internet on computers now!");			
speechSynthesis.speak(msg);
</pre>

You can also change the voice, the pitch, speed and culture.

<pre>
var msg = new SpeechSynthesisUtterance("This perpetual motion machine Lisa made today is a joke. It just keeps going faster and faster");			
msg.voice = speechSynthesis.getVoices().filter(function(voice) { return voice.name == 'Zosia'; })[0];
msg.rate = 1
msg.pitch = 0.3
speechSynthesis.speak(msg);
</pre>

Notice how we filter on getVoices() array. There are plenty to choose from.

Most importantly you can make noise. Here we create a really annoying siren sound using the AudioContext to create two oscillators that change frequency according to the mouse x and y coordinates on mousemove.

<pre>
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
</pre>

See the function with extra parentheses wrapped around the opening and closing of the code block

<pre>
(function(){
  // codes
}();
</pre>

That is called a self execution function which is why the code, when pasted into your browser console, will be immediately executed.

Doppler Pong
------------

Every heard of something called the Doppler effect? Its the change of frequency in audio when a sound is compressed as an object moves closer to the source of the sound, or as the source of the sound gets closer to the object hearing the source. For exampler the sound of an Ambulance siren as it gets closer. Here is a good [example](https://www.youtube.com/watch?v=imoxDcn2Sgo) in glorious 144p.

As we know HTML5 has an Audio Context that can output a sound at a constant frequency and it can also analyse sound coming back through the microphone. If we can measure the difference between the frequency emitted and recieved due to the compression of the sound wave resulting from say a hand gesture we could do something pretty neat, like say for instance 'Doppler Pong'.

Fortunately we live in the age of open source code where alot of amazing people make their projects freely available on sites like github for anyone to view and reuse.   

Lets take this [pong game](https://github.com/dasmikko/dasmikko.github.io/tree/master/public/games/pong-game) and this bit of [doppler code](http://danielrapp.github.io/doppler/), mash it together and see what we get.

First we need to run the Dopple Pong game website, which you can git clone from [here](https://github.com/michaelbramwell/dopplerPong.git) so start up a webserver, any will do.

<pre>
python -m SimpleHTTPServer
</pre>

Its worth having a look at doppler.js under the assets dir even if you don't understand eveything that is going on (I know I don't). Like the above simple examples an oscillator is generated at a constant frequency, inaudible to the human ears at 20000 khz, and anaylised as is read back via the microphone. Also have a quick look at the game code, most of the action is in game.js which replies heavily on a physics library for things like world creation, collision detection and particle creation. Like we are the doing the Pong game is also relying on another open source project to do alot of the heavily lifting. Open source is good like that :) .

First thing we need to do is initiate the doppler library in the game code. In index.html add the following code.

<pre>
window.addEventListener('load', function() {
	window.doppler.init(function(bandwidth) {
		var threshold = 4;
		if (bandwidth.left > threshold || bandwidth.right > threshold) {
			window.dopplerBandwidth = bandwidth;
		}
	});
});
</pre>

By calling doppler.js init function we now have access to its bandwidth object which contains left and right properties. The property with the highest count determines which direction to send the paddle, i.e up or down. While we are at it the Doppler.js code is a nice example of how to structure an object in javascript. Something Andy Osmani call [Immediately-invoked Function Expressions (IIFE)s](http://addyosmani.com/blog/essential-js-namespacing/).

The rest of our code goes in game.js which is where most of the game logic lies. Lets start by adding some properties to the constructor.

<pre>
var dopplerEasingUpCount;
var dopplerEasingDownCount;
var dopplerPaddleDirection;
</pre>

Then initialise them some default values. The variable names gives a good clue to what we will use them todo.

<pre>
this.dopplerEasingUpCount = 100;
this.dopplerEasingDownCount = 100;
this.dopplerPaddleDirection = 'up'
</pre>

We also need to put these three lines in the reset function.

The rest of code is in the update function which fires at 60 times/frames per second which is required for smooth game playback. The first thing we need to do here is make the left panel computer controlled so we only have to worry about controlling the right paddle.

<pre>
this.paddle1.body.y = this.ball.y;
</pre>
 more update code...

