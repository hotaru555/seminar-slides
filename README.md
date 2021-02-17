# seminar-slides 使用指南
Author: Wenxuan SHI

## seminar-slides Markdown 编写规范

### 空行

所有**非亲密**元素之间留有空行。具体规则如下：

- 分割符与上下文本内容之间留有空行
- 标题与内容之间留有空行
- 刻意换行处留一个空行

亲密性例外：
- 无序列表、有序列表之间不应留空行。
- 需要保持连续的内容之间不应留空行。

```markdown
this is the last line of the page, content ends.

---

## A Title

This is the line 1 in the page, aba, aba.

This is the line 2 in the page, bala, bala,
and this is also the line 2 in the page,
and this is still the line 2 in the page.

- My point 1
- My point 2
```

### 标题符号

- 使用`#`作为section分割符，使用`##`作为页面标题，使用`###`作为block分割符。
- `#` (section) 后必须有 `##`，不能紧跟内容。

```markdown
This is the last line of the page, content ends.

---

# Section name

## A title

This is content.

---

## B title

This is content.

### A block title

Foo Bar

### B block title

Dong Dua
```

---

## 🤷🏻 新的一周我该做什么
- git pull
- 直接修改 `slide.md` 文件。
- 如需要增加图片，增加在根目录即可。
- git pull，解决冲突
- git push

## 🖐 手动完成的工作
- 按需清除上周无用的图片。
- 每周定稿后给最后一个提交打 tag。
- 每周定稿后发布一个 release。

## 🖥️ 自动完成的工作
- 每次 git push，都会尝试编译 PDF 文件到 `output` 文件夹中。

## ⛔️ 不要做！
- 不要修改 Markdown 的文件名。
- 不要修改 Markdown 头部控制编译的 metadata (yaml header)。
- 不要删除 `.github`文件夹（这是一个隐藏文件夹）中的任何内容。
- 不要修改 `.gitignore`，因为这将影响自动化。

## 📢 还请注意
- pandoc 不支持 Marp 的一些高级用法，比如 *image syntax*