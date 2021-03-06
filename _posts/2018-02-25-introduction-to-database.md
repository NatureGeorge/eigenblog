---
title: Introduction to DataBase
author: Zefeng Zhu
date: 2018-02-25 08:30:00 +0800
categories: [Notes, Courses]
tags: [database, rdbms]
---

## 数据库的产生与发展

* 人工管理阶段
  * 20世纪50年代中期以前
  * 计算机用于科学计算
  * 数据不保存
  * 数据需要由应用程序自己管理 编写程序需要考虑数据结构
  * 数据面向程序 不共享
  * 数据不具有独立性 随其逻辑结构和物理结构而变化
* 文件系统阶段
  * 20世纪50年代后期-60年代中期
  * 计算机应用从科学技术扩大到管理
  * 数据以文件形式长期保存
  * 形式多样化
  * 数据的物理结构与逻辑结构有了区别
  * 数据共享性差
  * 程序与数据间有一定独立性
* 数据库系统阶段
  * 20世纪60年代后期以来
  * 计算机用于管理的规模更为扩大 数据量急剧增长
  * 数据结构化
  * 数据共享
  * 可控数据冗余度
  * 数据独立性高
  * 统一数据控制功能

### 数据库发展中的三个标志性事件

* IMS
* DBTG报告
* E.F.Codd发表 _大型共享数据库数据的关系模型_ 论文

### 数据库学科的研究领域

* 数据库理论
* 数据库管理系统软件的研制
* 数据库设计

## 相关概念

* 数据 (data)
  * 描述现实世界的各种信息的符号记录
  * 是信息的载体
  * 是信息的具体表现形式
    * 数字
    * 文字
    * 图形
    * 图像
    * 声音
* 数据处理
  * 利用计算机对各种形式的数据进行处理
  * 从大量原始数据中抽取有价值的信息，作为行为和决策的依据
  * 通常包括以下数据操作
    * 采集和整理
    * 编码
    * 输入
    * 存储
    * 加工/计算
    * 分类
    * 检索
    * 传输
    * 输出
* 数据库 (Data Base, DB)
  * 长期存储在计算机中的、有组织的、可共享的数据集合
  * 特点
    * 按照一定的数据模型组织、描述和存储
    * 具有较小的冗余度
    * 具有较高的数据独立性和易扩展性
    * 可为各种用户共享
* 数据库管理系统 (Data Base Management System, DBMS)
  * 是数据管理软件
  * 用于建立、运用和维护数据
  * 位于用户和操作系统之间
* 数据库系统 (Data Base System, DBS)
  * 是指在计算机系统中引入数据库后的系统而构成
  * 包括
    * DB
    * DBMS+OS
    * Users
    * DBA
    * 应用系统
* 数据模型
  * 对数据模型的要求
    * 较真实地模拟现实世界
    * 容易为人所理解
    * 便于在计算机上实现
  * 数据模型的三要素
    * 数据结构
    * 数据操作
    * 数据的约束条件
  * 对象的抽象过程
    * 现实世界->信息世界->计算机世界
    * 客观对象->概念模型(第一级抽象)->数据模型(第二级抽象)
* 概念模型
  * 信息世界中的基本概念
    * 实体 Entity
    * 实体集 Entity Set
    * 属性 Attribute (Type+Value)
    * 实体型 Entity Type (实体名+属性名, e.g. 学生(学号,姓名,性别))
    * 码 Key
    * 域 Domain
    * 联系 Relationship
  * 概念模型的表示方法
    * Entity-Relationship (E-R)
      * Entity Type: 矩形+实体名
      * Attribute: 椭圆形 无向边与实体连接
      * Reletionship: 菱形+联系名 无向边与实体连接 边上标注联系类型
* 数据模型
  * 层次模型 (Hierarchical Model)
    * e.g IBM的`IMS`
    * 数据结构: 树型结构 (一对多)
    * 多对多联系被转换为一对多，非树型结构先转化成树型结构
    * 操纵：查询 插入 删除 更新
    * 完整性约束：不能插入无双亲的子节点 子节点与双亲一起删除更新操纵要保证数据的一致性
    * 存储结构：邻接法 链接法
    * 优点：数据模型简单 若实体间关系固定则性能优于关系模型 具有良好的完整性支持
    * 缺点：描述现实世界的非层次关系很笨拙 插入和删除操纵限制较多 必须通过双亲才能找到子节点 由于结构严密而使层次命令趋于程序化
  * 网状模型 (Network Model)
    * e.g CODASYL的`DBTG`
    * 数据结构：网状结构
    * 允许多个节点无双亲 允许节点有多个双亲 允许节点间有多个联系
    * 操纵：查询 插入 删除 更新
    * 存储结构：链接法
    * 优点：能够直接描述现实世界 存储效率较高
    * 缺点：数据描述语言极其复杂 数据独立性差
  * 关系模型 (Relational Model)
    * e.g `Oracle`,`Sybase`,`VFP`,`Access`
    * 数据结构: 关系模型 (规范的二维表)
    * 操纵：查询 插入 删除 更新
    * 完整性约束：实体完整性 参照完整性 用户定义的完整性
    * 存储结构：以文件形式存储表
    * 优点：有严格的数学概念作基础 关系模型的概念单一 存取路径对用户透明
    * 缺点：查询效率不高
    * 相关概念
      * 关系：整个二维表
      * 关系名：表格名称
      * 元组：行数据 (记录Record/Node/Vertex/Data Element)
      * 属性：列数据 (字段, 整列?)
      * 主码 (关键字)
      * 域
      * 分量：元组中的一个属性值 (Data Item?)
      * 关系模式：关系名(属性...)

