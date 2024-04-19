---
title: Setting up Python, Selenium and Behave for automated user testing
nav: Introduction & Setup
---

### What is automated user testing and why it should be implemented

User testing is a method used in software development to evaluate a product's user interface and the overall flow 'user experience' of the product. For example, a user can log into the application, select three blue t-shirts and purchase them for a discount using a coupon. User tests have the added bonus of indirectly performing integration and regression tests. 

Ideally user tests are automatically implemented as part of CICD (continuous integration, continuous deployment), check the following out for more information about [CICD](https://www.redhat.com/en/topics/devops/what-is-ci-cd). The idea behind this is that any new code change that is pushed to the repository is run through user tests via CICD. If a user test fails then the incoming code changes will not be deployed to UAT, dev or production environments.

### Using Selenium and Behave within Python for automated user testing

There are many different ways of running automated user tests, in this tutorial we will be using the Python-Selenium-Behave framework. [Behave](https://behave.readthedocs.io/en/stable/) provides a syntax to write tests using a natural language syntax. Such a syntax encourages collaboration among developers, QA professionals, and non-technical or business participants in software projects.

[The python selenium library](https://pypi.org/project/selenium/) contains bindings that python uses to interact with the selenium webdriver. [Selenium webdriver](https://www.selenium.dev/documentation/webdriver/) is a set of bindings used to natively control web browsers. It lets us use code to interact with a webpage or web app in the same way a user would. i.e. it allows us to programatically click on buttons, write in text fields and download data.

Finally, you need to install a webdriver for your prefered web browser that the tests will run on, [chromedriver](https://developer.chrome.com/docs/chromedriver) and Firefoxes [geckodriver](https://firefox-source-docs.mozilla.org/testing/geckodriver/) are the most common.

The code for this tutorial is [here](https://github.com/gu-eresearch/selenium-python-code/tree/main/uat)
