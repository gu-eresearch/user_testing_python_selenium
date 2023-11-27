---
title: General python/selenium commands
nav: General python/selenium commands 
topics: General python/selenium commands 
description: General python/selenium commands 
---

## Get attributes of a web element

```python
dir(_css_class)

['__abstractmethods__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_abc_impl', '_execute', '_id', '_parent', '_upload', 'accessible_name', 'aria_role', 'clear', 'click', 'find_element', 'find_elements', 'get_attribute', 'get_dom_attribute', 'get_property', 'id', 'is_displayed', 'is_enabled', 'is_selected', 'location', 'location_once_scrolled_into_view', 'parent', 'rect', 'screenshot', 'screenshot_as_base64', 'screenshot_as_png', 'send_keys', 'shadow_root', 'size', 'submit', 'tag_name', 'text', 'value_of_css_property']

_css_class.get_attribute('value_of_css_property')
```



## Select an element using multiple attributes in a single xpath query
```python
"//*[@id='searchInputLabel' and @value='']"
```

## scroll using javascript
```python
context.browser.execute_script("window.scrollTo(X, Y)")
```
execute_script is a python function used to execute javascript


## accept a popup
```python
browser.switch_to.alert.accept()
```

## switch windows
```python
context.browser.switch_to.window(context.browser.window_handles[1])
```

split based on new lines
```python
_selector.text.split('\n')
```



## Select a value from a dropdown menu
```python
select = Select(context.browser.find_element(By.ID, 'importlabextractsform-importid'))
select.select_by_visible_text('Banana')
select.select_by_value('1679456710')
select.select_by_index(2)
select.select_by_visible_text('full text')
```

Assert that the string description and the string search_term are present as text in a table 
```python
description = "super powerful owls"
search_term = "flight"

table = context.browser.find_element(By.XPATH, '//table/tbody')
assert(description in table.text and search_term in table.text)
```


## Send keys
```python
from selenium.webdriver.common.keys import Keys
_element.send_keys(text)
_element.send_keys(Keys.ENTER)
_element.send_keys(Keys.BACKSPACE)
```


```python
_element.click()
_element.clear()
```

## swipe left on a button

```python
xsearch = "//*[@class=" + '"'  + css_class + '"' + "]"
download_button = context.browser.find_element(By.XPATH, xsearch)
actions = ActionChains(context.browser)
actions.drag_and_drop_by_offset(download_button, -80, 0)
actions.perform()
```

## offline / online

offline
```python
def step_impl(context):
    context.browser.set_network_conditions(
        latency=0,
        offline=True,
        download_throughput=500 * 1024,
        upload_throughput=500 * 1024)
    time.sleep(2)
```

online
```python
def step_impl(context):
    context.browser.set_network_conditions(
        latency=0,
        offline=False,
        download_throughput=500 * 1024,
        upload_throughput=500 * 1024)
    time.sleep(2)
```