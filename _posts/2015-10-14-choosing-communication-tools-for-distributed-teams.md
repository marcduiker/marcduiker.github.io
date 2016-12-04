---
layout: post
title: "Choosing communication tools for distributed teams"
tags: communication team tools
---

<a href="https://slack.com" target="_blank">
  <img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/14/slack_home.png" alt="Slack">
</a>

## Team communication
My work at [Tahzoo](http://www.tahzoo.com) and the work I do for the [Dutch Sitecore user group](http://www.sugnl.net) require me to do a lot of communication in distributed teams.
Effective and clear communication is difficult, even more so when the team members are at different locations. Having the right tools in place helps a lot.
Since there are so many communication tools out there I want to share my opinion on some of these so perhaps you can make a more informed judgment when deciding which tools to use.

<!--more-->

__Go straight to the [TL;DR](#tldr)__

### Types of communication
There are various types of communication I do during the day:

- Informing my local colleagues of a new technology meetup we should attend.
- Performing a code review with one of my team members who is located in another country.
- Show appreciation towards a colleague who did something great in a project.
- Discuss with colleagues (world wide) how to unify our continuous integration set-ups.
- Converse with fellow Sitecore user group organization members and hosts (located throughout the Netherlands) about the topics of the next user group meeting.

The type of communication is either:

- A one-off message (mostly asynchronous, such as a news update)
- A conversation (either text or video)

These different types and targets (individual, group, or organization) usually require different tools to support the specific communication needs. I haven't seen one tool that supports everything well.

## The tools
The focus of this post is mostly on tools for (a)synchronous text messaging (conversations) and not so much video conferencing tools.

_Please note that this is not an extensive list of communication tools out there.  It's far from that, it's a shortlist of tools I've used intensively over the last years._

### Slack
[Slack.com](https://slack.com) is definitely my favorite conversation platform nowadays. I like the minimal interface on the web and the desktop client, the ability to focus conversations around topics and the intuitive way of notifying people.
I haven't even started about the fantastic [integrations](https://slack.com/apps) it offers with other tools.

Slack is great for synchronous conversations, it's not meant for one-off messages.
With Slack you can communicate in three ways:

- Participate in a public channel to  discuss a specific topic. Anyone in the Slack team can join a channel.
- Participate in a private group to discuss a specific topic. You'll need to be invited for a group.
- Send a private (direct) message to another user.

What I really like about Slack is that you can easily notify other users in different ways:

- type `@<username>` in a channel to notify one user in specific. The others in the group/channel can also see this message but won't receive a notification.
- type `@channel` in a channel to notify all members of that channel with your message.
- type `@everyone` in a public channel to notify the entire team.

Next to the web interface Slack has [desktop clients](https://slack.com/downloads) for Windows, Mac and Linux. Mobile apps are available for Android, iOS and Windows.

_Am I a Slack fanboy? Oh yes :). I'm currently in three Slack teams which works seamlessly with the desktop client._

### Yammer
[Yammer](http://www.yammer.com) promotes itself as the enterprise social network for businesses. It's the Facebook for organizations. It handles one-off/asynchronous messages very well. It's great for news updates about an upcoming event you want to promote or showing appreciation to a colleague. Co-workers on Yammer can like the message and reply to it but it's not a platform for synchronous conversations. I'd say it's quite complementary to what Slack is offering. 

Next to the web interface Yammer offers a (quite limited) [desktop notifier](https://products.office.com/en/yammer/yammer-desktop-notifier) (Windows only) and mobile apps for [Android](https://play.google.com/store/apps/details?id=com.yammer.v1&hl=en), [iOS](https://itunes.apple.com/en/app/yammer/id289559439?mt=8) and [Windows](https://www.microsoft.com/en-us/store/apps/yammer/9wzdncrfhwmz).

My two major complaints with Yammer are:

- You [can't edit a message that is posted](https://community.office365.com/en-us/f/176/t/228840)! Support advises to delete and re-post the message... Seriously Microsoft, this user experience is _very_ bad.
- The [event functionality was dropped](https://community.office365.com/en-us/f/176/t/246121) some time ago.

### Skype
[Skype](http://www.skype.com) has long been the preferred tool of communication for my colleagues for both chat and video. I can see why; it's easy to use, allows you to communicate to a group and do video and screen sharing.

I would only recommend Skype when video or screen sharing is required and Google Hangouts can't be used. I wouldn't recommend it for chat conversations due to it's lack of channels, less advanced notifications and lack of integrations when compared to Slack.

When I use Skype I mostly use the Windows desktop client. I've also used the web version of Skype for Business (because that desktop client didn't want to install). Skype also has a [desktop clients](http://www.skype.com/en/download-skype/skype-for-computer/) for Mac and Linux and [mobile apps](http://www.skype.com/en/download-skype/skype-for-mobile/) for Android, iOS, Windows, Blackberry, Nokia X and Amazon Fire Phone. 

### Office365
With Office365 you can create [public or private groups](https://www.youtube.com/watch?v=t3OLvYXepvE) and start a 'conversation' there. Don't expect a fluent conversation experience such as Slack though. The conversation is actually made up of email like messages on which you can perform actions such as reply, reply all, forward and like. It's a good fit for more official communication, capturing information and documenting everthing long term, especially when you use the other integrated functionalities such as Files, Calender and Notebook. 
It still seems that Office365 is not 100% cross-browser compatible though. I couldn't access a group Notebook using Chrome :(. A colleague noticed that when you reply to a group conversation using Outlook instead of the web interface the reply starts in a new thread. Not very useful to keep track of the conversation.

The are mobile apps available for Android, iOS and Windows called _Outlook Groups_ (yes, it's _not_ called _Office365 Groups_). The apps are not available globally though as is mentioned in [this post](http://windowsitpro.com/blog/outlook-office-365-groups-app-for-mobile-devices).

### Google Hangouts
I use [Google Hangouts](https://hangouts.google.com/) frequently for video conferencing with other Sitecore user group members. Stability and video quality is good and screen sharing works flawlessly. If you have a Youtube channel as well you can even do live streaming. I hardly use Hangouts for chat anymore since it's far behind compared to Slack.

Besides the web interface Hangouts is available for Android, iOS and as [Chrome extension](https://chrome.google.com/webstore/detail/google-hangouts/knipolnnllmklapflnccelgolnpehhpl?hl=en).

## <a name="tldr"></a>TL;DR - My choice of tooling

_There is simply not just one tool that can supports everything well. So pick the right tool for the right job._

- For synchronous conversations/discussions use [Slack](http://www.slack.com) because the conversations are focused in channels/groups and it offers handy integrations.
- For one-off/asynchronous messages, targeted to either the whole organization or to a group, use [Yammer](http://www.yammer.com).
- For video conferencing use Google Hangouts or Skype. I have a slight preference for Hangouts since it integrates with Slack :).

### Can't use a single tool? Integrate!
I recently stumbled upon [Sameroom.io](http://sameroom.io). This is a platform that bridges otherwise isolated communication tools. 
For instance: when you have distributed teams where one team is using Slack and the other is using Skype. In Sameroom you can setup a so-called _tube_ and create a bridge between a Slack channel and a Skype group so both teams are in the same conversation.

### Other aspects
There are of course other things to consider when choosing communication tools. You need to think about integrating it into the existing application landscape, how to administer users, security etc. Try various tools with some stakeholders to see what is the best fit. Make an informed decision and define a plan before you roll-out to the entire organization.