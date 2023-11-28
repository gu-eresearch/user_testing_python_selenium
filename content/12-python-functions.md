---
title: Useful Python functions to perform specialised things
nav: Useful Python functions to perform specialised things
---


Check that a web element matches (test_item) a specified element (check_item)
```python
def check_exists(test_item, check_item):
    try:
        assert(test_item == check_item)
    except AssertionError:
        return False
    return True
```


function to wait up to timeout in seconds until a file that start with 'file_name' and ends with '.csv' is present in the work directory
```python
timeout = 30

work_dir = os.getcwd()

def download_wait(path_to_downloads = work_dir):
    seconds = 0
    dl_wait = True
    while dl_wait and seconds < timeout:
        time.sleep(1)
        dl_wait = False
        for fname in os.listdir(path_to_downloads):
            if fname.startswith('file_name') and fname.endswith('.csv'):
                dl_wait = True
        seconds += 1
    return seconds
```


function to delete files that start with 'file_name' and ends with '.csv' from the working directory
```python
work_dir = os.getcwd()
def download_delete(path_to_downloads = work_dir):
    for fname in os.listdir(path_to_downloads):
        if fname.startswith('file_name') and fname.endswith('.csv'):
            os.remove(fname)
```