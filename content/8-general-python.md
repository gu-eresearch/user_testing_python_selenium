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

## wait for scrolling to stop
```python
@when(u'scrolling has stopped')
# function that waits until scrolling down the page has stopped
def step_impl(context):
    initial_scroll_position = context.driver.execute_script("return window.scrollY;")

    seconds = 0.0
    _scrolling = True
    while _scrolling and seconds < timeout:
        after_click_scroll_position = context.driver.execute_script("return window.scrollY;")
        initial_scroll_position = after_click_scroll_position
        if initial_scroll_position:
            _scrolling = False
        seconds += 0.1
        time.sleep(0.1)
```



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



## Select a value from a dropdown menu
```python
select = Select(context.browser.find_element(By.ID, 'importlabextractsform-importid'))
select.select_by_visible_text('Banana')
select.select_by_value('1679456710')
select.select_by_index(2)
select.select_by_visible_text('full text')
```
OR 

```python
def select_option_by_value(driver, select_id, value):
    select_element = WebDriverWait(driver, timeout).until(
        EC.presence_of_element_located(
            (By.CSS_SELECTOR, f'[id="{select_id}"]'))
    )

    actions = ActionChains(driver)
    actions.move_to_element(select_element)
    actions.click().perform()

    actions.send_keys(value)
    actions.send_keys(Keys.RETURN)
    actions.perform()
    time.sleep(3)
```

## Assert that the string description and the string search_term are present as text in a table 
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
