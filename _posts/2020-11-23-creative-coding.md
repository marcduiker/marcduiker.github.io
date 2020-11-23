---
layout: post
title: "How I Got Started With Creative Coding"
tags: 'creative coding'
---

This article is part of [Festive Tech Calendar 2020](https://festivetechcalendar.com/). An online event organized by the tech community with content created by many kind individuals around the world.

---

I really enjoy coding. It think it's because coding allows me to create something from nothing. That 'something' could be a useful feature for a client I work for, but it can also be something for me alone to enjoy. Sometimes the result of what I code doesn't even do anything useful. It might something that I like looking at or listening to. That sort of coding is also known as creative coding.
Here's the Wikipedia definition:

> Creative coding is a type of computer programming in which the goal is to create something expressive instead of functional. It is used to create live visuals for VJing, as well as creating visual art and design, entertainment, art installations, projections and projection mapping, sound art, advertising, product prototypes, and much more.

Source: [Wikipedia](https://en.wikipedia.org/wiki/Creative_coding)

<!--more-->

For me, creative coding means creating something enjoyable for myself and, hopefully, also for others. Creative coding can be a myriad of things, allowing you to express yourself in many different ways. I'd like to share my experience with you, and I hope you'll be inspired to try some creative coding yourself!

## Games & Graphics

My fascination with creative coding started with computer graphics. This started in my early teenage years while playing [Prince of Persia](https://en.wikipedia.org/wiki/Prince_of_Persia_(1989_video_game)). The fascination grew over time when I began to create graphics myself. I created 3D landscapes with Bryce, fractal flames using [Apophisis](https://en.wikipedia.org/wiki/Apophysis_(software)), and other recursive structures using [L-system](https://en.wikipedia.org/wiki/L-system) generators. I'm still intrigued by how well recursive rules can describe nature. Look at these computer-generated fractal weeds 😍:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/Fractal_weeds.jpg" alt="Fractal Weeds">

## Photography

For a couple of years, my attention turned to photography and photo editing. I especially enjoyed urban exploration photography, visiting abandoned locations, and capturing the beauty of buildings and interiors in decay. The atmosphere of these locations is breathtaking.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/salve_mater_urbex.jpg" alt="Indoor shot of a decayed building">

*Decayed interioir of a former sanatorium.*

For about two years, I gave photography workshops at the local community college. This experience really helped me to speak in public with more confidence.

## Visual arts

When I learned to code, during my first job, my interest in creating computer graphics spiked again. I discovered [Processing](https://processing.org/), a software sketchbook. I created many sketches while watching tutorials from Daniel Schiffman ([The Coding Train](https://www.youtube.com/user/shiffman/)). He has a gift for explaining topics on software engineering, maths, and graphics in a very accessible way.
<a href="https://www.youtube.com/user/shiffman/" target="_blank"><img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/the_coding_train.jpg" alt="The Coding Train on YouTube"></a>

I really recommend [The Nature of Code playlist]( https://www.youtube.com/playlist?list=PLRqwX-V7Uu6aFlwukCmDf0-1-uSR7mklK), which features 83 videos ranging from Perlin noise, my favorite type of noise, to fractals and genetic algorithms.

<iframe width="600" height="600" src="https://editor.p5js.org/marcduiker/embed/LMF39YZcR"></iframe>

*[Noise worms](https://editor.p5js.org/marcduiker/sketches/LMF39YZcR): move your mouse (or tap) over the image to change the noise pattern.*

## Retro game development

I excel in starting new projects and never finishing them 😅. Sometimes I don't even start them at all 😬. About one and a half years ago, I read about retro game development using [PICO-8](https://www.lexaloffle.com/pico-8.php). I bought the software with the intent to create some cool games, but I didn't touch the software for a couple of months. Then, I read about PICO-8 again, this time in a [MagPi magazine](https://magpi.raspberrypi.org/articles/build-retro-game-pico-8-raspberry-pi). At about the same time, Scott Hanselman invited Joseph White, the creator of PICO-8, on one of his [podcasts](https://hanselminutes.com/703/tiny-games-with-the-pico-8-fantasy-console-and-joseph-white). This clearly was a sign I had to pick up PICO-8 and create a game with it. Since I am a big fan of Azure Functions, I created a game in which you play the Azure Functions logo and need to collect items to restore the power of an Azure data center.

<a href="https://marcduiker.itch.io/azure-functions-the-game" target="_blank"><img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/azure_functions_the_game.gif" alt="Animated gif of Azure Functions, The Game"></a>

*Azure Functions, The Game*

Creating this puzzle game took some effort, I spent many evenings and nights over a couple of weeks, but the process was so much fun! PICO-8 is a very restricted environment to create games in, the screen is only 128x128 pixels, sprites are 8x8 pixels, and there's even a restriction on the amount of code you can write. Once it was finished, I was thrilled the game was well received in the community, the Azure Functions team and others within Microsoft 😊.

My next game, YuckyYAML, is still in progress. It is a puzzle game, inspired by Sokoban, and is themed around Docker & Kubernetes, so it features whales and containers.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/yucky_yaml_3.gif" alt="Animated gif of the Yucky YAML game (work in progress).">

*Yucky YAML game (work in progress).*

PICO-8 is also an excellent platform to create mini-games. These are very basic games you can play in a minute or so. I created two interactive cards for a lovely 5-year-old, one for his birthday, where he needs to collect all the balloons. And another one for Easter, where he needs to collect eggs. These mini-games are around 100 lines of code and take only an hour or two to create.

You can play the above mentioned games at [marcduiker.itch.io](https://marcduiker.itch.io/).

If you want to learn about game development with PICO-8, I highly recommend [this YouTube playlist](https://www.youtube.com/playlist?list=PLea8cjCua_P0qjjiG8G5FBgqwpqMU7rBk) which contains over 70 videos on how to create a breakout game from start to finish.

## MS Build

I was very fortunate to be part of MS Build this year. First, I took part in a panel discussion on Serverless APIs, and second, I talked about one of my pet projects, the [Azure Functions Updates Twitterbot](https://twitter.com/az_func_updates).

### 8-bit avatars

 After my talks, I wanted to show appreciation to the hosts since I know it's a lot of effort to keep an event rolling smoothly. So I started creating 8-bit pixel portraits for the hosts. I used a 16x16 pixel format, which is really limited, so it was a challenge drawing everyone in a way they were still recognizable. I posted them on Twitter, and the responses I got were so kind. I kept making the portraits until I had one for each of the hosts. At the end of Build, the hosts gave me a shout-out for my work, which made me very proud. It was a wonderful experience to contribute to Build in this way 😊.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/Build_hosts_900.jpg" alt="Build 2020 hosts, as 8bit avatars.">

*Build 2020 hosts, as 8bit avatars.*

### Make Code

One of the last sessions of MS Build was about MakeCode, an educational programming platform backed by various organizations. During [this session](https://mybuild.microsoft.com/sessions/a1638103-16a8-4059-90ac-54c7e0dda8a2?source=sessions), Louanne Murphy and Scott Hanselman showed how versatile and accessible the platform is. One of the areas where you can use MakeCode is creating [arcade games](https://arcade.makecode.com/), so obviously, I had to try that. In about 1.5 hours, I completed this mini-game featuring Dona and Seth, competing to eat as many Cheeze-Its within 30 seconds.The game is far from perfect, but the process of making this was a lot of fun!

<a href="https://arcade.makecode.com/30800-46845-98441-86480" target="_blank"><img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/Cheeze-it_Deth_Match.jpg" alt="Screenshot of Cheeze-It Deth Match, a minigame made with MakeCode."></a>

*[Cheeze-It Deth Match](https://arcade.makecode.com/30800-46845-98441-86480) on MakeCode.*

## Post Build

I was really starting to like the 8-bit avatars, so I kept on making some for close (Twitter) friends. Others began to notice the avatars, and the demand kept increasing. Since I'm saving for a Surface Go 2, I decided to ask for a small donation when people ask me to create their avatar. So far, I've created 120 of these avatars, and I'm still receiving commissions.
Since the avatars I created for Build, I've doubled the resolution, I'm now using 32x32 pixels, which is still very limited but allows a bit more detail in hair and clothes. I'm using a limited 32 color palette, which ensures all portraits have the same look & feel.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/11/23/avatars_collage_107frames.gif" alt="Animation of various avatars I've made.">

## What's next

Creative coding has really become a part of my life. It is ever-changing, sometimes it's visuals, sometimes music, sometimes games. As long as I can keep creating something that I (and others) like, it gives me a very satisfying feeling, and I will continue doing this for a long time.

I encourage you to ty creative coding yourself! It doesn't matter if you are a programming veteran or a novice, you can start very small and build up your skills over time and try different tools and languages. I'm very curious about your creations! Please share them with me on Twitter [@marcduiker](https://twitter.com/marcduiker).

If you want to support my creative coding work, please consider a [donating a coffee](https://ko-fi.com/marcduiker) or commission me to create your personalized 8-bit avatar 😊, I draw them entirely by hand.
