以下是对《人工智能基础》课程中“分层规划（Hierarchical Planning）”三讲内容（Lecture 31–33）的**超详细学习笔记**。内容涵盖所有知识点，逐条展开，并配有例子帮助理解，适合复习、备考或深入掌握分层规划的核心思想与算法。

---

# 🧠 分层规划（Hierarchical Planning）学习笔记  
> 来源：Lecture 31–33（2025年10月，University of Aberdeen）

---

## 📘 第一部分：分层规划基础（Lecture 31）

---

### 1.1 为什么需要分层规划？

#### ✅ 知识点：组合爆炸问题
- **传统规划（如STRIPS）**是领域无关的，但面临**状态空间爆炸**问题。
- **例子**：旅行规划问题中，若不加限制，系统会尝试所有交通方式（步行、骑车、飞机、火车等）的组合，导致搜索空间巨大。

#### ✅ 知识点：控制知识（Control Knowledge）
- 引入**领域知识**来指导搜索，避免无意义的扩展。
- **例子**：人类不会考虑“从英国走到中国”，而是直接考虑“坐飞机”。

---

### 1.2 抽象搜索框架（Abstract Search Procedure）

#### ✅ 知识点：通用搜索框架
所有规划算法可抽象为以下步骤：

```text
function AbstractSearch(u)
    if Terminal(u) then return u
    u ← Refine(u)           // 精炼（如启发式计算）
    B ← Branch(u)           // 分支（生成后继）
    B' ← Prune(B)           // 剪枝（去掉无效节点）
    if B' = ∅ then return failure
    非确定性地选择 ν ∈ B'
    return AbstractSearch(ν)
```

#### ✅ 知识点：启发式搜索规划
- Refine：计算启发式值 h(u)
- Branch：应用可用动作
- Prune：剪去 h(u) = ∞ 的节点

---

### 1.3 领域知识的作用

#### ✅ 知识点：领域知识 vs 领域无关
- 领域无关规划器：通用但慢
- 领域知识：可大幅加速搜索，但需手动编码

#### ✅ 知识点：领域可配置规划（Domain-Configurable Planning）
- 核心思想：将**搜索引擎**与**领域知识**分离
- **例子**：旅行问题中，定义“长途旅行用飞机，短途用出租车”作为规则，避免系统尝试“从伦敦打车到巴黎”。

---

### 1.4 分层任务网络（HTN）规划

#### ✅ 知识点：HTN 的核心思想
- 用**任务**代替目标
- 用**方法（methods）**描述如何分解任务为子任务
- 用**操作符（operators）**执行原子任务

#### ✅ 知识点：方法（Method）结构
一个方法包括：
- 名称
- 任务（非原子）
- 前提条件
- 子任务序列（全序或偏序）

#### ✅ 例子：旅行任务分解
```lisp
Task: travel(x, y)
Method: air-travel(x, y)
Precondition: long-distance(x, y)
Subtasks:
  - get-ticket(a(x), a(y))
  - travel(x, a(x))
  - fly(a(x), a(y))
  - travel(a(y), y)
```

---

### 1.5 简单任务网络（STN）规划

#### ✅ 知识点：STN 的特点
- 原子任务 = 操作符
- 非原子任务 = 需要分解的任务
- 方法定义如何分解任务

#### ✅ 知识点：全序 vs 偏序方法
- **全序方法**：子任务必须按顺序执行
- **偏序方法**：子任务可并发或交错执行（更灵活）

---

## 📘 第二部分：HTN 算法与实现（Lecture 32）

---

### 2.1 全序前向分解算法（TFD）

#### ✅ 知识点：TFD 算法流程
```lisp
function TFD(s, ⟨t1,...,tk⟩, O, M)
    if k = 0 then return ⟨⟩
    if t1 is primitive then
        匹配操作符 a ∈ O，满足前提
        执行 a，递归处理剩余任务
    else
        匹配方法 m ∈ M，满足前提
        用 m 的子任务替换 t1
        递归处理新任务列表
```

#### ✅ 例子：旅行问题
- 初始任务：`travel(porto-alegre, sao-paulo)`
- 方法匹配：`air-travel`
- 子任务替换为：
  - `get-ticket(poa, gru)`
  - `travel(porto-alegre, poa)`
  - `fly(poa, gru)`
  - `travel(gru, sao-paulo)`

---

### 2.2 HDDL 表示法

