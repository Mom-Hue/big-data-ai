# 11 CSV与pandas

> 所属课次：阶段一 · 第 4 次课 ｜ 前置概念：文件读写与编码 ｜ 后续概念：AI与机器学习基础

## 一句话理解

CSV 是通用表格文件，`pandas` 是处理表格数据的利器。

## 为什么需要它

舆情数据、调查问卷结果，大多是一张表：一行一条记录，一列一个字段（如"标题、字数、点赞数"）。CSV 就是这种表格的通用格式，用记事本或 Excel 都能打开。`pandas` 能像操作 Excel 一样读表、筛选、算平均值、分组统计，是数据分析的标配。

## 核心语法

```python
import pandas as pd   # 约定俗成把 pandas 简写成 pd

# 造一张小表：两列（标题、字数）
df = pd.DataFrame({
    "标题": ["我市今日晴", "开学第一课", "地铁新线开通"],
    "字数": [1200, 800, 2600]
})

# 存成 CSV 文件（index=False 不写入行号）
df.to_csv("数据.csv", index=False, encoding="utf-8")

# 再读回来
df2 = pd.read_csv("数据.csv", encoding="utf-8")
print(df2)

# 统计
print(df2["字数"].mean())          # 平均字数
print(df2[df2["字数"] > 1000])     # 筛出字数大于 1000 的行
```

常用操作：

| 写法 | 作用 |
|------|------|
| `pd.read_csv(文件)` | 读 CSV |
| `df.head()` | 看前几行 |
| `df[列名].mean()` | 求某列平均值 |
| `df[条件]` | 按条件筛选行 |
| `df.groupby(列)` | 分组统计 |

## 常见错误

1. **忘了 `import pandas`**：直接用 `pd` → 报错 `NameError`。要先装库再导入。
2. **列名写错**：`df["字"]` 但列其实叫"字数" → 报错 `KeyError`。
3. **读文件路径或编码不对**：`read_csv` 找不到文件或中文乱码，先检查路径和 `encoding`。

## 动手小练习

**题**：用 pandas 造一个三行两列的表格（列名：城市、气温），存成 CSV 再读回来，并打印"气温"这一列的平均值。

**参考答案**：

```python
import pandas as pd

df = pd.DataFrame({
    "城市": ["北京", "上海", "广州"],
    "气温": [25, 28, 30]
})
df.to_csv("天气.csv", index=False, encoding="utf-8")

df2 = pd.read_csv("天气.csv", encoding="utf-8")
print(df2["气温"].mean())   # 27.666...
```

## 与你专业的连接

做数据新闻、舆情分析时，你拿到的数据十有八九是 CSV。`pandas` 帮你算均值、做筛选、完成画图前的数据整理，是通往阶段二"AI 与机器学习基础"的必经之路。
