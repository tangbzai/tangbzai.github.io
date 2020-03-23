---
title: SQL语言
date: 2019-10-28 13:18:04
tags: 软件设计师
---
# SQL语言
## 建表
```
CREATE TABLE <表名1> (<列名><数据类型>[列级完整性约束条件]
    [,<列名><数据库类型>[列级完整性约束条件]]...
    [,<表现级完整性约束条件>]);
```
数据类型|说明
---|---
char(N)|字符型
int    |整形
float  |浮点型
date YYYY-MM-DD|日期型

完整性约束条件|说明
---|---
NULL | 可以取空值
NOT NULL | 不能取空值
UNIQUE | 取值唯一
PRIMARY KEY (列名) | 设置为主键
FOREIGN KEY (列名1) PEFERENCES 表名2 (列名2) | 设置(列1)为外键且索引(表2)的(列2)

## 修改与删除表
- 修改
```
ALTER TABLE <表名>
[ADD <新列名><数据类型>[列级完整性约束条件]]
[DROP<列名/完整性约束名>]
[MODIFY/CHANGE<列名><数据类型>]
```
- 删除
```
DROP TABLE <表名>
```
## 查询
```
SELECT [ALL | DISTINCT] <目标表达式> [, <目标表达式>]...]
FROM <表名> [, <表名>]...
[WHERE <条件表达式>]
[GRORP BY <列名1> [HAVING<条件表达式>]]
[ORDER BY <列名2> [ASC | DESC]...]
```
<html>
<table>
<th>处理类型
</th><th>处理子类</th><th>示例/语法</th>
<tr><td>结果排序</td><td>升序或降序</td><td>ORDER BY 字段名 DESC\|ASC</td></tr>
<tr><td rowspan=5>集函数</td><td>统计</td><td>COUNT([DISTINCT|ALL]<列名>)</td></tr>
<tr><td>一列值的总和</td><td>SUM([DISTINCT|ALL]<列名>)</td></tr>
<tr><td>一列值的平均值</td><td>AVG([DISTINCT|ALL]<列名>)</td></tr>
<tr><td>求一列值中的最大值</td><td>MAX([DISTINCT|ALL]<列名>)</td></tr>
<tr><td>求一列值中的最小值</td><td>MIN([DISTINCT|ALL]<列名>)</td></tr>
<tr><td>对结果分组</td><td>将查询结果按列分组</td><td>GROUP BY <列名></td></tr>
<tr><td>对分组结果筛选</td><td>对分组结果筛选</td><td>HAVING <条件列达式></td></tr>
<tr></tr>
</table>
</html>
