## 📚 人工智能基础：规划算法与启发式（Planning Algorithms and Heuristics） - 详细版

### 1. 规划问题的形式化（Problem Formalization） - 详细解释

**STRIPS 模型**（Stanford Research Institute Problem Solver）是人工智能中规划问题的**标准形式化表示**，它将规划问题定义为四元组：

> **P = ⟨F, O, I, G⟩**  
> 其中：
> - **F**：所有可能的**基本事实**（ground atoms / fluents）——所有可能为真或假的原子命题（如 `cleanHands`, `garbage`, `dinner`）
> - **O**：**操作集合**（operators / actions）——每个操作有前提条件（preconditions）和效果（effects）
> - **I**：**初始状态**（initial state）——问题开始时为真的事实集合
> - **G**：**目标状态**（goal state）——问题结束时需要为真的事实集合

> 📌 **课件原文（L29, p.5）**:  
> "A STRIPS problem P= 〈F,O,I,G〉 determines state model S(P) where — the states s ∈ S are collections of atoms from F — the initial state s0 is I — the goal states s are such that G ⊆ s"

**通俗解释**：  
想象你正在规划一个"做一顿晚餐"的任务：
- **F** = {cleanHands, garbage, quiet, dinner, present, ¬garbage}（所有可能的事实）
- **O** = {cook(), wrap(), carry(), dolly()}（可能的操作）
- **I** = {cleanHands, garbage, quiet}（初始状态：手干净、有垃圾、环境安静）
- **G** = {dinner, present, ¬garbage}（目标状态：有晚餐、有礼物、垃圾被清理）

**操作示例**：
- `cook()`：前提 = {cleanHands}，效果 = {dinner, ¬cleanHands}（做饭需要手干净，做完手变脏，得到晚餐）
- `wrap()`：前提 = {quiet}，效果 = {present}（需要安静环境，包装礼物）
- `carry()`：前提 = {dinner}，效果 = {dinner, ¬dinner}（需要有晚餐，搬运后晚餐还在）
- `dolly()`：前提 = {quiet}，效果 = {¬quiet}（需要安静环境，但执行后环境变得嘈杂）

**为什么STRIPS重要**：  
STRIPS提供了一个**形式化框架**，使我们可以用计算机程序来处理规划问题。它将人类的"计划"转化为机器可理解的符号表示，是AI规划领域的基础。

---

### 2. 规划图（Planning Graph）与 Graphplan 算法 - 详细解释

#### 2.1 规划图的结构（Planning Graph Structure）

规划图是一种**松弛问题**（relaxed problem）的搜索空间表示，用于**高效估计规划可行性**。

- **交替层**（Alternating layers）：
  - **事实层**（Fact Level）$F_i$：在第 i 步可能为真的事实
  - **动作层**（Action Level）$A_i$：在第 i 步可执行的动作（其前提在 $F_i$ 中满足）

> 📌 **课件原文（L28, p.12）**:  
> "Alternating layers of ground literals and actions — Nodes at action-level i : actions possibly to executable at time i — Nodes at state-level i : literals true in s0"

**通俗解释**：  
想象规划图是一个"时间线"，每层表示在特定时间点可能发生的事件：
- 第0层（初始层）：$F_0 = I = \{cleanHands, garbage, quiet\}$（初始状态）
- 第1层（动作层）：$A_1 = \{cook(), wrap(), dolly()\}$（可执行的动作，前提在 $F_0$ 中满足）
- 第1层（事实层）：$F_1 = F_0 ∪ \{dinner, present, ¬quiet\}$（执行 $A_1$ 后可能为真的事实）
- 第2层（动作层）：$A_2 = \{carry()\}$（可执行的动作，前提在 $F_1$ 中满足）
- 第2层（事实层）：$F_2 = F_1 ∪ \{present, ¬garbage\}$（执行 $A_2$ 后可能为真的事实）

**为什么需要规划图**：  
规划图**避免了在状态空间中盲目搜索**，通过构建一个**松弛的表示**，可以快速判断规划问题是否可行，以及规划可能需要多少步骤。

#### 2.2 互斥关系（Mutex） - 详细解释

为了保证动作/事实不冲突，引入 **互斥**（mutual exclusion）概念：

