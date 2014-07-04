---
layout: post
title:  "Ruby Tip of the Day: ensure return"
date:   2013-11-27 17:53:45
categories: ['ruby', 'ruby tips', 'programming', 'development']
---

Here's a problem that I ran into recently in Ruby. Let's say you had the
following method:

{% highlight ruby %}
def sync_attributes
  # fetch the attributes
  attributes = fetch_attributes

  # try to save the attributes
  begin
    if !save(attributes)
      raise SaveError, "Could not save attributes"
    end
  rescue Net::HTTPRequestTimeOut
    log_error("Couldn't connect to save API.")
  ensure
    log(attributes)
    return attributes
  end
end
{% endhighlight %}

Can you spot the problem with the code? What if I told you that the
`SaveError` never gets raised even if `#save` returns false? First, let's recap
what ensure does. Like the `finally` keyword in Java, ensure always executes
its contents after a begin/rescue/etc blocks.

You can almost think of raise almost as a special type of return in that it
changes the flow of execution. However, in order to execute the `ensure`, the
raised exception gets put on the stack. The ensure then gets executed and when
the code hits the `return` in the ensure block, it returns thus negating the
`raise`.

Therefore, you should never call `return` in an ensure block. Doing so is like
calling `rescue Exception` in your code.
