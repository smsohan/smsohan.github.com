---
layout: post
title: "Exception Handling Anti-patterns"
date: 2019-05-23 10:25
comments: true
categories: [Coding]
---

Whenever faced with a production issue, I find exceptions to be an extremely useful
information source. A careful look at an
exeption has often led to quick discovery
of the source of a trouble. On the flip side, I have also
faced a lot of horror debugging sessions because of poor exception handling.
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

**Exceptions as if-else.** A line that causes exception introduces a sudden change in the control
flow of the code. As such, when exceptions are used as a logical
operator, it can lead to confusions about the effect of an offending
line.

```ruby
def create
  post.created_at = Time.now
  post.validate # It's a likely that some posts will be invalid
  post.save!
  post.notify_users
rescue => ValidationError
  show_validation_error
end
```

**Destructive Wrapping.** A new exception is raised hiding the original exception. In such cases,
if the exception is handled by the caller, critical context information
is lost since the orignal stacktrace is no longer available.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  raise CustomSaveError.new('Failed to save the post')
end
```

**Unncessary Custom Exception.** Introducing custom exception when a pre-defined exception suits just
fine.
```ruby
def create(text:)
  if text == nil
    #Could just use pre-defined ArgumentError
    raise EmptyTextException.new('Text can't be empty')   end
  #...
end
```

**Missing Cleanup.** Handling an error without cleaning system resources such as file
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

**Handler Uses Inappropriate Log Level.** Similar to silent handler since most production apps run in non-debug
log level.

```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  log.debug "failed to save post #{error.message} #{error.backtrace.join}"
end
```

**Handler Logs Without Actual Exception Info.** Lacks critical error info that can help debugging.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError
  log.warn "failed to save post"
end
```

**Handler Logs Without Exception Stacktrace.** Without Stacktrace, it gets very difficult to trace the root of a
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

**Handler Returns Nil or a Value.** Often the caller can't distinguish a successful vs. exception case and
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
anti-patterns. But those are rare. The most important concept with
exceptions is to internalize the belief that good exception handling
will lead your debugging adventure on a smooth path.

Happy coding.
