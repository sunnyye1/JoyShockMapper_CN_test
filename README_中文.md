* 此中文翻译更新于JoyShockMapper V3.3.0，请以原文档说明[README.md](README.md)为准。
* This Chinese translation is updated to JoyShockMapper V3.3.0. Please refer to the original document [README.MD](README.MD) shall prevail.

* 请在GitHub上查看以获得最佳阅读效果。
* Please view on GitHub for best reading experience.


# JoyShockMapper
索尼PlayStation DualSense(DS5)、DualShock 4(DS4)、任天堂Switch JoyCons和Switch Pro手柄有很多共同之处。它们具有许多现代游戏手柄应有的功能。此外，他们还拥有其最大的竞争对（微软的Xbox手柄）所没有的，了不起的硬件： 6轴运动传感器（以下简称“体感/陀螺仪”）。

JoyShockMapper(JSM）的目标是让你能够使用DS5、DS4、JoyCons、Pro、以及更多手柄畅玩PC游戏，性能和性能甚至超越各自的原装主机 —— 并证明更多游戏应该以这些方式使用这些功能。

**[点击这里](https://github.com/JibbSmart/JoyShockMapper/releases) 立即下载JoyShockMapper!**

对于开发者而言，JSM2.2以及更早的版本使用的是[JoyShockLibrary(JSL)](https://github.com/jibbsmart/JoyShockLibrary)来实现DS5、DS4，JoyCon和Pro手柄的输入抓取。现在JSM使用[SDL2](https://github.com/libsdl-org/SDL)来提供手柄支持，我已经为SDL2贡献了一些代码，以确保它涵盖了相同的功能支持。

JSM也是[GyroWiki（体感维基）](http://gyrowiki.jibbsmart.com)上很多设想/概念的最佳实践。

JSM主要基于Windows系统开发。JSM目前应该也可以在Linux系统上编译。请参阅下面的说明。如果你遇到了任何麻烦，请告诉我们。

## 目录
* **[开发者安装指南](#开发者安装指南)**
	* **[Linux系统须知](#linux系统须知)**
* **[玩家安装指南](#玩家安装指南)**
* **[快速入门](#快速入门)**
* **[命令](#命令)**
  * **[数字输入](#1-数字输入)**
    * **[点击 & 长按映射](#11-点击--长按映射)**
	* **[映射修改器](#12-映射修改器)**
	* **[同时组合映射](#13-同时组合映射)**
	* **[连续组合映射](#14-连续组合映射)**
	* **[双击映射](#15-双击映射)**
	* **[体感启用键](#16-体感按键)**
  * **[线性(模拟)扳机](#2-线性模拟扳机)**
    * **[线性（模拟）转按键（数字）](#21-线性模拟转按键数字)**
    * **[全下压与其他模式](#22-全下压与其他模式)**
    * **[自适应板机](#23-自适应板机)**
  * **[摇杆配置](#3-摇杆配置)**
    * **[标准瞄准模式](#31-标准瞄准模式)**
    * **[指向性瞄准及其衍生模式](#32-指向性瞄准及其衍生模式)**
    * **[其他鼠标模式](#33-其他鼠标模式)**
    * **[数字模式](#34-数字模式)**
    * **[体感摇杆与倾斜映射](#35-体感摇杆与倾斜映射)**
  * **[体感鼠标输入](#4-体感鼠标输入)**
  * **[真实世界校准(RWC)](#5-真实世界校准rwc)**
    * **[校准条件](#51-校准条件)**
    * **[计算3D游戏的RWC](#52-计算3d游戏的rwc)**
    * **[计算2D游戏的RWC](#53-计算2d游戏的rwc)**
  * **[ViGEm虚拟手柄](#6-vigem虚拟手柄)**
    * **[Xbox手柄映射](#61-xbox手柄映射)**
    * **[DS4手柄映射](#62-ds4手柄映射)**
    * **[虚拟手柄体感](#63-虚拟手柄体感)**
  * **[快速切换](#7-快速切换)**
  * **[触摸板](#8-触摸板)**
    * **[触摸板摇杆](#81-触摸板摇杆)**
  * **[其他命令](#9-其他命令)**
* **[配置文件](#配置文件)**
	* **[OnStartup.txt(启动配置)](#1-onstartuptxt启动配置)**
	* **[OnReset.txt(重制配置)](#2-onresettxt重制配置)**
	* **[自动加载功能](#3-自动加载功能)**
* **[难疑解答](#难疑解答)**
* **[已知问题](#已知问题)**
* **[制作组](#制作组)**
  * **[汉化组](#汉化组)**
* **[资源推荐](#资源推荐)**
* **[证书](#证书)**

## 开发者安装指南
JSM是由C++编写的，使用CMake编译。

此项目是由平台无关的头文件构成，而无平台的源文件可以在它们各自的子目录中找到。
以下的文件是与平台无关的：
1. ```include/JoyShockMapper.h``` - 此头文件提供了可以在整个项目中共享的重要类型定义。这里没有定义变量，只有常量。
2. ```include/InputHelpers.h``` - 这是OS函数调用和特性的包装器的平台无关声明。
3. ```include/PlatformDefinitions.h``` - 这是一组声明，在处理特定于平台的类型和定义时创建了一个公共基础。
4. ```include/TrayIcon.h``` - 这是一个自包含模块，用于在Windows系统中显示一个带有上下文菜单的系统托盘中的图标。
5. ```include/Whitelister.h``` - 这是另一个自包含的特定于Windows系统的模块，它使用套接字与HIDCerberus和whitelist JSM通信，Linux系统适配目前是一个存根。
6. ```include/CmdRegistry.h``` - 这个头文件定义了基本命令类型和处理它们的CmdRegistry类。
7. ```include/JSMAssignment.hpp``` - 模板类赋值命令的头文件。
8. ```include/JSMVariable.hpp``` - 模板化核心变量类的头文件和一些衍生物。
9. ```src/main.cpp``` - 这将完成应用程序的所有主要逻辑。核心处理逻辑应该尽可能地保存在其他文件中，并在这个文件中具有特定于JSM的逻辑。
10. ```src/CmdRegistry.cpp``` - 命令行处理入口点的适配。
11. ```src/operators.cpp``` - 中声明的自定义类型```JoyShockMapper.h```的所有流和比较运算符的适配。

Windows系统适配可以在以下文件中找到:
1. ```src/win32/InputHelpers.cpp```
2. ```src/win32/PlatformDefinitions.cpp```
3. ```src/win32/Whitelister.cpp.cpp```
4. ```include/win32/WindowsTrayIcon.h```
5. ```src/win32/WindowsTrayIcon.cpp```

Linux系统适配可以在以下文件中找到:
1. ```src/linux/InputHelpers.cpp```
2. ```src/linux/PlatformDefinitions.cpp```
3. ```src/linux/Whitelister.cpp.cpp```
4. ```include/linux/StatusNotifierItem.h```
5. ```src/linux/StatusNotifierItem.cpp```

在项目根目录下的命令行中运行以下命令生成项目:
- Windows:
  * ```mkdir build && cd build```
  * 创建Visual Studio x86配置: ```cmake .. -G "Visual Studio 16 2019" -A Win32 .```
  * 创建Visual Studio x64配置: ```cmake .. -G "Visual Studio 16 2019" -A x64 .```
- Linux:
  * ```mkdir build && cd build```
  * ```cmake .. -DCMAKE_CXX_COMPILER=clang++ && cmake --build .```

### Linux系统须知
请注意，JSM主要为Windows编写，并且是一个快速开发的程序。

虽然JSM可以针对Linux进行编译，但请注意，快速的开发速度和有限的Linux维护人员数量意味着Linux版本可能并不可以完全编译。

为了在Linux上编译，必须满足以下依赖项，以及它们各自的开发包:
- gtk+3
- libappindicator3
- libevdev
- libusb
- SDL2
- hidapi

**特定的包名称:**

* Fedora: ```SDL2-devel libappindicator-gtk3-devel libevdev-devel gtk3-devel libusb-devel hidapi-devel```
* Arch: ```clang sdl2 libappindicator-gtk3 libevdev gtk3 libusb hidapi```
* 请提供一个问题报告或拉取请求以获得添加额外的库列表。

因为一个GCC中的一个[bug](https://stackoverflow.com/questions/49707184/explicit-specialization-in-non-namespace-scope-does-not-compile-in-gcc)，该项目目前的形式将只编译于Clang。

JSM最初是为Windows系统开发的，这带来了一些副作用，通过代码库使用的类型是特定于Windows系统的。它们已经在Linux系统上重新定义了。这样做是为了将对应用程序核心逻辑的更改保持在最低限度，并降低对现有用户造成倒退的可能。

应用程序需要```rw```以访问```/dev/uinput```，```/dev/hidraw[0-n]```（实际设备取决于OS分配的节点）。可以通过将需要的设备节点```chown```到运行应用程序的用户，或者通过应用在```dist/linux/50-joyshockmapper.rules```中找到的udev规则，将用户添加到输入组，并重新启动计算机使更改生效来实现。关于udev规则的更多信息可以在https://wiki.archlinux.org/index.php/Udev#About_udev_rules 上找到。

该应用程序可以在X11和Wayland上运行，但聚焦窗口检测只在X11上工作。

## 玩家安装指南
最新版本的JSM总是可以在[这里](https://github.com/Electronicks/JoyShockMapper/releases)找到。你所要做的就是解压并运行JoyShockMapper.exe。

其中包括一个名为GyroConfigs的文件夹。这包括为2D和3D游戏创建配置的模板，以及包含用于简单[真实世界校准(RWC)](#5-真实世界校准rwc)设置的配置文件。

## 快速入门
1. 用USB线，蓝牙或接收器连接你的手柄。JSM支持大部分现代手柄，包括所有的Xbox, PS和Switch手柄，尽管Xbox和许多其他的手柄都没有体感控制所需的运动传感器。

2. 双击打开JoyShockMapper.exe，你会看到终端显示“Welcome to JoyShockMapper”的欢迎语句。
     * 在终端中，你可以开始输入映射:[按键名]=[映射名]。请参阅[数字输入](#1-数字输入)来了解按键和映射是如何命名的。
     * [摇杆](#3-摇杆配置)，[体感](#4-体感鼠标输入)以及[线性扳机](#2-线性模拟扳机)需要额外配置: 通常是你想要设置的一些模式、灵敏度值和一些其他的参数。在相应的部分中对此进行了解释。它们遵循相同的格式: [参数名称] = [数值]
	* 如果你只在终端中输入按键和参数的名称，它们将显示当前数值。
	* 输入```[参数名称]HELP```，可以查看简短的说明。
	* 有相当多的命令不像上面那样作赋值工作，而只是运行一个命令（函数）。例如，```RECONNECT_CONTROLLERS```将更新手柄列表，```RESET_MAPPINGS```将所有参数和映射重制为默认值。```README```将引导你找到此文档!
	* 你会在系统托盘中发现一个JSM图标（蓝色小手柄）：右键以显示命令和配置文件的简易列表。

3. JSM可以加载包含在文本文件中的所有命令。
	* 在终端中输入文件的路径和名称。如果文件路径太长，或者包含异常字符，请输入相对路径(例如:GyroConfigs/config.txt)。
    * 文本文件的每一行都将作为直接输入终端的命令运行。#符号开始注释直到行尾，并将被JSM忽略。
    * 你可以在“GyroConfigs”文件夹中找到配置文件示例。文件可以相互引用:例如，可以随意编辑GyroConfigs中的文件作为自定义的体感配置。
    * 在[配置文件](#配置文件)部分查看详情。

4. 如果你使用了一个带有体感的配置，陀螺将需要校准（告诉什么状态是“静止”的）。请参阅[体感鼠标输入](#4-体感鼠标输入)获得更多信息，但这里有一个简短的版本:
	* 将所有手柄放置在静止的平面上;
        * 输入命令```RESTART_GYRO_CALIBRATION```开始校准;
	* 只需几秒钟后，输入```FINISH_GYRO_CALIBRATION```命令，即可完成校准。
	* 这些命令也可以通过右键托盘图标访问。
    * JSM依赖于一些功能的真实世界校准值，如轻推摇杆。如果你在[在线数据库](http://gyrowiki.jibbsmart.com/games)中没有找到这个值，请参阅[真实世界校准(RWC)](#5-真实世界校准rwc)部分，自己计算它。

5. 如果你遇到一些问题，请确保检查[疑难解答](#疑难解答)部分和[已知问题](#已知问题)。如果你找不到你的答案，你可以在[GyroGaming subreddit](https://www.reddit.com/r/GyroGaming/)和它的[附属的Discord服务器](https://discord.gg/4w7pCqj)上找到更多的帮助。中国社区即将上线，敬请期待！

## 命令
可以通过简单地将命令输入到JSM终端并点击键盘上的'enter'键来执行。你可以通过输入```HELP```来查看所有可用命令的列表，或通过输入例如```HELP STICK```来查看所有包含摇杆的命令。因为它们的数量非常多，所以在本文中，它们是根据组件在手柄上的物理位置进行分类的。

命令可以*大致*分为8类:

1. **[按键](#1-数字输入)**: 这些是最简单的。将手柄按键按下或摇杆移动映射到键盘或鼠标键输入。有许多映射选项可用，如点击，长按，同时下压，组合键和更多。
2. **[线性扳机](#2-线性扳机)**: 许多手柄都有2个线性板机：例如DS上的L2和R2。JSM可以对“轻压”和“全下压”状态设置不同的映射，最大化地利用这些扳机。这一功能对于带有微动板机的手柄来说是不可用的，比如任天堂的Pro和JoyCons。
3. **[摇杆](#3-摇杆配置)**: 摇杆可以以许多不同的方式驱动鼠标或触发映射，如轻推摇杆或滚轮。他们需要设置一个模式，一些特定的模式参数。本节将对此进行说明。
4. **[体感](#4-体感鼠标输入)**: 用体感控制鼠标通常比用摇杆控制要精确得多。把陀螺仪想象成一个在隐形、无摩擦力鼠标垫上的鼠标。只要你可以旋转手柄，鼠标垫就会延伸到任何地方。对于直接控制视角的游戏来说，摇杆鼠标输入可以快速地以较小的精度完成大幅度旋转，而体感鼠标输入则可以让你在相对有限的范围内做出更精确、更快的移动。
5. **[真实世界校准(RWC)](#5-真实世界校准rwc)**: 这个校准值能够让“轻推摇杆”正常工作，让体感和瞄准摇杆设置具有现实参考意义，让玩家能够在不同游戏间共享相同的灵敏度与设置。
6. **[ViGEm虚拟手柄](#6-ViGEm虚拟手柄)**: JSM可以连接到nefarus的ViGEm bus软件，创建虚拟xbox和虚拟DS4手柄。要使用此功能，你需要[在此下载并安装最新版本的ViGEm](https://github.com/ViGEm/ViGEmBus/releases/latest)。
7. **[快速切换](#6-快速切换)**: 手柄的配置可以根据当前按键的按下而动态改变，类似于组合键。例如这可以很方便处理武器轮盘。这被称为模式切换/快速模式切换，以呼应Steam Input而命名。
8. **[其他命令](#7-其他命令)**: 这些不属于上述类别，但仍然有用。它们通常与JSM本身有关，而不是手柄配置。

因此，让我们深入研究一下可用的命令。

### 1. 数字输入
数字输入非常简单。它们的结构大致如下:

```[手柄输入] = [键盘/鼠标映射]```

例如，要将手柄的左摇杆下压映射到键盘的左SHIFT键，你需要输入:

```L3 = LSHIFT```

JSM的一个重要特性是，适用于DS手柄的配置也适用于一对JoyCons或Pro手柄。因为JoyCons比DS4有更多的输入（JoyCons有```SL```和```SR```），所以按键的名字大多来自任天堂设备。主要的例外是动作键和摇杆下压。因为它们的名称更简洁，所以摇杆下压以DS4命名：```L3``` 和 ```R3```.

而右侧的动作键则是个更复杂的存在。

Xbox的布局实际上已经成为PC手柄的常用布局。大多数使用某种手柄的PC玩家都熟悉Xbox布局，无论是Xbox手柄、Steam手柄，还是其他可以被游戏理解为Xbox手柄的第三方手柄。甚至PS的用户也会习惯解读Xbox的动作键名称。任天堂设备在不同的布局中拥有相同的动作键。X和Y互换了，A和B也互换了。任天堂的布局存在了很长的时间，但PC玩家并不太熟悉。

所以在我看来，最好的解决方案是使用“两者都不使用”的布局，并使用按键名称有明确意义的布局，即不为*任何*手柄所占用，但仍具有明显位置的“方向性名称”：北，东，南，西；```N```, ```E```, ```S```, ```W```

下面是完整的数字输入列表:

```
UP, DOWN, LEFT, RIGHT: 方向键 ↑，↓，←，→
L: L1 or L, 左侧小肩键
ZL: L2 or ZL, 左侧大肩键（或扳机）
R: R1 or R, 右侧小肩键
ZR: R2 or ZR, 右侧大肩键（或扳机）
ZRF: 全下压右扳机, 只有线性扳机可用
ZLF: 全下压左扳机, 只有线性扳机可用
-: Share，Back 或 -
+: Option，Start 或 +
HOME: PS，引导，西瓜键 或 Home
CAPTURE: 触摸板下压 或 Capture
LSL, RSL: JoyCons的SL, 或Xbox精英的左背键
LSR, RSR: JoyCons的SR, 或Xbox精英的右背键
L3: L3 或 左摇杆下压
R3: R3 或 右摇杆下压
N: 朝北按键, △, Y（Xbox）or X（任天堂）
E: 朝东按键, ○, B（Xbox）or A（任天堂）
S: 朝南按键, ⨉, A（Xbox）or B（任天堂）
W: 朝西按键, □, X（Xbox）or Y（任天堂）
LUP, LDOWN, LLEFT, LRIGHT: 左摇杆 上，下，左，右 倾斜
LRING: 左摇杆内环/外环
RUP, RDOWN, RLEFT, RRIGHT: 右摇杆 上，下，左，右 倾斜
RRING: 右摇杆内环/外环
MUP, MDOWN, MLEFT, MRIGHT: 体感摇杆 上，下，左，右 倾斜
MRING: 体感摇杆杆内环/外环
LEAN_LEFT, LEAN_RIGHT: 手柄 左，右 倾斜
TOUCH : 手指触摸触摸板
MIC: 索尼Dualsense麦克风键
```

这些都可以映射到以下的键盘，鼠标和命令输入:

```
0-9: 键盘顶部的数字键
N0-N9: 数字键盘上的数字键
ADD, SUBTRACT, DIVIDE, MULTIPLY, DECIMAL: 小键盘/数字键盘上的功能键
F1-F29: F1, F2, F3... 等等
A-Z: 字母键
UP, DOWN, LEFT, RIGHT: 键盘上的方向键
LCONTROL, RCONTROL, CONTROL: 左Ctrl，右Ctrl，通用Ctrl
LALT, RALT, ALT: left Alt, 左Alt，右Alt，通用Alt
LSHIFT, RSHIFT, SHIFT: 左Shift，右Shift，通用Shift
LWINDOWS, RWINDOWS, CONTEXT: 左Win，右Win，Context菜单
TAB, ESC, ENTER, SPACE, BACKSPACE, CAPS_LOCK, SCROLL_LOCK, NUM_LOCK, 
PAGEUP, PAGEDOWN, HOME, END, INSERT, DELETE
LMOUSE, MMOUSE, RMOUSE: 鼠标左键, 中键 和 右键
BMOUSE, FMOUSE: 鼠标后退键（第四键）和 鼠标前进键（第五键）
SCROLLUP, SCROLLDOWN: 滚轮 上，下
VOLUME_UP, VOLUME_DOWN, MUTE: 音量控制
NEXT_TRACK, PREV_TRACK, STOP_TRACK, PLAY_PAUSE: 媒体控制
SCREENSHOT: 截图键
NONE, DEFAULT: 无输入
CALIBRATE: 当按下这个输入时，重新校准陀螺仪
GYRO_ON, GYRO_OFF: 启用或禁用体感
GYRO_INVERT, GYRO_INV_X, GYRO_INV_Y: 反向体感，可分别作用在X轴和Y轴上
GYRO_TRACKBALL, GYRO_TRACK_X, GYRO_TRACK_Y: 保持上一次体感输入，可分别作用在X轴或Y轴上
; ' , . / \ [ ] + - `
“任何终端命令”:任何终端命令可以在按下按键时运行，包括加载文件
SMALL_RUMBLE, BIG_RUMBLE, Rhhhh: 震动命令。'h'是大写十六进制数字，如'R8000'或'RFFFF'。
```

例如，在一款游戏中，R键“换弹”，E键“使用”，你可以通过以下方法将□键映射为“换弹”，而△键则映射为“使用”:

```
W = R
N = E
```

熟悉Steam Input的人可以使用引号实现动作层和动作集来按需加载文件。该文件可以部分或全部重新映射手柄。这对于车辆、菜单或角色的不同方案非常有用。

```
# 加载驾驶映射配置文件.
HOME = "GTA_driving.txt" # 该文件应该映射HOME以加载行走映射配置文件!
```

注意，以这种方式映射的命令不能包含引号，因此不能包含命令本身的映射。因此，你应该将命令放入一个文件中并加载该文件。


#### 1.1 点击 & 长按映射

由于键盘比大多数手柄的按键更多，所以主机游戏通常会将多个操作映射到同一个按键上，而PC版本则会将这些操作映射到不同按键上。为了将键盘操作适配到手柄上，JSM允许你将点击和长按映射到不同的键盘/鼠标输入上。让我们以同样的游戏为例，让我们可以点击□“换弹”或长按□“使用”:

```
W = R E
```

如果你想要□在点击时“换弹”，但按住时什么也不做，你可以参考以下配置:

```
W = R NONE
```

在启用按住映射之前按住按键的时间可以通过给```HOLD_PRESS_TIME```（长按时间差）分配毫秒数来更改。默认值是150毫秒。

关于如何应用按键映射的更多信息，请参阅下面的映射修改器

#### 1.2 映射修改器

点击和长按是手柄上最常用的映射。但有时候，你会发现需要一些更复杂的映射，要么是因为你想围绕游戏内部机制工作，要么是因为你想创建一些不同寻常的按键组合。在任何情况下，JSM允许你通过映射修改器高度自定义映射如何分配到你的按键。

在我们开始之前，有一些概念需要理解。一个按键总是包含按下和松开的**动作**。在一个简单的映射中，JSM会将按下和松开**事件**与映射开始和映射结束动作分别匹配。然而，当你使用点击和长按映射时，JSM会将按键按下和松开映射到**不同的事件**上，这些事件会在按键向下和松开后的特定时间发生。有些映射没有匹配的“松开”操作，比如滚动轮映射和终端命令映射。

有两种**修饰符**可以应用于按键映射 ：***动作修饰符***和 ***事件（键值）修饰符***。它们分别由添加在映射名之前和之后的符号表示。每个映射只能有一个。但是，你可以将多个按键映射到同一个事件，从而一次发送多个键值。

**动作修饰符**影响**按下**和**松开**动作如何映射到事件。它们有两种: **开/关(^)** 和 **急速(!)**。
* ^ 开/关在每次按下时，该键会在按下和松开键之间交替。
* ! 急速会在按键按下后很短时间内发送按键松开动作，使其看起来急速触发。

**事件修饰符** 影响松开和按下动作所映射的按键事件。他们有五种：**按下(\\)**, **松开(/)**, **点击(')**, **长按(_)**, 和 **连发(+)** 。
* \\ 当只有一个按键映射时，按下时默认的修改器。默认状态下它会在按键按下时立即触发映射并在松开时结束映射。当你想在其他按键触发时启用一个映射，这个修改器会特别实用。
* \/ 当按键松开时触发映射，松开需要一个有效的动作修改器（即！）。
* ' 当有多个映射在同一按键时，点击是第一个按键的默认事件修饰符。如果总按下时间小于```HOLD_PRESS_TIME```，它将会在按键松开时触发映射。默认情况下，映射会在短时间后结束，与体感相关的动作和校准时间会更长。
* _ 当有多个映射在同一按键时，长按是第二个按键的默认事件修饰符。当只有在按键按下时间超过 ```HOLD_PRESS_TIME```的时候才会触发映射。 默认情况下，映射会在按键松开时结束。
* \+ 连发会反复触发映射（受动作修改器影响), 产生连点的效果。只有在按键按下时间超过```HOLD_PRESS_TIME```的时候才会触发连发。

这些修改器可以让你在游戏中绕过点击和长按，或将一种按压形式转换成另一种。下面是一些使用这些修饰符/修改器的例子。

```
ZL = ^RMOUSE\ RMOUSE_  # 点击持续开镜
E  = !C\ !C/         # 将持续蹲伏改为长按蹲伏
UP = !1\ 1           # 将双击投掷改为长按投掷
W  = R E\            # 在光环MCC中，点击即可换弹，但立即触发E以覆盖游戏内的长按机制
-,S = SPACE+         # 连击QTE，没人喜欢暴击自己的按键:(
R3 = !1\ LMOUSE+ !Q/ # 半条命近战按键
UP,UP = !ENTER\ LSHIFT\ !G\ !L\ !SPACE\ !H\ !F\ !ENTER/ # 预录制信息/宏
UP,E = BACKSPACE+    # 将预录制信息删除
```

需要注意的是，下面的**同时触发映射**会因为逻辑限制会延迟事件（特别是**按下**）。这些时间窗口不会被添加，但是事件将在一两个时间窗口中被合并到一起。

此外，双击映射有一些特殊的时间处理，以便让用户选择是否跳过第一个绑定。详情请参阅下面的[专用部分](#15-双击映射)

最后，这是一个包含按键事件在按下过程中何时被触发的全面描述的图表。
```
                                            小于 150 ms 按住时间
       150 ms 按住时间     80ms 连发时间              V         500 ms 陀螺仪校准
            V                 V                    |------|< 执行动作 或 等待 40 ms
______|-----|---|---|---|---|---|--|___________|---|____________
      \____________________________/           \___/      |
      |     |   |   |   |   |   |  |           |   |      |
     (a)   (b) (c) (c) (c) (c) (c)(d)         (a) (d)    (g)
           (c)                    (f)             (e)
a: 按下 \
b: 按住 _
c: 连发 +
d: 松开 /
e: 点击 '
f: 按住松开
g: 点击松开
 a, b, c, d 以及 e 事件在它们触发40 ms后，在它们上附加一个急速释放事件。
```

#### 1.3 同时组合映射
另外，JSM允许你设置同时按下的按键到不同的映射。例如，你可以将角色技能映射在你两侧的肩键上，并将大招映射在两者上，像这样:

```
L = LSHIFT # 技能 1
R = E      # 技能 2
L+R = Q    # 大招
```

为了实现同时下压键映射，两个按键都需要在很短的时间内同时按下。这样做将忽略单个的映射，并应用指定的映射，直到其中一个按键被释放。同时下压键映射也像其他映射一样支持点击 & 长按映射以及修改器。这个功能可以最大化的利用方向键对角线，或在不牺牲方便使用按键的情况下添加JSM特定的功能，如陀螺仪校准和体感控制。

可以通过修改```SIM_PRESS_WINDOW```（同时按下时间差）的毫秒数来改变同时下压的窗口的时间。此设置不能通过**快速切换**更改（稍后讨论）。

#### 1.4 连续组合映射
组合键与同时下压键工作方式不同，尽管乍一看很相似，组合键映射允许你在组合键触发时覆盖正常的映射，这就支持大量不同的实际组合，允许你拥有连续的动作，以下便是求生之路2的例子，玩家无需抬起左摇杆上的食指便能够装配道具。

```
W = R E # 换弹/使用
S = SPACE # 跳跃
E = CONTROL # 蹲伏
N = T # 语音

L = Q NONE # 其他武器，按住以使用动作键选择
L,W = 3 # 投掷物
L,S = 4 # 药片
L,E = 5 # 药包
L,N = F # 手电筒
```

一个按键可以与多个其他按键连接在一起。在这种情况下，最新的组合键优先于之前的组合键。这可以理解为每次按下组合键时，在映射的顶部放置一堆层，其中只有顶部的层是活动的。注意，你不需要使用```NONE```（空置）作为映射：例如，组合键可以映射到弹出武器轮的按键上。

#### 1.5 双击映射
你还可以将按键双击分配给不同的映射。双击表示法与组合表示法相同，不同之处是按键与它自己组合。它支持点击，长按和修改器，像所有以前的条目。

当同一个按键在第一次下压后的五分之一秒（150ms）内在此发生下压时触发双击映射。在这段时间内不能假定有其他映射，所以常规的点击将引入延迟。这个映射还支持点击 & 按住映射以及修改器。可以通过指定```DBL_PRESS_WINDOW```（双击时间差）不同的毫秒数来改变执行双击映射的时间窗口。

```
N = SCROLLDOWN # 循环武器
N,N = X # 循环武器开火模式

W = !E\        # 拾取物品
W,W = I        # 拾取物品并打开背包

E = C'         # 蹲下
E,E = Z        # 跳过蹲直接匍伏
```

#### 1.6 体感按键
最后，还有一种不同的数字输入，因为它可以与任何其他输入重叠。两个输入，但在给定的映射配置中你最多只会用到其中一个

```
GYRO_OFF
GYRO_ON
```

当你将一个按键分配给```GYRO_ON```（按住启动）时，体感鼠标只在按下按键时工作。```GYRO_OFF```（按住关闭）在按下按键时禁用体感鼠标。大多数游戏都缺少这个重要的功能，体感的目标——就像电脑游戏玩家可以将鼠标抬起以暂时“禁用”鼠标，重新复位。体感玩家可以暂时禁用体感为了重新复位与旋转手柄。这个映射不会影响与该按键相关联的其他映射。这样体感就可以在某些游戏内动作的同时启用，或者无论什么点击或控制都可以立即禁用或启用体感。

对于那些需要使用手柄上所有按键，但却很少使用且能够轻松切换的输入按键的游戏（例如蹲下)，我们便需要让输入只停留在点击上，并在长按时禁用体感:

```
E = LCONTROL NONE
GYRO_OFF = E
```

或者如果你真的不能腾出一个按键来禁用体感，你可以使用```LEFT_STICK```（左摇杆）或```RIGHT_STICK```（右摇杆）来禁用体感:

```GYRO_OFF = RIGHT_STICK # 使用右摇杆时禁用体感```

我喜欢能够在使用摇杆的同时使用体感瞄准，但这仍然可以比没有禁用体感或如果你的游戏没有一个明显关联体感的功能（如一个专门的“武器瞄准”按键,在第三人称动作游戏中很常见）。

```GYRO_ON```（按住启动）对于那些你只需要偶尔精确瞄准的游戏非常有用。如ZL使用弓箭瞄准（如《塞尔达传说:荒野之息》或《暗影魔多》），也许这是你唯一想让体感瞄准的时间：

```
ZL = RMOUSE   # 弓箭瞄准
GYRO_ON = ZL  # 当ZL被按下时启用体感
```

```GYRO_ON```和```GYRO_OFF``` 也可以作为一个操作映射到特定的按键。与上面的命令相反，这是操作映射的位置。但你仍然可以找到一些创造性的方法，通过点击和长按或和组合键，将正确的体感控制映射到你需要的地方。

请注意，轻击将与体感相关的映射延长至半秒钟。另一个选项是用```GYRO_INVERT```（按住反转体感）反转体感输入。如果你只使用一个摇杆，那么这种映射就很方便，因为你没有第二个摇杆。当该动作被启用时，反转使你可以通过继续转向相反的方向来重新调整方向!

```
SL + SR = GYRO_OFF GYRO_INVERT  # 当两个肩键被按下时禁用体感0.5秒/反转体感
```

这些映射的体感动作，如果它们冲突，按键将优先分配给体感映射。

```NO_GYRO_BUTTON```（无体感按键）命令可以被用于去除gyro-on或gyro-off映射，使体感永久打开。 如果想永久禁用，只需使用```GYRO_ON = NONE```或者将```GYRO_SENS```设为0。

如果你使用的是```GYRO_TRACKBALL```或它的单轴变体，你可以使用```TRACKBALL_DECAY```来选择轨迹球效应失去动量的速度。它可以设置为0，表示没有衰减。它的默认值1是体感轨迹球每秒钟动量的一半。2会在1/2秒内减半，3会在1/3秒内减半，以此类推。为了减小按下按键时干扰或手柄不稳定性的影响，对轨迹球初始速度进行了一定程度的平滑处理。

### 2. 线性(模拟)扳机

#### 2.1 线性（模拟）转按键（数字）

接下来的部分将不适用于JoyCons或Pro手柄，因为他们没有线性板机。

线性扳机会报告一个介于0%和100%之间的值，表示你下压扳机的进程（距离）。将线性板机映射到普通按键是通过一个固定值来实现的。当板机下压进程跨越threashold（阈值/触发点）值（介于0%和100%之间）时，按键信号将被发送。默认的触发点为0，意味着最轻微地按下扳机将触发键值。相应速度很快，但可能会导致误触。触发点可以通过运行以下命令自定义:

```
TRIGGER_THRESHOLD = 0.5   #将板机触发点设置在50%
```

两个板机使用相同的触发点设置，高于1.0的触发点将永远不可触发。

JSM也支持软件微动板机模式：将触发点设置为-1以激活。在使用微动扳机模式时，板机在向下运动或被按住时会触发键值，当向上运动或松开时会停止触发键值。可以轻松实现高频，精确的点射。

#### 2.2 全下压与其他模式

JSM可以为半下压与全下压分配不同的映射，允许你有两倍的板机映射数量。可以使用变量```ZR_MODE```和```ZL_MODE```为R2和L2设置处理这些映射的方式。一旦设置完毕，你就可以将按键分配给```ZRF```和```ZLF```，以便分别使用R2和L2的全下压映射。在这种情况下，```ZL```和```ZR```被称为半下压映射，因为它们在100%全下压映射之前激活。下面列出了所有可能的触发模式。

```
NO_FULL（默认): 忽略全下压。 数字扳机手柄将强制在这种模式下工作。
NO_SKIP: 永远不跳过半下压映射，全下压映射将会在扳机全下压时激活。
NO_SKIP_EXCLUSIVE: 永远不跳过半下压映射，当全下压映射激活时将断开半下压映射。
MUST_SKIP: 只会使用全下压映射，忽略半下压映射。
MAY_SKIP: 结合 NO_SKIP 和 MUST_SKIP: 当在快速全下压时半下压会被忽略, 并且全下压可以在半下压后正常工作。
MUST_SKIP_R: 快速反应版本的 MUST_SKIP. 见后文。
MAY_SKIP_R: 快速反应版本的 MAY_SKIP. 见后文。
```

例如，在《使命召唤》中，你可以在使用狙击瞄准时屏息。你可以像这样将瞄准映射至半下压并将屏息映射置全下压:

```
ZL_MODE = NO_SKIP   # 启用全下压映射，永不跳过开镜映射
ZL = RMOUSE         # 按住开镜
ZLF = LSHIFT        # 按住开镜后屏息
```

使用NO_SKIP模式使得LSHIFT只有在RMOUSE也处于触发状态时才可触发。然后在右扳机上，你可以映射不同的攻击键位：使用MUST_SKI功能，以避免它们之间的冲突，像这样:

```
TRIGGER_THRESHOLD = -1 # 使用微动模式开火
ZR_MODE = MUST_SKIP    # 启用全下压映射， 半下压和全下压映射不可同时触发
ZR = LMOUSE            # 开火
ZRF = V G              # 快速半下压以使用近战武器; 快速全下压以温雷并在松开时掷出
```

使用MUST_SKIP模式确保一旦你开始射击，全下压将**不会**使你停止射击并切换到近战。

SKIP模式的“快速响应版本”启用了不同的行为，可以在特定环境下提供比原版更好的体验。一个典型的例子是，半下压映射是一个类似于开镜或趴下状态的映射，而该半下压键没有触发或一直触发映射。不同之处在于，半下压映射在扳机越过出发点时就会被激活，从而提供所需的响应感（快速反应），但如果快速达到全压，则会被忽略，因此仍然允许你...比如说腰射。希望这在可忽略的范围内出错，但保证了一个更快速地开镜体验。

#### 2.3 自适应板机

DS5手柄上一项知名功能就是自适应板机，允许软件设置板机的力反馈状况。JSM利用了这项功能，并根据不同的板机模式，位置，产生不同类型的反馈。目前反馈的模式不能被修改，但是可以通过这条指令关闭：

```
ADAPTIVE_TRIGGER = OFF # 关闭自适应板机
```

当启用自适应板机时，DS5手柄将会忽略微动板机阈值，并直接默认为0。这是因为自适应板机能通过力反馈模拟微动。当自适应板机被关闭时，正常微动板机参数可被启用。

每个手柄都可能会有微微不同的板机属性，这会导致板机在回报参数上和设置参数上产生一定的误差。因此，每个板机都有两个新的设置；OFFSET（偏差）和RANGE（范围），均可以通过一个简单的校准得出。 你可以通过输入```CALIBRATE_TRIGGERS``` 指令开始：首先，轻轻的下压板机，直到感受到轻微的阻力。然后按下一个按钮，你会感受到板机慢慢的下降：请确保全程维持轻微的下压。当你完全按到底时JSM会自动计算出需要的校准值。可以在另一个板机上重复同样的步骤。

请将校准参数放入OnStartup.txt以便在每次启动JSM时应用这一校准。

```
LEFT_TRIGGER_OFFSET = 20     # 我的DS5手柄校准值
LEFT_TRIGGER_RANGE = 167
RIGHT_TRIGGER_OFFSET = 31
RIGHT_TRIGGER_RANGE = 175
```

用户Nielk1逆向工程了自适应板机的算法并为此开发了一个[C#工具](https://gist.github.com/Nielk1/6d54cc2c00d2201ccb8c2720ad7538db)。在他的允许下（以及MIT证书）我将其“C艹化”了这套算法并应用到了JSN中。创造了两项新的设置：```LEFT_TRIGGER_EFFECT```（左板机效果）和 ```RIGHT_TRIGGER_EFFECT```（右板机效果）。可以将他们设置为```ON```或```OFF```（JSM模式）或者Nielk1提供的模式；

```
RESISTANCE start[0 9] force[0 8]: 在某一点启动的阻力
BOW start[0 8] end[0 8] forceStart[0 8] forceEnd[0 8]: 逐渐变强的阻力
GALLOPING start[0 8] end[0 9] foot1[0 6] foot2[0 7] freq[Hz]: 两个交替变换的震动
SEMI_AUTOMATIC start[2 7] end[0 8] force[0 8]: 枪械板机效果
AUTOMATIC start[0 9] strength[0 8] freq[Hz]: 常规震动效果
MACHINE start[0 9] end[0 9] force1[0 7] force2[0 7] freq[Hz] period: 不规律震动效果
```


### 3. 摇杆配置
每一个摇杆都有7种不同的控制模式：

```
AIM: 传统瞄准
FLICK: 指向性瞄准
FLICK_ONLY: 仅指向，无旋转
ROTATE_ONLY: 仅旋转，无指向
MOUSE_RING: 摇杆角度映射至环状轨迹的光标
MOUSE_AREA: 摇杆位置映射至一块圆形的光标区域
NO_MOUSE: 不影响鼠标，用于按键/摇杆映射（默认）
SCROLL_WHEEL: 当顺时针或逆时针移动摇杆时启用左或右映射。
```

传统左右摇杆映射的例子：

```
LEFT_STICK_MODE = NO_MOUSE
RIGHT_STICK_MODE = AIM
```

不管你使用哪个模式，摇杆的半倾和全倾都可以分开映射。打个比方，也许你想让摇杆推到底时持续触发左SHIFT：

```
LEFT_RING_MODE = OUTER # OUTER（外环）默认就有，所以这行是选配的
LRING = LSHIFT
```

或你想在摇杆半倾时按左ALT

```
LEFT_RING_MODE = INNER
LRING = LALT
```

为了向下兼容性，有两项额外的设置：```LEFT_STICK_MODE```和 ```RIGHT_STICK_MODE``` 可以同时设置摇杆模式和摇杆环模式： 

```
INNER_RING: 等同于 _STICK_MODE = NO_MOUSE 和 _RING_MODE = INNER
OUTER_RING: 等同于 _STICK_MODE = NO_MOUSE 和 _RING_MODE = OUTER
```

如果你以不同的方式握持你的手柄（比如为了舒适性或者在使用单侧JoyCon时），你可以使用如下设置：
**CONTROLLER\_ORIENTATION** 参数名称
* **FORWARD** 朝前（默认）
* **LEFT** 朝左
* **RIGHT** 朝右
* **BACKWARD** 朝后
* **JoyCon_SIDEWAYS** 左JoyCon朝左，右JoyCon朝右

设置完成后，JSM会讲摇杆的XY轴重新配对至你设置的方向。 CONTROLLER\_ORIENTATION 只影响摇杆（包括体感摇杆）。它不会影响动作键，方向键，等等的布局。 请查阅[体感鼠标输入](#4-体感数鼠标输入)来重新映射体感的方向。

让我们来看看摇杆的不同模式；

#### 3.1 标准瞄准模式

当时用 ```AIM```（瞄准）摇杆模式时，有几项重要的参数：

* **STICK\_SENS** （摇杆灵敏度，默认360度每秒) - 摇杆在最大倾斜时以多快的速度旋转视角？当校准无误时，默认为360度每秒。添加第二项参数区分垂直与水平的灵敏度。
* **STICK\_POWER** （摇杆加速度，默认1.0) - 摇杆的加速曲线是什么形状的？ 1.0代表简单的线性关系（半倾斜摇杆将会以STICK\_SENS一半的速度旋转视角)，0.5将会是平方根关系，2.0将会是平方关系，以此类推。最小值为0.0，这意味着任何大于 STICK\_DEADZONE\_INNER（摇杆内死区) 的输入将会被视为最大倾斜。
* **LEFT\_STICK\_AXIS** and **RIGHT\_STICK\_AXIS** （左摇杆轴，右摇杆轴，默认标准 STANDARD) - 你可以通过这项设置反转摇杆轴的方向。 你可以选择STANDARD（默认） 或者 INVERTED （反转轴)。添加第二项参数区分垂直与水平的反转。
* **STICK\_ACCELERATION\_RATE** （摇杆加速度，默认0.0) - 当摇杆完全倾斜时，这项参数允许你继续让视角随时间加速。此项为STICK\_SENS的倍增数每秒。例如当加速度2.0和STICK\_SENS 100时，视角会在1秒中从100加速到300。
* **STICK\_ACCELERATION\_CAP** （摇杆最大加速度，默认1000000.0) - 你可能需要在 STICK\_ACCELERATION\_RATE （摇杆加速度）不为0时设置一个最大加速度。例如 STICK\_ACCELERATION\_CAP （摇杆最大加速度）为2.0你的视角不会超越两倍的STICK\_SENS（摇杆灵敏度）。当STICK\_ACCELERATION\_RATE（摇杆加速度）为0时此项没有效果。
* **STICK\_DEADZONE\_INNER** 和 **STICK\_DEADZONE\_OUTER** （内死区，外死区，默认分别为0.15和0.1 - 手柄摇杆有些时候没那么准确，当你释放摇杆是，往往不会回到完美的中心。 STICK\_DEADZONE\_INNER（内死区） 允许你将内环的一部分区域定义为“中心”。 当摇杆的输出在这个范围中，就会被自动忽略。 STICK\_DEADZONE\_OUTER（外死区）在外环做着近似的事情，如果摇杆输出在外环范围种，就会自动判定为满推。中间的全部范围将会等比例缩放。 你可以通过下列指令设置各个摇杆的死区 **LEFT\_STICK\_DEADZONE\_INNER**, **LEFT\_STICK\_DEADZONE\_OUTER**, **RIGHT\_STICK\_DEADZONE\_INNER**, **RIGHT\_STICK\_DEADZONE\_OUTER**。

#### 3.2 指向性瞄准及其衍生模式

当在使用 ```FLICK```（指向性） 摇杆模式时，我们有更少需要操心的参数。你不需要设置任何额外的死区或者灵敏度。当你将摇杆向一个方向倾斜时，JSM会在很短的时间内，将游戏内的视角，相对于起始点，对齐摇杆指向的方向。当摇杆倾斜后，继续向其他方向移动*已经倾斜的*摇杆可以继续旋转。这提供了一种非常自然舒适的视角控制方式，可以快速响应与瞄准屏幕上或屏幕外的任何东西，或者在完全不移动手柄的情况下完成旋转。

因为*指向性瞄准*只能水平移动视角，这通常需要结合体感来完成垂直瞄准。

*指向性瞄准*将沿用上面提到的STICK\_DEADZONE\_OUTER（外死区）来决定启动的倾斜范围。 *指向性瞄准*也依赖REAL\_WORLD\_CALIBRATION（真实世界校准）以正确的工作（[后面有提到](#5-真实世界校准rwc)，这项参数将会定义全部鼠标移动相关的功能）， 这是因为JSM只能通过*刚好的*鼠标移动轨迹，将视角指向正确的方向，而REAL\_WORLD\_CALIBRATION帮助JSM补偿游戏内的灵敏度。当然，原生支持*指向瞄准*的游戏不要求任何校准。*指向性瞄准* 有几项你可以调整的参数：

* **FLICK\_TIME** （指向时长，默认0.1秒) - 当你向一个方向倾斜摇杆时，需要耗费多少时间将视角移动至指向的方向？我发现0.1秒是个很好的范围，看着不错的同时响应迅速。延时太低时可能会被当成视角瞬间多向锁定的挂哥。
不要忘记，在指向瞄准后 你依然可以进一步移动摇杆进行旋转调节。中间没有任何额外的平滑\*；视角会牢牢的锁定在摇杆指向的方向上。 FLICK\_TIME只会影响一开始指向瞄准的过程。
* **FLICK\_TIME\_EXPONENT** （指向时长指数，默认0.0) - 有些人喜欢成比例的指向时长与指向角度，而另一些人可能无法适应如此浮夸的速度。因此这项参数允许FLICK\_TIME（指向时长） 随着指向角度变化。这里的数值不会改变在180度的范围下的FLICK\_TIME，但是更小范围的指向瞄准将会受到影响：0.0的数值意味着完全无变换 （任何指向瞄准都会耗费FLICK\_TIME秒)，1.0的数值意味着旋转时长与角度成线性比例（90度的旋转会耗费0.5 * FLICK\_TIME秒）。更高的数值将会（过度）加剧大小范围指向瞄准间的时间差。
* **FLICK\_SNAP\_MODE** （指向吸附模式，默认无 NONE) - 在没有任何练习的琴况下你可能会发现指向瞄准难以指向你想预期的方向。 如果你想限制指向瞄准的角度，FLICK\_SNAP\_MODE 给予了你三个选项。默认情况下NONE（无)，完全无吸附。我相信娴熟的玩家为了应对每个方向的危险都会禁用这项功能。但是对你可以设置为4，将指向方向锁定在向前，向后，向左或向右。以90度分割。如果你想要45度分割，你可以将FLICK\_SNAP\_MODE设置为8.
* **FLICK\_SNAP\_STRENGTH** （指向吸附强度，默认1.0) - 如果你启用了指向吸附，这项参数可以设置吸附的强度，从0（不吸附）到1（完全吸附）。
* **FLICK\_DEADZONE\_ANGLE** （指向死区角度，默认0.0度) - 有时你想在不触发指向瞄准的情况下用摇杆快速旋转。完美向前推动几乎是不可能的，所以一般情况下你会微微偏移一点，导致你失去现在维持的角度。这项设置为向前*指*创造了一个死区：在死区内向前推动摇杆将不再触发*指*，从而允许你直接进入旋转阶段。 这项参数将会对称的应用在左右方向上，如果你设了15度，那么一共将会有30度的死区。

**\*开发者提示：** DS5和DS4的摇杆因为分辨率较低的问题可能会在小范围旋转的过程中出现顿挫感。JSM会在旋转的数据中加入小量的平滑，有效的掩盖了瑕疵。 更大的旋转范围完全没被平滑。 这在[体感维基](http://gyrowiki.jibbsmart.com)上为想加入此功能的游戏开发者做了更详细的解释。JSM会自动为PS和Switch手柄计算摇杆平滑值, 但是你可以通过ROTATE\_SMOOTH\_OVERRIDE 设定任何平滑范围，通过输入0关闭平滑，或者-1来启用自动平滑的参数。

```FLICK_ONLY```（仅指向）和```ROTATE_ONLY```（仅旋转）摇杆模式可以将指向性瞄准中的某个部分禁用。前者意味着你只能获得一开始倾斜摇杆的*指*没有后续的旋转调整。而后者意味着你不能进行一开始的*指*，但是可以持续旋转调整。

你也可以在一个虚拟手柄上使用指向性瞄准，不过会受限一点。将**FLICK\_STICK\_OUTPUT**（指向性瞄准输出）映射至**RIGHT\_STICK**（左摇杆）或**LEFT\_STICK**（右摇杆）而不是默认的**MOUSE**（鼠标）。当将指向性瞄准输出至虚拟手柄时，FLICK_TIME和FLICK_TIME_EXPONENT 均无效。 虚拟手柄会向需要旋转的方向完全倾斜虚拟摇杆以提供足够的时间来完成旋转。通常来讲这比鼠标模式低效许多，但是依然非常实用。用过**VIRTUAL\_STICK\_CALIBRATION**来校准虚拟手柄摇杆的灵敏度。理想情况下，游戏中的水平灵敏度应该需要拉满。

#### 3.3 其他鼠标模式

当使用 ```MOUSE_RING```（鼠标环）摇杆模式时，倾斜摇杆将使鼠标光标的位置相对摇杆的移动机角度偏离屏幕中心的位置。这一模式原先为双摇杆平面射击游戏所设计。为了达成最佳效果，JSM需要知道你的屏幕分辨率（SCREEN\_RESOLUTION\_X，X分辨率和 SCREEN\_RESOLUTION\_Y，Y分辨率)以及你希望光标偏移多远 （MOUSE\_RING\_RADIUS，鼠标环大小)。 当这个模式启用时（或者说摇杆不在中心时），全部其他鼠标输入将会被忽略。

当使用```MOUSE_AREA```（鼠标区域） 摇杆模式时，摇杆的数值直接决定鼠标位置。所以慢慢的向右移动摇杆将会使鼠标光标移到```MOUSE_RING_RADIUS```（鼠标环大小）同等的位置； 而当摇杆移回中心时光标也会回到起始点。与之前的模式不同，其他鼠标输入，比如体感可以与鼠标区域叠加。

#### 3.4 数字模式

当使用```NO_MOUSE```（空置）摇杆模式时，JSM会将摇杆区域交叉切割为UP（上)、DOWN（下)、LEFT（左）、RIGHT（右)。

最后， ```SCROLL_WHEEL```（滚轮） 摇杆模式如名字一样会将摇杆变成一个滚轮。左映射将在逆时针旋转时脉冲，而右映射将会在顺时针旋转时脉冲。SCROLL_SENS（滚轮灵敏度）设置允许你改变触发一次脉冲的角度。与其他灵敏度选项不同，滚轮灵敏度越高脉冲频率越低。

```
# 左摇杆移动
LLEFT = A
LRIGHT = D
LUP = W
LDOWN = S
LEFT_RING_MODE = INNER
LRING = LALT # 慢走
```

#### 3.5 体感摇杆与倾斜映射
可以通过运动传感器将你的手柄看作一个摇杆。 “体感摇杆”可以实现任何普通摇杆的功能：
* **MOTION\_STICK\_MODE** （体感摇杆模式，默认 NO\_MOUSE) - 与LEFT\_STICK\_MODE和RIGHT\_STICK\_MODE相同。
* **MOTION\_RING\_MODE** （体感摇杆环模式，默认 OUTER) - 与LEFT\_RING\_MODE和RIGHT\_RING\_MODE相同。
* **MOTION\_DEADZONE\_INNER** （体感摇杆内死区，默认15度) - 启动体感摇杆的手柄倾斜角度。
* **MOTION\_DEADZONE\_OUTER** （体感摇杆外死区，默认135度) - 最大倾斜角度的阈值。 理论上最大当然是180度。所以默认的135度意味着倾斜至或者超过相对**neutral position**45度的倾斜将会被视作“最大斜率”。
* **MOTION\_STICK\_AXIS** （体感摇杆轴，默认标准，STANDARD) - 反转你想要的轴， 添加第二项参数区分垂直与水平的反转。
* **MLEFT**, **MRIGHT**, **MUP**, **MDOWN** - 体感摇杆对应的左，右，前，后映射。
* 这项参数也受**CONTROLLER\_ORIENTATION**（手柄方向）参数的影响。

需要正确的校准体感来获得最佳效果（请查阅下方体感鼠标输入中的校准步骤）。

默认情况下，**neutral position**（居中位置） 大约是手柄放置在一个平面上的状态。你可以通过输入```SET_MOTION_STICK_NEUTRAL```（居中体感摇杆）指令来重新设置居中位置。当这条指令被执行时，手柄当前的姿态将会被认定为“居中”。

一个常见的运动传感器用途就是左倾或者右倾映射。这与体感摇杆有些差别 —— 不管你的手柄处于放平或者立着的状态，倾斜映射都完全一致。
* **LEAN\_THRESHOLD** （倾斜阈值，默认15度）— 当手柄左或右的倾斜的角度超过此项的阈值时，将会触发对应的 **LEAN\_LEFT**（左倾斜）或者 **LEAN\_RIGHT**（右倾斜）映射。

### 4. 体感鼠标输入
**首先，关于体感鼠标最重要的信息，**手柄的陀螺仪（实现体感的组件）需要经常校准。这仅仅意味着告诉应用程序“归零点”在哪里。 就像“称重”一样，陀螺仪需要一个可供比较的参考点，以便准确地输出体感。一般通过将手柄放在静止不动的平面上并计算一段时间内的数据实现校准。这些数据因为含有一些“噪声”需要进行平均 —— 并不是由移动造成的移动 —— 但是相较于平时手持手柄的抖动，这些噪声可以忽略不计。

如果你启用了体感鼠标，并发现光标在手柄完全不移动时仍然在移动（就算非常缓慢） ，这就意味着你的手柄需要校准了，不必担心，这很常见 —— 我在每次游戏前都会快速校准一下自己的手柄，尤其是任天堂的。

如果你将**AUTO\_CALIBRATE\_GYRO**（自动校准陀螺仪）设置为**ON**（开), JSM会自动检测手柄静态的时刻，并校准。这并不完美 —— 每个自动校准算法*有时*都会将平滑，缓慢的移动视作体感飘移。这可能会导致打断瞄准远处或者慢速目标时的微小移动。同时这只是个刚推出的试验性功能， 并不包含任何你可以调整的阈值。因为上述原因，此功能默认为**OFF**（关闭），并且建议你手动校准陀螺仪。

如果要手动校准陀螺仪，将手柄放置到完全静止的平面上，保证手柄不会移动，然后输入以下指令：

* **RESTART\_GYRO\_CALIBRATION**（重启陀螺仪校准） - 所有已连接的体感设别将会开始采集体感数据，记住现在全部采集到的数据都会在后面设置为“归零点”。
* **FINISH\_GYRO\_CALIBRATION**（结束陀螺仪校准） - 停止收集校准用的体感数据。 JSM会将全部收集到，并平均的数据设做“归零点”。

只需要大约一秒来采集准确的校准数据，你也可以通过将按键映射至**CALIBRATE**（校准）。这里是假设你使用了内置映射的使用方法：

* 点击手柄上的PS，触摸板下压，房子，或者捕捉键来重启陀螺仪校准，或者结束正在进行的校准。
* 长按手柄上的PS，触摸板下压，房子，或者捕捉键来重启陀螺仪校准，并在松开时结束校准。**警告**：我发现使用我JoyCon上的房子键会干扰体感，因此在我使用长按校准时，松开此按键将会造成一定的误差。如果你也遇到了相似的问题，最好直接通过点击校准各个手柄，或者直接通过上述指令一次校准全部手柄。

因为陀螺仪的物理性质（比如温度）可以影响它们对“归零点”的感知，在开始游戏前校准一次通常可以满足剩下的游玩，但是也可能因为使用过程中的发热要求你重新校准体感。当你发现发现光标在手柄完全不移动时仍然在向一个方向稳定移动，你就知道手柄的“归零点”有误，需要校准了。

**其次，你需要知道**如何设置体感输入的灵敏度：

* **GYRO\_SENS**（体感灵敏度，默认0.0) - 手柄的旋转和游戏内视角的旋转呈什么关系？数值为1意味着手柄旋转的角度等同于游戏内视角旋转的角度（在2轴限制的情况下）。 数值为2意味着手柄旋转的角度和游戏内视角旋转的角度将呈2倍关系。增加GYRO\_SENS将允许你在姿势别扭前更任意的旋转游戏内的视角。但是降低灵明度允许你更精确的打击小目标。 
在不直接控制视角，而是光标的游戏中，GYRO\_SENS 为1意味着手柄需要旋转一圈将光标从屏幕的一侧移动至另一侧。这类游戏最好使用8或更高的 GYRO\_SENS，意味着你需要将手柄旋转360/8 = 45 度来将光标从屏幕的一侧移动至另一侧。

单个GYRO\_SENS可能无法满足你同时想精确瞄准小目标和大范围移动的需求。

JSM允许你设置"当缓慢移动时，我想要这个灵敏度。当快速旋转时，我想要这个灵敏度。"你可以通过设置两个阈值，以及对应阈值的灵敏度来实现这个效果。全部中间值将会被线性插入。使用MIN\_GYRO\_THRESHOLD, MAX\_GYRO\_THRESHOLD, MIN\_GYRO\_SENS 和 MAX\_GYRO\_SENS：

* **MIN\_GYRO\_THRESHOLD**（最低体感阈值)和 **MAX\_GYRO\_THRESHOLD**（最高体感阈值，默认0.0度每秒); **MIN\_GYRO\_SENS** 和 **MAX\_GYRO\_SENS**（最高体感灵敏度，默认 0.0) - MIN\_GYRO\_SENS 和 MAX\_GYRO\_SENS 就和 GYRO\_SENS 一样，但是MIN\_GYRO\_SENS 决定手柄旋转速度等于或低于 MIN\_GYRO\_THRESHOLD 时的灵敏度，而MAX\_GYRO\_SENS 决定手柄旋转速度等于或高于 MAX\_GYRO\_THRESHOLD 时的灵敏度。当手柄的旋转速度在这两个阈值的区间时，体感的灵明度将会被线性改变。阈值的参数基于现实的度每秒。打个比方，当你想要在一秒内旋转1/4个圆，那就是90度每秒。GYRO\_SENS设置将会覆写 MIN\_GYRO\_SENS 和 MAX\_GYRO\_SENS 至同一数值。你可以通过添加第二项参数设置不同的**垂直灵敏度**。

**最后**，如果你想的话，还有一些可以调整的设置：

* **GYRO\_SPACE**（体感参考系，默认本地 LOCAL) - 最简单的体感瞄准方案，将手柄体感的一个轴映射至视角/光标的水平瞄准，另一个映射至垂直瞄准。这将会是你在JSM中将 GYRO\_SPACE 设置为"LOCAL"的效果。这是最简单，也是最难出错的方案，但是瞄准会在你倾斜手柄时越来越奇怪。如果你希望采用一些结合加速计的高阶体感瞄准方案，PLAYER\_TURN（玩家旋转）正是你需要的。 或者，如果你喜欢通过倾斜手柄来进行瞄准，尝试一下 PLAYER\_LEAN（玩家倾斜）。最后，WORLD\_TURN（世界旋转）和 WORLD\_LEAN（世界倾斜）会更比 PLAYER\_* 选项更注重重力的方向。
* **GYRO\_AXIS\_X**（体感轴X）和 **GYRO\_AXIS\_Y**（体感轴Y，默认标准 STANDARD）- 需要时允许你反转体感轴的方向。想要体感向左旋转时视角向右旋转? 将 GYRO\_AXIS\_X 设置为 INVERTED（反转）即可。将其设置为STANDARD（标准）以获得正常模式。
* **MOUSE\_X\_FROM\_GYRO\_AXIS**（鼠标X体感轴源）和 **MOUSE\_Y\_FROM\_GYRO\_AXIS**（鼠标Y体感轴源，默认对应 Y 和 X）- 也许你想沿本地Z轴倾斜手柄来左右旋转视角，而不是本地Y轴。或者你想使用单个侧向握持的JoyCon。这将允许你那么做。你的选择是 X, Y, Z, 以及 NONE（当你不想让某个轴被体感影响时）这些参数只会在GYRO\_SPACE 设为LOCAL时生效。
* **GYRO\_CUTOFF\_SPEED**（体感截止速度，默认 0.0度每秒) - 有些游戏通过设置一个最低阈值来忽略微小的抖动。这就是那项设置。他从来就不好用。不要使用这项功能。有些游戏甚至不允许关闭这个“功能”。我尝试着把它变得更好并加入了这项功能。不过主要的目的就是为了让你体验这个功能有多烂，或者也许你能想我展示这个功能好的一面。
也许对于按键超大的UI来说无所谓，但是对于*任何*想要精确瞄准目标的玩家来说都非常糟糕，因为任何精确操作都会低于截止范围。就算是非常低的截止速度也可能会将瞄准慢速目标的移动截止掉。
有些人也许会反驳为什么不将截止速度继续降低？降低截止速度确实可以解决低速卡顿的问题，但是这么低的范围其实和没有差不多。
* **GYRO\_CUTOFF\_RECOVERY**（体感截止补偿，默认0.0度每秒）- 为了防止 GYRO\_CUTOFF\_SPEED 带来的慢速卡顿问题，JSM将截止速度与 GYRO\_CUTOFF\_RECOVERY 做了平滑处理。原先只计划将 GYRO\_CUTOFF\_SPEED 变得不那么糟糕，不过后面发现就算在GYRO\_CUTOFF\_SPEED为0.0时也可以很好的消除抖动。但是我仅仅将其（也许还会结合下面的平滑）作为最后不得已的手段。
* **GYRO\_SMOOTH\_THRESHOLD**（体感平滑阈值，默认0.0度每秒）- JSM会选择性的加入平滑以补偿高灵敏度下手带来的抖动。但是平滑也不可避免地带来了延迟，所以游戏*永远*不应该对*任何一个大于很小阈值的输入*作平滑处理。任何等于或者大于这个阈值的体感输入都不会被平滑。任何低于这一阈值的体感输入将根据 GYRO\_SMOOTH\_TIME 设置被平滑，从完全平滑过渡到
一半GYRO\_SMOOTH\_THRESHOLD的半平滑，再到 GYRO\_SMOOTH\_THRESHOLD 的无平滑。
* **GYRO\_SMOOTH\_TIME**（默认0.125秒）- 如果有任何应用至体感输入的平滑（由上述的GYRO\_SMOOTH\_THRESHOLD决定），GYRO\_SMOOTH\_TIME 决定了平滑的时长。数值越大意味着平滑越多，但也同时产生了极大的滞后和延迟感觉。数值过小时就没任何效果。

### 5. 真实世界校准(RWC)
*指向性瞄准*，标准瞄准，和体感瞄准全部依赖于 REAL\_WORLD\_CALIBRATION（RWC，真实世界校准）有参考意义，可以夸游戏与玩家的数值。进一步说，如果RWC的设置不正确，会导致*指向性瞄准*无法与摇杆方向吻合。

每个游戏都有独立的RWC数值来使其他功能正常运作。这极大的简化了分享配置的难度，因为只要有一人得到了一个游戏正确的RWC数值， 他就可以将这项数值分享给任何同一游戏的玩家。 [体感维基](http://gyrowiki.jibbsmart.com/games) 正有一个数据库，储存了各个游戏的 RWC（以及每个游戏独特的配置）。你可以通过直接使用这个数据库，避免自己计算，或者你没法在数据库中找到需要的游戏数值时，自己使用JSM内置的工具进行计算，可以将你的计算结果分享到[体感维基](http://gyrowiki.jibbsmart.com/games)来帮助以后的玩家!

对于鼠标控制视角的3D游戏，RWC是一个倍增器，可以将给定的角度转换为鼠标输入，从而在游戏中将视角转换为相同的角度。

对于鼠标控制光标的2D游戏，RWC是一个将角度转换为屏幕尺寸比例的倍增器（游戏分辨率影响光标速度的时候）。

#### 5.1 校准条件

在我们开始为一个游戏校准良好的RWC值前，我们先要弄明白什么影响了灵敏度：

* 游戏内的鼠标设定
* Windows（系统）的鼠标设定

就算设置RWC正确， **游戏内的鼠标设定**依然会改变视角和鼠标移动的关系：

* 当你在游戏中开启*鼠标加速*时（有些游戏有这个选项，不过一般默认关闭），你需要将其关闭以便JSM能够精确工作。
* 大多数游戏都会有*鼠标灵敏度*设置，一般都是个简单的鼠标输入倍增器。JSM无法直接读取这个数值，所以你需要手动定义这项数值以便JSM将其抵消掉。可以通过设置 ```IN_GAME_SENS```来告诉JSM游戏内的灵敏度。  
打个比方，当使用键鼠游玩时，我的《雷神之锤：冠军》鼠标灵敏度为1.8。所以在我JSM的《雷神之锤：冠军》配置文件中，或者当我使用别人的配置文件时，我会添加 ```IN_GAME_SENS = 1.8``` 以便JSM抵消游戏内的灵敏度差异。

*部分*游戏还需要面对的其他变量；**Windows（系统）的鼠标设定**:

* Windows系统的鼠标设置中，有一个叫“提高光标精准度”的选项，JSM无法准确的针对这一选项。  大部分玩家都会关闭这项功能，使用JSM时最好也关闭这个功能。
* Windows系统的光标速度设置通常也会影响鼠标在游戏中的灵敏度，但是JSM*可以*检测并抵消Windows的鼠标灵敏度设置。可以通过这项功能实现：```COUNTER_OS_MOUSE_SPEED```（抵消系统鼠标设置）。
唯一麻烦的一点就是有些游戏*不会*受到Windows光标设置的影响。 很多当代射击游戏都使用了“原始输入”来忽略Windows的鼠标设置，以便让玩家在不影响游戏的情况下任意使用“提高光标精准度”和其他系统灵敏度。如果你发现并不需要使用 COUNTER\_OS\_MOUSE\_SPEED 时可以通过```IGNORE_OS_MOUSE_SPEED```（忽略系统鼠标设置）命令来恢复默认，对于支持原始输入的游戏来说更合理。

综上所述，当你想创建一个他人能用的配置时, 请考虑包含 COUNTER\_OS\_MOUSE\_SPEED 。 当使用他人分享的配置时，请记住将 IN\_GAME\_SENS 设置为*你游戏中的*鼠标灵敏度。

完成这些步骤后，就准备好开始**计算游戏RWC值**吧。

#### 5.2 计算3D游戏的RWC

对于*鼠标控制视角的3D游戏*，精确计算RWC的方法是像这样打开指向性瞄准的同时先猜测一个RWC值：

```
RIGHT_STICK_MODE = FLICK
REAL_WORLD_CALIBRATION = 40
```

现在，在游戏中，用鼠标将准星或者某个可参考的刻度移动到很小的一个目标上，将右摇杆向前推，然后持续旋转直到你完成了一整周旋转，当准星或者参考物重新瞄准到那个很小的目标时松开摇杆。

JSM会记忆上一次指向性瞄准的指和旋转参数，现在只需要输入：

```CALCULATE_REAL_WORLD_CALIBRATION```（计算RWC）

这将告诉JSM你的上个指向性瞄准在游戏中完成了一整周旋转，你想知道需要将RWC值设置到多少。JSM会返回这样一条信息："Recommended REAL\_WORLD\_CALIBRATION: 151.5"（建议将RWC设置为：151.5。打个比方）。现在，你可以像这样将RWC设置为建议值来验证校准是否准确：

```REAL_WORLD_CALIBRATION = 151.5``` （151.5只是个比方，应当填写JSM所推荐的值。）

现在回到游戏，通过指向性瞄准测试准确性。指向的角度*应当*与摇杆指向的角度吻合。

如果你想获得更高的精确度，你可以多转几圈。比如...你旋转了10圈来最小化释放摇杆时带来的误差，你可以在计算RWC后加上旋转的圈数：

```CALCULATE_REAL_WORLD_CALIBRATION 10```

这也适用于非整数旋转，比如0.5，对应旋转半圈。

#### 5.3 计算2D游戏的RWC

对于*鼠标控制光标的2D游戏*，指向性瞄准在游玩过程中没有太大的实用性，但是它依然是计算RWC值最快的方法。和上面一样，确保 IN\_GAME\_SENS 和 COUNTER\_OS\_MOUSE\_SPEED 都根据情况正确设置，然后我们和以往一样打开指向性瞄准和猜测一个RWC值：

```
RIGHT_STICK_MODE = FLICK
REAL_WORLD_CALIBRATION = 1
```

值得注意的是这次，我们第一次猜测的RWC值为 *1* 而不是 *40*。相较于3D视角游戏，2D光标游戏往往有更低的RWC值，第一次猜低往往比猜高好，这样你需要将摇杆旋转更多次以获得更精确的结果。 

在2D光标游戏中，完成一周指向旋转应该将鼠标光标从屏幕的一侧边缘移动至另一侧。与3D视角游戏不同，屏幕的边缘会限制鼠标光标走的更远，并影响我们计算的结果。在计算3D视角游戏的RWC时过度旋转无关紧要，因为我们可以往反方向重新对其旋转，并得到准确的结果。但是在2D光标游戏中左右任何一个方向的过冲都会导致JSM读取额外的摇杆数据，而游戏中的光标因为已经抵达边缘不会继续前进。

所以，先从手动将鼠标光标移动到最右边开始，然后将右摇杆向前微微偏右的方向推进，防止指针卡到左侧边框。现在小心的顺时针旋转右摇杆直到光标微微碰到右侧边框，然后松开摇杆。就像上面一样，通过这条指令让JSM自动计算合适的RWC：

```CALCULATE_REAL_WORLD_CALIBRATION```

随后JSM会输出推荐的RWC值。
一般长这样： "Recommended REAL\_WORLD\_CALIBRATION: 5.3759"。

你不需要告诉JSM你想为2D游戏还是3D游戏校准。*指向性瞄准k*与其他设置靠这种方法计算的RWC来支持3D游戏，但是3D游戏（角度和角加速度）和2D游戏（2D平面）之间没有直接的隔应。所以2D游戏的计算就和描述的一样简单通用。

在RWC校准的2D游戏中，你可以赋予 GYRO\_SENS 或其他参数有意义的单位，比如旋转手柄多少度来从屏幕的一侧抵达另一侧。数值为 *1* 的 GYRO\_SENS 意味着手柄需要旋转一整圈来将光标从屏幕的一侧移动到另一侧，当然很不合理！但是数值为 *8* 的 GYRO\_SENS 只需要1/8圈旋转（45 度） to 来将光标从屏幕的一侧移动到另一侧，非常合理。

### 6. ViGEm虚拟手柄

JoyShockMapper can create a virtual xbox or DS4 controller thanks to Nefarius' ViGEm Bus and ViGEm Client softwares. The former needs to be installed by the user before the latter can be used. Once installed, you can set which virtual device you desire to create for each connected device using the command ```VIRTUAL_CONTROLLER = XBOX``` or ```VIRTUAL_CONTROLLER = DS4```. The default value is ```NONE```, which is no virtual controller at all. Rumble will then work on DS4 controllers, but obviously support is game dependant. Using virtual controllers is most likely to work well only if whitelisting is active (HIDGuardian/HIDCerberus), in order to hide the original controller entry from the game and only expose the virtual one. Funny thing to note is that hiding DS4s with HIDGuardian will also hide the virtual DS4 from ViGEm, since Windows cannot tell the virtual controller form the physical one.

#### 6.1 Xbox手柄映射
If you have set the virtual controller to the xbox scheme, then the following becomes available to you:
* **New digital bindings**
```
X_A, X_B, X_X, X_Y : The xbox face button diamond
X_LB, X_RB : The xbox bumper buttons
X_LS, X_RS : The xbox stick click buttons
X_BACK, X_START, X_GUIDE : The xbox control buttons
X_UP, X_DOWN, X_LEFT, X_RIGHT : The xbox dpad directions
X_LT, X_RT : Digital trigger bindings
# There is no CAPTURE / SHARE (Series X) button binding yet in ViGEm
```

* **New stick mode available**
```
LEFT_STICK_MODE = LEFT_STICK
RIGHT_STICK_MODE = RIGHT_STICK
```

* **New trigger mode available**
```
# Using analog triggers
ZL_MODE = X_LT
ZR_MODE = X_RT
```

Using both analog and digital trigger bindings at the same time leads to undefined behaviours. Use modeshift as defined in the next section to disable analog triggers while a digital trigger binding is active.

You will also find a default xbox layout in the GyroConfigs folder that you can use to set up a standard xbox controller configuration. But of course, you can remap buttons elsewhere, or combine them in using the event modifiers, chords, simultaneous presses and such.

```
GyroConfigs/xbox.txt # load the generic xbox mapping

# Map the dpad as chords of the face buttons
L3 = NONE
L3,N = X_UP
L3,W = X_LEFT
L3,S = X_DOWN
L3,E = X_RIGHT

S,S = X_LS # Double click jump to sprint instead

L+R = X_RS # I don't like to stick click often

MOTION_STICK_MODE = RIGHT_STICK # Gyro driving
```

#### 6.2 DS4手柄映射

ViGEm also the ability to emulate a Dualshock 4 controller. This can allow you to use a switch pro as a DS4 in a game that has this support built in for example. Setting the virtual controller to DS4 enables the use of these features as well. Take note that these names are aliases to the xbox names, so the logs might display the other label.

* **New digital bindings**
```
PS_CROSS, PS_CIRCLE, PS_SQUARE, PS_TRIANGLE : The playstation face button diamond
PS_L1, PS_R1 : The playstation bumper buttons
PS_L3, PS_R3 : The playstation stick click buttons
PS_SHARE, PS_OPTIONS : The playstation control buttons
PS_UP, PS_DOWN, PS_LEFT, PS_RIGHT : The playstation dpad directions
PS_HOME, PS_PAD_CLICK : The playstation home and pad click buttons
PS_L2, PS_R2 : The playstation digital trigger bindings
```

* **New stick mode available** These are exactly the same as the xbox names
```
LEFT_STICK_MODE = LEFT_STICK
RIGHT_STICK_MODE = RIGHT_STICK
```

* **New trigger mode available**. JoyShockMapper will display the trigger mode as the xbox name : the trigger will still work properly.
```
# Using analog triggers
ZL_MODE = PS_L2
ZR_MODE = PS_R2
```

Using both analog and digital trigger bindings at the same time leads to undefined behaviours. Use modeshift as defined in the next section to disable analog triggers while a digital trigger binding is active.

#### 6.3 虚拟手柄体感
While the virtual controller can't output gyro, JoyShockMapper can convert gyro input to stick output. For example:
```
GYRO_OUTPUT = RIGHT_STICK
```

```GYRO_OUTPUT``` can also be set to ```LEFT_STICK``` or left at its default value of ```MOUSE```, which means gyro will be converted to mouse input.

Because games tend to do a lof of processing on stick input to turn it into camera movement, you'll want to counter that processing to convert it to a decent camera movement. So far, here are your options:
* **RIGHT_STICK_UNDEADZONE_INNER** / **LEFT_STICK_UNDEADZONE_INNER** (default 0) - This will counter the game's inner deadzone. For example, if a game has a deadzone of 0.25 (that's 25% off the stick movement range), very small gyro inputs will convert to a stick radius of just over 0.25 so that the game detects them right away. When gyro output is assigned to this stick and no gyro is detected (if GYRO_SENS is 0, for example), this stick position will be set to the edge of this deadzone. This means you can tweak this deadzone value until you get the largest value that doesn't move the camera automatically to find the exact inner deadzone.
* **RIGHT_STICK_UNDEADZONE_OUTER** / **LEFT_STICK_UNDEADZONE_OUTER** (default 0) - Distance from the outer theoretical edge of the stick's range that will be considered "full tilt". Gyro velocity will be mapped to a stick position between \_STICK_UNDEADZONE_INNER and \_STICK_UNDEADZONE_OUTER.
* **RIGHT_STICK_UNPOWER** / **LEFT_STICK_UNPOWER** (default 0) - A power curve is often applied to stick input to give players more with small movements at the cost of less precision with very large movements. If you know what the exponent is, putting it here will counter that power curve to hopefully give you linearly proportional camera control. The default value of 0 does nothing, which is effectively the same as setting it to 1, since that assumes a linear curve.
* **RIGHT_STICK_VIRTUAL_SCALE** / **LEFT_STICK_VIRTUAL_SCALE** (default 1) - Use this to scale down virtual stick output. For example, if you're converting gyro to right stick aim (GYRO_OUTPUT = RIGHT_STICK), you'll want a high in game stick sensitivity so you can do fast flicks with the gyro. But you may not want your regular stick aim to be that high. You could set this setting to 0.5 to halve the strength of your stick aiming. JSM will take into account your "UNPOWER" and "UNDEADZONE" settings when calculating this new stick output.

Games sometimes do a lot of other processing to the stick input: easing in, acceleration, direction warping, angular deadzones, for example. JSM does not yet have a way to counter these effects.

### 7. 快速切换

Almost all settings described in previous sections that are assignations (i.e.: uses an equal sign '=') can be chorded like a regular button mapping. This is called a modeshift because you are reconfiguring the controller when specific buttons are pressed. The only *exceptions* are those listed here below.
```
AUTOLOAD
JSM_DIRECTORY
SIM_PRESS_WINDOW
TICK_TIME
GRID_SIZE
HIDE_MINIMIZED
VIRTUAL_CONTROLLER
```

Here's some usage examples: in DOOM (2016), you can use the right stick when you bring up a weapon wheel even when using flick stick:

```
RIGHT_STICK_MODE = FLICK # Use flick stick
GYRO_OFF = R3 # Use gyro, disable with stick click

R = Q # Last weapon / Bring up weapon wheel

R,GYRO_ON = NONE\ # Disable gyro when R is down
R,RIGHT_STICK_MODE = MOUSE_AREA # Select wheel item with stick
```

Other ideas include changing the gyro sensitivity when aiming down sights, or only when fully pulling the trigger.

```
GYRO_SENS = 1 0.8 # Lower vertical sensitivity

ZL_MODE = NO_SKIP # Use full pull
ZL = RMOUSE # ADS
ZLF = NONE # No binding on full pull

ZLF,GYRO_SENS = 0.5 0.4 # Half sensitivity on full pull
```

These commands function exactly like chorded press bindings, whereas if multiple chords are active the latest has priority. Also the chord is active whenever the button is down regardless of whether a binding is active or not. It is also worth noting that a special case is handled on stick mode changes where upon returning to the normal mode by releasing the chord button, the stick input will be ignored until it returns to the center. In the DOOM example above, this prevents an undesirable flick upon releasing the chord.

To remove an existing modeshift you have to assign ```NONE``` to the chord. There is special handling for the gyro button because NONE is a valid assignment. Add a backslash to indicate it is the button assignment rather than clearing the modeshift

```
GYRO_OFF = RIGHT_STICK   # Gyro off when using right stick
ZLF,GYRO_OFF = NONE\     # RS does not turn gyro off when ZLF is pressed
ZLF,GYRO_OFF = NONE      # oops undo
```

### 8. 触摸板

The touchpad always offers the ```TOUCH``` button binding. It will be pressed if there is any touch point active. This binding will overlap with other touch buttons and can be useful to disable gyro for example, or bring up the game map. There is also a dual stage mode setting for the touchpad touch and click: ```TOUCHPAD_DUAL_STAGE_MODE``` which can be any mode explained in the analog triggers, where CAPTURE is the full press or click and TOUCH is the soft press. The default setting is NO_SKIP.

The most important setting for the touchpad is simply ```TOUCHPAD_MODE``` which will determine the primary functionality of the touchpad. Here are two possible values:
* **GRID_AND_STICK** - Grid And Stick will create a button grid of equally sized buttons on the touch pad. You have to also assign to ```GRID_SIZE``` the number of columns and rows of the grid : the product of the two cannot be greater than 25 or lesser than 1. Touch buttons T1-TN will then become available for assignment: they are layed out in order from left to right, from top to bottom. There are also two touchsticks available. See below.
* **MOUSE** - Mouse mode turns the touchpad into a familiar laptop touchpad. Sensitivity can be adjusted via ```TOUCHPAD_SENS```. Gestures will be added to this mode in future releases. Taps and double taps are already usable via ```TOUCH```.

Here's an example of grid usage to add some more buttons that otherwise would not be worth putting on a controller
```
TOUCHPAD_MODE = GRID_AND_STICK
GRID_SIZE = 2 1   # split the pad in two buttons, left and right
GYRO_OFF = TOUCH  # disable the gyro when I touch either button

# Bind on clicks
CAPTURE = NONE   # Chorded with touch buttons
T1,CAPTURE = F1  # View Help
T2,CAPTURE = F10 # Quick Save
```

Or a typical touchapd in cursor mode
```
TOUCHPAD_MODE = MOUSE
TOUCH = LMOUSE'	           # Quick tap means select
TOUCH,TOUCH = RMOUSE       # Double tap for right click
CAPTURE = LMOUSE ^LMOUSE   # Or click pad to toggle click (dragging)
```

#### 8.1 触摸板摇杆

A touch stick is a virtual joystick mapped unto the touchpad. As such, a touch stick has and uses all of the familiar binding names and settings, plus one new setting.
```
TUP, TDOWN, TLEFT, TRIGHT, TRING : The touchstick four directions when in NO_MOUSE mode.
TOUCH_STICK_MODE: Set the touchstick to any  stick mode (AIM, FLICK_ONLY, MOUSE_AREA, etc...)
TOUCH_DEADZONE_INNER: Sets how large the area with no output is. There is no TOUCH_DEADZONE_OUTER, as it is replaced with TOUCH_STICK_RADIUS. See below.
TOUCH_RING_MODE: Sets what ring should be used for TRING, either INNER or OUTER.
TOUCH_STICK_RADIUS: Sets the size of the stick, or in other words, the amount of touchpad units to travel to the edge of the stick.
TOUCH_STICK_AXIS** (default STANDARD) - Select whether you want to invert the axis. To assign a separate vertical value, provide a second parameter.
```

The touchstick center is always the point of contact. As such, one can easily configure swipes by setting a very large touch stick radius and binding values to the 4 directions.

The touch stick differs from other input methods in one particular way. The four stick directions cannot be used as a chord for other buttons, but you can chord the four direction with the grid buttons. As such, you can control two touch sticks at the same time on either side of the touch pad with each having different bindings. The example below showcases the numbers 1 to 4 bound to swipe gestures on the left half of the pad, and 5 to 8 bound to swipe gestures on the right half of the pad.

```
TOUCH_STICK_MODE = GRID_AND_STICK
GRID_SIZE = 2 1 # Left and Right
TOUCH_STICK_RADIUS = 800 # Use a larger value to use stick as swipe gestures

T1,TLEFT = 1
T1,TUP = 2
T1,TRIGHT = 3
T1,TDOWN = 4

T2,TLEFT = 5
T2,TUP = 6
T2,TRIGHT = 7
T2,TDOWN = 8
```

### 9. 其他命令
There are a few other useful commands that don't fall under the above categories:

* **RESET\_MAPPINGS** - This will reset all JoyShockMapper's settings to their default values. This way you don't have to manually unset button mappings or other settings when making a big change. It can be useful to always start your configuration files with the RESET\_MAPPINGS command. The only exceptions to this are the gyro calibration state / settings and AUTOLOAD.
* **RECONNECT\_CONTROLLERS** - Controllers connected after JoyShockMapper starts will be ignored until you tell it to RECONNECT\_CONTROLLERS. When this happens, all gyro calibration will reset on all controllers. You can add MERGE or SPLIT to indicate whether you want all JoyCons under a single controller or separate controllers. The player LED will help you identify whether they are merged or split.
* **\# comments** - Any line or part of a line that begins with '\#' will be ignored. Use this to organise/annotate your configuration files, or to temporarily remove commands that you may want to add later.
* **JoyCon\_GYRO\_MASK** (default IGNORE\_LEFT) - Most games that use gyro controls on Switch ignore the left JoyCon's gyro to avoid confusing behaviour when the JoyCons are held separately while playing. This is the default behaviour in JoyShockMapper. But you can also choose to IGNORE\_RIGHT, IGNORE\_BOTH, or USE\_BOTH.
* **JoyCon\_MOTION\_MASK** (default IGNORE\_RIGHT) - To avoid confusing behaviour when the JoyCons are held separately while playing, you can have one JoyCon ignored for MOTION\_STICK related functions. Since we ignore the left JoyCon by default for gyro, we ignore the right JoyCon by default for motion stick. But you can also choose to IGNORE\_RIGHT, IGNORE\_BOTH, or USE\_BOTH.
* **SLEEP** - Cause the program to sleep (or wait) for a given number of seconds. The given value must be greater than 0 and less than or equal to 10. Or, omit the value and it will sleep for one second. This command may help automate calibration.
* **TICK\_TIME** (default 3) - The number of milliseconds to wait between between checking the state of connected controllers. Previous versions only sent new virtual keyboard and mouse inputs when there was a new message from the controller, but this made JoyCons clunky on a monitor with a refresh rate higher than 67Hz. Now, all connected devices are polled at the same rate, and you can change it here. The default of 3 milliseconds will give you a polling rate of approximately 333Hz.
* **LIGHT_BAR** - Set the DS4 light bar to the assigned color. You can assign either a 6 hex digit code precedded by 'x', three decimal values for red, green and blue between 0 and 255, or simply a [common color name](https://www.rapidtables.com/web/color/RGB_Color.html#color-table) in capitals and underscore.
* **HIDE_MINIMIZED** - Some users like having JSM hidden in the notification area. You can hide JSM when minimized by setting this to ON. OFF is the default value.
* **README** will lead you to this document.
* **HELP** Will display a list of all commands, all commands containing a given string, or the specific help for all the exact command names given to it.
* **CLEAR** Remove all text from the console screen.

## 配置文件

All of the commands layed out in the previous section can be saved in a text file and run all at once. In Windows, you can also drag and drop a file from Explorer into the JoyShockMapper console window to enter the full path of that file. These configuration files can additionally reference one another. This allows you to group a few settings as a "building block" for your configurations: such as your gyro sensitivity and acceleration preferences.

If you enter a relative path to the file, it should be relative to the folder where JoyShockMapper.exe is located. If however your files don't seem to get picked up, you can manually set where to look for the configuration files by entering the command ```JSM_DIRECTORY = D:\JSM``` for example. You can also set that working directory as a command line argument when running JoyShockMapper, which can be done in a shortcut properties. Putting all your configuration files in a synchronized folder allows you to have those configurations across all computers you use for gaming!

What more? There are some configuration files that can be run automatically to streamline your experience.

### 1. OnStartup.txt(启动配置)

When JoyShockMapper first boots up, it will attempt to load the commands found in the file OnStartup.txt. This file should be in the JSM_DIRECTORY, which is next to your executable by default. This is a great place to automatically calibrate the gyro, load a default configuration for navigating the OS, and/or whitelisting JoyShockMapper.

### 2. OnReset.txt(重制配置)

This configuration is found in the same location as OnStartup.txt explained above. This file is run each time RESET\_MAPPINGS is called, as well as before OnStartup.txt. This file is a good spot to set a CALIBRATE button for your controller and/or set your GYRO\_SPACE if you're not using the default value.

### 3. 自动加载功能

JoyShockMapper can automatically load a configuration file for your games each time the game window enters focus. Drop the file in the **AutoLoad** folder where JSM_DIRECTORY refers to. JoyShockMapper will look for a name based on the executable name of the program that's in focus. When it goes into focus and autoload is enabled (which it is by default), JoyShockMapper will tell you the name of the file it's looking for - case insensitive.

This enables the user to swap focus between your text editor of choice and the game, and each time the configuration will be automatically reloaded with your latest edits (assuming you've saved!). This system also avoids to step on your toes by **NOT** reloading your configuration if you do change focus between JoyShockMapper itself and the game: any mappings you enter by hand won't be thrown away when you return to your game.

Autoload can be turned off by entering the command ```AUTOLOAD = OFF```. You can enable it again with ```AUTOLOAD = ON```.


## 难疑解答
Some third-party devices that work as controllers on Switch, PS4, or PS5 may not work with JoyShockMapper. It only _officially_ supports first-party controllers. Issues may still arise with those, though. Reach out, and hopefully we can figure out where the problem is.

But first, here are some common problems that are worth checking first.

* In some circumstances, the JoyShockMapper console is responding to controller input and the mouse is responding to gyro movements, but the game you're playing isn't responding to it. This can happen when you launch the game (or it's launcher) as an administrator. JoyShockMapper must also be launched with administrator rights in order to send keyboard and mouse events to the game. Windows shortcuts can be set to always run as admininstrator in the properties window.

* The JoyShockMapper console will tell you how many devices are connected, and will output information with most inputs (button presses or releases, tilting the stick). However, the only way to test that the gyro is working is to enable it and see if you can move the mouse. The quickest way to check if gyro input is working without loading a config is to just enter the command ```GYRO_SENS = 1``` and then move the controller. Don't forget that you might need to calibrate the gyro if the mouse is moving even when the controller isn't.

* Many users of JoyShockMapper rely on tools like HIDGuardian to hide controller input from the game. If JSM isn't recognising your controller, maybe you haven't whitelisted JoyShockMapper. Enter WHITELIST_ADD to do so. You can also add this command to your OnStartup.txt script to do it everytime.

* Some users have found stick inputs to be unresponsive in one or more directions. This can happen if the stick isn't using enough of the range available to it. In this case, increasing STICK\_DEADZONE\_OUTER can help. In the same way, the stick might be reporting a direction as pressed even when it's not touched. This happens when STICK\_DEADZONE\_INNER is too small.

## 已知问题

### 蓝牙连接
JoyCons and Pro Controllers normally only communicate by Bluetooth. Some Bluetooth adapters can't keep up with these devices, resulting in **laggy input**. This is especially common when more than one device is connected (such as when using a pair of JoyCons). There is nothing JoyShockMapper or JoyShockLibrary can do about this. JoyShockMapper experimentally supports connecting Switch controllers by USB.

## 制作组
JoyShockMapper was originally created by **Julian "Jibb" Smart**. As of version 1.3, JoyShockMapper has benefited from substantial community contributions. Huge thanks to the following contributors:
* Nicolas (code)
* Bryan Rumsey (icon art)
* Contributer (icon art)
* Betta-Core (translation)
* Romeo Calota (linux and general portability)
* Garrett (code)
* Robin (linux and controller support)

### 汉化组
如果你认为翻译有任何问题，或者建议，请反馈致QQ：2896378152 或邮箱：lkitdqwb@gmail.com
未来我也会对体感维基（GyroWiki），YouTube视频和JoyShockMapper软件做翻译以及转载

As of version 3, JoyShockMapper development is lead by **Nicolas Lessard**, who was already a long-time contributor and responsible for many of JoyShockMapper's powerful mapping features, autoload, tray menus, and much more. Have a look at the CHANGELOG for a better idea of who contributed what. While Jibb continues on as a contributor, JoyShockMapper is Nicolas' project now. This means updates won't be bottlenecked by Jibb's availability to approve and build them, and Nicolas has final say on what features are included in new versions. As such, make sure you're on [Nicolas' fork](https://github.com/Electronicks/JoyShockMapper) for the latest developments.

JoyShockMapper versions 2.2 and earlier relied a lot on Jibb's [JoyShockLibrary](https://github.com/jibbsmart/JoyShockLibrary), which it used to read controller inputs. Newer versions use [SDL2](https://github.com/libsdl-org/SDL) to read from controllers, as the latest versions of SDL2 are able to read gyro and accelerometer input on the same controllers that could already be used with JoyShockLibrary, but also support many non-gyro controllers as well.

Since moving to SDL2, JoyShockMapper uses Jibb's [GamepadMotionHelpers](https://github.com/JibbSmart/GamepadMotionHelpers), a small project that provides the sensor fusion and calibration options of JoyShockLibrary without all the device-specific stuff.

## 资源推荐
* [GyroWiki（体感维基）](http://gyrowiki.jibbsmart.com) - 所有可以体感控制的好游戏:
* 为什么体感控制使游戏更好;
* 开发人员如何更好地实现体感控制;
* 如何使用JoyShockMapper;
* 用户可编辑的用户配置集合和使用JoyShockMapper与一堆游戏的提示。
* [GyroGaming subreddit](https://www.reddit.com/r/GyroGaming/)
* [GyroGaming discord服务器](https://discord.gg/4w7pCqj).

## 证书
JoyShockMapper is licensed under the MIT License - see [LICENSE.md](LICENSE.md).
