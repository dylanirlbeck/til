# Using the Ruby Double Splat Operator

Today I found a [Stack Overflow
post](https://stackoverflow.com/questions/18289152/what-does-a-double-splat-operator-do)
that finally connected the dots on how to use the Ruby double splat (`**`)
operator. In short, there are two ways to use it: in a function's argument list
and when calling a function that expects a hash argument.

> Note that the following examples are pulled directly from the linked post.

## Use #1:

The most common case I've seen is when the double splat is used to prefix an
argument in a function definition:

For this code:

```ruby
def foo(a, *b, **c)
  [a, b, c]
end
```

Here's a demo:

```ruby
> foo 10
=> [10, [], {}]
> foo 10, 20, 30
=> [10, [20, 30], {}]
> foo 10, 20, 30, d: 40, e: 50
=> [10, [20, 30], {:d=>40, :e=>50}]
> foo 10, d: 40, e: 50
=> [10, [], {:d=>40, :e=>50}]
```

## Use #2: Caller side

You can use the double splat operator when calling a function that expects
a hash argument like so:

```ruby
def foo(opts); p opts end
bar = {a:1, b:2}
foo(bar, c: 3)
=> ArgumentError: wrong number of arguments (given 2, expected 1)
foo(**bar, c: 3)
=> {:a=>1, :b=>2, :c=>3}
```

If you come from JavaScript land, I treat the spread operator (`...`) as
analagous to the double splat (on the calling side).

---

Disclaimer: There may be more uses than the ones I've listed.
