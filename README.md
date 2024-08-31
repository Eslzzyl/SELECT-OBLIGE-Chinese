# SELECT OBLIGE GPT-4o-mini 机翻汉化

## 当前状态

初翻已完成，正在润色；
- 共通线已润色完成
- 个人线计划顺序为黄毛->粉毛->妹妹->会长

## 使用方法

下载本仓库的代码（右上角 Code - Download Zip），然后将 `font` 和 `script` 文件夹拷贝到游戏根目录中，正常启动游戏即可。

> 提示：本仓库是个人学习使用，目前该作已有多个大模型机翻版本（如 [这个](https://github.com/tangqz/select-oblige-zh-machine-translation) 或 [这个](https://2dfan.com/downloads/25546)），它们的翻译质量可能比本仓库更好。

> 本仓库仅对游戏中的对话进行了汉化，UI 部分完全没有汉化。如果你希望能够汉化 UI，可以尝试下载 [这个](https://tieba.baidu.com/p/9094923400) 体验版汉化补丁，将该补丁提供的 `.pfs` 文件的文件名（不含扩展名）改成与游戏的 `exe` 文件名一致，并放置在游戏根目录，然后将本仓库的 `script` 重命名为 `script_CHS` 后放置在游戏根目录，然后启动游戏。此时无需再放置 `font` 文件夹。

> 如果你熟悉 Git 和 GitHub，当然也可以使用 `git clone`，这样在更新代码时可以减少下载的数据量。

## 基本情况

使用 4 个 API Token 并发调用 OpenAI GPT-4o-mini 对除 H 以外的部分进行初翻，H 部分使用 [SakuraLLM-GalTransl-v1.5](https://huggingface.co/SakuraLLM/GalTransl-7B-v1.5) 在本地进行初翻，并发量为 2。

翻译使用的是由 [GalTransl](https://github.com/XD2333/GalTransl) 提供的工具链。

## 已知问题

- 目前疑似由于 [VNTranslationTools](https://github.com/arcusmaximus/VNTranslationTools) 的 bug，注入后的脚本会缺失一部分 `{"rt2"}` 字符串，这会导致一些对话中的人名无法正常显示，但对话正文是没有问题的。
- H 部分因 SakuraLLM-GalTransl-v1.5 表现不及 GPT-4o-mini，可能存在较多日文残留、顺序错乱、人称错误、翻译代码残留问题。

## 贡献指南

首先 Fork 并克隆仓库。

目录说明：
- `font`：存储固定的字体，无需更改。
- `json_cn_gpt4omini`：由 GPT-4o-mini 完成初翻的 JSON 对话，也是本仓库的主要汉化版本。
  - 注意：该目录中的 H 部分直接复制自 `json_cn_sakura` 目录。
- `json_cn_sakura`：**供参考的**、由 SakuraLLM-GalTransl-v1.5 完成初翻的 JSON 对话。该版本的翻译质量明显不如 GPT-4o-mini，有较多残留问题。但该版本可能在语言表达上会更“二次元”一些。
- `json_jp`：供参考的原日文 JSON 对话。
- `script`：导出的汉化脚本。
- `script_jp`：供参考的原日文脚本。
- `tools`：实用工具。
- `export_script.cmd`：用于导出汉化脚本的 cmd 脚本。
- `pack_script.cmd`：用于打包汉化脚本的 cmd 脚本。
- `README.md`：本文件。

润色时，修改 `json_cn_gpt4omini` 中的 JSON 文件，修改后执行一遍 `export_script.cmd` 刷新 `script` 目录，然后发起 Pull Request。

## 经验总结

- GalTransl 是一个很方便的工具，极大地简化了 Galgame 的大模型机翻流程。使用时需要注意：
  - 写好 GPT 字典，GPT 字典用于向模型提供角色的姓名和设定等信息，可以有效保证人名的统一和人称的稳定；译后字典能够进行必要的人名替换，在开启 `name` 字段替换后可以完成对话框的人名翻译。在编写字典之前可以先查看与作品相关的百科条目或游玩体验版游戏来了解基本设定。
  - 对于能力较强的模型（如 GPT-4 家族），可以适当调大每次请求发送的对话数量。此外并发量可以根据能力调高。翻译过程可以随时打断，GalTransl工具的缓存功能可以保留翻译记录，确保不会重复请求。
    - 供参考：本项目使用 GPT-4o-mini 完成除 H 外的初翻，每次发送 10 句对话，实际消耗额度约 $1。
- 7B 大小的 SakuraLLM-GalTransl-v1.5 模型的翻译能力不及我的预期。如果希望保证翻译质量，尽量使用能力较强的闭源模型。
- 根据翻译结果来看，GPT-4o-mini 的问题主要集中在人称和性别的区分上。 

## 特别鸣谢

- [GalTransl](https://github.com/XD2333/GalTransl)
- [SakuraLLM](https://github.com/SakuraLLM/SakuraLLM)
- [VNTranslationTools](https://github.com/arcusmaximus/VNTranslationTools)
- Artemis 汉化教程： [1](https://www.bilibili.com/read/cv13105700/)、[2](https://hasen.hedgehog-qd.xyz/?p=106)