- **动作互斥**（Action Mutex）当：
  1. **效果冲突**（Inconsistent effects）：一个动作的效果否定另一个的效果  
     *例：`carry()` 产生 `¬garbage`，而 `maintenance(garbage)` 保持 `garbage` → 冲突*
  2. **干扰**（Interference）：一个动作删除另一个的前提  
     *例：`dolly()` 产生 `¬quiet`，而 `wrap()` 需要 `quiet` → 干扰*
  3. **竞争前提**（Competing needs）：两个动作的前提互斥  
     *例：`cook()` 需要 `cleanHands`，`dolly()` 需要 `quiet`，但 `cleanHands` 和 `quiet` 本身不冲突，但如果两个动作都需要 `cleanHands`，则竞争前提*

- **事实互斥**（Literal Mutex）当：
  - 一个是另一个的否定，或
  - 所有能达成它们的动作对都是互斥的

> 📌 **课件原文（L28, p.15）**:  
> "Two actions at the same action-level are mutex if — Inconsistent effects — Interference — Competing needs"

**通俗解释**：  
想象你在同时安排两个活动：
- 你不能同时"做饭"和"打扫"（因为做饭需要手干净，而打扫会使手变脏，冲突）
- 你不能同时"安静地包装礼物"和"播放音乐"（因为包装需要安静，播放音乐会破坏安静，干扰）
- 你不能同时"做饭"和"准备包装"（因为两者都需要手干净，但手只能干净一次，竞争前提）

**为什么互斥重要**：  
互斥关系帮助我们**避免无效的计划**。在规划图中，如果两个动作互斥，它们不能在同一个时间点执行；如果两个事实互斥，它们不能同时为真。

#### 2.3 Graphplan 算法流程 - 详细步骤

1. **图扩展**（Graph Expansion）：
   - 逐层构建规划图，直到目标事实全部出现在某事实层且**彼此不互斥**
   - 如果目标事实在同一个事实层但互斥，则需要继续扩展

2. **解提取**（Solution Extraction）：
   - 从目标层**反向搜索**，选择非互斥动作
   - 递归满足其前提，直到初始状态

> 📌 **课件原文（L28, p.18）**:  
> "procedure Solution-extraction(g,j) … nondeterministically choose an action to use … if any pair of chosen actions are mutex then backtrack"

**详细步骤示例**（晚餐惊喜问题）：
1. **初始状态**：$I = \{cleanHands, garbage, quiet\}$
2. **目标状态**：$G = \{dinner, present, ¬garbage\}$
3. **规划图构建**：
   - 第0层：$F_0 = \{cleanHands, garbage, quiet\}$
   - 第1层动作：$A_1 = \{cook(), wrap(), dolly()\}$（前提在 $F_0$ 中满足）
   - 第1层事实：$F_1 = \{cleanHands, garbage, quiet, dinner, present, ¬quiet\}$
   - 第2层动作：$A_2 = \{carry()\}$（前提在 $F_1$ 中满足）
   - 第2层事实：$F_2 = \{cleanHands, garbage, quiet, dinner, present, ¬quiet, ¬garbage\}$

4. **问题**：在第2层，`dinner` 和 `present` 的动作（`cook` 和 `wrap`）互斥（因为 `dolly` 破坏了 `quiet`，而 `wrap` 需要 `quiet`）
5. **解决方案**：继续扩展到第4层，找到一个可行的计划：`{carry, cook, wrap}`

**为什么 Graphplan 有效**：  
Graphplan 通过**规划图**避免了在状态空间中盲目搜索，通过**互斥关系**排除无效计划，大大提高了规划效率。

---

### 3. 启发式搜索规划（Heuristic Search Planning） - 详细解释

#### 3.1 核心思想（Core Idea）

将规划问题转化为**状态空间搜索**，使用**启发式函数 h(s)** 估计从当前状态 s 到目标的代价。

> 📌 **课件原文（L29, p.6）**:  
> "Explicitly searches graph associated with model S(P) with heuristic h(s) that estimates cost from s to goal"

**通俗解释**：  
就像用GPS导航，h(s) 告诉你"离目的地还有多远"，帮助你优先探索最有希望的路径。在规划问题中，h(s) 估计从当前状态 s 到目标状态的步骤数。

#### 3.2 可采纳启发式（Admissible Heuristic） - 详细解释

- **定义**：**绝不高估**实际代价的启发式（optimistic）
- **重要性**：A* 搜索使用可采纳启发式时，**保证找到最优解**

