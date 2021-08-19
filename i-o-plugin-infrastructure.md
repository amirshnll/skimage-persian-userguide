---
description: I/O Plugin Infrastructure
---

# ساختار ورودی/خروجی

یک افزونه از دو فایل منبع و توصیف کننده .ini تشکیل شده است. فرض کنید ما می خواهیم افزونه ای برای نمایش با استفاده از matplotlib ارائه دهیم. با افزونه mpl این کار را انجام می دهیم:

```python
skimage/io/_plugins/mpl.py
skimage/io/_plugins/mpl.ini
```

نام فایل های .py و .ini باید مطابقت داشته باشد. در داخل فایل .ini، متا داده های افزونه ارائه می شود:

```python
[mpl] <-- name of the plugin, may be anything
description = Matplotlib image I/O plugin
provides = imshow <-- a comma-separated list, one or more of
                      imshow, imsave, imread, _app_show
```

خط "provides" تمام توابع ارائه شده توسط افزونه را لیست می کند. از آنجا که افزونه ما imshow را ارائه می دهد، باید آن را در mpl.py تعریف کنیم:

```python
# This is mpl.py

import matplotlib.pyplot as plt

def imshow(img):
    plt.imshow(img)
```

توجه داشته باشید که به طور پیش فرض، imshow مسدود نیست، بنابراین یک تابع ویژه \_app\_show باید برای مسدود کردن GUI ارائه شود. ما می توانیم افزونه خود را تغییر دهیم تا به شرح زیر ارائه شود:

```python
[mpl]
provides = imshow, _app_show
```

```python
# This is mpl.py

import matplotlib.pyplot as plt

def imshow(img):
    plt.imshow(img)

def _app_show():
    plt.show()
```

هرگونه افزونه در فهرست \_plugins پس از وارد شدن به طور خودکار توسط skimage.io بررسی می شود. می توانید تمام افزونه های سیستم خود را لیست کنید:

```python
>>> import skimage.io as io
>>> io.find_available_plugins()
{'gtk': ['imshow'],
 'matplotlib': ['imshow', 'imread', 'imread_collection'],
 'pil': ['imread', 'imsave', 'imread_collection'],
 'qt': ['imshow', 'imsave', 'imread', 'imread_collection'],
 'test': ['imsave', 'imshow', 'imread', 'imread_collection'],}
```

یا فقط مواردی که قبلاً بارگیری شده اند:

```python
>>> io.find_available_plugins(loaded=True)
{'matplotlib': ['imshow', 'imread', 'imread_collection'],
 'pil': ['imread', 'imsave', 'imread_collection']}
```

یک افزونه با استفاده از دستور use\_plugin بارگیری می شود:

```python
>>> import skimage.io as io
>>> io.use_plugin('pil') # Use all capabilities provided by PIL
```

یا

```python
>>> io.use_plugin('pil', 'imread') # Use only the imread capability of PIL
```

توجه داشته باشید که اگر بیش از یک افزونه عملکرد خاصی را ارائه دهد، آخرین افزونه بارگیری شده استفاده می شود.

برای پرسش از قابلیت های یک افزونه، از plugin\_info استفاده کنید:

```python
>>> io.plugin_info('pil')
>>>
{'description': 'Image reading via the Python Imaging Library',
 'provides': 'imread, imsave'}
```



این بخش به پایان رسید اگر سوالی در ارتباط با هر یک از بخش های بالا دارید در بخش [issueها ](https://github.com/amirshnll/skimage-persian-userguide/issues)از من بپرسید.

