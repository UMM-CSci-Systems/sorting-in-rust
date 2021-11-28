# Sorting in Rust <!-- omit in toc -->

[![Sorting tests](../../workflows/sorting-tests/badge.svg)](../../actions?query=workflow%3A"sorting-tests")

- [Overview](#overview)
- [Traits](#traits)
- [To Do](#to-do)

## Overview

This lab uses various sorting algorithms as examples several features
of [the Rust programming language](https://www.rust-lang.org/):

- [Arrays](https://doc.rust-lang.org/book/ch03-02-data-types.html#the-array-type)
- [Slices](https://doc.rust-lang.org/book/ch04-03-slices.html#other-slices)
- [Borrowing and ownership](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)
- [Generic types and traits](https://doc.rust-lang.org/book/ch10-00-generics.html)

Here we've provided you with a sample Rust implementation of insertion sort, and
you'll implement two other common sorting algorithms:

- Quicksort
- Merge sort

Insertion sort and quicksort can be done "in place", so they take a
_mutable_ array of values and destructively sort the given array.

Merge sort can't be done "in place" ([it
requires allocation of additional storage](https://en.wikipedia.org/wiki/Merge_sort#Variants)),
so it is structured here to return a new `Vec` as its result.

There are _lots_ of comments in the code on the various algorithms, but
you might want to look them up in your favorite source if you're rusty
(ho, ho) on those sorting algorithms.

Since you've dealt (at least a little) with arrays, slices, borrowing,
and ownership in the
[Disemvowel in Rust lab](https://github.com/UMM-CSci-Systems/disemvowel-in-rust)
we won't discuss them here, but we will say a little about traits.

## Traits

Rust uses the concept of _traits_ in a manner that is somewhat similar
to interfaces in Java. A trait in Rust indicates a set of properties that
a given type must have. A simplified signature for `insertion_sort`, for
example, is:

```rust
    fn insertion_sort<T: PartialOrd>(v: &mut [T])
```

Here `insertion_sort` takes a single parameter `v` that is an array of items
of type `T`. Now because we want to sort the items in the array, we need to
be able to ask "less-than" questions about items of type `T`. That ability
is provided by the trait `PartialOrd`. The annotation `<T: PartialOrd>` says
that `T` can't just be _any_ type – it must implement
[the `PartialOrd` trait](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html),
which ensures support for useful things like `<` and `<=`, which we'll clearly
need to sort.

You can require a type to have several traits, joining them together with the
`+` operator like this:

```rust
    fn insertion_sort<T: PartialOrd + std::fmt::Debug>(v: &mut [T])
```

This is the actual signature for `insertion_sort` in the sample code, and says
that `T` has to support _two_ traits: `PartialOrd` and
[`Debug`](https://doc.rust-lang.org/std/fmt/trait.Debug.html). (The ability
to support multiple traits in Rust is similar to the ability to
implement multiple interfaces in Java.) The `Debug` trait allows you to
print data using the `{:?}` format, which generates output in a
programmer-friendly manner that is hopefully useful for debugging. We
don't _need_ this here (the code you're given doesn't actually use it),
but it might be helpful if you want/need to add some printing to debug
things as you're working on your implementation.

Lastly, one consequence of merge sort returning a new vector (instead of
sorting the array in place) is that it requires the ability to *copy*
elements from the input array into the output vector. Insertion sort and
quicksort don't need that because they can use the `.swap()` method on arrays
(which essentially swaps pointers on non-primitives instead of copying them).

This requires the addition of yet another trait [(`Copy`)](https://doc.rust-lang.org/std/marker/trait.Copy.html)
to `T` in the signature of `merge_sort`:

```rust
    fn merge_sort<T: PartialOrd + std::marker::Copy + std::fmt::Debug>(v: &[T]) -> Vec<T>
```

Adding this trait allows us to perform actions that require copying such as:

```rust
    result.push(v[i])
```

## To Do

The canvas rubric provides detailed information on how you will be graded. The
provided tests (which you can run with `cargo test`) should provide some
reasonable feedback on the correctness of your solution, but passing the tests
never guarantees _complete_ correctness. The badge at the top of this README
should also indicate whether your tests are passing in GitHub Actions.

- [ ] Implement `quicksort`
- [ ] Implement `merge sort`
- [ ] Ensure that the tests pass
- [ ] Code and commits should be understandable and useful