> 📌 **课件原文（L29, p.8）**:  
> "A heuristic is admissible, or 'optimistic' if it never overestimates the cost to reach the goal"

**为什么可采纳重要**：  
如果启发式高估了实际代价，A* 算法可能不会找到最优解。可采纳启发式保证了 A* 算法的**完备性**和**最优性**。

**例子**（8-puzzle）：
- **h1 = 错位方块数**（Hamming distance）：每个方块与目标位置不匹配的计数 → 可采纳
- **h2 = 曼哈顿距离**（Manhattan distance）：每个方块到目标位置的水平+垂直距离之和 → 可采纳，且 **h2 ≥ h1**

> 📌 **课件原文（L29, p.14）**:  
> "h2 is always greater than h1 — It therefore dominates h1 … And is more efficient"

**为什么 h2 > h1**：  
曼哈顿距离考虑了方块移动的路径长度，而错位方块数只考虑是否在正确位置。因此，h2 总是大于等于 h1，所以 h2 **支配**（dominates）h1。

**通俗解释**：  
h2 比 h1 更准确地估计了实际移动距离，因此 A* 算法使用 h2 时会搜索更少的节点，更快找到最优解。

#### 3.3 问题松弛（Problem Relaxation） - 详细解释

通过**简化原问题**（删除某些约束）得到一个更容易求解的问题，其最优解代价可作为原问题的启发式。

> 📌 **课件原文（L29, p.15）**:  
> "You define a class P′ of simpler problems, whose perfect heuristic h∗P′ can be used to estimate h∗P"

**经典松弛方法**：**删除列表松弛**（Delete-list Relaxation, P⁺）
- 忽略所有动作的**删除效果**（delete effects）
- 得到的问题 P⁺ 中，事实一旦为真就永远为真 → 更容易求解

> 📌 **课件原文（L29, p.18）**:  
> "Most common relaxation in planning, P⁺, obtained by dropping delete-lists from ops in P"

**通俗解释**：  
想象你规划一条从家到学校的路线：
- **原问题**：需要考虑交通规则、红绿灯、道路施工等复杂因素
- **松弛问题**：忽略交通规则和道路施工，假设你可以直线走到学校
- **启发式**：从家到学校的直线距离（忽略所有障碍）

**为什么有效**：  
松弛问题更容易求解，其解（直线距离）是一个**下界**（lower bound），用于估计原问题的最小代价。

---

### 4. 经典启发式函数（Classical Heuristics） - 详细解释

#### 4.1 Max 启发式（hₘₐₓ） - 详细解释

- **定义**：对目标中的每个原子 p，计算其最早可达的步骤
- **计算**：hₘₐₓ(s) = 所有目标原子中**最大**的步骤数
- **特性**：**可采纳**，但**信息量不足**（过于保守）

> 📌 **课件原文（L29, p.20）**:  
> "For sets of atoms C, replace sum by max: h(C; s) = max_{r∈C} h(r; s) … Heuristic admissible, but not very informative"

**例子**（L29, p.22）：
- 目标 G = {R, G, B}
- R 在第 1 步可达，G 在第 2 步，B 在第 3 步
- hₘₐₓ = max(1, 2, 3) = **3**

**为什么可采纳**：  
hₘₐₓ 估计的是"最慢到达"的原子，因此它永远不会高估实际需要的步骤数。

**为什么信息量不足**：  
它忽略了其他原子可能更快到达的事实，导致启发式过于保守。

#### 4.2 Additive 启发式（hₐdd） - 详细解释

- **定义**：假设目标原子**相互独立**，将各原子代价**相加**
- **计算**：hₐdd(s) = ∑_{r∈G} h(r; s)
- **特性**：**不可采纳**（可能高估），但**信息量大、速度快**

> 📌 **课件原文（L29, p.19）**:  
> "For sets of atoms C, assume independence: h(C; s) = ∑_{r∈C} h(r; s) … Heuristic not admissible, but informative and fast"

**为什么不可采纳**：  
如果原子之间存在依赖关系（如一个原子需要另一个原子为真才能达到），则它们的代价不能简单相加。

**通俗解释**：  
hₐdd 假设所有目标原子可以同时达到，但实际上可能需要按顺序达到，因此它可能高估了实际步骤数。

