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



## Different ways of selecting elements using xpath's

Select an element using multiple attributes in a single xpath query
```python
"//*[@id='searchInputLabel' and @value='']"
```

Select an element using a partial string
```python
"//*[starts-with(@class, 'ChoiceGroup')]"

"//*[ends-with(@id, 'ChoiceGroup')]"

"//*[contains(.,'AI Inaccurate')]"
```
## 
```python
selector_visible('video', 'selector').find_element(By.XPATH, "//*[contains(.,'AI Inaccurate')]").click()
selector_visible('video', 'selector').get_attribute('src')
```

# Scrolling and navigation

## scroll using ActionChains

```python
iframe = driver.find_element(By.TAG_NAME, "iframe")
ActionChains(driver)\
    .scroll_to_element(iframe)\
    .perform()
```

## scroll using javascript
```python
context.browser.execute_script("window.scrollTo(X, Y)")
```
execute_script is a python function used to execute javascript


## accept a popup
```python
context.browser.switch_to.alert.accept()
```

## switch windows
```python
context.browser.switch_to.window(context.browser.window_handles[1])
```

## navigate forward and back

```python
browser.back()
browser.forward()
```

## split based on new lines
```python
_selector.text.split('\n')
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

## Download file/ Delete file once downloaded

function to wait up until timeout files to download - then 2nd function to delete downloaded file
```python
def download_wait(fname_start, fname_end, path_to_downloads = os.getcwd()):
    seconds = 0
    dl_wait = True
    time.sleep(1)
    while dl_wait and seconds < timeout:
        # time.sleep(1)
        dl_wait = False
        for fname in os.listdir(path_to_downloads):
            if fname.startswith(fname_start) and fname.endswith(fname_end):
                dl_wait = True
        seconds += 1
    return seconds

def download_delete(fname_start, fname_end, path_to_downloads = os.getcwd()):
    for fname in os.listdir(path_to_downloads):
        if fname.startswith(fname_start) and fname.endswith(fname_end):
            os.remove(fname)
```
