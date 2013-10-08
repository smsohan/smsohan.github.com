---
layout: post
title: "Introducing Asset Pipeline to Older Apps?"
date: 2013-10-08 10:12
comments: true
categories: [Ruby, Rails, JavaScript]
---

Introducing Asset Pipeline to an old project is quite hard. Most pre-asset pipeline projects used small JavaScript/CSS files that are often scoped to a single page or a part of the application. A typical example is as follows:


```javascript login.js

$(function(){

  $('#login').on('click', function(){
    var isLoginValid = hasValue($('#user_name')) && hasValue($('#user_password'));

    if(isLoginValid){
      $('#login_errors').hide();
      $('#login_form').submit();
    } else {
      $('#login_errors').show();
      return false;
    }
  });

});


```

Now, within the scope of the login page this code executes just fine. However, with asset pipeline, if this file is included in the application manifest, then all pages that include the manifest will execute this code on load. This is wasteful and more importantly, may result in unexpected behaviors and conflicts.

To work around this problem, when introducing asset pipeline, the code needs to be wrapped in some method that can be called to initialize it only from the login page. Here's an example of the wrapper method:

```javascript login.js

  App.validateLogin = function(){

    $('#login').on('click', function(){
      var isLoginValid = hasValue($('#user_name')) && hasValue($('#user_password'));

      if(isLoginValid){
        $('#login_errors').hide();
        $('#login_form').submit();
      } else {
        $('#login_errors').show();
        return false;
      }
    });
  };

```

Now that the logic is wrapped inside a method, it can be included in all the pages without causing any wasteful execution and risking unexpected outcomes or conflicts. This method can be called from within the login page as shown in the following example:

```html login.html.erb

<script type="text/javascript">
  $(function(){
    App.validateLogin();
  });
</script>

```

This is of course only a minimum change approach that'll get asset pipelines working for an existing app. I'd recommend refactoring the code to make it testable and adding unit tests as you go.

We have a 4 year old Ruby on Rails project, and now running 3.2 with asset pipelines. It has only one manifest file. We used this simple approach to convert all existing js code and it worked great. Hope it helps when you start upgrading your assets to use the pipelines.

