---
layout: post
title: "Hosting a Hands-on Session: Lessons Learned"
date: 2014-06-02 16:08
comments: true
categories: [Presentation]
---

Every year, I wish to run a couple of public speaking sessions/presentations/demos. This year, I had the opportunity to teach a hands-on session at [CAMUG](http://www.meetup.com/Calgary-Agile-Methods-User-Group/events/182303372/) and wanted to share some lessons learned with my readers on this blog.

![Session Photo](http://photos2.meetupstatic.com/photos/event/e/5/e/e/600_366838862.jpeg)

Thanks [Terence](http://www.meetup.com/Calgary-Agile-Methods-User-Group/members/29446812) for taking this photo.

## Scheduling Breaks

Because this was a 3 hour session, and I believe my attention span cannot be stretched beyond 40 mins, the 3 hour session was chuncked as follows:

1. Chunk 1
  __08:45 - 09:00:__ __Played a techno music__ to set the tone as people are entering the room,
  __09 - 09:10:__ meet and greet,
  __09:10 - 09:20:__ present a deck of 6 slides,
  __09:20 - 10:00:__ show the basics of AngularJS in a 40 min stretch

2. Chunk 2
  __10:00 - 10:10:__ first coffee/snack/questions break, again with the music played on the background,
  __10:10 - 10:50:__ another 40 min strech of coding,
  __10:50 - 11:00:__ 2nd break, with the music

3. Chunk 3
  __11:00 - 11:30:__ last strech of coding,
  __11:30 - 12:00:__ Questions, discussions

While the chunks helped me keeping on track, I thought I could do a better job with switching between code reading and writing mini chunks. Next time I run a hands on, I'll have mini chunks as follows:

> 5 min code reading, followed by 10 min code writing

Calling out a separate read vs. write time should help with the fact that some people are still typing when I moved to the next topic.

## Practice Runs

This time I ran a few rounds of practice runs using QuickTimePlayer's screen recorder. This helped me a great deal since I was able to correct a few mistakes and got some ideas for improvements even before showing this to my colleage.

I also did a 1 hour compressed practice run with [Mo](http://mokhan.ca/) and he had some great advices.

I kind of miss the fact that, I don't have a video captured for the real session. That would help me for my next talk for sure. So, lessons learned from here is

> to have a video recording of the future sessions if possible.


## Topic Selection

Introducing a large Framework like [AngularJS](http://angularjs.org) is tricky. So, instead of showing the framework for its sake, I decided to build a simple application that could actually be used. The application we built together was called Lookahead. A markdown based slide deck editor/presenter with live preview. Because I used it for my initial slide deck, and then told the attendees that we were gonna build this with just 54 lines of JavaScript code, I think it got people interested about it.

Lessons learned from here:

> find a topic for a presentation that has a value proposition in itself, apart from just being a hello world example.

## Backup Code

I've tried to write the code myself a few times ahead of the session, and always had some typos that'd break things here and there. So, I decided to have a backup of the code with the seed project, and distributed it to the attendees. This helped me quite a bit, as people could simple copy-paste segments of the code when frustrated by that missing curly brace or a sneaky syntax error.

However, during the live session, I decided to deviate a bit from the backup code to clarify some questions. While the deviation made it easy to explain certain things, it also introduced some confusions for people that were following the backup and didn't see the exact same code on the projector.

Lessons learned from here:

> If backup code is used, stick to it.


## Conclusion

It was really good to see a full house on a morning of a summer weekend in Calgary. The audience had a really good mix of people, ranging from students to people with 35 years of experience. It was also inspiring to hear some of the feedback from them and I wish to deliver a better talk next time.

Btw, if you are interested, check out the live demo of [Lookahead](http://smsohan.com/lookahead) and its [54 lines of JS](http://smsohan.com/lookahead/app.js).