#### 4.3 松弛规划图启发式（Relaxed Planning Graph, RPG） - 详细解释

- **定义**：RPG 是规划图的**简化版**：
  - **忽略所有删除效果**（与 P⁺ 一致）
  - **不计算互斥关系**（mutex）
- **hₘₐₓ 等价于 RPG 中首次包含所有目标的事实层编号**

> 📌 **课件原文（L30, p.9）**:  
> "hmax value is equivalent to the first level in RPG containing G"

> 📌 **课件原文（L30, p.5）**:  
> "Ignores negative effects from actions, no mutex relations"

**RPG 构建算法**（L30, p.6）：
```python
function BuildFullRPG(A, I, G):
    i ← 0
    RPG.FactLevel[0] ← I
    while G ⊈ RPG.FactLevel[i]:
        RPG.ActionLevel[i] ← {a ∈ A | pre(a) ⊆ RPG.FactLevel[i]}
        RPG.FactLevel[i+1] ← RPG.FactLevel[i] ∪ {add effects of actions in ActionLevel[i]}
        if RPG.FactLevel[i+1] == RPG.FactLevel[i]:  # fixed point
            return ∞  # goal unreachable
        i ← i + 1
    return i  # this is hmax
```

**通俗解释**：  
RPG 通过忽略删除效果和互斥关系，构建一个**更简单**的规划图。在 RPG 中，一旦一个事实为真，它就永远为真，没有动作能删除它。这使得 RPG 更容易构建，并且 hₘₐₓ 的值就是 RPG 中首次包含所有目标事实的层数。

**为什么有用**：  
RPG 提供了一个**快速计算 hₘₐₓ**的方法，而 hₘₐₓ 是一个可采纳启发式。

#### 4.4 Fast Forward 启发式（hFF） - 详细解释

- **定义**：基于 RPG，但使用**反向提取算法**构建一个**松弛计划**
- **特性**：**不可采纳**（可能高估），但通常比 hₘₐₓ **更接近真实代价**

> 📌 **课件原文（L30, p.12）**:  
> "A planning heuristic similar to hmax, but with no-ops in action levels — Inadmissible … Usually underestimates and provides a closer bound than hmax"

**通俗解释**：  
Fast Forward 通过构建一个**松弛计划**来估计实际代价。它从目标状态开始，反向构建一个可行的计划，忽略互斥关系和删除效果。hFF 通常比 hₘₐₓ 更准确，但可能高估实际代价。

**为什么比 hₘₐₓ 更好**：  
hₘₐₓ 只考虑目标事实的最早到达时间，而 hFF 考虑了**整个计划**，因此更接近实际需要的步骤数。

---

### 5. 基于可满足性测试的规划（Planning via Satisfiability Testing） - 详细解释

#### 5.1 核心思想（Core Idea）

将**有界长度 n 的规划问题**（P, n）**编码为布尔可满足性问题**（SAT），若 SAT 公式 Φ 可满足，则存在长度为 n 的解。

> 📌 **课件原文（L30, p.14）**:  
> "Try translating classical planning problems into satisfiability problems, and solving them that way"

**通俗解释**：  
想象你有一个复杂的谜题，需要在一定步数内解决。SAT 规划将这个谜题转化为一个布尔逻辑问题，如果这个问题有解，就表示在指定步数内可以解决这个谜题。

#### 5.2 编码方法（Encoding Method） - 详细解释

使用 **Fluents**（事实在时间点上的表示）：
- **Fluent**：表示"某事实 l 在第 i 步为真"，记为 **lᵢ**
- **动作 Fluent**：aᵢ 表示"动作 a 在第 i 步执行"

**编码公式 Φ 包含**：
1. **初始状态**：⋀{l₀ | l ∈ s₀} ∧ ⋀{¬l₀ | l ∉ s₀}
2. **目标状态**：⋀{lₙ | l ∈ g⁺} ∧ ⋀{¬lₙ | l ∈ g⁻}
3. **动作效果**：aᵢ ⇒ (⋀{pᵢ | p ∈ pre(a)} ∧ ⋀{eᵢ₊₁ | e ∈ eff(a)})
4. **互斥公理**：¬aᵢ ∨ ¬bᵢ （同一时间只能执行一个动作）
5. **框架公理**（Frame Axioms）：描述**什么不变**

