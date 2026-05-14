# 以HBR为例的AWB和USM音视频资源提取技术原理
**声明**：本博客仅供学习交流记录，无任何盈利行为。

**注**：已编写应用程序`HBR_tools`，包含`SoundSort`和`CridUsm`工具，可直接实现资源分类与批量处理。本文主要记录技术原理与实现思路。

<!-- ![image-blog1](.\blog1.png) -->
<!-- ![image-blog2](.\blog2.png) -->

## 目录
- [1. 资源文件概述](#1-资源文件概述)
  - [1.1 资源位置](#11-资源位置)
  - [1.2 文件类型与用途](#12-文件类型与用途)
  - [1.3 文件头查看方法](#13-文件头查看方法)
- [2. 资源文件自动分类](#2-资源文件自动分类)
  - [2.1 分类脚本实现](#21-分类脚本实现)
  - [2.2 使用方法](#22-使用方法)
- [3. AWB/ACB音频文件处理](#3-awbacb音频文件处理)
- [4. USM音视频文件解密与处理](#4-usm音视频文件解密与处理)
  - [4.1 未加密USM文件处理](#41-未加密usm文件处理)
  - [4.2 加密USM文件解密](#42-加密usm文件解密)
    - [4.2.1 工具获取](#421-工具获取)
    - [4.2.2 命令行使用方法](#422-命令行使用方法)
    - [4.2.3 HCA密钥获取方法](#423-hca密钥获取方法)
    - [4.2.4 批量处理工具开发](#424-批量处理工具开发)
  - [4.3 音视频合并](#43-音视频合并)
- [5. 文件名恢复](#5-文件名恢复)
  - [5.1 配置文件说明](#51-配置文件说明)
  - [5.2 哈希计算函数](#52-哈希计算函数)
  - [5.3 增强版分类脚本](#53-增强版分类脚本)
  - [5.4 文件名命名规则](#54-文件名命名规则)
- [6. 工具汇总与扩展](#6-工具汇总与扩展)
  - [6.1 已开发工具](#61-已开发工具)
  - [6.2 扩展方向](#62-扩展方向)
- [写在最后](#写在最后)

---

## 1. 资源文件概述
### 1.1 资源位置
- **Android平台**：音视频资产部分存储在AB包中，需先从AB包提取（本文略过AB包解包过程）
- **PC平台**：所有音频资产统一存放于游戏根目录下的`sound`文件夹

### 1.2 文件类型与用途
HBR游戏中的音视频资源主要分为三类，且文件头均未加密：

| 文件类型 | 文件头（十六进制） | 标识字符串 | 用途说明 |
|---------|-------------------|-----------|---------|
| AWB | `41 46 53 32` | `AFS2` | 原始音频文件（完整语音、音乐） |
| ACB | `40 55 54 46` | `@UTF` | 快进版音频文件（用于快速预览） |
| USM | `43 52 49 44` | `CRID` | 动态卡面、外包制作的播片CG和Live视频<br>（游戏内3D播片为实时演算，存储在AB包中） |

### 1.3 文件头查看方法
可通过以下方式查看文件头：
- IDE内置的十六进制编辑器
- 在线十六进制编辑器：[hexed.it](https://hexed.it/)

---

## 2. 资源文件自动分类
手动逐个查看文件头并分类效率低下，可通过Python脚本实现自动分类。

### 2.1 分类脚本实现
<details>
<summary>点击展开 HBRSoundSort.py 完整代码</summary>

```python
# HBRSoundSort.py
import os
import shutil

# 源文件夹路径
src_folder = 'sounds'
# 目标文件夹路径
awb_folder = 'output/awb'
usm_folder = 'output/usm'
acb_folder = 'output/acb'

# 确保目标文件夹存在
os.makedirs(awb_folder, exist_ok=True)
os.makedirs(usm_folder, exist_ok=True)
os.makedirs(acb_folder, exist_ok=True)

# 遍历源文件夹中的所有文件
for filename in os.listdir(src_folder):
    file_path = os.path.join(src_folder, filename)
    
    # 检查是否为文件（跳过子文件夹）
    if os.path.isfile(file_path):
        # 读取文件的前4个字节作为文件头
        with open(file_path, 'rb') as f:
            file_header = f.read(4)
        
        # 根据文件头判断类型并移动
        if file_header == b'\x41\x46\x53\x32':  # AFS2 -> AWB
            new_filename = f"{filename}.awb"
            new_file_path = os.path.join(awb_folder, new_filename)
            shutil.move(file_path, new_file_path)
            print(f"已移动: {new_filename} -> {awb_folder}")
        
        elif file_header == b'\x43\x52\x49\x44':  # CRID -> USM
            new_filename = f"{filename}.usm"
            new_file_path = os.path.join(usm_folder, new_filename)
            shutil.move(file_path, new_file_path)
            print(f"已移动: {new_filename} -> {usm_folder}")
        
        elif file_header == b'\x40\x55\x54\x46':  # @UTF -> ACB
            new_filename = f"{filename}.acb"
            new_file_path = os.path.join(acb_folder, new_filename)
            shutil.move(file_path, new_file_path)
            print(f"已移动: {new_filename} -> {acb_folder}")
        
        else:
            print(f"未知文件类型: {filename}")
```

</details>

### 2.2 使用方法
1. 确保电脑已安装Python 3.x
2. 新建工作文件夹，将`HBRSoundSort.py`放入其中
3. 在工作文件夹内创建`sound`子文件夹，将游戏`sound`目录下的所有资源复制进去
4. 按下`Win+R`打开运行窗口，输入`cmd`启动命令提示符
5. 切换到工作文件夹，执行命令：
   ```bash
   python HBRSoundSort.py
   ```
6. 等待脚本执行完成，分类后的文件将出现在`output`目录下

---

## 3. AWB/ACB音频文件处理
AWB和ACB文件**未被加密**，仅隐藏了后缀名和文件名，使用专用播放器即可直接播放。

**推荐播放器**：[foobar2000](https://www.foobar2000.org/)（需安装相应的音频解码插件）

---

## 4. USM音视频文件解密与处理
USM是日本动画公司常用的音视频封装格式，HBR中的USM文件均经过加密处理，无法直接播放。

### 4.1 未加密USM文件处理
对于未加密的USM文件，可直接使用[vgmtoolbox](https://sourceforge.net/projects/vgmtoolbox/)进行解码和提取。

### 4.2 加密USM文件解密
HBR使用的USM加密算法未做修改，仅使用了特定的HCA密钥。我们可以使用开源工具`CRID-usm-Decrypter`进行解密。

#### 4.2.1 工具获取
- 项目地址：[kokarare1212/CRID-usm-Decrypter](https://github.com/kokarare1212/CRID-usm-Decrypter)
- 下载地址：[v1.0.0 Release](https://github.com/kokarare1212/CRID-usm-Decrypter/releases/tag/1.0.0)
- 支持x86和x64两个版本

#### 4.2.2 命令行使用方法
<details>
<summary>点击展开 命令行使用示例</summary>

```bash
CRID-umd-Decrypter.exe -a <A> -b <B> -o <Output> <Source>
```
- `<A>`：HCA密钥的**下8位**
- `<B>`：HCA密钥的**上8位**
- `<Output>`：输出文件路径
- `<Source>`：输入USM文件路径

</details>

#### 4.2.3 HCA密钥获取方法
HBR的PC端`global-metadata`和SO文件均已加密，但手机端未加密。可通过以下方式获取密钥：
1. 使用`il2cpp dumper`解析手机端游戏文件
2. 配合IDA和dnSpy定位密钥函数
3. 使用Frida Hook出密钥（需绕过游戏的Frida检测）
4. 也可通过互联网检索获取已公开的HBR密钥

#### 4.2.4 批量处理工具开发
为方便批量处理，可开发GUI工具封装命令行程序，实现以下功能：
1. 支持批量导入USM文件
2. 支持用户自定义输出目录
3. 支持预设和自定义HCA密钥（兼容其他使用相同加密算法的游戏）

<!-- ![image-blog3](.\blog3.png) -->

### 4.3 音视频合并
解密后的USM文件会分离为独立的视频文件和音频文件，可使用FFmpeg进行合并。

<details>
<summary>点击展开 FFmpeg 合并命令示例</summary>

```bash
ffmpeg -i video.h264 -i audio.adx -c:v copy -c:a aac output.mp4
```

</details>

已开发`EasyFFmpeg-HBR`工具，通过GUI封装FFmpeg命令，实现一键批量合并。

---

## 5. 文件名恢复
提取后的资源文件默认显示为哈希值乱码，可通过游戏配置文件恢复原始文件名。

### 5.1 配置文件说明
游戏使用`LilyLotusSoundKeyTable.json`配置文件管理音频资源，文件结构如下：

<details>
<summary>点击展开 配置文件示例</summary>

```json
{
  "cueSheetName": "VSB_IrOhshimaNightlifeBattleUniqueAttackAll01",
  "cueNameList": [
    "VB_Z_907700376_IrOhshima_Short",
    "VB_Z_907700376_IrOhshima_Short_g",
    "VB_Z_907700375_IrOhshima_b_Long",
    "VB_Z_907700376_IrOhshima_c_Short",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_01",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_02",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_03",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_04",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_05",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_06",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_07",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_08",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_09",
    "BS_IrOhshimaNightlifeBattleUniqueAttackAll01_10"
  ],
  "acbAddress": "Sound/VSB_IrOhshimaNightlifeBattleUniqueAttackAll01.acb",
  "acbFileHash": "b30344819e09783409c6377b17058020b5c8d584",
  "acbFileSize": 74784,
  "awbAddress": "Sound/VSB_IrOhshimaNightlifeBattleUniqueAttackAll01.awb",
  "awbFileHash": "445883538e6cc2282be100664a3536b9b1a9a94c",
  "awbFileSize": 363840
}
```

</details>

游戏通过**SHA-1哈希值**匹配文件，因此我们可以计算本地文件的哈希值，与配置文件中的哈希值进行比对，从而恢复原始文件名。

### 5.2 哈希计算函数
<details>
<summary>点击展开 SHA-1 哈希计算函数</summary>

```python
import shutil
import json
import os
import argparse
import compare_json
import soundsort_nomap
import upgrademode
import hashlib
import datetime

def calculate_sha1(file_path):
    """计算文件的SHA-1哈希值（小写40位字符串）"""
    sha1 = hashlib.sha1()
    try:
        with open(file_path, 'rb') as f:
            while chunk := f.read(4096):  # 分块读取大文件
                sha1.update(chunk)
        return sha1.hexdigest()
    except Exception as e:
        print(f"计算哈希失败（{file_path}）：{str(e)}")
        return None
    
def move_files_dict(correct_name, new_file_path,acb_folder, awb_folder, usm_folder):
    # 移动文件到对应的文件夹
    if correct_name.endswith('.acb'):
        final_path = os.path.join(acb_folder, correct_name)
    elif correct_name.endswith('.awb'):
        final_path = os.path.join(awb_folder, correct_name)
        #活动文件夹
        if 'AC' in correct_name:
            if not os.path.exists(os.path.join(awb_folder, 'AC')):
                os.makedirs(os.path.join(awb_folder, 'AC'))
            final_path = os.path.join(awb_folder, 'AC', correct_name)
        #卡面语音文件夹
        if ('VP_' in correct_name) or ('VSB_' in correct_name):
            if not os.path.exists(os.path.join(awb_folder, 'CardVoice')):
                os.makedirs(os.path.join(awb_folder, 'CardVoice'))
            final_path = os.path.join(awb_folder, 'CardVoice', correct_name)
        #背景音乐文件夹    
        if correct_name[:1] == 'S':
            if not os.path.exists(os.path.join(awb_folder, 'BGM')):
                os.makedirs(os.path.join(awb_folder, 'BGM'))
            final_path = os.path.join(awb_folder, 'BGM', correct_name)
    elif correct_name.endswith('.usm.bytes'):
        final_path = os.path.join(usm_folder, correct_name)
    else:
        print(f'未知文件类型: {correct_name}')
    shutil.move(new_file_path,final_path)

def rename_files_hash(hash_map,src_folder, acb_folder, awb_folder, usm_folder):
    for root, dirs, files in os.walk(src_folder):
        for filename in files:
            file_path = os.path.join(root, filename)
            # 计算文件哈希值
            file_hash = calculate_sha1(file_path)
            if file_hash in hash_map:
                #print(f'filename: {hash_map[file_hash]}')
                new_file_path = os.path.join(os.path.dirname(file_path), hash_map[file_hash])
                if os.path.exists(new_file_path):
                    print(f'文件已存在: {new_file_path}')
                    continue
                os.rename(file_path, new_file_path)
                move_files_dict(hash_map[file_hash], new_file_path,acb_folder, awb_folder, usm_folder)
                print(f'已移动并重命名文件: {filename} 到 {hash_map[file_hash]}')
            else:
                print(f'未找到匹配的文件哈希: {filename} (哈希: {file_hash})')

def update_file_name(hash_map,src_folder):
    for root, dirs, files in os.walk(src_folder):
        for filename in files:
            file_path = os.path.join(root, filename)
            # 计算文件哈希值
            file_hash = calculate_sha1(file_path)
            if file_hash in hash_map:
                #print(f'filename: {hash_map[file_hash]}')
                new_file_path = os.path.join(os.path.dirname(file_path), hash_map[file_hash])
                if os.path.exists(new_file_path):
                    print(f'文件已存在: {new_file_path}')
                    continue
                os.rename(file_path, new_file_path)
                print(f'已移动并重命名文件: {filename} 到 {hash_map[file_hash]}')
            else:
                print(f'未找到匹配的文件哈希: {filename} (哈希: {file_hash})')


def main(args):    
    #获得当前日期
    now = datetime.datetime.now()

    src_folder = args.src
    # 目标文件夹路径
    awb_folder = os.path.join(args.output, 'awb')
    usm_folder = os.path.join(args.output, 'usm')
    acb_folder = os.path.join(args.output, 'acb')

    if (args.model == 0 | os.path.exists(args.map)==False):
        print("无字典模式\n")
        # 确保目标文件夹存在
        if not os.path.exists(src_folder):
            print(f'缺少音频文件: {src_folder}')
            exit()
        if not os.path.exists(awb_folder):
            os.makedirs(awb_folder)
        if not os.path.exists(usm_folder):
            os.makedirs(usm_folder)
        if not os.path.exists(acb_folder):
            os.makedirs(acb_folder) 
        soundsort_nomap.soundsort_nomap(src_folder, awb_folder, usm_folder, acb_folder)
        print("文件分类完成\n")
        print('感谢使用HBRSoundSort！这里是KETSUII てへぺりんこ☆\n')
        
    elif args.model == 1:
        print("字典模式\n")
        #print(f'正在测试: {args.map}')
        if not os.path.exists(src_folder):
            print(f'缺少音频文件: {src_folder}')
            exit()
        if not os.path.exists(awb_folder):
            os.makedirs(awb_folder)
        if not os.path.exists(usm_folder):
            os.makedirs(usm_folder)
        if not os.path.exists(acb_folder):
            os.makedirs(acb_folder) 
        hash_map =compare_json.hash_map(args.map)
        rename_files_hash(hash_map, src_folder, acb_folder, awb_folder, usm_folder)
        print("文件重命名和移动完成\n")
        print('感谢使用HBRSoundSort！这里是KETSUII てへぺりんこ☆\n')

    elif args.model == 2:
        print("从无字典模式进行更新\n")
        hash_map =compare_json.hash_map(args.map)
        update_file_name(hash_map, src_folder)
        print("文件重命名完成\n")
        print('感谢使用HBRSoundSort！这里是KETSUII てへぺりんこ☆\n')

    elif args.model == 3:
        print("获得更新的json文件\n")
        #01 运行compar_json.py
        last_folder = 'last_map' 
        for file in os.listdir(last_folder):
            if file.endswith('.json'):
                compare_json.compare_json_files(os.path.join(last_folder, file), args.map,'diff_file.json')
                if not os.path.exists(args.output):
                     os.makedirs(args.output)
                shutil.move('diff_file.json', os.path.join(args.output, 'diff_file.json'))
                print(f'已比较文件: {file}')
        # 删除diff_file.json
        # os.remove(mapping_file)
        diff_file_path = os.path.join(os.getcwd(), 'diff_file.json')
        print(f"diff_file.json生成完成\n路径: {diff_file_path}")
        # 复制liliylotussoundkeytable.json到last_map文件夹 
        shutil.copy(args.map, os.path.join(last_folder, now.strftime('%Y%m%d')+'LilyLotusSoundKeyTable.json'))
    # shutil.move('LilyLotusSoundKeyTable.json', os.path.join(last_folder, 'LilyLotusSoundKeyTable.json'))
        # 打印完成信息

if __name__ == '__main__':
    now = datetime.datetime.now()
    parser = argparse.ArgumentParser("欢迎使用HBRSoundSortV2 这里是KETSUII てへぺりんこ☆\n")
    parser.add_argument('--model',type=int, metavar='M', help='模式(默认1) \t无字典模式0,字典模式1,更新模式2,生成diff_json3')
    parser.add_argument('--src', metavar='D',help='源文件夹路径',default='sounds')
    parser.add_argument('--output',metavar='O', help='目标文件夹路径',default=now.strftime('%Y%m%d')+'_output')
    parser.add_argument('--map',metavar='J',help='字典文件名',default='LilyLotusSoundKeyTable.json')
    args = parser.parse_args()
    main(args)
```

</details>

### 5.3 增强版分类脚本
在基础分类脚本上增加以下功能：
1. 基础分类模式（无配置文件时使用）
2. 字典模式（根据配置文件恢复文件名）
3. 更新模式（对已分类文件进行文件名恢复）

### 5.4 文件名命名规则
HBR的文件名遵循统一的命名规范，可通过前缀快速识别内容类型：

| 前缀 | 内容类型 | 示例 |
|------|---------|------|
| `MC_` | 主线故事语音 | `MC05A_` |
| `AC_` | 活动故事语音 | `AC07_` |
| `SC_` | 剪影故事语音 | `SC01_` |
| `SB_` | 背景音乐 | `SB0260` |
| `SI_` | 歌曲乐器版 | `SI0260` |
| `_Jamaisvu_` | 记忆剧情语音 | `VC_Jamaisvu_ADate_03` |
| `VSB_` | 战斗音效 | `VSB_INatsumeUniqueAttackSingle01_AC47` |
| `VP_` | 卡面/主界面语音 | `VP_ADate_Home_03` |
| `_top` | 战斗大招语音 | `1007606_MKurosawa_top.usm` |
| `_1080p` | 1080P动态卡面 | `ADate06.1080p.usm` |
| `_RegularLive` | 演唱会视频 | `AC47_RegularLive.usm` |

USM文件通常直接使用角色名或活动代号命名。

---

## 6. 工具汇总与扩展
### 6.1 已开发工具
1. **HBRSoundSort**：资源文件自动分类与文件名恢复
2. **CridUsm**：USM文件批量解密GUI工具
3. **EasyFFmpeg-HBR**：音视频批量合并工具

### 6.2 扩展方向
1. 集成AB包解包功能，实现从原始游戏文件到可播放资源的一键处理
2. 增加音频格式转换功能，将AWB/ACB转换为MP3/FLAC等通用格式
3. 开发资源预览功能，在分类前可直接预览音频内容

---

## 写在最后
本文详细介绍了HBR游戏中AWB、ACB和USM音视频资源的提取原理与实现方法。通过文件头识别、自动分类、密钥解密和哈希匹配等技术，可以完整提取并恢复游戏中的音视频资源。

需要注意的是，游戏资源受版权保护，提取的资源仅可用于个人学习和研究目的，不得用于商业用途或非法传播。

---
