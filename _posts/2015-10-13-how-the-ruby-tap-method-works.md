---
title: How The Ruby Tap Method Works
---

First, let's define a couple of terms:  *caller* and *mutated*.

The **caller** is what `tap` is being called on.  In the below, the *caller* is `arr`:

{% highlight ruby %}
arr = [2, 3, 1]
arr.tap do |x|
  x.sort
end
{% endhighlight %}

**Mutated** permanently changes the value of the thing in question.  Here is an example of the variable `x` **not** being mutated:

{% highlight bash %}
$ x = 7
$ x.to_s
=> "7"
$ x
=> 7
{% endhighlight %}

Here `x` is not mutated.  Something is done to it, temporarily, but it is not mutated, which is why when it is called at the end, it is still an integer.

Here is an example of an array *being mutated*:

{% highlight bash %}
$ arr = [2, 3, 1]
$ arr.sort!
$ arr

=> [1, 2, 3]
{% endhighlight %}

Because of the `!`, `arr` was permanently changed, or rather *mutated*.

There are two major points to appreciate around how the `tap` method works.

### It is not an iterator

When you call `tap` on something, like an array for example, it does not loop over all of the items in the array.  It applies your block of code to the caller one time.

For example:

{% highlight ruby %}
counter = 0
arr = [2, 3, 1]
arr.tap do |x|
  counter += 1
  x.sort
end

$ counter
=> 1
{% endhighlight %}

The above is illustrating that the array, `arr`, did not get iterated over three times, rather only one time.  This is why the counter's value is `1`, not `3`.


### Only the caller is returned, either mutated or un-mutated

The **caller** is returned, one way or another.  If the caller gets mutated through the process, the mutated version of the caller is returned.  If the caller does **not** get mutated through the process, then the un-mutated (or rather, the original) version of the caller gets returned.

Here are some examples explaining this further.

{% highlight ruby %}
arr = [2, 3, 1]
arr.tap do |x|
  x.sort
end

=> [2, 3, 1]
{% endhighlight %}

In the above example, the caller was not **mutated**.  Something was done to it, to be sure, but it was not mutated.  Since it was not mutated, the original version (or rather the non-mutated version) of the caller was returned.

Here is an example of the `tap` method returning a mutated version of the caller:

{% highlight ruby %}
arr = [2, 3, 1]
arr.tap do |x|
  x.sort!
end

=> [1, 2, 3]
{% endhighlight %}

Here we add the `!` at the end of `sort`, which permanently mutates the caller, `arr`.  Therefore, the mutated version of the caller is returned.
