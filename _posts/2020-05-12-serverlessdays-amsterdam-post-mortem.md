---
layout: post
title: "ServerlessDays Amsterdam, a personal post-mortem"
tags: azure live-streaming serverlessdays
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/00-desk.jpg" alt="My deskat the start of the conference.">

It is now May 12th, four days after the ServerlessDays Amsterdam 2020 virtual conference. The conference was incredibly fun and slightly terrifying at the same time.

I had two roles during the conference; in the morning, I would do the hosting with Lian Li (who really has a talent for MC-ing), and during the entire day, I would be responsible for the technical part. In specific that means that I'd be calling in speakers & panelists into the Skype group call, and controlling the streaming software (OBS), switching scenes, configuring webcam sources, shared screens, names, and starting the prerecorded sessions.

<!--more-->

I gained some practice with live streaming over the last month since we did two virtual ServerlessDays Meetups. Still, I knew the conference would be a next-level challenge, and it sure was 😅. So I spent most of my free time over the last three weeks on perfecting my streaming setup and preparing a detailed script that I could use during the conference.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/01-script.jpg" alt="First few lines of the script for the conference.">

*First few lines of the script for the conference.*

## May 7th

I want to highlight some of the work I did before the conference day because this saved me quite some time and effort on the day itself.

### Prerecorded sessions

Since half of the speakers provided us with a prerecorded session (thank you!), I put those recordings in a VLC Media Source playlist in OBS with 5-second bumpers in between. When a new recorded session would start, I  only needed to remove the first recording from the playlist, and I was good to go.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/02-vlc-media-source.jpg" alt="The VLC Media source in OBS with all the prerecorded videos.">

*The VLC Media source in OBS with all the prerecorded videos.*

An annoying thing about the VLC Media Source (and the regular Media Source) is that you don't see the remaining playtime of the video. Having these times is essential if you want to give a heads up to the hosts, and speaker. To fix this, I added timers for each prerecorded session to my StreamDeck. So when I switched to the OBS scene with the VLC Media Source, I would also press the corresponding timer on the StreamDeck, which started a countdown.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/03-streamdeck-timers.jpg" alt="Using timers for each recorded session.">

*Using timers for each recorded session.*

### Text Files & Chatlog Mode

Something I didn't want to spend much time on during the day itself would be configuring the speaker's names in OBS. I was looking into the read from file option, and then I noticed the Chatlog Mode. With this option enabled, with a row count of 1, only the last line is used as the input for a text field. So I put all the speaker names in a text file in reverse order and used that as the source for the speaker name field in OBS. When a new speaker was joining, I only needed to remove the last line and save the file.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/04-chatlogmode.jpg" alt="Using text files with chatlog mode enabled.">

*Using text files with chatlog mode enabled.*

## May 8th, the conference day

### 7:00

My day started at a reasonable time. I woke up at 7:00, took a shower, had some breakfast, and made *a lot of coffee* ☕. I also prepared some lunch, snacks, and fruits to take with me upstairs since I would spend my entire day in my home office in the attic. I did allow myself some bathroom breaks though.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/05-food.jpg" alt="Refreshments for during the day.">

*Refreshments for during the day.*

### 8:00 AM

Once settled behind my laptop, I RDP-ed to the virtual machine in Azure, which is the heart of the streaming setup. It's an NV6 machine, GPU enabled, and with enough CPU and memory to use for streaming.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/06-obs.jpg" alt="The OBS setup running in an Azure VM.">

*The OBS setup running in an Azure VM.*

Since we're using a combination of Skype, NDI Tools, and OBS, I started a Skype group call on the VM and invited myself, so I call in from my laptop. 

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/07-skype-group.jpg" alt="The Skype group call">

*The Skype group call.*


I verified the webcam source in OBS since I was also doing a hosting role in the morning. I once more checked the order of the speaker names in the text file, and the prerecorded sessions in the playlist. I also checked the scene transitions using my StreamDeck since I updated the software earlier this week (risky, I know). 

### 8:30

I invited Lian to the group call and asked her to share her screen so I could set up the sources in OBS.

### 8:45

Our keynote speaker, Ant Stanley, joined the group call. We three had a nice and relaxed chat while I was setting up the sources in OBS. Ant was going to give his session live, so we needed to make sure screen sharing was working. I added his webcam feed to the screen sharing scene so the audience could see his face during his session (more on this later). Lian quickly asked Ant for a fun fact she could use for his intro.

