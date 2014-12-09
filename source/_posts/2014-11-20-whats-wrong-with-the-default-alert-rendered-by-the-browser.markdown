---
layout: post
title: "What's wrong with the default alert rendered by the browser?"
date: 2014-11-20 22:49
comments: true
categories: [Others]
---

Browsers have carefully crafted the alert and confirm dialogs for everyone under really simple APIs as follows:

```
alert('Failed to send the email. Check your network connection.');
confirm('Are you sure you want to delete this email?');
```

While writing cross-browser JavaScript is not as painful today, custom implementation of these natively supported dialogs seem to be a __complete waste__ of money and time to me. With all the different browsers that we have to support today, including the mobile browsers, it seems a lot of wasted effort in trying to render a custom alert and confirm dialog under any circumstance. I've never implemented one, but if you ever attempted, you probably know how much work it is to get it right and consistent. Here's a short list of requirements if you want to build one:

1. Should work on all browsers.
2. Needs to disable all other controls on the website.
3. Needs to be on the view port so the user can take an action immediately.
4. Should not leak any memory if used repeatedly on a single screen.
5. Should be fast.

While I've seen some magicians to make these requirements look silly and do a decent job of implementing a custom version of the alert and confirm dialogs, I'm yet to see one that satisfies all the above essential requirements, and does a better job than the browser's native implementations. If you haven't looked recently, see the current rendering of the confirm dialogs across different browsers:

![Safari](/images/safari_confirm.png)
![Chrome](/images/chrome_confirm.png)
![FireFox](/images/firefox_confirm.png)
![iPhone](/images/iphone_confirm.png)

If you look carefully, you'll see subtle things. For example, did you notice the __nice shadows__ around the confirm dialogs rendered on Chrome and Safari? There are live effects on the cancel and ok buttons as well, where you'll see a little __shining effect__ on the ok button. These are carefully crafted buttons. The Cancel and OK button are perfectly sized for __fat fingers__ on the iOS safari. Also, these are __familiar__ to the users, so they immediately know what they are clicking on. __Keyboard navigation__ works as expected. And lastly. the buttons will change labels based on your __preferred language__ settings. All for __free__. Now, try to replicate that on your custom control, across all these browsers and show me how it does any better.

If your sole business is indeed building these custom dialogs, you can probably invest the time and money here. But as far as I'm concerned, I'd not put a single $ from my pocket if I was funding a project to either build such custom implementations or using your custom implementation. I'm aware of the fact that, even if it's free and open source, it still requires additional maintenance dollars.

As a user of web, I've never been bothered by the websites that show me these familiar browser rendered alerts and confirms. Were you?



