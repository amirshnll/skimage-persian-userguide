---
description: Getting started
---

# از اینجا شروع کنید

کتابخانه ی scikit-image یک پکیج پایتون برای پردازش تصویر است که با آرایه های [numpy ](https://numpy.org/)کار می کند. این پکیج به عنوان skimage به برنامه import می شود:

```python
>>> import skimage
```

بیشتر عملکردهای skimage در زیر ماژول ها یافت می شود:

```python
>>> from skimage import data
>>> camera = data.camera()
```

لیستی از زیر ماژول ها و توابع در [صفحه مرجع API](https://scikit-image.org/docs/stable/api/api.html) یافت می شود.

در scikit-image ، تصاویر به عنوان آرایه های NumPy نشان داده می شوند، به عنوان مثال آرایه های 2 بعدی برای تصاویر دو بعدی در مقیاس خاکستری یا grayscale :

```python
>>> type(camera)
<type 'numpy.ndarray'>
>>> # An image with 512 rows and 512 columns
>>> camera.shape
(512, 512)
```

زیرمجموعه skimage.data مجموعه ای از توابع را که تصاویر نمونه را برمی گردانند ، ارائه می دهد که می توان از آنها برای شروع سریع استفاده از توابع scikit-image استفاده کرد:

```python
>>> coins = data.coins()
>>> from skimage import filters
>>> threshold_value = filters.threshold_otsu(coins)
>>> threshold_value
107
```

> تصاویر نمونه را می توان به نوعی تصاویر **بنچ مارک یا benchmark** دانست که برای راحتی کار در کتابخانه موجود می باشند و شما نیاز به اینکه آن تصویر را با برنامه ی خود جابجا کنید ندارید و در کتابخانه به راحتی می توانید از آن ها استفاده کنید.

البته امکان بارگذاری تصاویر دلخواه خود را به عنوان آرایه NumPy از فایل های تصویری با استفاده از skimage.io.imread \(\) نیز هم دارید که در زیر می توانید نمونه ی آن را مشاهده کنید:

```python
>>> import os
>>> filename = os.path.join(skimage.data_dir, 'moon.png')
>>> from skimage import io
>>> moon = io.imread(filename)
```

برای بارگذاری چندین تصویر از [natsort ](https://pypi.org/project/natsort/)استفاده کنید:

```python
>>> import os
>>> from natsort import natsorted, ns
>>> from skimage import io
>>> list_files = os.listdir('.')
>>> list_files
['01.png', '010.png', '0101.png', '0190.png', '02.png']
>>> list_files = natsorted(list_files)
>>> list_files
['01.png', '02.png', '010.png', '0101.png', '0190.png']
>>> image_list = []
>>> for filename in list_files:
...   image_list.append(io.imread(filename))
```



این بخش به پایان رسید اگر سوالی در ارتباط با هر یک از بخش های بالا دارید در بخش [issueها ](https://github.com/amirshnll/skimage-persian-userguide/issues)از من بپرسید.

