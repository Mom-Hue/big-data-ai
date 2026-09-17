# 12 第三方库与jieba分词

> 所属课次：阶段一 · 第 4 次课 ｜ 前置概念：模块与标准库、文件读写与编码 ｜ 后续概念：NLP与文本预处理

## 一句话理解

`pip` 安装别人分享的库，`jieba` 能把中文句子切成一个个词。

## 为什么需要它

标准库再全也覆盖不了一切。比如中文分词——Python 自己并不知道"我爱新闻学"该怎么切。用 `pip install jieba` 装一个第三方库，一行代码就能把句子切成"我 / 爱 / 新闻学"。装上别人写好的库，等于站在别人肩膀上干活。

jieba 还有"精确模式""全模式"等不同切法，初学先用 `lcut` 的默认精确模式即可。

## 核心语法

```python
# 第一步：在命令行（终端）里执行  pip install jieba
# 第二步：导入并分词
import jieba

text = "我是一名新闻学专业的学生"
words = jieba.lcut(text)   # lcut 返回一个词的列表
print(words)               # ['我', '是', '一名', '新闻学', '专业', '的', '学生']

# 统计切出了几个词
print(len(words))
```

装库与用库三步：

| 步骤 | 命令 / 代码 |
|------|-------------|
| 1. 安装 | `pip install jieba` |
| 2. 导入 | `import jieba` |
| 3. 使用 | `jieba.lcut(文本)` |

## 常见错误

1. **没装就导入**：没执行 `pip install jieba` 就 `import jieba` → 报错 `ModuleNotFoundError`。先装再用。
2. **分完词忘了拼接**：分词得到的是列表，要还原成带空格的句子用 `" ".join(words)`。
3. **装错 Python 环境**：电脑里有多个 Python，库装到了另一个环境里。确认当前用的是同一个 Python。

## 动手小练习

**题**：用 jieba 给一句新闻标题"全国多地迎来开学第一课"分词，打印词列表和词数。

**参考答案**：

```python
import jieba

title = "全国多地迎来开学第一课"
words = jieba.lcut(title)
print(words)
print(len(words))
```

## 与你专业的连接

中文分词是文本分析的第一道工序：不切词就没法数词频、没法做情感分析。下一阶段"文本预处理"就是"清洗 → 分词 → 去停用词"三步走，而分词这一步靠的就是 jieba。