### 8:50

I started the 10 min countdown timer and started streaming. We were streaming simultaneously to YouTube, Twitch, and Periscope (using Restream). There were already some people waiting on YouTube, amazing!

### 9:00

Just before 9:00, I started counting down out loud so Lian would know when the exact moment of go-live would be. As soon as the timer reached 0:00, I switched to the scene with webcam feeds of Lian and me, and we are [LIVE](https://www.youtube.com/watch?v=cqaewpYtYTA&t=595s) 🎉. Lian did most of the intro work here, as agreed upfront, and she did great! There was an issue with Lian her webcam feed, unfortunately. The entire frame was resizing now and then. The resizing kept on occurring during the whole day and also happened to a few others. I don't know the cause yet, could be Skype, NDI Tools, or the OBS plugin. I was quite distracted by this and tried to rescale the webcam source whenever this happened, it felt like a continuous battle 😟.

- Issue 1: Uncontrolled resizing of webcam/NDI source.
- Solution: ~~unknown~~, -- [UPDATE May 14th] -- 
Thanks to the Twitterverse I now know the source of the issue and the solution! NDI Tools scales the video feed based on available bandwidth. Apparently everyone knew this but me! 😅 Thanks to [Maarten Balliauw](https://twitter.com/maartenballiauw) and [Henk Boelman](https://twitter.com/hboelman), who already [wrote about this](https://www.henkboelman.com/articles/online-meetups-with-obs-and-skype/). The fix is to apply a Transform in OBS to prevent the scaling as is [described here](https://support.skype.com/en/faq/FA34853/what-is-skype-for-content-creators). Note to self: RTFM! 😂

### 9:15

Ant started his keynote session, and I switched to the scene with his shared screen and his webcam feed. It became apparent quickly that there was not a single good spot for this small webcam inside the shared screen feed since Ant's slides were ever-changing. I moved his webcam feed around during this presentation, which must have looked hilarious to the audience 😂.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/08-ant.jpg" alt="Ant being moved from right to left during his presentation.">

*Ant being moved from right to left during his live presentation.*

- Issue 2: Webcam source hides info on slides.
- Solution: Don't compose a scene where the webcam source is overlaying the presentation source. Or instruct speakers to leave some space available for their webcam feed. 

### 9:50-12:15

After Ant's keynote, there were three prerecorded sessions, each with live Q&A from Tomasz Konieczny, Riccardo Mocchetti, and Aymen Chetoui. About five minutes before each session, I would add the speaker to the Skype group call, set up their webcam source, update the VLC Media Source playlist, and update the speakers.txt file. All these sessions went quite smoothly. A live ten-minute lightning session with Ebru Cucen followed without me requiring to move around her webcam source all the time, thank you, Ebru! 😄

### 12:15

Just before 12:15, the members of the expert panel joined the group call. I set up their webcams/NDI  sources in the scene called Panel OBS. This scene was new and hadn't been tested during any of the meetups. And this became quite evident since I forgot to mute the panelists' audio sources, which resulted in a loud and annoying echo (sorry for your ears!). I received multiple warnings from the team and attendees, so this was fixed relatively quickly. It could have easily been prevented if I had tried the scene with the four panelists in advance. Besides the audio issue, the resizing issue was also prominent in this scene, for both Lian and Sara's webcams. The panel discussion itself went very well, but due to the technical issues, I was too distracted to follow it closely. 

- Issue 3: Echo due to multiple audio sources in an untested scene.
- Solution: Always do a trial run with scenes that use multiple NDI sources.

### 12:40

Time for the lunch break! Even though I had prepared my lunch up-front and took it with me upstairs, I really enjoyed being able to have a small break. At this moment, I was still annoyed with the weird resizing webcam issues, so it was good to have a short break for my laptop. Lian dropped out of the call because we would now switch over to Floor Drees and Marek Kuczynski as the hosts. I invited them to the group call, set up their webcam sources, and updated the hosts' names in OBS to match theirs. I also added the next speaker, Sven Al Hamad, to the group call and configured his prerecorded session and name. So far, so good.

### 13:20

When the break was almost over, I counted down out loud again, so Floor and Marek would know the exact moment of go live. I LOL'd about the introduction Floor gave when she mentioned that Marek would explain why they are 'arch enemies' 🤣. Floor works at Microsoft and Marek at AWS. I love the fact that they could host together, it shows we have a fun and respectful community 🧡.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/08-floorandmarek.jpg" alt="Hilarious intro by Floor and Marek.">

*Hilarious intro by Floor and Marek.*

### 13:20 - 15:25

The next three sessions from Sven Al Hamad, Josh Carlise, and Farrah Campbell were all prerecorded and went pretty smoothly, at least from the perspective of the audience. My Skype call actually dropped during the prerecorded session by Sven. It took me about 1 minute to reconnect again, nobody noticed, but this freaked me out quite a bit! 😱

- Issue 4: Skype call dropped unexpectedly.
- Solution: Don't use Skype for events that last several hours?

At the start of Josh his session, I was a bit too slow with updating the speaker name source in OBS. Also, Josh's audio was not working initially, but it was all fixed quickly. His session ended pretty hilariously thanks to a question from Floor; "What is behind door number 3?". It was a bathroom, and Josh said he didn't want to open the door in case someone would be in there 🤣.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/09-doorno3.jpg" alt="Josh pointing to door no 3.">

*Josh pointing to door no 3.*

### 15:25 - 16:25

Two live sessions followed; one by Sia Ghassemi, who showed some awesome tricks with Excel and Azure Functions, and one lightning session by Sebastien Goasguen about AWS EventBridge. Both went well regarding the tech, no weird issues with resizing webcam feeds or placement of the webcam on the presentations, woohoo! The success didn't last long though...

During the lightning session, the panelists joined the group call again for the expert panel session, but two of the three webcam feeds were not showing in OBS! I removed the non-working NDI sources from the scene and added them again, now it did work again, phew. But of course, I forgot something which would result in more issues...

- Issue 5: Webcam feeds not available for people rejoining the call.
- Solution/Workaround: Remove NDI sources and add them again.

### 16:25 - 17:30

As soon as the second expert panel session started, the annoying echo was back! When I added the NDI sources for the panelists again, I forgot to mute their corresponding audio sources, aarrgh! I did fix it a bit quicker than the first panel session, I think.

Near the end of the panel discussion, I added our closing keynote speaker to the call, Simona Cotin. I was a bit late with switching to the scene with just two webcam feeds for Floor and Marek. So while the panelists were leaving the group call, their scene was still active, and empty rectangles were appearing where their webcam used to be. Not the most elegant. Then I switched to the scene with both hosts and Simona, and then Floor her webcam feed started resizing out of the blue. Luckily during Simona her session, everything went smoothly again. The keynote was very well done, both in style and content.

- Issue 6: Empty spots in the scene due to people dropping out of the call.
- Solution: Agree upfront that people will stay in the group call a bit longer with webcam enabled. 

During the final keynote session, I created a new scene in OBS with the webcam feeds of all four hosts, so Floor, Marek, Lian, and myself. I wanted to have a moment where we would all be visible to the audience so I could say thanks for their excellent work.

### 17:30 - 17:55

After Simona's keynote, I switched to the new scene, and again two of the four webcam sources had resizing issues. I pretty much gave up by now to correct it. 😫

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/10-4hosts.jpg" alt="Unexpected resizing of webcam feeds again">

*Unexpected resizing of webcam feeds again.*

After thanking Floor and Marek, I switched to the scene with just Lian and me so we could close the conference. Again this scene suffered from webcam feed resizing now and then. 
When it was time to raffle the prizes, the Google Sheets script for selecting the winners did not cooperate, it took way longer than we expected. We spent quite some time online waiting for the script to finish, and Lian did a great job of filling the void with her creativity. Also, Floor helped by asking questions to keep the conversation going, great teamwork! In the end, we decided not to wait for the script to finish and announce the winners on Discord.

- Issue 7: Script to select winners was taking too long.
- Solution: Always have a backup plan when doing a live raffle of prizes.

Lian continued the closing of the conference and thanked sponsors, speakers, and organizers. At the exact moment, Lian was thanking me, my Skype connection froze, and eventually, my call dropped again! I was really was annoyed since this was the second time it happened during the day. I had to replay the YouTube recording to hear Lian's compliments, so thank you, Lian, for your kind words. I was lucky my frozen image was looking ok and I'm not making a weird face 😅.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/11-frozen.jpg" alt="My frozen but happy image during the close out of the conference.">

*My frozen but happy image during the close out of the conference.* 

- Issue 8: Skype call dropped unexpectedly again.
- Solution: Never use Skype again for an event that lasts an entire day! 😞

After I rejoined the call, Lian & I continued our conversation for a bit, and we encouraged everyone to attend the after-party event by Sam Aaron.

### 17:55 

As soon as I dropped off the group call, I stopped streaming the conference event and got in contact with Sam via Discord to test the stream for the after-party. I was using another Restream account for the after-party, which I had configured earlier that week; unfortunately, I had to reconnect the Twitch account in Restream. Next, the ServerlessDays after-party event on YouTube was not showing Sam's live stream. I had to create a new YouTube event via Restream, and Sam had to stop and start streaming to get the live streaming to work again.

- Issue 9: Connection issues with streaming and very little time to fix it.
- Solution: Perform a live test at least a day in advance. Use a paid version for Restream, which allows multiple simultaneous streams and better options for restarting streams.

### 18:10

Once both Twitch and YouTube streams were up and running, which was 10 minutes later than planned, I could finally relax, grab a beer, and enjoy Sam's performance.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/12-afterparty.jpg" alt="Me enjoying the afterparty!">

*Me enjoying the afterparty!*

The performance was awesome! Sam was clearly having a good time as well since we could see hem dancing while coding his music. I had prepared a short audio recording where I welcome everyone to the after-party. During the performance, I sent it to Sam, hoping that he would use it, and he did! 

<iframe width="560" height="315" src="https://www.youtube.com/embed/8RX2WkBiXk0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


Once the after-party was done, we thanked Sam on Discord. There was plenty of positive feedback there, which was great to see.

Once the after-party was over, I watched some Netflix, and then went to bed. I couldn't fall asleep, though, as I still felt the adrenaline rushing through my body. When I did fell asleep, I probably dreamt of continuously resizing webcam feeds in OBS 😫.

## May 9th

When I looked at the Restream statistics the day after, there were, on average, 82 people watching simultaneously. This is a bit lower than I expected since we received 380 registrations for the event. A lot of people must have thought the recording would appear online anyway. Or perhaps they had an online conference overload. Honestly, I can understand that. This was a free conference in the end, so some virtual no-show was expected.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/05/12/13-restream.jpg" alt="Restream statistics showing the view count throughout the day.">

*Restream statistics showing the view count throughout the day.*

You can clearly see a drop around lunchtime (the chart is in UTC) and a slow decline towards the afternoon, as is expected for any conference. Most of the viewers watched on YouTube (61.1%), followed by Twitch (38.9%). Apparently, nobody viewed the event on Periscope/Twitter, so I'll be dropping that channel for future streaming sessions.

## May 12th

The YouTube viewcount as of today is 775. So the views are steadily increasing, which is great!

Yesterday, Lian sent out a questionnaire asking for feedback, regarding both the format and the content. I hope we will receive enough information so we can make an even better conference next year 😊.

## Until next time?

If you've read until this point, you're either really interested in this or slightly masochistic. If you want to see all the mistakes for yourself, please enjoy the recording of the conference on YouTube. I do suggest you use the links in the description of the video so you can skip ahead to the individual sessions.

<iframe width="560" height="315" src="https://www.youtube.com/embed/cqaewpYtYTA" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

So, would I ever 
do this again? Yes and no. I would do this again since I enjoy working with the kind people in this awesome community. I also like to play around with the technology and learn new things. I would not do it in the exact same way, though. I'd prepare more, investigate in different tools, and, most importantly, share the responsibility for running the technical setup. Because now, I was solely responsible for doing tech work, which was quite a risk. If I had fallen down the stairs and had broken my leg, nobody could have stepped in quickly and continued the streaming. So my most important lesson learned is to involve others early on and to help them get familiar with the technical setup. This way, we can share the responsibility and I can sleep better in the nights leading to the event 😉.

I hope to see you all at the conference next year, or at one of our [(virtual) Meetups](https://www.meetup.com/ServerlessDays-Amsterdam/)!

*Marc*






