---
layout: post
title:  "Ruby Tip of the Day: #tap"
date:   2013-07-30 14:53:45
categories: ['ruby', 'ruby tips', 'programming', 'development', 'rails']
---

Have you ever encountered the following problem: you go to find out what's
being returned by a method that's being called several places but you can't
find a good place to add a `puts`? For example, let's say you had the following
method called `#full_name` that's being called in 10 different places. How do
you find what's being returned by `#full_name` in each instance with writing
only one `puts`?

{% highlight ruby %}
def full_name(first_name, last_name)
  "#{first_name.titlecase} #{last_name.titlecase}"
end
{% endhighlight %}

Your first thought is to probably create a variable like so:

{% highlight ruby %}
def full_name(first_name, last_name)
  name = "#{first_name.titlecase} #{last_name.titlecase}"
  puts name
  name
end
{% endhighlight %}

But there's a better way!

{% highlight ruby %}
def full_name(first_name, last_name)
  "#{first_name.titlecase} #{last_name.titlecase}".tap { |name| puts name }
end
{% endhighlight %}

Here, we need no variable and yet the full name still gets returned. Moreover,
our debug code is all at the end of one line instead of spread out over three
lines.

The `#tap` method also works well with method chaining:

{% highlight ruby %}
# before
sentence.split.map(&:reverse).join(" ")

# after
sentence.split.map(&:reverse).tap { |n| puts n.inspect }.join(" ")
{% endhighlight %}

Be sure to check out `#tap` the next time you need to inspect an element!
