# awtk-port-ingenic
AWTK port for Ingenic

# AWTK和君正针对mips-linux平台的移植。

[AWTK](https://github.com/zlgopen/awtk)是为嵌入式系统开发的GUI引擎库。

[awtk-port-ingenic](https://github.com/zlgopen/awtk-port-ingenic)是君正对AWTK在mips-linux上的移植。

本项目以ZLG周立功 linux开发套件 君正m200 x1000 x1830开发板 为载体移植，其它开发板可能要做些修改，有问题请请创建issue。
君正商务邮箱:sale@ingenic.com

## 使用方法

* 1.获取源码

```
git clone https://github.com/zlgopen/awtk.git
git clone https://github.com/zlgopen/awtk-examples.git
git clone https://github.com/zlgopen/awtk-port-ingenic.git
cp -r awtk-port-ingenic/awtk-linux-fb_egl .
cd awtk-linux-fb_ingenic
```

* 2.配置环境变量，设置工具链的路径

```
export COMPILER_PATH=/home/user/platform/prebuilts/toolchains/mips-gcc520-glibc222
```

* 3.如果使用GPU加速，请将diff文件夹中的diff合并到awtk中
     cp diff/awtk适配ingenic_GPU.diff ../awtk
     cd ../awtk
     git apply awtk适配ingenic_GPU.diff

```
export COMPILER_PATH=/home/user/platform/prebuilts/toolchains/mips-gcc520-glibc222
```

* 4.编辑 awtk_config.py 设置编译模式

```
使用GPU绘制时 ：NANOVG_BACKEND='GLES2'
使用CPU绘制时 ：NANOVG_BACKEND='AGGE'
在x1830开发环境使用CPU绘制时 ：NANOVG_BACKEND='AGGE' BOARD_PLATFORM= 'x1830'
```

* 5.编辑 awtk-port/main\_loop\_linux.c 修改输入设备的文件名

```
#define FB_DEVICE_FILENAME "/dev/fb0"
#define TS_DEVICE_FILENAME "/dev/input/event0"
#define KB_DEVICE_FILENAME "/dev/input/event1"
```

* 6.编译(请先安装scons)

生成内置 demoui 例子，生成结果在 build/bin 文件夹下的 demoui 文件
生成内置 washing_machine 例子，生成结果在 build/bin 文件夹下的 washing_machine 文件

```
scons
```

也可以指定生成其他 Demo，生成结果在 build/bin 文件夹下的 demo 文件

```
scons APP=../awtk-examples/HelloWorld-Demo
```

* 7.生成发布包

对于内置的 demoui 例子

```
./release.sh
```

对于内置的 washing_machine 例子

```
./release.sh WashingMachine
```

对于其他 Demo，需要加入资源文件夹参数，指向应用程序 assets 的父目录

```
./release.sh ../awtk-examples/HelloWorld-Demo/res
./release.sh ../awtk-examples/Chart-Demo/res_800_480
```

* 8.运行

把 release.tar.gz 上传到开发板，并解压，然后根据发布包选择运行：

```
./release/bin/demoui
./release/bin/demo
./release/bin/washing_machine
```

## 其他问题

#### 修改项目路径

默认情况下，scons 脚本假设以下文件夹在同一个目录

```
zlgopen
  |-- awtk
  |-- awtk-examples
  |-- awtk-port-ingenic
  |-- awtk-linux-fb_ingenic
```

如果实际存放的路径与默认不同，则需要修改以下 awtk-linux-fb_ingenic/SConstruct 代码，例如：

```
TK_ROOT = joinPath(os.getcwd(), '../awtk')
APP_ROOT=joinPath(os.getcwd(), '../awtk-examples/HelloWorld-Demo')
```
####fluent_scroll_view说明
牺牲大量内存提高scroll_view绘制滑动效果的控件
使用时将文件替换/合并到awtk/src/ext_widgets/scroll_view中
