#### 通过命令设置笔记本kali_linux的屏幕亮度

参考资料:  [Backlight - ArchWiki](https://wiki.archlinux.org/index.php/Backlight)

1. 调整屏幕亮度第一种方法：xrandr ,优点是简单方便  
缺点是:However, this is a software only modification, if your hardware has   support to actually change the brightness, you will probably prefer to use   xbacklight.  

        #xrandr | grep connected | cut -f1 -d" "
        Use the output name found (in this case "eDP-1") and adjust the brightness
        #xrandr --output eDP-1 --brightness 0.7

2. 调整屏幕亮度第二种方法：`xbacklight`，始终无法使用`xbacklight -set` 命令,不论怎么在`/etc/X11/xorg.conf`里面尝试各种内容，结果都是失败，最后放弃了用命令，而是改成了直接`echo 6|tee` 这种方式.

        #安装
        apt-get install xbacklight

        root@kali:~# xbacklight -set 7        
        No outputs have backlight property

        root@kali:~# readlink /sys/class/backlight/acpi_video0
        ../../devices/pci0000:00/0000:00:02.0/backlight/acpi_video0

        root@kali:~# find /sys/ -type f -iname '*brightness*'
        /sys/devices/pci0000:00/0000:00:1c.3/0000:02:00.0/leds/phy0-led/max_brightness
        /sys/devices/pci0000:00/0000:00:1c.3/0000:02:00.0/leds/phy0-led/brightness
        /sys/devices/pci0000:00/0000:00:02.0/backlight/acpi_video0/max_brightness
        /sys/devices/pci0000:00/0000:00:02.0/backlight/acpi_video0/brightness
        /sys/devices/pci0000:00/0000:00:02.0/backlight/acpi_video0/actual_brightness
        /sys/devices/platform/i8042/serio0/input/input0/input0::capslock/max_brightness
        /sys/devices/platform/i8042/serio0/input/input0/input0::capslock/brightness
        /sys/devices/platform/i8042/serio0/input/input0/input0::scrolllock/max_brightness
        /sys/devices/platform/i8042/serio0/input/input0/input0::scrolllock/brightness
        /sys/devices/platform/i8042/serio0/input/input0/input0::numlock/max_brightness
        /sys/devices/platform/i8042/serio0/input/input0/input0::numlock/brightness
        /sys/module/video/parameters/brightness_switch_enabled
        /sys/module/i915/parameters/invert_brightness

        #最大亮度只有９，多数笔记本最大亮度应该是１５
        root@kali:~# cat /sys/devices/pci0000:00/0000:00:02.0/backlight/acpi_video0/max_brightness
        9

        #当前亮度５
        cat /sys/devices/pci0000:00/0000:00:02.0/backlight/acpi_video0/actual_brightness
        5

        ＃设置新的亮度
        echo 7 |tee /sys/devices/pci0000:00/0000:00:02.0/backlight/acpi_video0/brightness

