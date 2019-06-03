---
layout: post
title: "Exception Handling Anti-patterns"
date: 2019-05-23 10:25
comments: true
categories: [Coding]
---

![confusing road sign](/images/confusing_road_sign.jpg)

<sub>
Source: [Henry Burrows](https://www.flickr.com/photos/foilman/2803261256/in/photolist-5gHrUC-5YEVYt-ocZHz7-2bDb3w8-aCGGSo-cB9opW-dUUDRS-6qbnVw-ppkgWu-cYsKjw-4HSS8t-aAJCk5-XBWQ5q-cYsKDA-NNefT4-p8HF-bfJwPB-6SibT9-ubSQL-mvaYX-7uNS7V-473w41-HABo5-5SL6FL-2f4rrkN-SazHLx-2eaMrNW-2eaMsbQ-24J1WkB-24CQREz-24CQR5g-bvyvt7-RvjAiK-6asxLk-9zRJ1e-6zLy6Z-9yuCpf-24FpgMX-95dVq3-hERZkd-4JKe8s-hESYoP-hESYnX-4oPtJ8-6gvogb-5skgvk-4Pu7Hp-8AmdYp-2t55t-24FpgYt)
</sub>

Whenever faced with a production issue, I find exceptions to be an extremely useful
information source. A careful look at an
exeption has often led to quick discovery
of the source of a trouble. On the flip side, I have also
faced a lot of chaotic debugging sessions because of poor exception handling.
Here, I present the common anti-patterns that I recommend fixing while
reviewing pull-requests. Most programmers are already familiar with the mechanics of exception
handling. Yet, I see these anti-patterns everyday.

I primarily see these anti-patterns to be control-flow or logging related as shown below:

## Control-flow Anti-patterns

**Unhandled.** When an exception is unhandled, if often results in a
clueless user experience for the end user as well as the developer.
```ruby
def notify
  post.email! #May fail due to configuration, network, or authentication
end
```

**Catch-all.** With catch-all errors, it's often difficult to quickly detect the
original problem. For the same reason, the end users don't get specific
and actionable error messages.
```ruby
def create
  post.save! #May fail due to database issues
rescue => error
  # handle
end
```

**If-else Exceptions.** Exceptions mean something unexpected took
place. If-else is used for logical known code paths. For example, when
accepting an API request, invalid input data is often a known logical
path. Using exceptions for it will trigger false alarms.

```ruby
def create
  post = Post.new(params)
  post.save!
rescue ValidationError => error
  log_exception(error)
end
```

**Wrapped Exception.** A new exception is raised hiding the original exception. In such cases,
if the exception is handled by the caller, **critical context information
is lost** since the orignal stacktrace is no longer available.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  raise CustomSaveError.new('Failed to save the post')
end
```

**Useless Custom Exception.** Introducing a new exception type when a pre-defined exception suits just
fine.
```ruby
def create(text:)
  if text == nil
    #Could just use pre-defined ArgumentError
    raise EmptyTextException.new("Text can't be empty")   end
  #...
end
```

**Leaky Handler.** Handling an error without cleaning system resources such as file
handles, open network connections, can cause cascading system outage.

```ruby
def create
  #Will leak this file handle if read succeeds, but write fails
  file = File.open('/some/new.txt', 'w')
  file.write('some text')
rescue FileNotFoundError, FileSaveError => error
  log.warn('...')
  raise error
end
```


## Logging Anti-Patterns

**Silent Handler.** Makes it very difficult to debug problems.
```ruby
def create
  post.save! #May fail due to database issues
rescue
end
```

**Debug-only Handler.** Similar to silent handler since most production apps run in non-debug
log level.

```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  log.debug "failed to save post #{error.message} #{error.backtrace.join}"
end
```

**Custom Message-only Handler.** Some exception handlers only log a custom
message leaving the details of the exceptions. As a result, critical
information is lost that can be very useful for debugging.

```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError
  log.warn "failed to save post"
end
```

**Message-only Handler.** Without Stacktrace, it gets very difficult to trace the root of a
problem since often times exception handlers wrap a few lines of code.
```ruby
def create
  email = User.find(params[:id]).email
  post = Post.find(params[:id])
  comment = post.comments.create!(name: user.name)
rescue NotFoundError => error # Could happen in line 2 or 4
  log.warn "failed to save post #{error.message}"
end
```

**Sneaky Handler.** Some exception handlers return nil or a value.
The caller can't distinguish between a successful vs. exception case and
fails in subsequent steps.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  log.warn "failed to save post #{error.message} #{error.backtrace.join}"
  return null
end
```

There are times when you intentionally have to use some of these
anti-patterns. But those are rare. It's critical for the developers to
think about the information that'd help in swiftly debugging a production problem. As such,
developers must avoid the noise and provide all context information for
errors to help diagnose potential system problems.

Happy coding.
