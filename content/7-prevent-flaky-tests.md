---
title: What are flaky tests and how do we minimise them
nav: Minimising flakyness
---

Flaky tests are a common and sometimes midly annoying part of developing automated user tests. A flaky test will produce inconsistent results, or, will inconsistently pass or fail without any changes to the underlying codebase that your testing. 
When you first come across a flaky you should determine if the flakyness is a result of the application under test, or the test itself.

The following (write-up)[https://testing.googleblog.com/2020/12/test-flakiness-one-of-main-challenges.html] contains a mountain of valuable information about causes and remedies for flaky tests.

One very common reason for flaky tests occurs when an action has been performed, and then the next step times out as it has not loaded yet.
{% include figure.html img="7-prevent-flaky-tests.png" alt="Timeout error" caption="A typical timeout error will look something like this" width="75%" %}