---
title: What are flaky tests and how do we minimise them
nav: Minimising flakyness
---

Flaky tests are a common and sometimes midly annoying part of developing automated user tests. A flaky test will produce inconsistent results, or, will inconsistently pass or fail without any changes to the underlying codebase that your testing. 
When you first come across a flaky you should determine if the flakyness is a result of the application under test, or the test itself.

The following (write-up)[https://testing.googleblog.com/2020/12/test-flakiness-one-of-main-challenges.html] contains a mountain of valuable information about causes and remedies for flaky tests.

One very common reason for flaky tests is a timeout error when an element has not loaded, even though the element is there. This occurs when an action has been performed, for exmaple click on a button to load a form, then the next step fills in the name field in the form, however selenium runs to fast and the name field has not yet loaded, throwing a timeout error.

{% include figure.html img="7-prevent-flaky-tests.png" alt="Timeout error" caption="A typical timeout error will look something like this" width="75%" %}


