# Testing expectations

This page describes what we're looking for with testing on new code being added to Dynamo.

So. You've got a new node that you want to add. Awesome. It's time to add some tests. There are two reasons to do this.

1. It helps to find out where it doesn't work
2. When someone else changes something that breaks your node, it should break the tests. That way that person who broke the tests needs to go and fix it. If it doesn't break the tests, then it's largely your problem to deal with the users' whose models get broken.

Testing in Dynamo comes in two broad types: Unit Tests, System Tests.

## Unit Tests

Unit tests should test as little as possible. If you built a node that computes square roots via a C# zero touch node, the test should test only that DLL and make C# calls directly to that code. The unit tests shouldn't even include Dynamo.

It should include:

* Positive tests (it does the right thing)
* Negative tests (it doesn't barf when given garbage input)
* Regression tests (when someone finds a bug in your code, write a test to ensure that it doesn't reoccur)

They should be small, fast and reliable. The majority of tests should be unit tests.

## System Tests

System tests operate on multiple components and test how they fit together. They should be used and crafted carefully. 

Why? They're expensive to run. We need to keep the test suite running with as little latency as possible.

When writing a system test, the first thing to do is to look at [this](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) diagram and work out which sub-systems you're covering.

Ideally, there would be a progressive series of tests covering increasing sets of the interacting blocks so when the tests start failing it's very quick to find out where the problem is.

Examples of things that need System Tests:

* A new type of Revit node that stores multiple elements in trace rather than a single element
* A new watch node that displays data differently

Example of things that don't need System Tests:

* A new math node
* A string processing library

System tests should:

* Assert the correct behavior
* Assert an absence of pathological behaviors, e.g. no exceptions