### 数据库系统结构

* DBMS角度：三级模式结构
  * 外模式
    * 也称为用户模式 子模式
    * 外部级 用户级 外层 用户层 外视图 个别用户视图
    * 是数据库用户看见和使用的局部数据的逻辑结构和特性的描述
    * 数据库用户的数据视图 描述数据的局部逻辑结构 模式的子集
    * 不同用户有不同的外模式 用户通过外模式访问数据库 是保证数据库安全的有力措施
  * 模式
    * 逻辑模式 是数据库中全体数据的逻辑结构和特性的描述 是所有用户的公共数据视图
    * 概念层、用户共同视图、概念视图
    * 所有个别用户视图综合起来的用户共同视图
    * 描述数据的全局逻辑结构
    * 只有一个模式
  * 内模式
    * 存储模式 内层 内视图 存储视图
    * 与实际存储数据方式有关的层
    * 数据的物理结构和存储结构的描述 数据在数据库内部的表示方式 描述数据的物理存储结构
    * 存储方式、索引、压缩加密
    * 只有一个内模式
* 最终用户角度：单用户结构 主从式结构 分布式结构 客户/服务器结构

#### 数据库的二级映象功能与数据独立性

* 外模式/模式的映象
* 模式/内模式的映象

#### 三级结构带来的优点

* 保证数据的独立性
* 简化了用户的使用
* 有利于数据的共享
* 有利于数据的安全操作

### Main Function of DBMS

1. 数据库定义功能
2. 数据操纵功能
   * 数据查询功能(不改变数据值，只找出符合要求的数据)
   * 数据更新操作(增删改)
3. 数据库运行管理功能
4. 数据库建立和维护功能

## 关系数据库及关系系统

### 关系数据库的数学方法基础

* 1962年CODASYL发表的“信息代数”。
* 1968年David Child在7090机上实现的“集合论的数据结构”（Set-theoretic data structure）
* 1970年美国IBM的E.F.Codd连续发表多篇论文, 系统而严格地提出了关系模型，从此越来越多的关系型数据库系统被研制

### 关系数据库

* 关系数据库系统RDBS：支持关系模型的数据库系统
* 关系模型：关系数据结构 关系操作集合 完整性约束

### 关系数据结构

见上

#### 关系

* Domain是值的集合
  * 一组具有相同数据类型的值的集合
  * 在关系中用域表示属性的取值范围
  * 基数：域中所包含的值的个数，用m表示
* 笛卡尔积 (Cartesian Product)
  * $D_1\times D_2\times \ldots\times D_n=\left\{(d_1,d_2,\ldots,d_n)\right\}$
  * $m=\prod m_{i}$ 即所有域的基数的乘积
* $D_1\times D_2\times \ldots\times D_n$的子集叫做在域$(D_1,D_2,\ldots,D_n)$上的关系，用$R(D_1,D_2,\ldots,D_n)$表示

#### 码

* 码：唯一标识实体的属性集
* 若关系中的某一属性组的值能唯一地标识一个元组，而其真子集不行，则称该属性组为候选码(candidate key)
* 若一个关系有多个候选码，则选择一个为主码(primary key)
* 当所有属性组是这个关系模式的候选码，我们称为全码(all-key)

#### 关系的类型

* 基本关系 基本表 基表
  * 实际存储数据的逻辑表示
* 查询表
* 视图表

#### 基本关系的性质

1. 列是同质的：同一属性名下的诸属性值是同类型数据，且必须来自同一个域。
2. 属性必须有不同的属性名，不同的属性可来自同一个域。
3. 属性的顺序是非排序的：列的次序无所谓，可以随意交换。
4. 元组是唯一的：任意两个元组不能完全相同。
5. 元组的顺序无关紧要：元组的次序可以任意交换。
6. 所有的属性值都是原子的：每一个分量必须是不可分的数据项

#### 规范化

> 关系模型要求关系必须是规范化的

#### 关系模式

* 关系的描述
* 应该包括
  * 关系名[R]
  * 诸属性名[U]
  * 诸属性的取值范围[D]
  * 属性向域的映象[DOM]
  * 属性间数据的依赖关系[F]
* 关系模式可以形象化地表示为
  * $R(U, D, DOM, F)$
  * U 为该关系的属性名集合
  * 通常可以简记为：$R(U)$或$R(A_1,A_2,\ldots,A_n)$
  * 关系实际上是关系模式(型)在某一时刻的状态或内容，是值

#### 关系数据库

* 在一个给定的现实世界领域中，相应于所有实体及实体之间的联系的关系的集合构成一个关系数据库
* 也有型和值之分
* 关系数据库模式和关系数据库通常统称为关系数据库