#### ✅ 知识点：HDDL = HTN 的 PDDL 扩展
- 支持 `:method` 定义
- 支持 `:ordered-subtasks` 和 `:subtasks`

#### ✅ 例子：HDDL 方法定义
```lisp
(:method travel-by-plane
  :parameters (?x ?y ?ax ?ay)
  :task (travel ?x ?y)
  :precondition (and (long-distance ?x ?y) ...)
  :subtasks (and
    (task0 (get-ticket ?ax ?ay))
    (task1 (travel ?x ?ax))
    (task2 (fly ?ax ?ay))
    (task3 (travel ?ay ?y)))
  :ordering (and (task0 < task2) (task1 < task2) (task2 < task3)))
```

---

### 2.3 偏序前向分解算法（PFD）

#### ✅ 知识点：PFD 与 TFD 的区别
- PFD 允许子任务**交错执行**
- 使用**任务节点集合**和**偏序关系**
- 更复杂但更灵活

#### ✅ 例子：搬运两个包裹
- TFD 必须一次性搬完一个再搬另一个
- PFD 可以中途切换任务（如先走到 A，拿包裹，再走到 B，拿另一个）

---

### 2.4 STN 与经典规划的表达能力对比

#### ✅ 知识点：STN 更强大
- 任何 STRIPS 问题都可多项式转换为 STN
- 但某些 STN 问题**无法用经典规划表达**
- **例子**：语言 `{a^n b^n | n > 0}` 可用 STN 描述，但无法用 STRIPS 描述

---

## 📘 第三部分：分层目标网络与高级主题（Lecture 33）

---

### 3.1 分层目标网络（HGN）

#### ✅ 知识点：HGN vs HTN
- HTN：任务是“做什么”
- HGN：目标是“实现什么状态”
- 方法分解的是**目标**，不是任务

#### ✅ 例子：物流运输
```lisp
Head: (move-between-cities ?o ?l1 ?c1 ?l2 ?c2 ?a1 ?a2)
Pre: (obj-at ?o ?l1) (in-city ?l1 ?c1) (in-city ?l2 ?c2) ...
Sub: ((obj-at ?o ?a1) (obj-at ?o ?a2) (obj-at ?o ?l2))
```

---

### 3.2 GDP 算法（Goal-Driven Planning）

#### ✅ 知识点：GDP 流程
- 每次处理一个目标
- 若当前状态已满足，跳过
- 否则选择合适动作或方法
- 方法会将子目标插入目标列表

#### ✅ 知识点：支持启发式
- 可用 RPG（Relaxed Planning Graph）估计目标距离
- 替代非确定性选择，提升效率

---

### 3.3 工具与系统

| 名称       | 特点                             | 语言/平台 |
|------------|----------------------------------|------------|
| **SHOP2**  | 偏序 HTN，AIPS 获奖               | Lisp       |
| **SHOP3**  | SHOP2 升级，持续维护              | Lisp       |
| **PyHop**  | Python 实现，简洁易用              | Python     |
| **Panda**  | 支持 HTN 与偏序方法                | HDDL       |
| **HyperTensioN** | Ruby 实现，2020 IPC 冠军       | Ruby       |
| **Europa** | 用于 NASA 火星车、空间站任务规划   | C++        |

---

### 3.4 总结：分层规划优劣

| 特点               | 经典规划         | 分层规划           |
|--------------------|------------------|--------------------|
| 描述难度           | 简单             | 复杂（需写方法）   |
| 求解效率           | 慢（需搜索）     | 快（利用知识）     |
| 表达能力           | 弱               | 强（可表达语言等） |
| 是否支持领域知识   | 否               | 是                 |
| 是否支持任务分解   | 否               | 是                 |

---

## ✅ 附录：经典例子汇总

### 例子 1：旅行规划（HTN）
- 任务：`travel(UMD, LAAS)`
- 方法：`air-travel`
- 子任务：
  - `get-ticket(BWI, TLS)`
  - `travel(UMD, BWI)`
  - `fly(BWI, TLS)`
  - `travel(TLS, LAAS)`

---

### 例子 2：交换物品（STN）
- 初始状态：`have(kiwi)`
- 任务：`swap(kiwi, banjo)`
- 方法：`swap1`
- 子任务：
  - `drop(kiwi)`
  - `pickup(banjo)`

---

## 📌 总结一句话
> 分层规划通过“任务分解 + 领域知识”让 AI 从“ brute-force 搜索”变为“聪明地按部就班”，代价是更复杂的建模，换来的是指数级提速和更强表达能力。
