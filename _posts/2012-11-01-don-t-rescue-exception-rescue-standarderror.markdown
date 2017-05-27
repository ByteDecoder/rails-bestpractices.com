---
layout: post
title: Don't rescue Exception, rescue StandardError
author: Richard Bradley
description: In C# or Java, you can `catch (Exception)` to rescue all exception types. However, in Ruby you should almost never catch `Exception`, but only catch `StandardError`.
tags:
- rescue
- exception
---
## Before

Explicitly rescuing `Exception` will rescue even not normally recoverable errors such as SyntaxError, LoadError, and Interrupt.

    begin
      foo
    rescue Exception => e
      logger.warn "Unable to foo, will ignore: #{e}"
    end

## Refactor

If you omit the `Exception` type qualifier, then Ruby will catch only `StandardError`, which is probably what you want:

    begin
      foo
    rescue => e
      logger.warn "Unable to foo, will ignore: #{e}"
    end

See also <http://stackoverflow.com/questions/10048173/why-is-it-bad-style-to-rescue-exception-e-in-ruby>

