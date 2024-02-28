---
title: Setting up Python, Selenium and Behave for automated user testing
nav: Setup
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

Here is a look at the search.feature file. This set of tests logs into a chemical compound library app, searches for a specific compound, and then checks that the compound is returned. Notice the keywords given-when-then that are used as acceptance criteria. Following this format makes it easier for the entire team to stay on the same page with expectations for how the software should work.

```gherkin

Feature: Search for compounds based on different compound properties

    Scenario: Search for compound using a smiles code
        Given I am logged in
        When I search for the compound "Cc1ccc(c(=O)[nH]1)C(=O)N1CCCN(CC1)CC(F)(F)F" in the search bar
        Then The compound "Cc1ccc(c(=O)[nH]1)C(=O)N1CCCN(CC1)CC(F)(F)F" is returned

```
The steps within these feature file are executed using python code. The first line imports the given-when-then criteria "from behave import given, when" into python and decorators are used to turn the acceptance tests from the feature files into python functions.

### python step files
The python steps file, search.py contains the code that executes the steps in feature files. There are a suite of [selenium specific libraries](https://selenium-python.readthedocs.io/) for python.
Notice the given and when hooks, they indicate what code python runs for each step in a feature file.

Below is the python code for the first step in the above Feature file "Given I am logged in"

```python
from behave import given, when
from selenium.common.exceptions import NoSuchElementException
import time

@given('I am logged in')
def login_admin(context):
    # sends the url to the browser
    context.browser.get(context.config.userdata.get("https://my-app.uni.edu.au"))

    # click login button
        username = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.ID, "username"))
            )
        username.send_keys("my_user_name")

        password = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.ID, "password"))
            )
        password.send_keys("my_password")
    
        login_button = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.ID, "login"))
            )
        login_button.click()
```

### Python Environment File
A python environment file sits within the feature folder. This module defines code to run before and after certain events, and can include opening/closing a web browser and taking snapshots if a step fails.

You will need to setup [drivers](https://selenium-python.readthedocs.io/installation.html#drivers) 'the web browser', either locally or within your CICD environment. The following environment file has a fixture to run a chrome web browser in headless 'without a graphical user interface' and normal states. Headless is required to run in CICD pipelines such as Jenkins, while a normal state is good for troubleshooting while running tests locally.

```python
from behave import fixture, use_fixture
from selenium import webdriver
import logging
import os
from selenium.common.exceptions import NoSuchElementException
import re

work_dir = os.getcwd()

@fixture
def add_check_exists_by_xpath_func(context):
    def check_exists_by_xpath(xpath):
        try:
            context.browser.find_element_by_xpath(xpath)
        except NoSuchElementException:
            return False
        return True
    context.check_exists_by_xpath = check_exists_by_xpath

@fixture
def selenium_browser_chrome(context):
    context.browser = webdriver.Chrome()
    yield context.browser
    context.browser.quit()

@fixture
def selenium_browser_chrome_headless(context):
    options = webdriver.ChromeOptions()
    options.add_argument('headless')
    options.add_argument('--no-sandbox')
    options.add_argument('window-size=1200x600')
    prefs = {"download.default_directory": work_dir}
    options.add_experimental_option("prefs", prefs)
    context.browser = webdriver.Chrome(chrome_options=options)
    yield context.browser
    context.browser.quit()

def before_all(context):
    browser = os.getenv("BEHAVE_BROWSER", "chrome_headless")
    fixture_func = globals().get("selenium_browser_{}".format(browser.lower().strip()), None)
    if fixture_func is None:
        raise LookupError("No such browser fixture: selenium_browser_{}".format(browser.lower().strip()))
    logging.info("Using browser '{}'".format(browser))
    use_fixture(fixture_func, context)
    use_fixture(add_check_exists_by_xpath_func, context)


def after_all(context):
    context.browser.quit()

def slugify(text):
    slug = text.replace(" ", "_")
    return re.sub(r'(?u)[^-\w.]', '', slug)

def after_step(context, step):
    if step.status == 'failed':
        step_str = step.name
        scen_str = context.scenario.name
        filename = slugify("{}-{}".format(scen_str, step_str))
        context.browser.save_screenshot("{}.png".format(filename))
```



