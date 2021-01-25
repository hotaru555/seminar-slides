# seminar-slides 使用说明

## 🤷🏻 新的一周我该做什么
- 直接修改上周的 Markdown 文件。
- 如需要增加图片，增加在根目录即可。

## 🖐 手动完成的工作
- 按需清除无用图片。
- 每周定稿后给最后一个提交打 tag。

## 🖥️ 自动完成的工作
- 每次 git push，都会尝试编译 PDF 文件到 `output` 文件夹中。

## ⛔️ 不要做！
- 不要修改 Markdown 的文件名。
- 不要修改 Markdown 头部控制编译的 metadata (yaml header)。
- 不要删除 `.github`文件夹（这是一个隐藏文件夹）中的任何内容。
- 不要修改 `.gitignore`，因为这将影响自动化。
- 不需要在本地编译 PDF，因为它会被自动覆盖。

## 📢 还请注意
- pandoc 不支持 Marp 的一些高级用法，比如 *image syntax*

## 🎉️ 就这些啦
Author: Wenxuan SHI
