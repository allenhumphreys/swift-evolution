# Closure Parameter Names and Labels

* Proposal: [SE-0118](https://github.com/apple/swift-evolution/blob/master/proposals/0118-closure-parameter-names-and-labels.md)
* Author(s): * Authors: [Dave Abrahams](https://github.com/dabrahams), [Dmitri Gribenko](https://github.com/gribozavr), [Maxim Moiseev](https://github.com/moiseev)
* Status: **[Awaiting review](#rationale)**
* Review manager: TBD

## Introduction

We propose a revision to the names and argument labels of closure
parameters in standard library APIs.

Swift-evolution thread:
[Take 2: Stdlib closure argument labels and parameter names](http://thread.gmane.org/gmane.comp.lang.swift.evolution/22188)

Discussion of earlier revision of the proposal:
[Stdlib closure argument labels and parameter names ](http://thread.gmane.org/gmane.comp.lang.swift.evolution/21046)

## Motivation

The names and argument labels of the standard library's closure
parameters have been chosen haphazardly, resulting in documentation
comments that are inconsistent and often read poorly, and in code
completion/documentation that is less helpful than it might be.
Because of trailing closure syntax, the choice of argument labels for
closures has less impact on use-sites than it might otherwise, but
there are many contexts in which labels still do appear, and
poorly-chosen labels still hurt readability.

## Proposed Solution

The table below summarizes how this proposal changes usage of the APIs
in question.

Before | After
-------|------
<br/>`s.withUtf8Buffer(invoke: processBytes)`|<br/>`s.withUtf8Buffer(processBytes)`|
<br/>`lines.split(`<br/>`  isSeparator: isAllWhitespace)`|<br/>`lines.split(`<br/>`  whereSeparator: isAllWhitespace)`|
<br/>`words.sort(isOrderedBefore: >)`|<br/>`words.sort(by: >)`|
<br/>`words.sorted(isOrderedBefore: >)`|<br/>`words.sorted(by: >)`|
<br/>`smallest = shapes.min(`<br/>`  isOrderedBefore: haveIncreasingSize)`|<br/>`smallest = shapes.min(`<br/>`  by: haveIncreasingSize)`|
<br/>`largest = shapes.max(`<br/>`  isOrderedBefore: haveIncreasingSize)`|<br/>`largest = shapes.max(`<br/>`  by: haveIncreasingSize)`|
<br/>`if a.lexicographicallyPrecedes(`<br/>`  b,`<br/>`  isOrderedBefore: haveIncreasingWeight)`|<br/>`if a.lexicographicallyPrecedes(`<br/>`  b, by: haveIncreasingWeight)`|
<br/>`ManagedBuffer<Header,Element>.create(`<br/>`  minimumCapacity: 10,`<br/>`  initialValue: makeHeader)`|<br/>`ManagedBuffer<Header,Element>.create(`<br/>`  minimumCapacity: 10,`<br/>`  makingValueWith: makeHeader)`|
<br/>`if roots.contains(isPrime) {`|<br/>`if roots.contains(where: isPrime) {`|
<br/>`if expected.elementsEqual(`<br/>`  actual, isEquivalent: haveSameValue)`|<br/>`if expected.elementsEqual(`<br/>`  actual, by: haveSameValue)`|
<br/>`if names.starts(`<br/>`  with: myFamily,`<br/>`  isEquivalent: areSameExceptForCase) {`|<br/>`if names.starts(`<br/>`  with: myFamily,`<br/>`  by: areSameExceptForCase) {`|
<br/>`let sum = measurements.reduce(0, combine: +)`|<br/>`let sum = measurements.reduce(0, +)`|
<br/>`UTF8.encode(`<br/>`  scalar, sendingOutputTo: accumulateByte)`|<br/>`UTF8.encode(`<br/>`  scalar, into: accumulateByte)`|

Note: this summary does not illustrate *parameter name* changes that
have also been made pervasively and have an effect on usability of
documentation and code completion.  Please see the diffs for examples
of those.

## Detailed design

A complete patch, showing API, documentation, and usage changes, is
available for review at
https://github.com/apple/swift/pull/2981/files.

## Impact on existing code

This change will break code that depends on the current argument
labels.  Availability annotations can be used make correcting this
breakage a normal part of the migration process.

## Alternatives considered

The initial proposal used much more-verbose labels that were designed
to be very explicit about the required semantics of the closure
parameters.  That information, however, is mostly useful to the author
of the call, and, we found, didn't do much to enhance readability at
the point of use.

## Future Extensions (Out of Scope)

The process of making these changes revealed that there are compelling
arguments for diverging from terms-of-art for the standard functional
methods. For example, the polarity of the argument to `filter` would
be crystal clear if it were renamed to `where`, and the description of
the semantics of `reduce` in the documentation strongly suggest that
`accumulated` might be an improvement.  Changing the names of these
methods should be considered in a separate proposal.

-------------------------------------------------------------------------------

# Rationale

On [Date], the core team decided to **(TBD)** this proposal.
When the core team makes a decision regarding this proposal,
their rationale for the decision will be written here.