

  # Frida + Mumu 模拟器 + ADB HOOK 实战指南

  本文档详细讲解**Windows 环境下，Mumu 模拟器 + Frida + ADB** 环境搭建、连接配置与 HOOK 实战全流程，全程可直接复现，适配 Android x64 架构模拟器。

  ## 一、环境准备

  ### 1. 工具下载

  | 工具  | 下载地址                                | 备注                                                 |
  | ----- | --------------------------------------- | ---------------------------------------------------- |
  | ADB   | https://adbdownload.com/                | 安卓调试桥，用于连接模拟器、传输文件                 |
  | Frida | https://github.com/frida/frida/releases | 必须下载 **Android x86_64** 版本（对应 Mumu 模拟器） |

  ### 2. 核心依赖安装（Windows 本地）

  打开**命令提示符 / 终端**，执行以下命令安装 Frida 客户端：

  ```bash
  # 安装frida核心库
  pip install frida
  # 安装frida命令行工具（frida-ps、frida等命令依赖）
  pip install frida-tools
  ```

  #### 安装异常解决方案

  若执行命令时提示： `Defaulting to user installation because normal site-packages is not writeable`

  **解决方案：配置 Python Scripts 环境变量**

  1. 按下 `Win + X`，选择「系统」
  2. 点击「高级系统设置」→「环境变量」
  3. 在**用户变量**中找到 `Path`，双击打开
  4. 点击「新建」，添加你的 Python Scripts 路径（示例）：
     1. `C:\Users\97311\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p\LocalCache\local-packages\Python312\Scripts`
  5. 依次点击「确定」保存，重启终端生效

  ## 二、文件部署

  ### 1. 确定 Mumu 模拟器路径

  默认路径示例：

  ```Plain
  D:\Program Files\Netease\MuMu\nx_device\12.0\shell
  ```

  > 该路径为 Mumu 模拟器自带的 ADB 工作目录，后续所有操作均基于此目录。

  ### 2. 解压文件

  将下载好的：

  - ADB 压缩包内所有文件
  - Frida Android x64 压缩包内的 `frida-server` 文件

  **全部解压到上述 Mumu 的 shell 文件夹中**。

  ## 三、Mumu 模拟器配置

  1. 打开 Mumu 模拟器
  2. 开启**ROOT 权限**（模拟器设置中开启）
  3. 打开模拟器「问题诊断」，获取 ADB 连接端口
     1. 默认地址：`127.0.0.1:16384`

  ## 四、ADB 连接模拟器 + Frida 服务端部署

  进入 Mumu 的 `shell` 文件夹，**在此处打开终端**（右键 + 在此处打开终端）。

  ### 1. ADB 连接模拟器

  ```bash
  # 连接Mumu模拟器
  .\adb.exe connect 127.0.0.1:16384
  # 查看已连接设备（验证连接是否成功）
  .\adb.exe devices
  ```

  ### 2. 推送 Frida 服务端到模拟器

  ```bash
  # 将frida-server推送至模拟器临时目录（修改文件名对应你下载的版本）
  .\adb.exe -s 127.0.0.1:16384 push frida-server-17.9.8-android-x86_64 /data/local/tmp
  ```

  ### 3. 授权并启动 Frida 服务端

  ```bash
  # 连接模拟器shell
  .\adb.exe -s 127.0.0.1:16384 shell
  # 获取ROOT权限（必须执行）
  su
  # 进入frida-server所在目录
  cd /data/local/tmp/
  # 赋予可执行权限
  chmod 777 frida-server-17.9.8-android-x86_64
  # 启动frida服务端（终端保持打开，不要关闭）
  ./frida-server-17.9.8-android-x86_64
  ```

  ## 五、Frida 调试环境配置

  **保持上一个终端运行 Frida 服务端**，**新建一个 CMD 终端**执行以下命令：

  ### 1. 端口转发

  ```bash
  # 转发Frida通信端口
  .\adb.exe -s 127.0.0.1:16384 forward tcp:27043 tcp:27043
  ```

  ### 2. 验证连接成功

  ```bash
  # 列出模拟器中所有应用（能正常输出即环境搭建完成）
  frida-ps -Ua
  ```

  ## 六、HOOK 实战

  编写HOOK 脚本（如 `hook.js`），放置在任意目录下，执行命令启动 HOOK：

  ```bash
  # -U：连接USB设备（模拟器） -f：指定包名  -l：加载HOOK脚本
  frida -U -f com.heavenburnsred -l hook.js
  ```

  

  出现问题：程序有frida检测，并非检测脚本和监听端口。尝试使用魔改的frida
  
  1.尝试rusda https://github.com/taisuii/rusda
  
  结果 连接正常，运行HOOK时闪退。
  
  2.尝试安装对应版本的frida 
  
  pip install frida-tools==14.5.2 
  
  pip install frida==17.6.2
  
  frida和frida-tools对应表https://github.com/thelicato/frida-compatibility-matrix
  
  ## 七、常见说明
  
  1. **frida-server 版本必须与本地 frida 版本一致**，否则会连接失败
  2. 启动 frida-server 的终端必须保持开启，关闭则服务停止
  3. 包名替换为你需要 HOOK 的目标应用包名
  4. Mumu 模拟器端口以「问题诊断」中显示的为准，若不是 16384 请替换
  
  ### 总结
  
  1. 核心流程：**安装依赖 → 部署文件 → 配置模拟器 → ADB 连接 → 启动 Frida 服务端 → HOOK 调试**
  2. 关键要求：Frida 客户端 / 服务端版本一致、模拟器开启 ROOT、端口正确转发
  3. 验证命令：`frida-ps -Ua` 是环境是否搭建成功的核心判断标准

HOOK准备 

使用 frida-il2cpp-bridge https://github.com/vfsfitvnm/frida-il2cpp-bridge

安装 Node.js https://nodejs.org/zh-cn/download



其他Tools

过crc32校验 修改注入 https://github.com/AXiX-official/Unity_CRC32_Bypass