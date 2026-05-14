# 游戏文件格式解包对照表
| 文件头 | 文件头原始文本/标志 | 备注 | 解包方法或转换格式方法 |
| ---- | ---- | ---- | ---- |
| 43 50 4B | CPK | 游戏文件打包格式 | 使用garbro、CpkFileBuilder、Noesis等工具解包 |
| 48 43 41 | HCA | criware的音频格式 | foobar2000、格式工厂、ffmpeg |
| 40 55 54 46 | @UTF | criware的音频打包格式，内部包含多个音频 | Foobar2000、vgm工具箱 |
| 41 46 53 32 | AFS2 | criware的音频打包格式，内部包含多个音频 | Foobar2000、vgm工具箱 |
| 43 52 49 44 | CRID | criware的视频格式 | vgm工具箱、ffmpeg，一些加密的usm只能通过民间大神开发的工具解包 |
| 30 26 B2 75 8E 66 CF 11 | 0&瞮巉? | 地雷社游戏的一种视频格式 | 实际是wmv视频，修改后缀名为wmv，FFmpeg可转换mkv、mp4 |
| 55 6E 69 74 79 46 53 | UnityFS | unity大多数游戏常见的文件头 | assetstudio |
| 46 53 42 35. | FSB5 | fmod引擎的音频格式 | assetstudio解包时选择转换为wav、foobar2000 |
| 70 66 38 | pf8 | galgame游戏的一种打包格式 | 使用garbro |
| 42 4B 48 44 | BKHD | wwise引擎的音频打包格式，内部包含多个音频 | foobar2000 |
| 50 41 4D 46 00 34 31 | PAMF0041 | 一种视频格式 | vgm工具箱可提取，没使用ffmpeg测试 |
| 42 49 4B 69 | BIKi | bink视频，rad games tool开发的视频格式 | 格式工厂、ffmpeg |
| 4B 42 32 6A | kb2j | bink视频，rad games tool开发的视频格式 | radvideo和vgm工具箱，海王星系列的游戏经常伪装成usm格式 |
| 4B 42 32 69 | kb2i | bink视频，rad games tool开发的视频格式 | radvideo |
| 4B 42 32 6E | kb2n | bink视频，rad games tool开发的视频格式 | radvideo |
| 50 44 41 | PDA | 一种游戏文件打包格式 | bra音频可尝试用dargon unpacker解包 |
| 57 42 4E 44 | WBND | 地雷社游戏中的音频打包格式，内部包含多个音频 | unxwb提取很快，foobar2000提取极慢，如果不是为了更好的音质不建议用foobar2000 |
| 4D 5A | MZ | windows应用程序执行文件 | 鼠标右键解压 |
| 4D 5A | MZ | windows应用程序扩展文件 | 鼠标右键解压 |
| 58 50 33 | XP3 | galgame游戏常见的打包格式 | 可尝试用garbro解包 |
| 4D 53 43 46 | MSCF | 一种压缩格式 | 鼠标右键解压 |
| 58 57 53 46 49 4C 45 | XWSFILE | 3DS游戏的一种音频打包格式，内部包含多个音频 | 3DS Audio Ripper，提取出bcwav格式的音频 |
| 43 47 46 58 | CGFX | 3DS游戏的一种图片格式 | EveryFileExplorer |
| 43 47 46 58 | CGFX | 3DS游戏的一种模型文件 | EveryFileExplorer |
| 43 53 41 52 | CSAR | 3DS游戏的一种音频打包格式，内部包含多个音频 | 使用3DS SoundArchiveTool或者vgm工具箱的Advanced Cutter/Offset Finder解包 |
| 43 57 41 56 | CWAV | 3DS游戏bcsar文件的音频解包格式 | foobar2000、citric composer |
| 43 53 45 51 | CSEQ | 3DS游戏bcsar文件解包的音频格式 | citric composer可以打开修改参数，可以转换成bfseq |
| 43 57 41 52 | CWAR | 3DS游戏bcsar文件解包的音频格式 | 使用citric composer可以播放和转换成wav,不要使用CubeMediaPlayer2转换 |
| 56 41 47 70 | VAGp | PSP游戏的一种音频格式 | foobar2000、ffmpeg |
| 46 69 6C 65 6E 61 6D 65 | Filename | steam单机游戏的一种pck文件 | 除了音频还存储图片，使用dragon unpacker解包 |
| 41 4B 50 4B | AKPK | 使用wwise引擎打包的音频格式，内部包含多个音频 | dragon unpacker解包，改后缀为bnk或wem使用foobar2000转换，不建议用extractor |
| 不固定 | 不固定 | criware的音频格式 | foobar2000 |
| 50 53 4D 46 00 30 | PSMF00 | PSP游戏的视频专用打包格式 | 使用pmftools转换有声音，有些游戏无法转换，使用ffmpeg转换无声音 |
| AB 4B 54 58 20 31 31 BB | 獽TX 11? | 手游中的一种图片格式 | TexturePackerGui |
| 30 26 B2 75 8E 66 CF 11 | 0&瞮巉? | 一种视频格式 | ffmpeg、格式工厂等 |
| 4F 67 67 53 | OggS | theora编码，开放免费的视频压缩格式 | ffmpeg、格式工厂 |
| 不固定 | 不固定 | galgame游戏的一种打包格式 | winasar可以完全解包、dragon unpacker只能提取图片和音频 |
| 52 50 41 | RPA | galgame游戏的一种打包格式 | garbro可以解包 |
| 43 43 5A | CCZ | 一种图片格式 | TexturePackerGUI |
| 59 50 46 | YPF | galgame游戏的一种打包格式 | 使用garbro解包 |
| 52 47 53 53 41 44 | RGSSAD | 一种2d rpg游戏的打包格式 | garbro可以解包 |
| 不固定 | 不固定 | mpeg1编码的视频格式 | ffmpeg、格式工厂等 |
| 不固定 | ....ftypmp42或者...$ftypM4V | 常见的mp4视频 | ffmpeg、格式工厂等 |
| 00 00 00 18 66 74 79 70 6D 70 34 32 | ....ftypmp42 | 罕见的视频格式 | ffmpeg、格式工厂 |
| 02 00 10 00 44 AC | ....D? | 罕见的音频格式 | 格式工厂和ffmpeg无效，可以用foobar2000转换 |
| 42 55 52 49 48 4F | BURIKO | galgame游戏的一种打包格式 | garbro提取立绘和视频、音频 |
| 89 50 4E 47 | 塒NG | 一种图片格式 | 格式工厂、honeyview、ps、topaz全家桶的降噪、锐化、无损放大 |
| 4E 58 50 4B | NXPK | 网易neox引擎游戏打包的一种格式 | NPKExtractor |
| 49 44 33 | ID3 | cocos2d或者一些卡牌手游用到的音频格式 | 格式工厂、foobar2000、ffmpeg |
| 30 26 B2 75 8E 66 CF 11 | 0&瞮巉? | 一种音频格式 | 格式工厂、ffmpeg |
| 00 00 00 1C 66 74 79 70 4D 34 41 20 | ....ftypM4A | 一种音频格式 | 格式工厂、ffmpeg |
| 不固定 | 不固定 | 一种视频格式 | 格式工厂和ffmpeg |
| 00 00 01 BA | ...? | PS2游戏的视频格式 | vgm工具箱、格式工厂、ffmpeg、cube media player |
| 53 53 68 64 | SShd | PS2游戏PSS视频文件分离的音频格式 | 格式工厂、ffmpeg |
| 不固定 | 不固定 | PS2游戏rom | vgm工具箱解压、鼠标右键解压 |
| 不固定 | 不固定 | PSP游戏rom | 鼠标右键解压 |
| 63 6B 6D 6B | ckmb | 一种音频打包格式，内部包含多个音频 | foobar2000 |
| 4F 67 67 53 | OggS | 一种音频格式 | foobar2000、格式工厂、ffmpeg |
| 4F 67 67 53 | OggS | 一种音频格式 | foobar2000、格式工厂、ffmpeg |
| 52 49 46 46 | RIFF | PSP、PS3游戏的一种音频格式，实际上等同于wav | foobar2000、格式工厂、ffmpeg可以转换，文件头后面是wave |
| 52 49 46 46 | RIFF | 一种音频格式 | foobar2000、格式工厂、ffmpeg可以转换，文件头后面是wave |
| 52 49 46 46 | RIFF | 一种音频打包格式，内部包含多个音频 | foobar2000，文件头后面是wave |
| 52 49 46 46 | RIFF | 一种音频打包格式，内部包含多个音频 | foobar2000，文件头后面是fev |
| 52 49 46 46 | RIFF | 一种视频格式 | 格式工厂、ffmpeg可以转换，文件头后面是avi |
| 52 49 46 46 | RIFF | 一种图片格式 | 格式工厂，文件头后面是webp |
| 52 49 46 46 | RIFF | 一种音频格式 | foobar2000、格式工厂、ffmpeg |
| 52 49 46 46 | RIFF | wwise的音频打包格式 | 其实它就跟pck、bnk差不多，最好使用vgm工具箱，输出格式选择wem |
| 50 4B | PK | 一种可直接解压的PK类文件 | 很常见，电脑使用7z、手机使用MT管理器解压 |
| 50 4B | PK | 一种可直接解压的PK类文件 | 很常见，电脑使用7z、手机使用MT管理器解压 |
| 50 4B | PK | 一种可直接解压的PK类文件 | 很常见，电脑使用7z、手机使用MT管理器解压 |
| 50 4B | PK | 一种可直接解压的PK类文件 | 很常见，电脑使用7z、手机使用MT管理器解压 |
| 50 4B | PK | 一种可直接解压的PK类文件 | 不解包的根本没见过，电脑使用7z、手机使用MT管理器解压 |
| 50 4B | PK | 一种可直接解压的PK类文件 | 虽然没用过苹果手机，但它也是一种pk文件，直接解压就行 |
| 50 4B | PK | 存储Java类文件、相关的元数据和资源的文件 | 鼠标右键解压 |
| 不固定 | 不固定 | 虚幻引擎游戏的打包格式 | 此pak文件或大或小，大的十几二十多G，小的几十kb，fmodel、umodel输入AES密钥解包 |
| 不固定 | 不固定 | 世纪天成csol的资产文件 | 此pak文件较小，把pak拖到cospak上解包 |
| 不固定 | 不固定 | re引擎游戏的打包格式 | 此pak包巨大，30多个G，使用retool解包 |
| 不固定 | 不固定 | 任天堂wiiu平台的一种资产文件 | 此pak文件较小，最大几十mb，最小几十kb |
| 66 69 6C 65 6D 61 72 6B | filemark | 安卓游戏的一种打包格式 | dragon unpacker、extract2.5、vgm工具箱 |
| 不固定 | 不固定 | nds游戏rom | 使用ndstool可解包，使用vgmtrans可直接提取音频 |
| 不固定 | 不固定 | PS游戏的一种文件类型，里面打包了avi格式的视频 | jpsxdec |
| 2E 73 6E 64 | .snd | PS游戏STR文件解包后的一种音频格式 | 格式工厂、foobar2000、ffmpeg |
| 46 4F 52 4D | FORM | PS游戏STR文件解包后的一种音频格式 | 格式工厂、foobar2000、ffmpeg |
| 不固定 | 不固定 | wiiu游戏loadiine格式的一种音频格式 | 有些游戏可用foobar2000播放，格式工厂和ffmpeg无效 |
| 46 53 54 4D | FSTM | wiiu游戏loadiine格式的一种音频格式 | 格式工厂、foobar2000、ffmpeg、citric composer |
| 46 53 41 52 | FSAR | wiiu游戏中的音频打包格式，内部包含多个音频 | BFSAR_Split、Retsuko Sound Tool、Citric Composer |
| 46 57 41 56 | FWAV | wiiu游戏中bfsar文件解包的一种音频格式 | EveryFileExplorer、foobar2000、citric composer转换为wav |
| 46 47 52 50 | FGRP | wiiu游戏中的sarc解包的音频文件 | 删除FWAV前面的字节保存为bfwav，再使用EveryFileExplorer转换为wav格式 |
| 46 53 45 51 | FSEQ | wiiu游戏中的bfsar解包的音频文件 | 删除FWAV前面的字节保存为bfwav，再使用EveryFileExplorer转换为wav格式 |
| 46 57 53 44 | FWSD | wiiu游戏中的bfsar解包的音频文件 | Citric Composer可以播放并转换为wav格式 |
| 46 57 41 52 | FWAR | wiiu游戏中的bfsar解包的音频文件 | Citric Composer可以播放并转换为wav格式 |
| 53 41 52 43 | SARC | wiiu游戏中的一种文件打包格式 | 把sarc文件拖到sarc_tool.exe上即可解包 |
| 不固定 | 不固定 | wiiu游戏中的sarc文件解包的音频格式 | 使用Audacity转换成wav、mp3等格式 |
| 57 55 50 | WUP | wiiu游戏rom的一种打包格式 | Uwizard输入密钥转换成loadiine格式，解完就像pc单机游戏了 |
| 57 55 58 | WUX | wiiu游戏wud格式rom压缩后的一种打包格式 | 未知，一般都是提取loadiine格式的资源 |
| 43 49 53 4F | CISO | ISO镜像转换而来的一种rom格式 | 未知，一般都是提取ISO格式的rom |
| 00 50 42 50 | .PBP | ISO镜像转换而来的一种rom格式 | PS Multi Tools可直接解压PBP |
| 52 61 72 21 | Rar！ | 一种压缩格式 | 鼠标右键解压 |
| 53 53 4E 44 | SSND | 3DS游戏的一种音频打包格式，内部包含多个音频 | 3DS Audio Ripper，提取出bcwav格式的音频 |
| 不固定 | 不固定 | PS2游戏的一种音频格式 | 格式工厂和ffmpeg无效，可以用foobar2000播放和转换 |
| 7F 50 4B 47 | .PKG | PS3游戏的rom格式 | PS3GameExtractor |
| 4D 53 46 43 | MSFC | PS3游戏的一种音频格式 | foobar2000、格式工厂、ffmpeg |
| 1F 8B 08 00 | ..?.. | 一种压缩格式 | 7z右键解压 |
| 44 57 5F 50 41 43 4B | DW_PACK | 地雷社游戏的一种文件打包格式 | NR2_unpacker-PAC |
| 不固定 | 不固定 | ONScripter是一个通用GalGame引擎，rom格式为nsa | garbro可以解包 |
| 不固定 | 不固定 | 世嘉DC游戏rom格式 | 软碟通 |
| 47 42 49 58 | GBIX | 一种图片格式 | TexturePackerGui，逆战的可以正常转换，生化危机3无效 |
| 47 49 46 | GIF | 一种图片格式 | 格式工厂 |
| 41 46 53 | AFS | 一种文件打包格式 | AFSExplorer、SimpleAFSExtractor、vgm工具箱 |
| 00 00 01 BA | ...? | 一种视频格式 | vgm工具箱、格式工厂 |
| 49 44 53 54 | IDST | csol的模型文件 | HLMV1.3.5汉化版、Noesis |
| 4E 41 52 | NAR | csol除了pak之外的一种打包格式 | Nar extractor |
| 41 43 54 52 48 45 41 44 | ACTRHEAD | 虚幻引擎游戏解包提取的一种模型 | fmodel、umodel |
| 57 42 46 53 | WBFS | 任天堂wii平台的rom格式 | WiiBackupManager或者wbfstoiso转换成iso格式 |
| 不固定 | 不固定 | 任天堂wii游戏wbfs格式的rom转换的一种格式 | WIIScrubber只能转换iso格式的wii游戏rom |
| 54 48 50 | THP | 任天堂wii游戏解包后的一种视频格式 | 格式工厂可以转换，vgm工具箱可以分离音频视频 |
| 52 53 44 4D | RSTM | 任天堂wii平台的一种音频格式 | 格式工厂、ffmpeg、foobar2000 |
| 52 53 41 52 | RSAR | 任天堂wii平台的音频打包格式，内部包含多个音频 | vgm工具箱advanced cutter/offset finder的RWAV Streams |
| 52 57 41 56 | RWAV | 任天堂wii平台的brsar文件解包的音频格式 | foobar2000 |
| 55 AA 38 2D | U?-. | 任天堂wii平台的档案文件 | EveryFileExplorer |
| 50 40 44 35 | POD5 | 微软xbox360平台的一种文件打包方式 | dragon unpacker |
| 50 4B | PK | PSV游戏rom的格式 | 7z右键解压 |
| 不固定 | 不固定 | PS1游戏的一种rom格式 | UltraISO |
| 不固定 | 不固定 | PS1游戏bin镜像解包后的一种文件 | jpsxdec、软碟通 |
| 80 00 00 20 | €.. | afs文件里解出的一种音频格式 | foobar2000、格式工厂支持转换，ffmpeg无效 |
| 1F 8B 08 08 | .？.(乱码只显示3位) | 一种压缩格式 | 7z右键解压 |
| 13 AB A1 5C | 乱码 | cocos2d游戏的一种图片格式 | 使用文件多开器打开TexturePackerGui再一个个的转换成png |
| 41 52 43 34 | ARC4 | galgame游戏的一种打包格式 | garbro |
| 4B 49 46 00 | KIF. | galgame游戏的一种打包格式 | 此文件已加密，使用garbro解包 |
| 50 41 43 4B | PACK | galgame游戏的一种打包格式 | p和mus文件的文件头都是PACK，使用garbro解包，所以这种文件不看扩展名 |
| 50 46 53 30 | PFS0 | Switch游戏的rom格式 | NSCB转换成xci或者nsz格式，也可以解包获得nca文件，NSGManager可直接解包 |
| 50 46 53 30 | PFS0 | Switch游戏的rom格式 | NSCB转换成nsp格式，NSGManager可直接解包 |
| 不固定 | 不固定 | Switch游戏的rom格式 | NSCB转换成xci格式，NSGManager可直接解包 |
| 不固定 | 不固定 | Switch游戏的rom格式 | NSCB将nsp或者xcz可转换成xci，NSGManager和hactool可以解包 |
| 不固定 | 不固定 | Switch游戏nsp或xci解包的文件 | NSGManager和hactool可以解包 |
| 54 49 44 | TID | 地雷社游戏pac文件解包的一种图片格式 | 拖到nr2_tidtool上，报毒，小心，建议使用文件多开器批量解包 |
| 4D 50 4B | MPK | 一种游戏打包格式 | garbro可以解包，和网易的mpk不一样，psv版命运石之门的mpk加密了 |
| 65 61 33 | ea3 | pam视频分离的音频格式 | 格式工厂、ffmpeg、foobar2000 |
| 53 44 41 54 | SDAT | nds游戏的音频打包格式，内部包含多个音频 | VG MusicStudio或者vgmtrans解包 |
| 53 54 52 4D | STRM | 使用vgm工具箱解包sdat获得的一种音频格式 | foobar2000 |
| 53 57 41 56 | SWAV | 使用vgm工具箱解包sdat获得的一种音频格式 | foobar2000 |
| 4D 41 44 50 | MADP | 3DS游戏的一种音频格式 | foobar2000、格式工厂、ffmpeg |
| 02 00 00 00 01 00 00 01 | ........ | 一种音频格式 | foobar2000 |
| 4D 54 68 64 | MThd | nds、gba游戏解包的一种音频格式 | vgmtrans解包nds，使用foobar2000转换 |
| 不固定 | 不固定 | PS1的一种打包格式 | jpsxdec，有些游戏可提取出音频，有些游戏无法提取 |
| 70 42 41 56 | pBAV | PS1的一种音频打包格式 | foobar2000 |
| 42 57 41 56 | BWAV | switch游戏的一种音频格式 | foobar2000 |
| 4B 54 53 52 | KTSR | 任天堂的一种音频打包格式 | 使用vgm工具箱的Advanced Cutter/Offset Finder的KTSS file解包 |
| 4B 54 53 52 | KTSR | 任天堂的一种音频打包格式或ktsl2stbin的索引文件 | foobar2000可以直接播放并转换 |
| 4B 54 53 53 | KTSS | 任天堂ktsl2stbin文件解包的音频格式 | foobar2000 |
| 4D 49 43 4F 43 47 30 31 | MICOCG01 | 一种未知的图片格式 | garbro将其转换成png |
| C1 83 2A 9E | 乱码 | 虚幻引擎游戏的资产文件或索引文件 | fmodel或者umodel |