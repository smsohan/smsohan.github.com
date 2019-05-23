---
layout: post
title: "Exception Handling Anti-Patterns"
date: 2019-05-23 10:25
comments: true
categories: [Coding]
---

There are a fairly [good number of articles](https://users.encs.concordia.ca/~shang/pubs/ICPC_gui.pdf) written on exception handling
and related anti-patterns. I wanted to show code examples for the ones I
found interesting so that impatient developers can take a quick
glance at the code instead of having to read long form text.

## Under-handled / Ignored
Produces bad user experience when an exception happens.
```ruby
def create
  post.save! #May fail due to database issues
end
```

## Over-handled / Catch-all
Makes it difficult to debug since the posibilities are too large.
```ruby
def create
  post.save! #May fail due to database issues
rescue => error
  # handle
end
```

## Exceptions as if-else
Introduces `goto/jump` like control-flow instead of a logical flow.
```ruby
def create
  post.created_at = Time.now
  post.validate # It's a likely that some posts will be invalid
  post.save!
  post.notify_users
rescue => ValidationError
end
```

## Silent handler
Makes it very difficult to debug problems.
```ruby
def create
  post.save! #May fail due to database issues
rescue
end
```

## Handler uses inappropriate log level
Similar to silent handler since most production apps run in non-debug
log level.

```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  log.debug "failed to save post #{error.message} #{error.backtrace.join}"
end
```

## Handler logs without actual exception info
Lacks critical error info that can help debugging.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError
  log.warn "failed to save post"
end
```

## Handler logs without exception stacktrace
Lacks critical context such as files and line numbers that can help debugging.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  log.warn "failed to save post #{error.message}"
end
```

## Handler returns null
Often the caller can't distinguish a successful vs. exception case and
fails in subsequent steps.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  log.warn "failed to save post #{error.message} #{error.backtrace.join}"
  return null
end
```

## Destructive wrapping
The caller loses the original stacktrace and message of the exception
making it difficult to locate the root cause.
```ruby
def create
  post.save! #May fail due to database issues
rescue SaveError => error
  raise CustomSaveError.new('Failed to save the post')
end
```
## Wide handler
The exception handler has larger than required number of lines, thus it becomes
difficult to cleanup resources acquired before the line that triggered
exception.
```ruby
def create
  file = File.open('/some/file.pdf')
  pdf = Pdf.new(file)
  post_text = pdf.text
  post = Post.new(text: pdf)
  post.save! #The exception handler only triggers on this line
rescue SaveError => error
  #...
end
```
## Missing cleanup
Handling an error without cleaning up system resources.
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
## Unncessary custom exception
Introducing custom exception when a pre-defined exception suits just
fine.
```ruby
def create(text:)
  if text == nil
    #Could just use pre-defined ArgumementError
    raise EmptyTextException.new('Text can't be empty')   end
  #...
end
```