> 📌 **课件原文（L30, p.20）**:  
> "Explanatory frame axioms — Says that if l changes between si and si+1, then the action at step i must have caused it"

**解释性框架公理**（Explanatory Frame Axioms）：
- (¬lᵢ ∧ lᵢ₊₁) ⇒ ⋁{aᵢ | l ∈ eff⁺(a)}（如果 l 在 i+1 步为真，而 i 步为假，则必须有动作 a 在 i 步执行，且 l 是 a 的添加效果）
- (lᵢ ∧ ¬lᵢ₊₁) ⇒ ⋁{aᵢ | l ∈ eff⁻(a)}（如果 l 在 i 步为真，而 i+1 步为假，则必须有动作 a 在 i 步执行，且 l 是 a 的删除效果）

**通俗解释**：  
框架公理确保了事实的变化**只由动作引起**。例如，如果机器人在第 i 步在位置 l1，而在第 i+1 步在位置 l2，那么一定有一个动作（如 move）在第 i 步执行，将机器人从 l1 移动到 l2。

**例子**（机器人移动）：
- 动作：move(r1, l1, l2)
- 效果：¬at(r1, l1), at(r1, l2)
- 框架公理：  
  `at(r1, l1, 0) ∧ ¬at(r1, l1, 1) ⇒ move(r1, l1, l2, 0)`

**为什么需要框架公理**：  
如果没有框架公理，SAT 求解器可能认为事实变化是随机发生的，而不是由动作引起的。框架公理确保了规划的**逻辑一致性**。

#### 5.3 解提取与局限性（Solution Extraction and Limitations） - 详细解释

- **解提取**：若 SAT 求解器找到满足 Φ 的赋值，则对每个 i，**唯一为真的 aᵢ** 构成计划
- **局限性**：单独使用效率低（内存和时间开销大），但可与规划图等结合

> 📌 **课件原文（L30, p.25）**:  
> "By itself, not very practical (takes too much memory and time) — But it can be combined with other techniques e.g., planning graphs"

**通俗解释**：  
SAT 规划就像用一个强大的逻辑引擎来解决规划问题。它能提供精确解，但计算成本高。在实际应用中，它通常与 Graphplan 等算法结合使用，以提高效率。

**为什么可以与规划图结合**：  
规划图提供了规划的**结构信息**，可以帮助 SAT 求解器更快地找到解，避免不必要的搜索。

---

### 6. 总结：三大规划算法范式 - 详细对比

> 📌 **课件原文（L30, p.26）**:  
> "Planning Algorithms — Planning Graphs — Planning based on Heuristic Search — Satisfiability Testing"

| 方法 | 核心思想 | 优点 | 缺点 | 适用场景 |
|------|--------|------|------|----------|
| **规划图**（Graphplan） | 构建松弛图 + 反向提取 | 比传统规划快很多，能处理大规模问题 | 生成大量无关事实，内存消耗大 | 大规模规划问题，如机器人任务规划 |
| **启发式搜索**（Heuristic Search） | 状态空间搜索 + h(s) | 主流方法，可扩展，能处理复杂问题 | 依赖启发式质量，可能不精确 | 通用规划问题，如路径规划、任务调度 |
| **SAT 规划**（Satisfiability Testing） | 编码为布尔公式 | 利用高效 SAT 求解器，能保证找到解 | 单独使用不实用，内存和时间开销大 | 小规模问题，或作为其他算法的补充 |

**为什么这三种方法互补**：  
- **Graphplan** 通过规划图快速缩小搜索空间
- **启发式搜索** 通过启发式函数指导搜索方向
- **SAT 规划** 提供精确解，但计算成本高

**实际应用中的选择**：
1. 对于**大规模问题**，优先考虑 Graphplan 或启发式搜索
2. 对于**小规模、需要精确解的问题**，可以使用 SAT 规划
3. 对于**复杂问题**，通常结合多种方法（如 Graphplan + 启发式搜索）

**通俗解释**：  
想象你在规划一次旅行：
- **规划图**：像使用地图APP，快速找到主要路线
- **启发式搜索**：像GPS导航，告诉你"往东北方向走"，并估计剩余时间
- **SAT 规划**：像精确计算每一步，确保在最短时间内到达目的地

---

这份详细版学习笔记覆盖了人工智能规划领域的所有核心概念、算法、例子和原理，确保您能深入理解每个知识点。希望对您的学习有所帮助！