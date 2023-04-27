---
title: Common python behave functions that are highly recycable
nav: Common
---





```python
from behave import when, then, given
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.common.keys import Keys
from pathlib import Path
import os.path
import time

timeout = 30

def check_exists_by_xpath(context, xpath):
    try:
        context.browser.find_element(By.XPATH, xpath)
    except NoSuchElementException:
        return False
    return True


def check_exists_by_css_selector(context, css_selector):
    try:
        context.browser.find_element(By.CSS_SELECTOR, css_selector)
    except NoSuchElementException:
        return False
    return True


########################################################################################################################
########################################################################################################################
@when(u'I click on the button that contains the text "{text}"')
def step_impl(context, text):
    xsearch = "//*[contains(text()," + '"' + text + '"' + ")]"
    _button= WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    _button.click()

########################################################################################################################
########################################################################################################################
@when(u'I click on the button that contains the id "{id}"')
def step_impl(context, id):
    _button = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.ID, id))
        )
    
    _button.click()

########################################################################################################################
########################################################################################################################
@when(u'I click on the button that contains the href "{href}"')
def step_impl(context, href):
    xsearch = "//*[@href = " + '"'  + href + '"' + "]"
    _button= WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    
    _button.click()

########################################################################################################################
########################################################################################################################
@when(u'I click on the button that contains the selector "{selector}"')
def step_impl(context, selector):
    _button= WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, selector))
        )
    
    _button.click()

########################################################################################################################
########################################################################################################################
@when(u'I send the text "{text}" to the id "{id}"')
def step_impl(context, text, id):
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.ID, id))
        )

    _field.send_keys(text)

########################################################################################################################
########################################################################################################################
@when(u'I send the text "{text}" to the field with the xpath name "{name}" and click enter')
def step_impl(context, text, name):
    xsearch = "//*[@name = " + '"'  + name + '"' + "]"
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

    _field.send_keys(text)
    _field.send_keys(Keys.ENTER)

########################################################################################################################
########################################################################################################################
@when(u'I click on the button that contains the value "{value}"')
def step_impl(context, value):
    xsearch = "//*[@value = " + '"'  + value + '"' + "]"
    _button= WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    
    _button.click()

########################################################################################################################
########################################################################################################################
@then(u'The text "{text}" is visible')
def step_impl(context, text):
    xsearch = "//*[text() = " + '"'  + text + '"' + "]"
    _ = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

########################################################################################################################
########################################################################################################################
@then(u'The id "{id}" is visible')
def step_impl(context, id):
    _ = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.ID, id))
        )


########################################################################################################################
########################################################################################################################
@then(u'The text "{text}" is visible as a "{tag}"')
def step_impl(context, text, tag):
        xsearch = "//" + tag + "[text() = " + '"'  + text + '"' + "]"
    _ = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    
########################################################################################################################
########################################################################################################################
@then(u'The text "{text}" is NOT visible as a "{tag}"')
def step_impl(context, text, tag):
xsearch = "//" + tag + "[text() = " + '"'  + text + '"' + "]"
    text_visiblity = check_exists_by_xpath(context, xsearch)
    assert(text_visiblity == False)

########################################################################################################################
########################################################################################################################
@then(u'The text "{text}" is NOT visible')
def step_impl(context, text):
    xsearch = "//*[text() = " + '"'  + text + '"' + "]"
    text_visiblity = check_exists_by_xpath(context, xsearch)
    assert(text_visiblity == False)

########################################################################################################################
########################################################################################################################
@then(u'The xpath "{xpath}" is NOT visible')
def step_impl(context, xpath):
    xpath_visiblity = check_exists_by_xpath(context, xpath)
    assert(xpath_visiblity == False)

########################################################################################################################
########################################################################################################################
@then(u'The xpath "{xpath}" is visible')
def step_impl(context, xpath):
    xpath_visiblity = check_exists_by_xpath(context, xpath)
    assert(xpath_visiblity == True)

########################################################################################################################
########################################################################################################################
@then(u'The css_selector "{css_selector}" is NOT visible')
def step_impl(context, css_selector):
    css_visiblity = check_exists_by_css_selector(context, css_selector)
    assert(css_visiblity == False)


########################################################################################################################
########################################################################################################################
@when(u'I search the term "{search_term}" in the field "{field}"')
def step_impl(context, search_term, field):
    xsearch = "//*[@name=" + '"'  + field + '"' + "]"
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    
    _field.send_keys(search_term)
    _field.send_keys(Keys.RETURN)

########################################################################################################################
########################################################################################################################
@when(u'I click on the button that contains the title "{title}"')
def step_impl(context, title):
    xsearch = "//*[@title = " + '"'  + title + '"' + "]"
    _button = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

    _button.click()

########################################################################################################################
########################################################################################################################
@when(u'I click on and clear the field that contains the id "{id}"')
def step_impl(context, id):
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.ID, id))
        )

    _field.click()
    _field.clear()

########################################################################################################################
########################################################################################################################
@when(u'I click on and clear the field that contains the name "{name}"')
def step_impl(context, name):    
    xsearch = "//*[@name= " + '"'  + name + '"' + "]"
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

    _field.click()
    _field.clear()

########################################################################################################################
########################################################################################################################
@when(u'I click on the field that contains the class "{_class}"')
def step_impl(context, _class):
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.CLASS_NAME, _class))
        )

    _field.click()

########################################################################################################################
########################################################################################################################
@then(u'The Navbar Item "{text}" is NOT visible')
def step_impl(context, text):
    xsearch = "//*[text() = " + '"'  + text + '"' + "]"
    text_visiblity = check_exists_by_xpath(context, xsearch)
    assert(text_visiblity == False)

########################################################################################################################
########################################################################################################################
@then(u'The Navbar Item "{text}" is visible')
def step_impl(context, text):
    xsearch = "//*[text() = " + '"'  + text + '"' + "]"
    _ = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

########################################################################################################################
########################################################################################################################
@when(u'I attach the file "{file}" to the form element with id "{id}"')
def step_impl(context, file, id):
    file_upload = WebDriverWait(context.browser, 10).until(
        EC.presence_of_element_located((By.ID, id))
    )
    file_upload.send_keys(os.path.abspath(Path(__file__).parent.absolute() / file))

########################################################################################################################
########################################################################################################################
@when(u'I click on the checkbox that has the value "{value}"')
def step_impl(context, value):
    xsearch = "//input[@type='checkbox'][@value = " + '"'  + value + '"' + "]"
    _button= WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    
    _button.click()
    
########################################################################################################################
########################################################################################################################
@when(u'I click on the "{index}"th checkbox of name "{name}"')
def step_impl(context, index, name):
    xsearch = "(//input[@type='checkbox'][@name='" + name + "'])[" + index + "]"
    _button= WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    
    _button.click()    

########################################################################################################################
########################################################################################################################
@when(u'I send the text "{text}" to the field with the xpath "{xsearch}" and click enter')
def step_impl(context, text, xsearch):
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )
    _field.send_keys(text)
    _field.send_keys(Keys.ENTER)

########################################################################################################################
########################################################################################################################
@when(u'I send the text "{text}" to the field with the css selector "{cssselector}" and click enter')
def step_impl(context, text, cssselector):
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, cssselector))
        )
    _field.send_keys(text)
    _field.send_keys(Keys.ENTER)    

########################################################################################################################
########################################################################################################################
@then(u'I wait "{timer}" seconds')
def step_impl(context, timer):
    time.sleep(int(timer))

```

