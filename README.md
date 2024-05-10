# FYSETC-Prusa-MK3S-clone
FYSETC Prusa MK3S clone Kit Information

![image-Prusa MK3S](/Prusa_MK3S.jpg)
1.Prusa MK3S+ STL file updates 2020.7.10
These STL files are compatible with the FYSETC prusa MK3S+ 3D printer kit. When compared to the official kit from Prusa, we have added three parts: PSU-cover-MK3_bottom, PSU-cover-MK3_top, and Spool-holder. All the other parts are the same as the official MK3S+ from Prusa. 
You can also download the files here：
https://www.thingiverse.com/thing:4728793

2.Prusa MK3S assembly tutorial  
<https://help.prusa3d.com/en/category/original-prusa-i3-mk3s-kit-assembly_280>

3.Prusa MK3S knowledge base  
<https://help.prusa3d.com/en/tag/mk3s/>

4.Prusa MK3S assembly tutorial update 2020.8.3  
The tutorial is very thorough and well explained. We also provide a video tutorial for the assembly process. It is advisable to check both tutorials as you build，below you can watch the video tutorial divided into 5 parts:

https://youtu.be/e2qvbB0Xqfg

https://youtu.be/ntqnIuPzF5k

https://youtu.be/NayX_Eky94o

https://youtu.be/t37JR-865cU

https://youtu.be/UVHUfH4zhIA

5.Go to buy link：https://es.aliexpress.com/item/32968736910.html?spm=a219c.12010615.8148356.3.26ed28558FsEev


# BugFix：
## MK3S+ no Bightness menu fix

### Phenomenon:

Originally, there would be a brightness menu under the setting to adjust the screen backlight, but in fact there isn't;

### Reason:

In order to be more stable during the production of the 2004 screen, we upgraded the triode (BC817) that controls the backlight with a MOS tube (AO3400); The brightness menu will only be available when it is in a low state, so many people who use this screen do not see the brightness menu.

### Solution:

#### 1. Software Method :

Compile the firmware by yourself, and turn off some functions of power-on detection low level.
Open the backlight.cpp file and find the following location:

```cpp
void backlight_init()
{
//check for backlight support on lcd
    SET_INPUT(LCD_BL_PIN);
    WRITE(LCD_BL_PIN,HIGH);
    _delay(10);
    backlightSupport = !READ(LCD_BL_PIN);
    if (!backlightSupport) return;

//initialize backlight
```
Change to：
```cpp
void backlight_init()
{
//check for backlight support on lcd
    SET_INPUT(LCD_BL_PIN);
    WRITE(LCD_BL_PIN,HIGH);
    _delay(10);
    backlightSupport = 1 // !READ(LCD_BL_PIN);// 更改这一行
    if (!backlightSupport) return;

//initialize backlight
```

Then save the file, compile the firmware and download it to the motherboard.

We also provide the compiled firmware on github, copy the following address to the browser to download it directly.

https://github.com/FYSETC/FYSETC-Prusa-MK3S-clone/blob/master/Prusa.mk3s%2B_3.12.rc1_add_brightness.hex

#### 2. Hardware Method :

Replace T1 with a transistor BC817 or BC847 and other compatible NPN transistors. Screens versions produced before 2021 may also need to replace R3 with a 100R resistor.

![image-20221229175345343](/image-20221229175345343.png)
