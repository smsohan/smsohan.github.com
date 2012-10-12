---
layout: post
title: "MvcMailer new API"
date: 2012-10-02 10:23
comments: true
categories: [MvcMailer, .Net, C#]
---

With the help of [@TylerMercier](http://codecuriosity.com/), and many active users of MvcMailer, we have just released the new API for MvcMailer. This is a summary post capturing the work and lessons learned in the process.

The bulk of the work has been done on removing hard dependencies on dll files for 3rd party libraries in favor of NuGet packages. For example, we used NUnit, for running our tests. Instead of referencing the dlls directly, we are now using the NuGet package. This will help the contributors to get up and running with the source code without having to worry about the depedencies being in the right place.

NuGet command line also added a dependency as a Nuget package!

But this cleanup is not gonna be directly visible to the users of MvcMailer. If you install MvcMailer today, you should see a few changes as follows:

1. MvcMailer now uses T4Scaffolding 1.0.7 instead of the older version that was causing issue with ASP.NET MVC 4.
2. MvcMailer package is now exclusively for ASP.NET MVC 4. And a new package MvcMailer3 is published for ASP.NET MVC3. After looking into options, we found this was probably the best way to release an upgrade, while still remaining compatible with MVC3.
3. The MailerBase has a sweet new API. The old API still works, but I'd highly discourage using it. This is how it'd look:

``` csharp Example code showing old API

class WelcomeMailer : MailerBase{

  public MailMessage Welcome(){

    var mailMessage = new MailMessage(){Subject = 'Welcome to the world!'};
    mailMessage.To.Add("hello@example.com");

    PopulateBody(mailMessage, 'Welcome');

    return mailMessage;
  }

}

```

As you see here, the old API would require you to initialize your own <code>MailMessage</code>, set some parameters to it and then hand it over to MailerBase so it can render the view into its body.

We found it would be nice to reverse this workflow using lambdas. So, there's a new populate method, that will call you back with an already instantiated <code>MvcMailMessage</code> object so you can set its properties as required. <code>MvcMailMessage</code> is an extension of <code>MailMessage</code> from the core .Net library, with new properties added so you can specify the <code>ViewName, MasterName, LinkedResources</code> etc.

So, with this new API, the code from above will look like the following:

``` csharp Example code with the new API

class WelcomeMailer : MailerBase{

  public MvcMailMessage Welcome(){

    return Populate(x => {
      x.Subject = 'Welcome to the world!';
      x.ViewName = 'Welcome';
      x.To.Add("hello@example.com");
    });

  }

}

```

As you see here, the new API provides a nice way get rid of some of the repiting parts of your mailer code. This API is available on both MvcMailer3 and MvcMailer.

We'd like to hear your feedback on this new API. Thank you for using MvcMailer.