## Interactive

Swipe left on a button (i.e. mac iphone style button)
```python
@then(u'I swipe left on the button with the class "{css_class}"')
def step_impl(context, css_class):
    xsearch = "//*[@class=" + '"'  + css_class + '"' + "]"
    download_button = context.browser.find_element(By.XPATH, xsearch)
    actions = ActionChains(context.browser)
    actions.drag_and_drop_by_offset(download_button, -80, 0)
    actions.perform()
```

```python
@when(u'I adjust the Slider Threshold to "{num}"')
def step_impl(context, num):
    # The slider Threshold is between 0-1, the html for the slider is measured in pixels based on window size
    # So get the size of the slider in pixels and scale the 0-1 Slider threshold to suit
    num = float(num) * context.browser.find_element(By.XPATH, "//*[@class='mb-0']").size['width']

    _slider = context.browser.find_element(By.XPATH, "//*[@class='slider-handles']/div")
    actions = ActionChains(context.browser)
    actions.drag_and_drop_by_offset(_slider, num, 0)
    actions.perform()

@when(u'I adjust the Slider Threshold back to 0')
def step_impl(context):
    # Slider Threshold is between 0-1, the html for the slider is measured in pixels based on window size
    # So get the size of the slider in pixels and scale the 0-1 Slider threshold to suit
    current = float(context.browser.find_element(By.XPATH, "//*[@class='mb-0']").text.split(' ')[2]) * context.browser.find_element(By.XPATH, "//*[@class='mb-0']").size['width']
    _slider = context.browser.find_element(By.XPATH, "//*[@class='slider-handles']/div")
    actions = ActionChains(context.browser)
    actions.drag_and_drop_by_offset(_slider, -current, 0)
    actions.perform()
```