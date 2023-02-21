## LVGL二维码显示（遇到的问题以及解决思路)
具体情况是，用官方的例程，创建绘制二维码界面，显示二维码正常，且能扫出字符串
但是移植到单片机上，就先后出现
### - 单片机可用栈空间不足导致死机
解决方法如下：
在 lv_qrcode_update 函数中，我们可以看到定义了两个特别大的数组
```C
  uint8_t qr0[qrcodegen_BUFFER_LEN_MAX];
  uint8_t data_tmp[qrcodegen_BUFFER_LEN_MAX];
```
(用VScode或者自己计算，你能看到qr0就近4k的大小了，然后两个数组，近8k！！)
跳转到 qrcodegen_BUFFER_LEN_MAX 定义的地方，可以找到 qrcodegen_VERSION_MAX 这个宏，默认是40，我修改成10字节
**qrcodegen_BUFFER_LEN_MAX** -- 就是最坏情况下存储一个二维码所需的字节数(实际看需求调整)


### - 解决栈空间后，不管调整显示大小，都是显示==no data==
解决方法如下：
在lv_conf.h 中，修改
```C
  #  define LV_MEM_SIZE    (26U * 1024U)
```
因为这个lv_conf.h是lvgl的配置文件，LV_MEM_SIZE是提供给LVGL的空间，具体看自己剩余空间能不能再大一点

==如果处理完上面的，你还是不行，显示no data那么十有八九就是你宏开少了，别问，问就是我也这样子==
```C
  /* 1: Enable indexed (palette) images */
  #define LV_IMG_CF_INDEXED       1	//启用索引(调色板)图像

  /* 1: Enable alpha indexed images */
  #define LV_IMG_CF_ALPHA         1	//启用alpha索引图像
```
把对应绘制，和混色的两个宏开起来，就可以了
