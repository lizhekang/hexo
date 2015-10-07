title: 蜂鸟A31开发板入门——下
date: 2014-10-16 15:30:49
categories: 
- 开发板
tags: 
- 开发板
- linux
---

## 蜂鸟A31开发板入门——下

由于最近忙着准备大创的中期检查，所以今天才想起来更新博客。

### 关于高分屏补丁

回到正题，当时在给A31选触摸屏的时候看到有两种分辨率：800*480和1024*600。正常人看到都会选1024*600吧，反正我当时就什么都没想就选了高分辨率的。

当初问过技术人员，给我们卡发的sdk里面是带了触摸屏的驱动，按理来说应该直接编译sdk打包镜像烧录，开发板就能带起屏幕。然后让人蛋疼的情况出现了。。。屏幕是亮了，但是一直白屏无论我们对板子做任何操作（当然不包括拔电源）。

由于蜂鸟给的文档太不全了（蛋疼死了），所以跑去全志的官网上面看文档，终于发现蜂鸟给的sdk默认使用低分辨率的驱动，所以我们需要手动更换高分屏的驱动，并相应更改一些驱动。具体操作如下：

```
1、进入 lichee/linux/driver/input/touchscreen/
删除gslX680.ko后，
替换目录下的gsl1680_2.h文件 gsl1680_2.h	(下载24)
修改sdk配置sys_config.fex
———————————–
lcd 选项：为
———————————–
[lcd0_para]
lcd_used = 1 lcd_if = 0
lcd_driver_name = “default_lcd”
lcd_x =1024
lcd_y =600
lcd_width = 162
lcd_height = 121
lcd_size = “7.0”
lcd_model_name = “CHIMEI HJ080IA-01B”
lcd_dclk_freq = 51
lcd_pwm_freq = 50000
lcd_pwm_pol = 1
lcd_pwm_max_limit = 150
lcd_hbp = 160
lcd_ht = 1344
lcd_hspw = 20
lcd_vbp = 23
lcd_vt = 635
lcd_vspw = 3
lcd_lvds_if = 0
lcd_lvds_colordepth = 1
lcd_lvds_mode = 0
lcd_frm = 1
lcd_gamma_en = 0
lcd_bright_curve_en = 1
lcd_cmap_en = 0 deu_mode = 0
lcdgamma4iep = 22
smartbl_low_limit = 85
smart_color = 90 lcd_bl_en = port:PH27
lcd_power = port:PC27
lcd_pwm = port:PH13 lcdd0 = port:PD00
lcdd1 = port:PD01
lcdd2 = port:PD02
lcdd3 = port:PD03
lcdd4 = port:PD04
lcdd5 = port:PD05
lcdd6 = port:PD06
lcdd7 = port:PD07
lcdd8 = port:PD08
lcdd9 = port:PD09
lcdd10 = port:PD10
lcdd11 = port:PD11
lcdd12 = port:PD12
lcdd13 = port:PD13
lcdd14 = port:PD14
lcdd15 = port:PD15
lcdd16 = port:PD16
lcdd17 = port:PD17
lcdd18 = port:PD18
lcdd19 = port:PD19
lcdd20 = port:PD20
lcdd21 = port:PD21
lcdd22 = port:PD22
lcdd23 = port:PD23
lcdclk = port:PD24
lcdde = port:PD25
lcdhsync = port:PD26
lcdvsync = port:PD27 ————————————————
tp 触摸屏选项：为
———————————————— [ctp_para]
ctp_used = 1
ctp_name = “gsl1680_2”
ctp_twi_id = 1
ctp_twi_addr = 0x40
ctp_screen_max_x = 1024
ctp_screen_max_y = 600
ctp_revert_x_flag = 0
ctp_revert_y_flag = 0
ctp_exchange_x_y_flag = 0 ctp_int_port = port:PA23
ctp_wakeup = port:PA24
```

做完以上更改后，对sdk重新编译打包，一切大功告成了！