---
title: Setting up Python, Selenium and Behave for automated user testing
nav: Introduction
---

### What is automated user testing and why it should be implemented

User testing is a method used in software development to evaluate a product's user interface and the overall flow 'user experience' of the product. For example, a user can log into the application, select three blue t-shirts and purchase them for a discount using a coupon. User tests have the added bonus of indirectly performing integration and regression tests. 

Ideally user tests are automatically implemented as part of CICD (continuous integration, continuous deployment), check the following out for more information about [CICD](https://www.redhat.com/en/topics/devops/what-is-ci-cd). The idea behind this is that any new code change that is pushed to the repository is run through user tests via CICD. If a user test fails then the new code will not be deployed.

### Using Selenium and Behave within Python for automated user testing

There are many different ways of running automated user tests, in the following we will be using Python, Selenium and Behave. [Behave](https://behave.readthedocs.io/en/stable/) provides a syntax to write tests using a natural language syntax. Such a syntax encourages collaboration among developers, QA professionals, and non-technical or business participants in software projects.

[The python selenium library](https://pypi.org/project/selenium/) contains bindings that python uses to interact with the selenium webdriver. [Selenium webdriver](https://www.selenium.dev/documentation/webdriver/) is a set of bindings used to natively control web browsers. It lets us use code to interact with a webpage or web app in the same way a user would. i.e. it allows us to programatically click on buttons, write in text fields and download data.ß

Finally, you need to install a webdriver for your prefered web browser that the tests willß run on, [chromedriver](https://developer.chrome.com/docs/chromedriver) and Firefoxes [geckodriver](https://firefox-source-docs.mozilla.org/testing/geckodriver/) are the most common.


### File Structure

The file structure is important when running python/behave tests. Two folders are required, features, and steps. Features contains the files with the .feature extension, the python environment file and the steps folder. The steps folder contains the .py files that contain the code to execute the Behave tests. 

{% include figure.html img="file-structure.png" alt="file structure" caption="The file structure required for running selenium user tests" width="75%" %}

It is not necessary to name the steps and feature file the same, however I find that it keeps the tests organised and themed.

### Behave feature files
The Behave feature files are written in [Gherkin syntax](https://cucumber.io/docs/gherkin/) and are human readable steps, they usually follow user requirements.

Consider the [search_for_a_course.feature](https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/feature/search_for_a_course.feature) feature file. This feature contains a set of tests that search for degrees on the Griffith University website and validate the returned search query. Notice the given-when-then keywords that are used as acceptance criteria. Following this format makes it easier for the entire team to stay on the same page with expectations for how the software should work.

The steps within these feature file are executed using python code, which is found within the steps file.

### python step files
The python steps file, [search_for_a_course.py](https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/steps/search_for_a_course.py) contains the code that executes the steps in feature file. There are a suite of [selenium specific libraries](https://selenium-python.readthedocs.io/) for python.
Notice the given, when and then hooks, they indicate what code python runs for each step in a feature file.

### Python Environment File
A python environment file sits within the feature folder. This module defines code to run before and after certain events, and can include opening/closing a web browser and taking snapshots if a step fails.

You will need to setup [drivers](https://selenium-python.readthedocs.io/installation.html#drivers) 'the web browser', either locally or within your CICD environment. The following environment file has a fixture to run a chrome web browser in headless 'without a graphical user interface' and normal states. Headless is required to run in CICD pipelines such as Jenkins, while a normal state is good for troubleshooting while running tests locally.

(Here)[https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/environment.py] is an example of a python environment file.
