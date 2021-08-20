---
description: Image Viewer
---

# نمایشگر تصویر

**هشدار**

در واقع scikit-image viewer از نسخه ی 0.18 منسوخ شده است و در نسخه ی 0.20 حذف می شود. بهتر است [بخش نمایش داده ها ](data-visualization.md)را بخوانید.



### شروع سریع

در واقع skimage.viewer یک بوم مبتنی بر matplotlib برای نمایش تصاویر و یک جعبه ابزار GUI مبتنی بر Qt است که با هدف ایجاد سهولت ویرایشگرهای تصویر تعاملی، ارائه شده است. به سادگی می توانید از آن برای نمایش تصویر استفاده کنید:

```python
from skimage import data
from skimage.viewer import ImageViewer

image = data.coins()
viewer = ImageViewer(image)
viewer.show()
```

البته، شما می توانید به آسانی از imshow از matplotlib \(یا متناوباً، skimage.io.imshow که از چندین افزونه io پشتیبانی می کند\) برای نمایش تصاویر استفاده کنید. مزیت ImageViewer این است که می توانید به راحتی افزونه هایی را برای دستکاری تصاویر وجود دارند را اضافه کنید. در حال حاضر، فقط چند پلاگین برای آن وجود دارند که استفاده از آن ها بسیار ساده است. قبل از پرداختن به جزئیات هر یک از این ها، نمونه ای از نحوه افزودن افزونه از پیش تعریف شده به بیننده را مشاهده کنید:

```python
from skimage.viewer.plugins.lineprofile import LineProfile

viewer = ImageViewer(image)
viewer += LineProfile(viewer)
overlay, data = viewer.show()[0]
```

روش show\(\) بیننده لیستی از چندتایی را برای هر افزونه پیوست برمی گرداند. هر تاپل شامل دو عنصر است: یک روکش با همان شکل تصویر ورودی و یک فیلد داده \(که ممکن است هیچکدام\). یک کلاس افزونه مقدار برگشتی آن را در روش خروجی خود مستند می کند.

در این مثال، فقط یک افزونه ضمیمه شده است، بنابراین لیستی که نمایش داده می شود دارای طول 1 خواهد بود. در اینجا، همپوشانی حاوی تصویری از خط کشیده شده روی بیننده است و داده ها شامل مشخصات شدت تصویر به صورت یک بُعدی در امتداد آن خط هستند.

در حال حاضر، تعداد زیادی افزونه از پیش تعریف شده وجود ندارد، اما رابط کاربری بسیار ساده ای برای ایجاد افزونه شخصی شما وجود دارد. ابتدا، اجازه دهید افزونه ای برای فراخوانی تابع denoise\_tv\_bregman برای تنوع کل حذف نویز ایجاد کنیم:

```python
from skimage.filters import denoise_tv_bregman
from skimage.viewer.plugins.base import Plugin

denoise_plugin = Plugin(image_filter=denoise_tv_bregman)
```

**یادداشت**

این افزونه فرض می کند اولین آرگومانی که به فیلتر تصویر داده می شود، تصویر از بیننده تصویر است. در آینده، این باید تغییر کند تا بتوانید تصویر را به آرگومان متفاوتی از عملکرد فیلتر منتقل کنید.

برای تعامل واقعی با فیلتر، باید ابزارک هایی را تنظیم کنید که پارامترهای عملکرد را تنظیم می کنند. به طور معمول، این بدان معناست که یک ویجت لغزنده اضافه کنید و آن را به پارامتر فیلتر و حداقل و حداکثر مقادیر لغزنده متصل کنید:

```python
from skimage.viewer.widgets import Slider
from skimage.viewer.widgets.history import SaveButtons

denoise_plugin += Slider('weight', 0.01, 0.5, update_on='release')
denoise_plugin += SaveButtons()
```

در اینجا، ما یک ویجت لغزنده را به آرگومان "weight" فیلتر متصل می کنیم. ما همچنین تعدادی دکمه برای ذخیره تصویر در فایل یا پشته تصویر scikit-image اضافه کردیم \(به skimage.io.push و skimage.io.pop مراجعه کنید\).

تنها چیزی که باقی می ماند ایجاد یک نمایشگر تصویر و افزودن افزونه به آن بیننده است.

```python
viewer = ImageViewer(image)
viewer += denoise_plugin
denoised = viewer.show()[0][0]
```

در اینجا، ما فقط به پوششی که افزونه باز می گرداند، دسترسی داریم که حاوی تصویر فیلتر شده برای آخرین تنظیم وزن مورد استفاده است.

![Image Viewer](.gitbook/assets/denoise_viewer_window.png)



این بخش به پایان رسید اگر سوالی در ارتباط با هر یک از بخش های بالا دارید در بخش [issueها ](https://github.com/amirshnll/skimage-persian-userguide/issues)از من بپرسید.

