好的，这是整合了所有伪代码并确保格式整齐、注释清晰的完整版 Markdown 笔记。

# Artificial Intelligence Foundation - Course Notes
# 人工智能基础 - 课程笔记

---

## Lecture 1-3: Intelligent Agents
## 讲座 1-3：智能体

### Definition of Agent
### 智能体定义
- An agent is a computer system that is capable of **independent action** on behalf of its user or owner. / 智能体是一个能够代表其用户或所有者进行**独立行动**的计算机系统。
- It figures out what needs to be done to satisfy design objectives, rather than constantly being told. / 它能自行判断需要做什么来满足设计目标，而不是一直被指令。

### Multi-Agent Systems (MAS)
### 多智能体系统 (MAS)
- A multiagent system consists of a number of agents which **interact with one-another**. / 多智能体系统由多个相互**交互**的智能体组成。
- Agents act on behalf of users with **different goals and motivations**. / 智能体代表具有**不同目标和动机**的用户行事。
- To successfully interact, they require the ability to **cooperate, coordinate, and negotiate**. / 为了成功交互，它们需要具备**合作、协调和协商**的能力。

#### Two Key Problems in MAS
#### 多智能体系统中的两个关键问题
1.  **Agent Design**: How to build agents capable of independent, autonomous action? / **智能体设计**：如何构建能够独立自主行动的智能体？
2.  **Society Design**: How to build agents capable of interacting with other agents, especially when they don't share the same interests/goals? / **社会设计**：如何构建能够与其他智能体交互的智能体，特别是在目标利益不一致时？

### Intelligent Agent
### 智能体
- Agents are **autonomous**: capable of acting independently and exhibiting control over their internal state. / 智能体是**自主的**：能够独立行动并控制其内部状态。
- Typically exhibit three types of behaviour: / 通常表现出三种行为：
    - **Reactive**: Responds to changes in the environment. / **反应式**：对环境变化做出反应。
    - **Pro-active**: Takes initiative to pursue goals. / **主动式**：主动追求目标。
    - **Social**: Interacts and cooperates with other agents/humans. / **社会性**：与其他智能体/人交互合作。

#### Agents vs. Objects
#### 智能体 vs. 对象
| Feature / 特性        | Objects / 对象                                      | Agents / 智能体                                     |
| :-------------------- | :-------------------------------------------------- | :-------------------------------------------------- |
| **Autonomy** <br>自主性   | Lower autonomy <br>自主性较低                         | **High autonomy**, decides on actions <br>**高自主性**，自行决定行动 |
| **Intelligence** <br>智能 | Encapsulates state <br>封装状态                       | **Flexible behavior** (reactive, proactive, social) <br>**灵活行为**（反应、主动、社交） |
| **Initiative** <br>主动性 | Passive service providers <br>被动服务提供者         | **Proactive**, not just passive <br>**主动**，而非仅仅被动 |

### Environment Types & Dimensions
### 环境类型与维度
| Dimension <br>维度        | Type 1 <br>类型1                          | Type 2 <br>类型2                              |
| :------------------------ | :---------------------------------------- | :-------------------------------------------- |
| **Observability** <br>可观测性 | Accessible/Fully Observable <br>可访问/完全可观测 | Inaccessible/Partially Observable <br>不可访问/部分可观测 |
| **Determinism** <br>确定性   | Deterministic <br>确定性                  | Non-deterministic <br>非确定性                |
| **Dynamicity** <br>动态性    | Static <br>静态                          | Dynamic <br>动态                              |
| **Discreteness** <br>离散性  | Discrete <br>离散                        | Continuous <br>连续                           |
| **Episodic** <br>情景性      | Sequential <br>序列式                    | Episodic <br>情景式                           |

---

## Lecture 4: Search I – Uninformed Search
## 讲座 4：搜索 I – 无信息搜索

### Problem-Solving Agent
### 问题求解智能体
1.  Update state based on percept and previous action. / 根据感知和先前动作更新状态。
2.  If no current plan: / 若当前无计划：
    - Formulate a goal. / 制定目标。
    - Define the problem. / 定义问题。
    - Search for a solution. / 搜索解决方案。
3.  Execute the first action in the plan. / 执行计划中的第一个动作。

### Problem Formulation
### 问题形式化
A problem is defined by five components: / 问题由五个要素定义：
1.  **Initial State** / **初始状态**
2.  **Actions** / **动作集合**
3.  **Successor Function** / **后继函数**
4.  **Goal Test** / **目标测试**
5.  **Path Cost** / **路径代价**

### Tree Search Algorithm (Generic Framework)
### 树搜索算法（通用框架）
```python
# Initialize the frontier using the initial state of the problem
# 使用问题的初始状态初始化前沿
frontier = [INITIAL-STATE]

while frontier is not empty:
    # Choose a node from the frontier based on the search strategy (BFS, DFS, etc.)
    # 根据搜索策略（BFS、DFS等）从前沿中选择一个节点
    node = frontier.remove_choice()

    # If the node contains a goal state, return the solution
    # 如果节点包含目标状态，返回解决方案
    if node.STATE is goal:
        return SOLUTION(node)

    # Otherwise, expand the node (generate all successors) and add them to the frontier
    # 否则，扩展该节点（生成所有后继节点）并将其添加到前沿
    for each successor in EXPAND(node, problem):
        frontier.add(successor)

# If the frontier is empty and no solution is found, return failure
# 如果前沿为空且未找到解，返回失败
return failure
```

### Search Strategies Evaluation
### 搜索策略评估标准
| Criterion <br>标准        | Description <br>描述                                                                               |
| :------------------------ | :------------------------------------------------------------------------------------------------- |
| **Completeness** <br>完备性 | Guaranteed to find a solution if one exists? <br>是否能保证找到解（如果存在）？                      |
| **Optimality** <br>最优性   | Guaranteed to find the least-cost solution? <br>是否能保证找到最低代价的解？                         |
| **Time Complexity** <br>时间复杂度 | Number of nodes generated/expanded (often using \(b\), \(d\), \(m\)). <br>生成/扩展的节点数（常用分支因子b, 解深度d, 最大深度m）。 |
| **Space Complexity** <br>空间复杂度 | Maximum number of nodes stored in memory. <br>内存中存储的最大节点数。                             |

### Uninformed Search Algorithms Summary
### 无信息搜索算法总结
| Algorithm <br>算法       | Completeness <br>完备性 | Optimality <br>最优性 | Time Complexity <br>时间复杂度 | Space Complexity <br>空间复杂度 | Notes <br>备注                                 |
| :----------------------- | :---------------------- | :-------------------- | :----------------------------- | :------------------------------ | :--------------------------------------------- |
| **BFS** <br>广度优先搜索   | ✅ Yes                    | ✅ Yes*                | \(O(b^d)\)                     | \(O(b^d)\)                      | \*If step costs are identical <br>\*假设每步代价相同 |
| **UCS** <br>一致代价搜索   | ✅ Yes                    | ✅ Yes                 | \(O(b^{1 + \lfloor C^*/\epsilon \rfloor})\) | Same as Time <br>同时间复杂度    | Optimal for varying step costs <br>适用于不同代价的情况 |
| **DFS** <br>深度优先搜索   | ❌ No                     | ❌ No                  | \(O(b^m)\)                     | \(O(bm)\)                       | Low memory, but not optimal <br>内存需求低，但不保证最优 |
| **DLS** <br>深度受限搜索   | ❌ No (if \(L < d\))      | ❌ No                  | \(O(b^L)\)                     | \(O(bL)\)                       | Requires depth limit \(L\) <br>需要深度限制L     |
| **IDS** <br>迭代加深搜索   | ✅ Yes                    | ✅ Yes*                | \(O(b^d)\)                     | \(O(bd)\)                       | Combines benefits of BFS & DFS <br>结合BFS和DFS的优点 |

---

## Lecture 7: Search II – Informed Search II
## 讲座 7：搜索 II – 有信息搜索 II

### A* Search Algorithm (Graph Search Version)
### A* 搜索算法（图搜索版本）
*假设启发函数 h(n) 是一致的，保证最优性。如果 h(n) 不一致，可能需要重新打开已关闭节点。 / Assumes heuristic h(n) is consistent, guaranteeing optimality. If h(n) is inconsistent, reopening closed nodes might be needed.*
```text
function A*-GRAPH-SEARCH(problem, h):
    // Initialize: Create the start node and frontier (priority queue by f = g + h)
    // 初始化：创建起始节点和前沿（按 f = g + h 排序的优先队列）
    node = a node with STATE = problem.INITIAL-STATE, PATH-COST = 0, PARENT = null
    frontier = a priority queue ordered by f = g + h, with node as the only element
    // 'reached' tracks the best g-cost found so far for each state
    // 'reached' 记录到目前为止找到的每个状态的最佳 g 代价
    reached = a lookup table; key=state, value=lowest g-cost found so far
    Add node.STATE to reached with value node.PATH-COST

    while frontier is not empty:
        node = frontier.pop() // Remove node with smallest f-value / 移除具有最小 f 值的节点
        if problem.GOAL-TEST(node.STATE):
            return SOLUTION(node) // Reconstruct path by following parent pointers / 通过父指针回溯路径
        for each child in EXPAND(problem, node): // Generate all successors / 生成所有后继节点
            s = child.STATE
            g_child = node.PATH-COST + problem.ACTION-COST(node.STATE, child.ACTION, s)
            // Check if this is a new state or a cheaper path to a known state
            // 检查这是新状态还是到已知状态的更便宜路径
            if s is not in reached or g_child < reached[s]:
                reached[s] = g_child // Update best g-cost for this state / 更新该状态的最佳 g 代价
                child.PATH-COST = g_child
                child.PARENT = node // Essential for path reconstruction / 路径回溯所必需
                f_child = g_child + h(s)
                add child to frontier with priority f_child
    return failure
```

### Iterative Deepening A* (IDA*) Algorithm
### 迭代加深 A* (IDA*) 算法
```text
function IDA*(problem, h):
    root = a node with STATE = problem.INITIAL-STATE, g = 0
    f_limit = h(root.STATE) // Initial f-cost limit / 初始 f 代价限制

    while true:
        // Perform a depth-first search within the current f-cost contour
        // 在当前 f 代价等高线内执行深度优先搜索
        solution, new_f_limit = CONTOUR-SEARCH(root, problem, h, f_limit)
        if solution is not failure:
            return solution
        if new_f_limit is infinity: // No nodes exceeded the current limit? No solution exists.
            return failure           // 没有节点超过当前限制？则无解。
        f_limit = new_f_limit // Increase the f-cost limit for the next iteration / 增加 f 代价限制以进行下一次迭代

function CONTOUR-SEARCH(node, problem, h, f_limit):
    f_node = node.g + h(node.STATE)
    if f_node > f_limit:
        return (failure, f_node) // Return failure and the exceeded f-value / 返回失败和超出的 f 值
    if problem.GOAL-TEST(node.STATE):
        return (SOLUTION(node), f_limit)
    next_f_limit = infinity // Track the smallest exceedance / 跟踪最小的超出值
    for each child in EXPAND(problem, node):
        child.g = node.g + problem.ACTION-COST(node.STATE, child.ACTION, child.STATE)
        solution, new_f = CONTOUR-SEARCH(child, problem, h, f_limit) // Recursive DFS / 递归DFS
        if solution is not failure:
            return (solution, f_limit)
        next_f_limit = min(next_f_limit, new_f) // Update the next limit / 更新下一次的限制
    return (failure, next_f_limit)
```

### Heuristic Function Properties
### 启发式函数属性
| Property <br>属性      | Description <br>描述                                                                 | Importance <br>重要性                                                          |
| :--------------------- | :----------------------------------------------------------------------------------- | :---------------------------------------------------------------------------- |
| **Admissible** <br>可采纳性 | Never overestimates the true cost to the goal (\(h(n) \leq h^*(n)\)). <br>从不高估到达目标的真实代价。 | Guarantees optimality for A* (Tree Search) <br>保证A*（树搜索）的最优性         |
| **Consistent** <br>一致性 | For every node \(n\) and successor \(n'\): \(h(n) \leq c(n,a,n') + h(n')\). <br>对任意节点n及其后继n'：\(h(n) \leq c(n,a,n') + h(n')\)。 | Guarantees optimality for A* (Graph Search) <br>保证A*（图搜索）的最优性         |
| **Domination** <br>支配性 | If \(h_2(n) \geq h_1(n)\) for all \(n\), \(h_2\) dominates \(h_1\) and is better. <br>若对所有n有\(h_2(n) \geq h_1(n)\)，则h2支配h1且更优。 | Tighter heuristics expand fewer nodes. <br>更紧的启发式扩展的节点更少。           |

---

## Lecture 8: Search III – Local Search I
## 讲座 8：搜索 III – 局部搜索 I

### Hill Climbing (Steepest Ascent)
### 爬山算法（最陡上升）
```text
function HILL-CLIMBING(problem):
    current = problem.INITIAL-STATE
    while true:
        // Find the best among all neighbors (successor states)
        // 找出所有邻居（后继状态）中最好的一个
        neighbor = a highest-valued successor state of current // "Steepest ascent" / "最陡上升"
        // For minimization problems, choose the lowest-valued successor
        // 对于最小化问题，选择值最低的后继

        // If no neighbor is better than the current state, stop
        // 如果没有邻居比当前状态更好，则停止
        if VALUE(neighbor) <= VALUE(current):
            return current // Return the local optimum / 返回局部最优解

        current = neighbor // Move to the better neighbor / 移动到更好的邻居
```

### Simulated Annealing
### 模拟退火
```text
function SIMULATED-ANNEALING(problem, schedule):
    // schedule(t) returns the "temperature" T at time step t, decreasing over time
    // schedule(t) 返回时间步 t 的"温度" T，随时间递减
    current = problem.INITIAL-STATE
    for t = 1 to ∞:
        T = schedule(t)
        if T == 0:
            return current
        // Randomly select a neighbor / 随机选择一个邻居
        next = a randomly selected successor state of current
        // Calculate the change in value (ΔE). Assume maximization problem.
        // 计算值的变化 (ΔE)。假设是最大化问题。
        ΔE = VALUE(next) - VALUE(current)

        // Always accept improving moves; probabilistically accept worse moves
        // 总是接受改进的移动；以一定概率接受更差的移动
        if ΔE > 0:
            current = next // Accept improving move / 接受改进的移动
        else:
            // Acceptance probability decreases as T decreases and as the move is worse
            // 接受概率随着 T 的降低和移动变差而减小
            acceptance_probability = exp(ΔE / T) // Note: ΔE is negative / 注意：ΔE 为负
            if random(0, 1) < acceptance_probability:
                current = next // Accept worse move probabilistically / 以一定概率接受更差移动
```

---

## Lecture 9: Search III – Local Search II
## 讲座 9：搜索 III – 局部搜索 II

### (Local) Beam Search
### （局部）束搜索
```text
function BEAM-SEARCH(problem, k): // k is the beam width / k 是束宽度
    // Initialize population with k states (could be random or just the initial state)
    // 用 k 个状态初始化种群（可以是随机的，或者只是初始状态）
    population = [problem.INITIAL-STATE] // Or k random states / 或者是 k 个随机状态

    while true:
        all_candidates = [] // List to store all generated successors / 存储所有生成的后继的列表
        // Generate all successors for each state in the current population
        // 为当前种群中的每个状态生成所有后继
        for each state in population:
            for each successor in SUCCESSORS(state):
                all_candidates.append(successor)

        // Check if any candidate is a solution
        // 检查是否有候选者是解
        for each candidate in all_candidates:
            if problem.GOAL-TEST(candidate):
                return candidate

        // If no solution, select the k best candidates based on their VALUE
        // 如果无解，根据 VALUE 选择 k 个最佳候选者
        sort all_candidates by VALUE descending // For maximization / 对于最大化问题降序排序
        population = the first k states from sorted all_candidates // New population / 新的种群
```

### Standard Genetic Algorithm
### 标准遗传算法
```text
function GENETIC-ALGORITHM(population, FITNESS-FN):
    // population: a set of individuals (e.g., strings representing states)
    // population: 一组个体（例如，代表状态的字符串）
    // FITNESS-FN: a function that measures the fitness of an individual (higher is better)
    // FITNESS-FN: 衡量个体适应度的函数（值越高越好）

    while termination condition not met (e.g., time, max generations):
        new_population = []
        for i = 1 to SIZE(population):
            // 1. Selection: Choose two parents based on fitness
            // 1. 选择：根据适应度选择两个父代
            x = RANDOM-SELECTION(population, FITNESS-FN) // e.g., Roulette Wheel / 例如轮盘赌
            y = RANDOM-SELECTION(population, FITNESS-FN)

            // 2. Crossover: With probability P_c, create child by combining x and y
            // 2. 交叉：以概率 P_c，通过组合 x 和 y 创建子代
            child = REPRODUCE(x, y, P_c) // Might return a parent if no crossover / 若无交叉可能返回一个父代

            // 3. Mutation: With small probability P_m, mutate the child
            // 3. 变异：以较小的概率 P_m 变异子代
            if random(0,1) < P_m:
                child = MUTATE(child)

            new_population.append(child)

        population = new_population

    // Return the best individual found after all generations
    // 在所有世代后返回找到的最佳个体
    return the individual in population with the highest FITNESS-FN(individual)

// Helper function for Roulette Wheel Selection
// 轮盘赌选择的辅助函数
function RANDOM-SELECTION(population, FITNESS-FN):
    total_fitness = sum(FITNESS-FN(individual) for individual in population)
    pick = random(0, total_fitness) // Spin the wheel / 转动轮盘
    current = 0
    for individual in population:
        current += FITNESS-FN(individual)
        if current > pick:
            return individual // This individual's segment was selected / 选择了个体对应的片段

// Helper function for Single-Point Crossover
// 单点交叉的辅助函数
function REPRODUCE(x, y, P_c):
    if random(0,1) > P_c: // With probability 1-P_c, no crossover
        return random_choice([x, y])
    n = LENGTH(x)
    c = random integer between 1 and n-1 // Crossover point / 交叉点
    child = SUBSTRING(x, 1, c) + SUBSTRING(y, c+1, n) // Combine genetic material / 组合遗传物质
    return child
```

### Local Search Summary
### 局部搜索总结
| Algorithm <br>算法          | Key Idea <br>核心思想                                                                 | Pros & Cons <br>优缺点                                                      |
| :-------------------------- | :------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------- |
| **Hill Climbing** <br>爬山算法   | Move to the best neighbor. <br>移动到最佳邻居。                                       | Simple, fast. Gets stuck in local optima. <br>简单、快速。易陷入局部最优。     |
| **Simulated Annealing** <br>模拟退火 | Allow worse moves initially (probabilistically). <br>初始时（概率性地）允许更差的移动。 | Can escape local optima. Slower. <br>能逃离局部最优。较慢。                   |
| **Beam Search** <br>束搜索      | Keep k best states at each step. <br>每步保留k个最佳状态。                             | Parallel-like, limited memory. May lack diversity. <br>类并行，内存有限。可能缺乏多样性。 |
| **Genetic Algorithms** <br>遗传算法 | Evolve population using selection, crossover, mutation. <br>使用选择、交叉、变异进化种群。 | Good for complex spaces. Complex to tune. <br>适用于复杂空间。参数调整复杂。   |

---

## Lecture 10: Search IV: Adversarial Search I
## 讲座 10：搜索 IV：对抗搜索 I

### 本讲概览 (Overview)
*   **核心主题**：从**单人**问题求解转向**多人**环境下的决策，特别是**对抗性环境**。
*   **主要内容**：
    *   介绍**博弈 (Games)** 作为多智能体环境的基本模型。
    *   学习博弈的两种标准表示形式：**标准式 (Normal Form)** 和**扩展式 (Extensive Form)**。
    *   通过经典例子（如囚徒困境、协调博弈等）理解博弈论的基本概念。
    *   为下一讲学习具体的对抗搜索算法（如 Minimax）打下基础。

---

### 1. 从单智能体到多智能体 (From Single-Agent to Multi-Agent)
*   之前的搜索算法（无信息搜索、有信息搜索、局部搜索）主要针对**单智能体**环境，智能体与环境互动，目标是找到到达目标状态的路径或最优配置。
*   在**多智能体环境**中：
    *   每个智能体必须考虑**其他智能体的行为**。
    *   智能体之间可能需要**协作 (Coordination)** 以保证行动的一致性。
    *   智能体之间也可能是**竞争 (Competitive)** 关系，即一个智能体的成功可能意味着另一个的失败。这是**博弈论 (Game Theory)** 研究的核心。

---

### 2. 博弈 (Games)
#### 2.1 什么是博弈？(What is a Game?)
*   在AI和博弈论中，“博弈”是一个广义概念，指任何涉及多个决策者（玩家）互动的场景。
*   **例子**：
    *   传统游戏：象棋、围棋、井字棋。
    *   现实场景：海盗海域的海军巡逻、警力部署、金融市场交易、人群管理。
*   AI 早期尤其关注**确定性、轮流行动、双人、零和、完全信息**的博弈。
    *   **确定性 (Deterministic)**：行动结果确定。
    *   **轮流行动 (Turn-taking)**：玩家交替行动。
    *   **双人 (Two-player)**：通常简化为 MAX（我方）和 MIN（对手）。
    *   **零和 (Zero-sum)**：一方的收益等于另一方的损失，总效用为零。
    *   **完全信息 (Perfect Information)**：所有玩家对当前游戏状态完全了解（如象棋，所有棋子可见）。

#### 2.2 为什么研究博弈？(Why Study Games?)
*   博弈对搜索算法的**效率要求极高**。例如，国际象棋的分支因子约为35，一场对局可能持续50回合，产生的搜索树节点数量巨大（\(35^{100}\)级别）。
*   在实时对抗中，**决策过慢会导致失败**。

---

### 3. 博弈的形式化表示 (Game Formalism)

#### 3.1 博弈的关键要素 (Key Ingredients of a Game)
1.  **玩家 (Players)**：决策者，如个人、政府、公司。
2.  **行动 (Actions)**：玩家在每个决策点可以做什么，如出价、投票、卖出股票。
3.  **收益 (Payoffs/Utility)**：激励玩家的因素，即他们关心什么（如利润、其他玩家的效用）。

#### 3.2 标准式（正规形式）(Normal Form / Strategic Form)
*   用于描述**静态博弈**（玩家似乎同时行动）。
*   定义为一个三元组 \(\langle N, A, u \rangle\)：
    *   \(N = \{1, \ldots, n\}\)：有限的玩家集合。
    *   \(A_i\)：玩家 \(i\) 的行动集合。所有行动的组合构成**行动剖面 (Action Profile)** \(a = (a_1, \ldots, a_n) \in A = A_1 \times \cdots \times A_n\)。
    *   \(u_i : A \mapsto \mathbb{R}\)：玩家 \(i\) 的**效用函数 (Utility Function)**，表示在某个行动剖面下的收益。\(u = (u_1, \ldots, u_n)\) 是效用函数剖面。
*   **表示**：通常用**矩阵 (Matrix)** 表示，尤其适用于双人博弈。行是玩家1的行动，列是玩家2的行动，单元格内是（玩家1收益，玩家2收益）。

#### 3.3 扩展式（扩展形式）(Extensive Form)
*   用于描述**动态博弈**（玩家序贯行动），包含了行动的**时序**和玩家的**信息集**（知道什么历史信息）。
*   定义为一个搜索问题，包含以下元素：
    *   \(S_0\)：初始状态。
    *   \(Player(s)\)：在状态 \(s\) 行动的玩家。
    *   \(Actions(s)\)：在状态 \(s\) 可用的行动。
    *   \(Result(s, a)\)：转移模型，执行行动后的结果状态。
    *   \(TerminalTest(s)\)：判断状态 \(s\) 是否为终止状态（游戏结束）。
    *   \(Utility(s, p)\)：在终止状态 \(s\) 下，玩家 \(p\) 的效用（零和博弈中 \(Utility(s, p_1) = -Utility(s, p_2)\)）。
*   **表示**：通常用**博弈树 (Game Tree)** 表示。例如井字棋的博弈树。

---

### 4. 博弈论与示例 (Game Theory & Examples)
课件通过多个经典博弈示例说明了关键概念。下表总结了这些例子及其特点：

| 博弈名称 <br>Game Name       | 玩家目标 <br>Player Objectives                               | 关键概念 <br>Key Concepts Illustrated                               | 收益矩阵示例 <br>Payoff Matrix Example (P1, P2)                                                                       |
| :-------------------------- | :---------------------------------------------------------- | :----------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------ |
| **囚徒困境 (Prisoner's Dilemma)** | 个体理性与集体理性的冲突 <br>Conflict between individual and collective rationality | **占优策略 (Dominant Strategy)**: 无论对方选什么，某个策略总是最优的（此处是背叛`D`）。 <br>**纳什均衡 (Nash Equilibrium)**: 给定对方策略，自己没有动机单方面改变策略（(`D`, `D`)是均衡点，但非集体最优）。 <br>**帕累托最优 (Pareto Optimality)**: 无法在不损害他人的情况下使某人受益（(`C`, `C`)是帕累托最优，但非均衡）。 | <code>C | (4, 4) | (0, 6) </code><br><code>D | (6, 0) | (2, 2) </code><br>**均衡/占优**: (`D`, `D`) |
| **协调博弈 (Coordination Game)** | 双方都希望行动一致 <br>Both players want to coordinate their actions | **多重纳什均衡 (Multiple Nash Equilibria)**: 存在多个稳定结果（(`Left`, `Left`) 和 (`Right`, `Right`)）。 | <code>Left | (1, 1) | (0, 0) </code><br><code>Right | (0, 0) | (1, 1) </code><br>**均衡**: (`L`,`L`) 和 (`R`,`R`) |
| **匹配博弈 (Matching Game)** | 双方都希望“匹配” <br>Both players want to "match"           | 类似于协调博弈，但收益可能不对称。存在多个纳什均衡。 | <code>B | (2, 1) | (0, 0) </code><br><code>F | (0, 0) | (1, 2) </code><br>**均衡**: (`B`,`B`) 和 (`F`,`F`) |
| **猜硬币 (Matching Pennies)** | 一方想匹配，另一方想不匹配 <br>One wants to match, the other to mismatch | **零和博弈 (Zero-Sum Game)** 的典型例子。**混合策略纳什均衡 (Mixed Strategy Nash Equilibrium)**: 不存在纯策略均衡，玩家必须随机化行动。 | <code>Heads | (1, -1) | (-1, 1) </code><br><code>Tails | (-1, 1) | (1, -1) </code><br>**均衡**: 双方以50%/50%的概率随机选择。 |
| **石头剪刀布 (RPS)**        | 一方想赢过对方 <br>One wants to beat the other              | 同样是**零和博弈**和**混合策略均衡**的例子。                         | <code>Rock | (0, 0) | (-1, 1) | (1, -1) </code><br><code>Paper | (1, -1) | (0, 0) | (-1, 1) </code><br><code>Scissors | (-1, 1) | (1, -1) | (0, 0) </code><br>**均衡**: 双方以1/3的概率随机选择每种行动。 |
| **退出博弈 (Quitting Game)** | 类似于“胆小鬼游戏”，是一种**反协调博弈 (Anti-coordination Game)** | 玩家希望避免与对方选择相同的行动。存在两个纯策略纳什均衡（(`C`,`D`) 和 (`D`,`C`)），以及一个混合策略均衡。 | <code>C | (0, 0) | (-1, 1) </code><br><code>D | (1, -1) | (-2, -2) </code><br>**均衡**: (`C`,`D`) 和 (`D`,`C`) |

---

### 5. 本讲总结 (Summary of This Lecture)
1.  **对抗搜索**处理的是**多智能体、竞争性**环境下的决策问题。
2.  博弈论为这类问题提供了形式化框架。
3.  博弈有两种主要表示法：
    *   **标准式 (Normal Form)**：适用于静态（同时行动）博弈，用矩阵表示。
    *   **扩展式 (Extensive Form)**：适用于动态（序贯行动）博弈，用博弈树表示，是下一讲算法（如Minimax）的基础。
4.  通过经典博弈示例引入了**占优策略、纳什均衡、帕累托最优、零和博弈、混合策略**等核心概念。
5.  下一讲将深入探讨用于**扩展式博弈**的具体搜索算法，如 **Minimax 算法** 和 **Alpha-Beta 剪枝**。

---
### 关键术语 (Key Terms)
*   **Adversarial Search / 对抗搜索**
*   **Game Theory / 博弈论**
*   **Players / 玩家**
*   **Actions / 行动**
*   **Payoffs / Utility / 收益 / 效用**
*   **Normal Form / 标准式 / 正规形式**
*   **Extensive Form / 扩展式 / 扩展形式**
*   **Game Tree / 博弈树**
*   **Zero-Sum Game / 零和博弈**
*   **Perfect Information / 完全信息**
*   **Dominant Strategy / 占优策略**
*   **Nash Equilibrium / 纳什均衡**
*   **Pareto Optimality / 帕累托最优**
*   

---

## Lecture 13: CSP I - 定义与基础

### 1. CSP 的定义
约束满足问题（CSP）是一种识别问题，其目标是找到一组满足给定约束的变量赋值。
CSP由三个组成部分构成：
- **变量（X）**: \( X = \{X_1, X_2, ..., X_n\} \)，表示问题中的未知数。
- **域（D）**: \( D = \{D_1, D_2, ..., D_n\} \)，每个变量 \( X_i \) 有其对应的可能取值集合 \( D_i \)。
- **约束（C）**: 规定了变量之间允许的取值组合。

**核心概念**:
- **赋值**: 为变量指定值的过程。
- **完全赋值**: 每个变量都被赋予一个值。
- **部分赋值**: 仅部分变量被赋值。
- **解**: 一个满足所有约束的**完全赋值**。
- **部分解**: 一个满足所有约束的**部分赋值**。

### 2. CSP 与标准搜索问题的区别
| 特征 | 标准搜索问题 | CSP |
| :--- | :--- | :--- |
| **状态** | 不透明的“黑箱”数据结构 | 由变量及其赋值明确定义 |
| **目标测试** | 针对整个状态的单一函数 | 一组约束条件，检查变量间的值组合 |
| **优势** | 通用性强 | 利用结构信息，有更强大、高效的专用算法 |

### 3. 示例：地图着色问题
- **变量**: WA, NT, Q, NSW, V, SA, T (澳大利亚的州和地区)
- **域**: \( D_i = \{red, green, blue\} \) (每个变量都可取三种颜色)
- **约束**: 相邻的区域必须颜色不同，例如 \( WA \neq NT \)，或更形式化地表示为允许的(WA, NT)颜色对集合。
- **解示例**: `{WA=red, NT=green, Q=red, NSW=green, V=red, SA=blue, T=green}`

### 4. 约束图
- **二元CSP**: 所有约束最多涉及两个变量。
- **约束图**: 节点代表变量，边代表约束关系。
    - **价值**: 图结构可以极大地加速搜索。例如，在澳大利亚地图中，塔斯马尼亚岛(T)与大陆是独立的子问题，可以分开求解。

### 5. CSP 的分类
#### a. 按变量类型
- **离散变量**
    - **有限域**: 例如布尔CSP（命题可满足性，是NP完全问题）。共有 \(d^n\) 种完全赋值。
    - **无限域**: 例如作业调度（变量是开始/结束日期）。需要约束语言（如 \(StartJob_1 + 5 \leq StartJob_3\)）。线性约束可解，非线性约束不可判定。
- **连续变量**
    - 例如哈勃望远镜观测调度。线性约束可通过线性规划方法在多项式时间内求解。

#### b. 按约束类型
- **一元约束**: 涉及单个变量（如 \( SA \neq green \)）。
- **二元约束**: 涉及两个变量（如 \( SA \neq WA \)）。
- **高阶约束**: 涉及三个及以上变量（如密码算术题中的列约束）。
- **软约束（偏好）**: 并非必须满足，但更好的赋值具有更高价值（如红色比绿色好），可转化为约束优化问题。

### 6. 现实世界的 CSP 应用
- 任务分配（谁教哪门课）
- 时间表编排（课程安排在何时何地）
- 硬件配置
- 运输调度
- 工厂调度
- 平面布局规划

### 7. 案例研究：ML vs. GOFAI (传统AI)
- **Flatland 挑战** (NeurIPS 2020): 一个大型铁路网络调度问题。
- **结果**: 尽管吸引了大量机器学习方案，但最终多数获胜方案使用了基于CSP的传统符号AI方法（GOFAI）。
- **启示**: 对于高度结构化的优化问题，CSP 等传统方法目前仍常优于纯粹的机器学习方法。

---

## Lecture 14: CSP II - 回溯搜索

### 1. 回溯搜索基础
- **核心观察**: 在CSP中，变量赋值的**顺序不影响结果**（[WA=red then NT=green] 与 [NT=green then WA=red] 相同）。
- **回溯搜索**: 一种为CSP设计的深度优先搜索。
    - 每个节点为一个变量赋值。
    - 分支因子 \(b = d\) (域大小)，叶子节点数为 \(d^n\)。
    - 是CSP的基本**无信息**算法，可解决 n≈25 的n皇后问题。

### 2. 回溯搜索算法（伪代码）

```python
# 主函数：启动回溯搜索过程
function BACKTRACKING-SEARCH(csp):
    # 从空赋值开始递归搜索
    return BACKTRACK({}, csp)

# 核心递归回溯函数
function BACKTRACK(assignment, csp):
    # 基础情况：如果赋值已经包含所有变量，说明找到解
    if assignment is complete:
        return assignment

    # 启发式：选择下一个要赋值的变量（如使用MRV）
    var = SELECT-UNASSIGNED-VARIABLE(csp, assignment)
    
    # 启发式：按特定顺序尝试该变量的所有可能值（如使用最少约束值）
    for value in ORDER-DOMAIN-VALUES(csp, var, assignment):
        # 检查该值是否与当前赋值一致（满足所有约束）
        if value is consistent with assignment:
            # 暂时将该赋值加入当前解
            add {var = value} to assignment
            
            # 可选：进行推理（如前向检查、弧相容），可能推导出新的约束或减少值域
            inferences = INFERENCE(csp, var, assignment)
            
            # 如果推理没有发现矛盾
            if inferences != failure:
                # 将推理结果加入赋值
                add inferences to assignment
                
                # 递归调用，继续深度搜索
                result = BACKTRACK(assignment, csp)
                
                # 如果递归调用返回解，则逐层返回解
                if result != failure:
                    return result
                
                # 如果递归失败，回溯：移除推理结果
                remove inferences from assignment
            
            # 回溯：移除当前尝试的赋值，准备尝试下一个值
            remove {var = value} from assignment
    
    # 如果所有值都尝试失败，则返回失败信号给上一层
    return failure
```

### 3. 提高回溯效率的通用策略
1.  **下一步选择哪个变量？** (`SELECT-UNASSIGNED-VARIABLE`)
2.  **按什么顺序尝试变量的值？** (`ORDER-DOMAIN-VALUES`)
3.  **能否尽早检测到必然失败？** (`INFERENCE`)
4.  **能否利用问题结构？** (见L15)
5.  **能否保存和重用部分搜索结果？**

### 4. 变量选择启发式
- **最小剩余值 (MRV)**:
    - **策略**: 选择**合法值最少的变量**。
    - **原理**: 优先处理约束最紧的变量，能最快地导致成功或失败（"失败优先"）。

- **度启发式 (Degree Heuristic)**:
    - **策略**: 作为MRV的平局决胜器，选择**对未赋值变量约束最多**的变量。
    - **原理**: 约束最多的变量更可能很快导致失败，避免在无关分支上浪费时间。

### 5. 值选择启发式
- **最少约束值 (Least Constraining Value)**:
    - **策略**: 给定一个变量，优先选择**淘汰其他变量值最少**的值。
    - **原理**: 保留其他变量的灵活性，减少未来回溯的可能性。

### 6. 推理与约束传播
- **前向检查 (Forward Checking)**:
    - **思想**: 当一个变量被赋值后，立即检查并删除其未赋值邻居变量值域中与之冲突的值。
    - **作用**: 能尽早发现某个变量值域为空的情况，立即回溯。

- **约束传播 (Constraint Propagation)**:
    - **思想**: 比前向检查更深入。不仅检查直接邻居，还通过约束网络传播影响。
    - **目标**: 通过局部相容性检查，进一步缩小值域，提前发现不一致。

### 7. 弧相容 (Arc Consistency) 与 AC-3 算法
- **弧相容定义**: 对于约束图中的一条弧 (X, Y)（代表约束），如果对于X值域中的**每一个**值，Y值域中都**至少存在一个**值能满足该约束，则称弧 (X, Y) 是相容的。
- **AC-3算法思想**: 维护一个弧的队列。不断从队列中取出弧进行检查，如果为了使其相容而删除了X的某些值，那么所有指向X的弧（即 (Z, X)）可能变得不相容，需要重新加入队列检查。直到队列为空或某个变量的值域为空。

```python
# AC-3 算法：实现弧相容
function AC-3(csp):
    # 初始化队列，包含csp中的所有弧（双向）
    queue = a queue of arcs, initially all the arcs in csp

    while queue is not empty:
        # 从队列中取出一条弧 (X_i, X_j)
        (X_i, X_j) = POP(queue)

        # 修订X_i的值域：删除所有没有对应合法值in X_j的值
        if REVISE(csp, X_i, X_j):
            # 如果X_i的值域被修订后为空，则问题无解
            if size of D_i == 0:
                return false
            # 否则，因为X_i的值域变了，需要重新检查所有指向X_i的弧（除了X_j）
            for each X_k in X_i.Neighbors - {X_j}:
                add (X_k, X_i) to queue  # 注意方向是 (邻居 -> X_i)

    return true  # CSP是弧相容的

# 辅助函数：修订X_i的值域以使其相对于X_j满足弧相容
function REVISE(csp, X_i, X_j):
    revised = false
    for each x in D_i:  # 遍历X_i的当前值域中的每个值
        # 检查在X_j的值域中是否存在一个值y，使得(x, y)满足X_i和X_j之间的约束
        if no value y in D_j allows (x, y) to satisfy the constraint:
            delete x from D_i  # 如果没有这样的y，则x是非法的，从值域中删除
            revised = true
    return revised  # 返回是否对值域进行了修订
```
- **复杂度**: 最坏情况 \(O(n^2d^3)\)，可优化至 \(O(n^2d^2)\)。实现完全相容是NP难的，但弧相容作为预处理或搜索中每一步的推理非常有效。

---

## Lecture 15: CSP III - 局部搜索与问题结构

### 1. 局部搜索用于 CSP
- **完全状态形式化**: 与回溯搜索从空赋值开始不同，局部搜索从**一个完全赋值**（可能违反约束）开始。
- **基本操作**: 通过**改变一个变量的值**来生成邻居状态。
- **目标函数**: 通常定义为**被违反约束的数量**。目标是使这个数量最小化为0。

### 2. 最小冲突算法（伪代码）

```python
# 最小冲突算法：使用局部搜索解决CSP
function MIN-CONFLICTS(csp, max_steps):
    # 1. 初始化：随机生成一个完全赋值（可能包含冲突）
    current = an initial complete assignment for csp

    # 2. 迭代改进：最多尝试 max_steps 步
    for i = 1 to max_steps:
        # 如果当前赋值已经满足所有约束（冲突数为0），则返回解
        if current is a solution for csp:
            return current

        # 3. 变量选择：随机选择一个当前处于冲突中的变量（即其值违反了至少一个约束）
        var = a randomly chosen conflicted variable from csp.Variables

        # 4. 值选择：在var的所有可能值中，选择使总冲突数最小的那个值
        #    注意：这里比较的是将var赋值为v后，整个赋值的冲突数，而不仅仅是var引起的冲突
        value = the value v in var.domain that minimizes CONFLICTS(csp, var, v, current)

        # 5. 赋值：将选定的值赋给变量，移动到邻居状态
        set var = value in current

    # 如果超过最大步数仍未找到解，则返回失败
    return failure
```

### 3. 局部搜索的特性与优化
- **最小冲突启发式**: 选择使新状态总冲突数最小的值。这是一种**爬山法**。
- **平台问题**: 搜索可能陷入高原（许多状态冲突数相同）。允许**侧向移动**（移动到冲突数相等的状态）有助于逃离高原。
- **约束加权**: 为每个约束赋予权重。当约束被违反时增加其权重。搜索会倾向于解决权重高的约束，从而将精力集中在难题部分。
- **性能**: 对许多CSP（如n-皇后）效果惊人，即使n很大（如1000万），也能以高概率在近似常数时间内找到解。但在约束数量与变量数量的比率（R）处于临界区域时可能失效。

### 4. 问题结构的重要性
- **独立子问题**: 如果约束图可分解为连通分量（如澳大利亚地图中的大陆和塔斯马尼亚岛），这些子问题可独立求解。
    - **复杂度收益**: 从 \(O(d^n)\) 降为 \(O(\frac{n}{c} \cdot d^c)\)，其中c是子问题大小。例如，n=80, d=2, c=20，时间从40亿年降至0.4秒（假设每秒1000万次操作）。

### 5. 树结构 CSPs
- **定理**: 如果约束图是无环的（一棵树），则CSP可以在 \(O(n d^2)\) 时间内解决，与一般CSP的 \(O(d^n)\) 形成鲜明对比。
- **求解算法**:
    1.  **排序**: 选一个根变量，进行拓扑排序，使每个节点的父节点排在其之前。
    2.  **向后传递（去除不相容）**: 从叶子到根，确保每个节点相对于其父节点是弧相容的。
    3.  **向前传递（赋值）**: 从根到叶子，为每个变量选择一个与其父节点值相容的值。
- **意义**: 展示了语法（结构）限制与推理复杂度之间的重要关系。

### 6. 近似树结构 CSPs：割集条件
- **思想**: 如果图不是树，可以通过实例化一个变量子集（**割集**），使得剩余图变成树。
- **过程**:
    1.  以所有可能的方式实例化割集（\(d^{|cutset|}\) 种方式）。
    2.  对于每种实例化，求解剩余的树结构CSP。
- **复杂度**: \(O(d^c \cdot (n-c) d^2)\)，其中c是割集大小。只要割集c很小，这就非常高效。

### 7. CSP 总结
- CSP是一种状态由变量赋值定义的特殊问题。
- **回溯搜索**是基础系统方法，结合变量/值排序启发式、前向检查、约束传播（如弧相容）可极大提升效率。
- **局部搜索**（如最小冲突）适用于大规模CSP，通常不保证完备性但速度很快。
- **分析问题结构**（如树宽度、割集）能带来指数级的效率提升。
- 对于许多结构化的组合优化问题，**CSP方法目前仍优于最先进的机器学习方法**。

以下是根据你提供的三个课件（L16, L17, L18）整理的**详细双语学习笔记**，涵盖所有知识点，并配有例子和通俗解释，帮助你理解逻辑代理（Logic Agents）在人工智能中的基础与应用。

---

# 📘 Lecture 16: Logic Agents I – 逻辑代理（一）

## 1. 知识型代理 | Knowledge-based Agents

- **知识库（Knowledge Base, KB）**：一个用形式语言表达的句子集合。
  - A set of sentences in a formal language.

- **推理引擎（Inference Engine）**：执行推理的算法，与领域无关。
  - Domain-independent algorithms.

- **声明式方法（Declarative Approach）**：
  - 告诉代理它需要知道的内容（Tell），代理自行推理该做什么（Ask）。
  - Tell it what it needs to know, so it can Ask itself what to do.

---

## 2. 逻辑代理的结构 | Structure of a Logic Agent

- **感知 → 更新知识库 → 查询动作 → 执行**
  - **Percept → Update KB → Query Action → Execute**

- **伪代码示例**：

```lua
function KBAgent(percept):
  static KB, t = 0
  tell(KB, makePerceptSentence(percept, t))
  action = ask(KB, makeActionQuery(t))
  tell(KB, makeActionSentence(action, t))
  t = t + 1
  return action
```

---

## 3. Wumpus World 环境 | Wumpus World Environment

### 环境规则：

- **Stench**：与Wumpus相邻的格子有臭味。
- **Breeze**：与Pit相邻的格子有微风。
- **Glitter**：金子所在的格子闪光。
- **Shoot**：射杀面前的Wumpus（只有一支箭）。
- **Grab**：捡起金子（若在同一格）。

### 性能评估：

- 找到金子：+1000
- 死亡：-1000
- 每走一步：-1
- 使用箭：-10

---

### 环境特性：

- **可观察性**：否，只有局部感知。
- **确定性**：是，结果明确。
- **静态性**：是，Wumpus和Pit不动。
- **离散性**：是。
- **单代理**：是，Wumpus视为自然特征。

---

# 📘 Lecture 17: Logic Agents II – 逻辑代理（二）

## 1. 逻辑基础 | Logic Basics

### 语法与语义：

- **语法（Syntax）**：定义语言中的合法句子。
- **语义（Semantics）**：定义句子的真值条件。

> 例：  
> \( x + 2 \geq y \) 是合法句子；  
> \( x^2 + y > \) 不是合法句子。

---

## 2. 蕴含与模型 | Entailment and Models

- **蕴含（Entailment）**：  
  \( KB \models \alpha \) 表示在所有 \( KB \) 为真的世界中，\( \alpha \) 也为真。

- **模型（Model）**：一个使句子为真的世界结构。

- **模型集合**：  
  \( M(KB) \subseteq M(\alpha) \) 表示 \( KB \) 蕴含 \( \alpha \)。

---

## 3. Wumpus World 中的推理 | Inference in Wumpus World

### 使用命题逻辑：

- \( P_{i,j} \)：格子 \([i,j]\) 有陷阱。
- \( B_{i,j} \)：格子 \([i,j]\) 有微风。

规则示例：

- \( B_{1,1} \Leftrightarrow (P_{1,2} \lor P_{2,1}) \)
- \( B_{2,1} \Leftrightarrow (P_{1,1} \lor P_{2,2} \lor P_{3,1}) \)

---

## 4. 推理方法 | Inference Methods

### a. 枚举法 | Inference by Enumeration

- 遍历所有可能的真值赋值，检查 \( KB \) 为真时 \( \alpha \) 是否也为真。
- 复杂度：\( O(2^n) \)，NP完全问题。

### b. 前向链接 | Forward Chaining

- 从已知事实出发，应用规则直到推出目标。
- 适用于 Horn 子句（最多一个正文字）。

> 例：  
> 规则：\( A \land B \Rightarrow L \)，已知 \( A \) 和 \( B \)，则推出 \( L \)。

### c. 后向链接 | Backward Chaining

- 从目标出发，反向寻找支持目标的子目标。
- 适用于目标导向的推理。

---

## 5. 前向 vs 后向链接 | Forward vs Backward Chaining

| 类型 | 特点 | 适用场景 |
|------|------|----------|
| 前向链接 | 数据驱动，自动处理 | 对象识别、常规决策 |
| 后向链接 | 目标驱动，搜索导向 | 问题解决、规划 |

---

# 📘 Lecture 18: Logic Agents III – 逻辑代理（三）

## 1. 一阶逻辑 | First-Order Logic (FOL)

### 基本元素：

- **常量**：KingJohn, 2, CMU, ...
- **谓词**：Brother, >, ...
- **函数**：Sqrt, LeftLegOf, ...
- **变量**：x, y, a, b, ...
- **量词**：∀（全称），∃（存在）

---

## 2. 谓词与原子句 | Predicates and Atomic Sentences

- 谓词表示关系或属性。
- 原子句：\( P(x) \), \( Q(x, y) \)

> 例：  
> \( P(Wumpus) \) 表示“Wumpus 是臭的”。

---

## 3. 合一 | Unification

- 使两个表达式在语法上一致的过程。

> 例：  
> \( UNIFY(Knows(John, x), Knows(John, Jane)) = \{x/Jane\} \)

---

## 4. 广义Modus Ponens | Generalized Modus Ponens (GMP)

- 用于一阶逻辑的推理规则：

\[
\frac{p_1', \ldots, p_n', \quad (p_1 \land \cdots \land p_n \Rightarrow q)}{q\theta}
\]

> 例：  
> 规则：\( King(x) \land Greedy(x) \Rightarrow Evil(x) \)  
> 事实：\( King(John) \), \( Greedy(John) \)  
> 结论：\( Evil(John) \)

---

## 5. 前向与后向链接在一阶逻辑中 | Forward and Backward Chaining in FOL

### 前向链接：

- 从已知事实出发，应用规则直到推出目标。
- 适用于 Datalog（无函数的一阶定句子句）。

### 后向链接：

- 从目标出发，反向寻找支持目标的子目标。
- 用于逻辑编程（如 Prolog）。

---

## 6. 示例知识库：武器贩卖案例 | Example KB: Weapons Sale

### 规则：

- \( American(x) \land Weapon(y) \land Sells(x, y, z) \land Hostile(z) \Rightarrow Criminal(x) \)
- \( Missile(x) \Rightarrow Weapon(x) \)
- \( Enemy(x, America) \Rightarrow Hostile(x) \)

### 事实：

- \( American(West) \)
- \( Missile(M1) \)
- \( Owns(Nono, M1) \)
- \( Sells(West, M1, Nono) \)
- \( Enemy(Nono, America) \)

### 结论：

- \( Criminal(West) \)

---

## 7. 前向与后向链接的性质 | Properties of Forward and Backward Chaining

| 方法 | 完备性 | 终止性 | 适用场景 |
|------|--------|--------|----------|
| 前向链接 | 是（定句子句） | 是（Datalog） | 数据驱动 |
| 后向链接 | 是（定句子句） | 可能不终止 | 目标驱动 |

---

# ✅ 总结 | Summary

| 主题 | 内容 |
|------|------|
| 知识型代理 | 使用 KB 和推理引擎进行推理 |
| 命题逻辑 | 语法、语义、蕴含、模型 |
| Wumpus World | 环境规则与逻辑建模 |
| 推理方法 | 枚举、前向链接、后向链接 |
| 一阶逻辑 | 谓词、量词、合一、GMP |
| 实际应用 | 武器贩卖案例、前向/后向链接 |

---


# 人工智能基础 — 不确定性与概率推理（Lecture 19–21）

## 目录 / Contents

1. 推理中的不确定性（Reasoning Under Uncertainty）
2. 决策论与决策-理论代理（Decision Theory & Decision-Theoretic Agent）
3. 概率论基础（Probability Theory Basics）

   * 先验（Prior）与分布
   * 条件概率与乘法规则（Product Rule）
   * 联合分布与独立性（Joint Distribution & Independence）
   * 边缘化（Marginalisation）与条件化（Conditioning）
4. 贝叶斯规则（Bayes’ Rule）与例题（Cookie Problem）
5. 链式法则（Chain Rule）与简化（Conditional Independence）
6. 贝叶斯网络（Bayesian Networks）

   * 定义与语义
   * 条件概率表（CPT）
   * Markov Blanket 与条件独立性
   * 推理（Inference）步骤与示例（Toothache、Alarm networks）
7. 小结与复习题（Summary & Practice Questions）

---

### 1. 推理中的不确定性 / Reasoning Under Uncertainty

**中文**：现实世界的问题常包含不确定性，来源包括部分可观测（partial observability）、非确定性（nondeterminism）和对手（adversaries）。用纯逻辑表达（“如果……则……”）在这种场景下往往不现实或会非常臃肿（例如牙痛不一定是蛀牙）。
**English**: Real-world problems contain uncertainty due to partial observability, nondeterminism, or adversaries. Pure logical rules (if→then) often fail or become impractically large (e.g., toothache → cavity is not exhaustive). 

**通俗解释 / Plain English**：逻辑想把每种可能都列出来，但现实太复杂，我们用概率来表达“相信程度”（degree of belief）。
**Example（例子）**：牙痛可能是蛀牙、牙龈问题、脓肿等多个原因中的一个，用概率比用一长串规则更合适。 

---

### 2. 决策论（Decision Theory）与决策-理论代理（Decision-Theoretic Agent）

**中文**：决策论把概率（beliefs）和效用（utility）结合起来：理性代理应该选择使期望效用（expected utility）最大的动作（原则：最大期望效用 MEU）。
**English**: Decision theory = probability theory + utility theory. A rational agent chooses the action with maximum expected utility (MEU). 

**代理伪代码 / Agent pseudocode**（中文+英文）：

```text
function DT-Agent(percept) returns action
 persistent: beliefState  ▷ agent's probabilistic beliefs about world
 update beliefState based on action and percept
 calculate outcome probabilities for actions given beliefState
 select action with highest expected utility
 return action
```

（中文）解释：代理维护概率性的信念状态（beliefState），以此计算各动作可能产生结果的概率，再结合效用函数挑选最优动作。 

---

### 3. 概率论基础 / Probability Theory Basics

#### 3.1 随机变量与先验（Random variables & Priors）

**中文**：我们把命题或量化对象视为随机变量（Boolean、离散或连续）。先验概率（prior）是在没有额外信息时对事件的相信程度，例如公平硬币 P(heads)=0.5。
**English**: We treat propositions as random variables (Boolean, discrete, continuous). Prior probabilities indicate degree of belief absent extra evidence (e.g., P(heads)=0.5). 

#### 3.2 条件概率与乘法规则（Conditional probability & Product rule）

**中文**：条件概率定义为 (P(a \mid b) = \dfrac{P(a \land b)}{P(b)})。乘法规则：(P(a \land b) = P(a \mid b)P(b))。
**English**: Conditional probability: (P(a|b)=P(a\land b)/P(b)). Product rule: (P(a\land b)=P(a|b)P(b)). 

**例子 / Example**：投两次硬币，独立时 (P(H,H)=P(H)\times P(H))。如果每次 (P(H)=0.5)，那么 (P(H,H)=0.25)。 

## 3.3 联合分布与独立性（Joint distribution & Independence）

**中文**：联合概率分布 (P(X_1,\dots,X_n)) 给出所有变量同时取某值的概率。若变量独立，联合概率可写成各自概率的乘积；否则不能。独立的定义是 (P(b|a)=P(b))。
**English**: Joint distribution gives probability of all variables taking specific values. If variables are independent, joint equals product of marginals; independence means (P(b|a)=P(b)). 

#### 3.4 边缘化（Marginalisation）与条件化（Conditioning）

**中文**：要得到某些变量的概率，可以对其他变量求和（边缘化）。给定证据 (E=e)，要计算查询变量 (X) 的后验，使用条件化：
[
P(X=x \mid E=e) = \frac{P(X=x, E=e)}{P(E=e)} = \alpha P(X=x, E=e)
]
其中 (\alpha = 1/P(E=e)) 为归一化常数。
**English**: Marginalise by summing out irrelevant variables. Conditioning gives posterior (P(X|E)=P(X,E)/P(E)=\alpha P(X,E)), where (\alpha) normalises. 

**例子（牙痛/探针/蛀牙表）**：课件给出完整联合表并通过边缘化/条件化得到 (P(\text{Cavity})=0.2) 或者 (P(\text{Cavity} \mid \text{Catch}=\text{true})=0.53) 的计算示例（见课件数据）。 

---

### 4. 贝叶斯规则（Bayes’ Rule）与 Cookie 问题（Cookie Problem）

**贝叶斯公式 / Bayes' Rule**：
[
P(a\mid b)=\frac{P(b\mid a)P(a)}{P(b)}
]
（中文）解释：把“我们想要的后验（a|b）”换成“已知 a 时 b 的概率 × a 的先验”，再除以 b 的边缘概率。
（English）Interpretation: express posterior via likelihood × prior normalized by evidence. 

**Cookie Problem（饼干问题，完整算式）**（中文+英文）：

* Bowl1: 30 vanilla, 10 chocolate → (P(\text{vanilla}|B1)=30/40=3/4)
* Bowl2: 20 vanilla, 20 chocolate → (P(\text{vanilla}|B2)=20/40=1/2)
* 选碗等概率 (P(B1)=P(B2)=1/2)
  我们观察到一块香草饼干（vanilla），求来自 Bowl1 的概率：
  [
  P(B1\mid vanilla)=\frac{P(vanilla\mid B1)P(B1)}{P(vanilla)}
  =\frac{(3/4)\times(1/2)}{(3/4)\times(1/2)+(1/2)\times(1/2)}=\frac{3/8}{5/8}=\frac{3}{5}
  ]
  **English**: Using Bayes, (P(B1|V)=3/5). 

**实用提示 / Practical note**：在实际问题中通常把右边拆成：先验（prior）、似然（likelihood）和证据（evidence），证据常用全概率（law of total probability）计算。 

---

### 5. 链式法则（Chain Rule）与条件独立的简化作用

**链式法则 / Chain rule**：
[
P(X_1,\dots,X_n)=\prod_{i=1}^n P(X_i \mid X_{1},\dots,X_{i-1})
]
（中文）解释：任意联合可按条件概率分解，但每一项可能仍依赖很多前驱变量。
**English**: Chain rule expresses any joint as product of conditional probabilities. 

**条件独立 (Conditional Independence)**：如果在给定某些变量（例如原因）时，多个子变量相互独立，那么可把复杂表格大幅压缩：
[
P(\text{Cause}, \text{Effect}_1,\dots,\text{Effect}_n)=P(\text{Cause})\prod_i P(\text{Effect}_i\mid \text{Cause})
]
（中文）这种模式使表从 (O(2^n)) 缩小到 (O(n))。 

---

### 6. 贝叶斯网络（Bayesian Networks, BNs）

#### 6.1 定义与直观语义

**中文**：贝叶斯网络是有向无环图（DAG），节点是随机变量，边表示直接影响（父→子）。每个节点带条件概率表（CPT）：(P(X_i \mid Parents(X_i)))。整网的联合概率可写为：
[
P(x_1,\dots,x_n)=\prod_{i=1}^n P(x_i \mid parents(x_i))
]
**English**: A BN is a DAG where nodes are random variables and edges encode direct influence. Joint is product of local conditionals. 

#### 6.2 CPT 与参数数量举例（Toothache 网络）

（中文）例子：Toothache—Cavity—Catch—Weather 的小网络，课件给出 P(Cavity)=0.2, P(Weather)=0.5，以及 (P(Toothache|Cavity)), (P(Catch|Cavity)) 的表格。对于完全指定的情况，直接把相关概率相乘即可，例如：
[
P(Cav \land Cat \land T) = P(Cav)P(T\mid Cav)P(Cat\mid Cav)=0.2\times0.6\times0.9=0.108
]
（English）Partially specified queries need marginalisation and normalization. 

#### 6.3 拓扑语义与条件独立（Markov assumptions）

**中文**：在 BN 中，每个节点在给定其父节点条件下，与其非后代（non-descendants）条件独立。节点的 Markov blanket（父、子、子之父）把该节点与网络其它部分隔离。
**English**: Each node is conditionally independent of non-descendants given its parents. The Markov blanket (parents, children, children's parents) shields the node from the rest. 

#### 6.4 推理（Inference）在 BN 中的步骤（中文+英文）

**Naïve/基于联合表的做法**：

* 把变量分成：查询变量（Query）、证据变量（Evidence）、隐藏变量（Hidden）
* 把涉及的 CPT 行选出来，固定证据行（conditioning），对隐藏变量求和（marginalise），最后归一化（normalize）得到概率分布。
  **English**: Separate variables into Query, Evidence, Hidden. Sum out hidden variables, fix evidence, compute unnormalized probabilities for query values, then normalise.

**示例：Alarm / Burglary 网络（课件数据）**
课件给出的网络及 CPT（部分关键值）：

* (P(Burglary)=0.001, P(Earthquake)=0.002) 等；
* (P(Alarm|B=true,E=true)=0.95) 等；
* (P(JohnCalls|Alarm=true)=0.9, P(MaryCalls|Alarm=true)=0.7)。
  （中文）问：当警报响但没有地震时，报警来自入室盗窃的概率（计算示例）——课件给出逐项乘加并归一化的计算，最终近似 (P(\text{Burglary} \mid Alarm) \approx \langle 0.97, 0.03\rangle)（分配到 true / false 的比例）。
  **English**: The Alarm/Burglary worked example shows summing combinations of John/Mary call outcomes, multiplying by relevant CPT entries and priors, then normalizing to get posterior ≈ ⟨0.97,0.03⟩ for (alarm due to burglary vs not) in that scenario. 

**计算步骤高层次列举（中文）**：

1. 将所有涉及项按链式法则展开（chain rule）。
2. 利用已知的条件独立性将条件概率替换为局部CPT（大量项被忽略）。
3. 对隐藏变量求和得到非归一化的 query 值。
4. 归一化（除以证据概率或用 α 常数）。
   （English）This yields a correct posterior without enumerating an exponentially large joint table. 

---

### 7. 小结（Summary）

* 不确定性是AI中的核心问题，概率论提供表达“相信程度”的工具；Decision theory 把概率与效用结合做决定。 
* 概率基础包括先验、条件概率、联合分布、边缘化与条件化，贝叶斯规则把后验与似然和先验联系起来。
* 贝叶斯网络通过图结构+局部 CPT 压缩表示联合分布，条件独立性是关键的语义工具；推理通过求和（marginalise）、固定证据（condition）、和归一化实现。

---
# 人工智能基础学习笔记 - 第22讲：时间不确定性 I
## Artificial Intelligence Foundation Study Notes - Lecture 22: Uncertainty over Time I

---

## 1. 时间与不确定性 | Time and Uncertainty

### 1.1 离散时间模型 | Discrete-time Models
- 将世界视为一系列快照或时间切片 | View the world as a series of snapshots or time slices
- 时间间隔Δ在每个区间都相同 | The time interval Δ is the same for every interval
- **状态变量** X_t：在时间t的状态变量（不可观察）| State variables at time t (unobservable)
- **证据变量** E_t：在时间t的观察变量（可观察）| Evidence variables at time t (observable)

### 1.2 转移模型和传感器模型 | Transition and Sensor Models
**转移模型**：P(X_t | X_{0:t-1}) - 给定先前状态的最新状态概率分布 | Probability distribution over latest state given previous values

**问题**：当t增加时，X_{0:t-1}集合的大小无限制 | Problem: unbounded size as t increases

**解决方案**：马尔可夫假设 | Solution: Markov Assumption
- 当前状态仅依赖于有限固定数量的先前状态 | Current state depends on only finite fixed number of previous states

**传感器模型**：P(E_t | X_t) - 传感器马尔可夫假设 | Sensor Markov Assumption

### 1.3 马尔可夫假设 | Markov Assumption
**一阶马尔可夫过程**：当前状态仅依赖于前一个状态 | First-order: current state depends only on previous state
```
P(X_t | X_{0:t-1}) = P(X_t | X_{t-1})
```

**二阶马尔可夫过程**：当前状态依赖于前两个状态 | Second-order: depends on previous two states

### 1.4 雨伞世界示例 | Umbrella World Example
- 一阶马尔可夫过程：下雨概率仅取决于前一天是否下雨 | Rain probability depends only on previous day's rain
- **转移模型**：P(Rain_t | Rain_{t-1})
- **传感器模型**：P(Umbrella_t | Rain_t)

**提高准确性的方法** | Ways to improve accuracy:
- 增加马尔可夫过程的阶数 | Increase order of Markov process
- 增加状态变量集合 | Increase set of state variables

---

## 2. 时间模型中的推断 | Inference in Temporal Models

### 2.1 基本推断任务 | Basic Inference Tasks

#### 2.1.1 过滤/状态估计 | Filtering/State Estimation
计算信念状态：P(X_t | e_{1:t}) | Compute belief state given all evidence

#### 2.1.2 预测 | Prediction
计算未来状态的后验分布：P(X_{t+k} | e_{1:t}) | Posterior distribution over future state

#### 2.1.3 平滑 | Smoothing
计算过去状态的后验分布：P(X_k | e_{1:t})，其中k < t | Posterior over past state given all evidence

#### 2.1.4 最可能解释 | Most Likely Explanation
找到最可能生成观察序列的状态序列 | Find state sequence most likely to generate observations

#### 2.1.5 学习 | Learning
从观察中学习转移和传感器模型 | Learn transition and sensor models from observations

---

## 3. 过滤和预测的递归公式 | Recursive Formula for Filtering and Prediction

### 3.1 问题表述 | Problem Formulation
**目标**：推导时间t+1的信念状态，给定：
- 新证据e_{t+1}
- 当前的信念状态P(X_t | e_{1:t})

**数学表达**：我们要求P(X_{t+1} | e_{1:t+1})

### 3.2 完整推导过程 | Complete Derivation Process

#### 步骤1：证据分割 | Step 1: Evidence Partitioning
```
P(X_{t+1} | e_{1:t+1}) = P(X_{t+1} | e_{1:t}, e_{t+1})
```
这里我们将证据序列e_{1:t+1}分割为已知部分e_{1:t}和新证据e_{t+1}

#### 步骤2：应用贝叶斯定理 | Step 2: Apply Bayes' Theorem
根据贝叶斯定理的条件形式：
```
P(A | B, C) = [P(B | A, C) × P(A | C)] / P(B | C)
```

应用到我们的问题：
```
P(X_{t+1} | e_{1:t}, e_{t+1}) = [P(e_{t+1} | X_{t+1}, e_{1:t}) × P(X_{t+1} | e_{1:t})] / P(e_{t+1} | e_{1:t})
```

由于分母P(e_{t+1} | e_{1:t})是归一化常数，我们写作α：
```
P(X_{t+1} | e_{1:t+1}) = α × P(e_{t+1} | X_{t+1}, e_{1:t}) × P(X_{t+1} | e_{1:t})
```

#### 步骤3：传感器马尔可夫假设 | Step 3: Sensor Markov Assumption
传感器马尔可夫假设指出：给定当前状态，**观察值独立于所有过去的观察值**
```
P(e_{t+1} | X_{t+1}, e_{1:t}) = P(e_{t+1} | X_{t+1})
```

代入公式：
```
P(X_{t+1} | e_{1:t+1}) = α × P(e_{t+1} | X_{t+1}) × P(X_{t+1} | e_{1:t})
```

#### 步骤4：展开预测项 | Step 4: Expand Prediction Term
现在展开P(X_{t+1} | e_{1:t})，这是从时间t到t+1的预测步骤：

```
P(X_{t+1} | e_{1:t}) = Σ_{x_t} P(X_{t+1} | x_t) × P(x_t | e_{1:t})
```

这里：
- P(X_{t+1} | x_t)是转移模型
- P(x_t | e_{1:t})是时间t的信念状态
- 求和是对所有可能的状态x_t

#### 步骤5：完整递归公式 | Step 5: Complete Recursive Formula
将步骤4代入步骤3，得到完整的递归更新公式（**全概率**）：

```
P(X_{t+1} | e_{1:t+1}) = α × P(e_{t+1} | X_{t+1}) × Σ_{x_t} P(X_{t+1} | x_t) × P(x_t | e_{1:t})
```

### 3.3 公式组件解释 | Formula Components Explanation

**α**：归一化常数，确保概率和为1
```
α = 1 / Σ_{x_{t+1}} [P(e_{t+1} | x_{t+1}) × Σ_{x_t} P(x_{t+1} | x_t) × P(x_t | e_{1:t})]
```

**P(e_{t+1} | X_{t+1})**：传感器模型，描述在给定状态下观察到证据的概率

**P(X_{t+1} | x_t)**：转移模型，描述状态如何随时间演变

**P(x_t | e_{1:t})**：当前信念状态，包含到目前为止的所有信息

**Σ_{x_t}**：对所有可能的前一状态求和（边际化）

---

## 4. 雨伞世界详细示例 | Umbrella World Detailed Example

### 4.1 模型参数设置 | Model Parameters
假设以下概率值：

**先验概率**：
- P(Rain_0 = true) = 0.5
- P(Rain_0 = false) = 0.5

**转移模型**：
- P(Rain_t = true | Rain_{t-1} = true) = 0.7    // 下雨后继续下雨
- P(Rain_t = true | Rain_{t-1} = false) = 0.3   // 不下雨后开始下雨
- P(Rain_t = false | Rain_{t-1} = true) = 0.3  // 下雨后停止下雨  
- P(Rain_t = false | Rain_{t-1} = false) = 0.7 // 不下雨后继续不下雨

**传感器模型**：
- P(Umbrella_t = true | Rain_t = true) = 0.9    // 下雨时带伞
- P(Umbrella_t = true | Rain_t = false) = 0.2   // 不下雨时带伞
- P(Umbrella_t = false | Rain_t = true) = 0.1   // 下雨时不带伞
- P(Umbrella_t = false | Rain_t = false) = 0.8  // 不下雨时不带伞

### 4.2 第1天详细计算 | Day 1 Detailed Calculation

#### 步骤1：初始状态 | Step 1: Initial State
```
P(Rain_0 = true) = 0.5
P(Rain_0 = false) = 0.5
```

#### 步骤2：预测（t=0→t=1）| Step 2: Prediction (t=0→t=1)
计算P(Rain_1 | e_{1:0})，由于还没有证据，e_{1:0}为空：

```
P(Rain_1 = true) = P(Rain_1 = true | Rain_0 = true) × P(Rain_0 = true)
                 + P(Rain_1 = true | Rain_0 = false) × P(Rain_0 = false)
                 = 0.7 × 0.5 + 0.3 × 0.5 = 0.35 + 0.15 = 0.5

P(Rain_1 = false) = 1 - 0.5 = 0.5
```

#### 步骤3：观察证据（U_1 = true）| Step 3: Observe Evidence
现在观察到U_1 = true（带伞），应用贝叶斯更新：

**计算未归一化的后验**：
```
P'(Rain_1 = true | U_1 = true) = P(U_1 = true | Rain_1 = true) × P(Rain_1 = true)
                               = 0.9 × 0.5 = 0.45

P'(Rain_1 = false | U_1 = true) = P(U_1 = true | Rain_1 = false) × P(Rain_1 = false)
                                = 0.2 × 0.5 = 0.10
```

**计算归一化常数α**：
```
α = 1 / (0.45 + 0.10) = 1 / 0.55 ≈ 1.8182
```

**计算归一化后验**：
```
P(Rain_1 = true | U_1 = true) = 0.45 × 1.8182 ≈ 0.8182
P(Rain_1 = false | U_1 = true) = 0.10 × 1.8182 ≈ 0.1818
```

### 4.3 第2天详细计算 | Day 2 Detailed Calculation

#### 步骤1：预测（t=1→t=2）| Step 1: Prediction (t=1→t=2)
使用第1天的后验概率进行预测：

```
P(Rain_2 = true | U_1 = true) = P(Rain_2 = true | Rain_1 = true) × P(Rain_1 = true | U_1 = true)
                             + P(Rain_2 = true | Rain_1 = false) × P(Rain_1 = false | U_1 = true)
                             = 0.7 × 0.8182 + 0.3 × 0.1818
                             = 0.5727 + 0.0545 = 0.6272

P(Rain_2 = false | U_1 = true) = 1 - 0.6272 = 0.3728
```

#### 步骤2：观察证据（U_2 = true）| Step 2: Observe Evidence
再次观察到带伞，U_2 = true：

**计算未归一化的后验**：
```
P'(Rain_2 = true | U_1 = true, U_2 = true) = P(U_2 = true | Rain_2 = true) × P(Rain_2 = true | U_1 = true)
                                          = 0.9 × 0.6272 = 0.5645

P'(Rain_2 = false | U_1 = true, U_2 = true) = P(U_2 = true | Rain_2 = false) × P(Rain_2 = false | U_1 = true)
                                           = 0.2 × 0.3728 = 0.0746
```

**计算归一化常数**：
```
α = 1 / (0.5645 + 0.0746) = 1 / 0.6391 ≈ 1.5647
```

**计算归一化后验**：
```
P(Rain_2 = true | U_1 = true, U_2 = true) = 0.5645 × 1.5647 ≈ 0.883
P(Rain_2 = false | U_1 = true, U_2 = true) = 0.0746 × 1.5647 ≈ 0.117
```

---

## 5. 过滤消息概念 | Filtering Messages Concept

### 5.1 消息传递视角 | Message Passing Perspective
我们可以将过滤过程视为消息在前向传递：

**定义消息**：f_{1:t} ≡ P(X_t | e_{1:t})

**递归更新**：
```
f_{1:t+1} = FORWARD(f_{1:t}, e_{t+1})
          = α × P(e_{t+1} | X_{t+1}) × Σ_{x_t} P(X_{t+1} | x_t) × f_{1:t}(x_t)
```

### 5.2 算法步骤 | Algorithm Steps
1. **初始化**：f_{1:0} = P(X_0)
2. **对于每个时间步t=1,2,...**：
   - **预测**：从f_{1:t-1}计算P(X_t | e_{1:t-1})
   - **更新**：结合新证据e_t得到f_{1:t}
3. **重复**直到处理完所有证据

这种递归方法的时间复杂度是线性的，使得它能够高效处理长序列数据。

---

## 6. 关键洞见 | Key Insights

### 6.1 计算效率 | Computational Efficiency
递归公式避免了重复计算，每个时间步只需要常数时间操作，使得算法能够实时处理数据流。

### 6.2 概率解释 | Probabilistic Interpretation
每个步骤都有清晰的概率意义：
- 预测步骤：基于动力学模型传播不确定性
- 更新步骤：基于观察证据修正信念

### 6.3 扩展性 | Extensibility
这个框架可以扩展到更复杂的模型，如高阶马尔可夫过程、连续状态空间（使用卡尔曼滤波）等。

# 学习笔记：Uncertainty over Time II（修订版）

## 1. 时间模型中的推理 (Inference in Temporal Models)

### 1.1 平滑 (Smoothing)

**通俗解释**：平滑就像"回头看"——当我们有了完整的观测数据序列后，重新评估过去某个时刻的状态概率。这比仅基于当时信息的过滤更准确。

**数学定义**：平滑计算 $P(X_k \mid e_{1:t})$，即在给定从时刻1到t的完整观测序列条件下，过去某个时刻k的状态后验概率分布。

**完整数学推导**：

**步骤1：基础贝叶斯分解**
$$P(X_k \mid e_{1:t}) = \frac{P(X_k, e_{1:t})}{P(e_{1:t})} = \alpha P(X_k, e_{1:t})$$

**步骤2：证据序列分割**
将证据序列分为三部分：$e_{1:k-1}$, $e_k$, $e_{k+1:t}$
$$P(X_k, e_{1:t}) = P(X_k, e_{1:k}, e_{k+1:t})$$

**步骤3：条件独立性应用**
利用马尔可夫性质：
$$P(e_{k+1:t} \mid X_k, e_{1:k}) = P(e_{k+1:t} \mid X_k)$$

因此：
$$P(X_k, e_{1:t}) = P(X_k \mid e_{1:k}) \cdot P(e_{1:k}) \cdot P(e_{k+1:t} \mid X_k)$$

**步骤4：最终平滑公式**
$$P(X_k \mid e_{1:t}) = \alpha P(X_k \mid e_{1:k}) \cdot P(e_{k+1:t} \mid X_k)$$

其中：
- $\alpha$ 是归一化常数
- $P(X_k \mid e_{1:k})$ 是前向消息（通过过滤得到）
- $P(e_{k+1:t} \mid X_k)$ 是后向消息

### 1.2 后向消息的递归推导

**定义后向消息**：$b_{k:t} = P(e_{k:t} \mid X_{k-1})$

**递归关系推导**：
$$b_{k:t} = P(e_{k:t} \mid X_{k-1})$$
$$= \sum_{x_k} P(e_k, e_{k+1:t}, x_k \mid X_{k-1})$$
$$= \sum_{x_k} P(e_k \mid x_k) \cdot P(e_{k+1:t} \mid x_k) \cdot P(x_k \mid X_{k-1})$$

**最终递归公式**：
$$b_{k:t} = \sum_{x_k} P(e_k \mid x_k) \cdot b_{k+1:t} \cdot P(x_k \mid X_{k-1})$$

**边界条件**：$b_{t+1:t} = P(\emptyset \mid X_t) = 1$

### 1.3 雨伞例子详细数学计算

**问题参数**：
- 初始概率：$P(R_0) = ⟨0.5, 0.5⟩$
- 转移概率：$T = \begin{bmatrix} 0.7 & 0.3 \\ 0.3 & 0.7 \end{bmatrix}$
- 观测概率：$P(u|Rain) = 0.9$, $P(u|¬Rain) = 0.2$

**已知前向计算结果**：
$$P(R_1 \mid u_1) = ⟨0.818, 0.182⟩$$

**计算后向消息** $b_{2:2} = P(u_2 \mid R_1)$：

对于 $R_1 = Rain$：
$$P(u_2 \mid Rain) = P(u_2 \mid R_2=Rain)P(R_2=Rain \mid Rain) + P(u_2 \mid R_2=¬Rain)P(R_2=¬Rain \mid Rain)$$
$$= 0.9 × 0.7 + 0.2 × 0.3 = 0.69$$

对于 $R_1 = ¬Rain$：
$$P(u_2 \mid ¬Rain) = P(u_2 \mid R_2=Rain)P(R_2=Rain \mid ¬Rain) + P(u_2 \mid R_2=¬Rain)P(R_2=¬Rain \mid ¬Rain)$$
$$= 0.9 × 0.3 + 0.2 × 0.7 = 0.41$$

**平滑计算**：
$$P(R_1 \mid u_1,u_2) = \alpha × P(R_1 \mid u_1) × P(u_2 \mid R_1)$$
$$= \alpha × ⟨0.818, 0.182⟩ × ⟨0.69, 0.41⟩$$
$$= \alpha × ⟨0.564, 0.075⟩$$

**归一化**：
$$\alpha = \frac{1}{0.564 + 0.075} = \frac{1}{0.639} ≈ 1.565$$
$$P(R_1 \mid u_1,u_2) = ⟨0.564×1.565, 0.075×1.565⟩ = ⟨0.883, 0.117⟩$$

### 1.4 维特比算法的数学推导

**目标**：找到最可能的状态序列 $\arg\max_{x_{1:t}} P(x_{1:t} \mid e_{1:t})$

**概率分解**：
$$P(x_{1:t} \mid e_{1:t}) = \frac{P(x_{1:t}, e_{1:t})}{P(e_{1:t})} = \alpha P(x_{1:t}, e_{1:t})$$

**联合概率分解**：
$$P(x_{1:t}, e_{1:t}) = P(x_1)P(e_1 \mid x_1) \prod_{k=2}^t P(x_k \mid x_{k-1})P(e_k \mid x_k)$$

**定义最大概率**：
令 $m_{1:t}(x_t) = \max_{x_{1:t-1}} P(x_{1:t}, e_{1:t})$

**递归关系推导**：
$$m_{1:t}(x_t) = \max_{x_{1:t-1}} \left[ P(x_{1:t-1}, e_{1:t-1}) \cdot P(x_t \mid x_{t-1}) \cdot P(e_t \mid x_t) \right]$$
$$= P(e_t \mid x_t) \cdot \max_{x_{t-1}} \left[ P(x_t \mid x_{t-1}) \cdot \max_{x_{1:t-2}} P(x_{1:t-1}, e_{1:t-1}) \right]$$
$$= P(e_t \mid x_t) \cdot \max_{x_{t-1}} \left[ P(x_t \mid x_{t-1}) \cdot m_{1:t-1}(x_{t-1}) \right]$$

**最终递归公式**：
$$m_{1:t}(x_t) = P(e_t \mid x_t) \cdot \max_{x_{t-1}} \left[ P(x_t \mid x_{t-1}) \cdot m_{1:t-1}(x_{t-1}) \right]$$

**初始条件**：$m_{1:1}(x_1) = P(x_1)P(e_1 \mid x_1)$

## 2. 隐马尔可夫模型 (Hidden Markov Models, HMM)

### 2.1 HMM的矩阵表示数学推导

**状态转移矩阵**：
$$T_{i,j} = P(X_t = j \mid X_{t-1} = i)$$

**观测概率矩阵**：
对于离散观测：$O_{i,k} = P(E_t = k \mid X_t = i)$
对于连续观测：使用概率密度函数

**前向算法的矩阵形式**：
$$f_{1:t} = \alpha \cdot O_t \cdot T^T \cdot f_{1:t-1}$$

其中：
- $f_{1:t}$ 是时刻t的前向概率向量
- $O_t$ 是以 $P(e_t \mid X_t=i)$ 为对角元素的对角矩阵
- $T$ 是转移矩阵

### 2.2 固定滞后平滑的数学推导

**目标**：计算 $P(X_{t-d} \mid e_{1:t})$

**数学推导**：
$$P(X_{t-d} \mid e_{1:t}) = \alpha P(X_{t-d} \mid e_{1:t-d}) \cdot P(e_{t-d+1:t} \mid X_{t-d})$$

**后向变换矩阵**：
定义 $B_{t-d:t} = P(e_{t-d+1:t} \mid X_{t-d})$

**递归更新**：
$$B_{t-d:t} = P(e_{t-d+1} \mid X_{t-d+1}) \cdot P(X_{t-d+1} \mid X_{t-d}) \cdot B_{t-d+1:t}$$
$$= O_{t-d+1} \cdot T \cdot B_{t-d+1:t}$$

**矩阵形式**：
$$B_{t-d:t} = \prod_{k=t-d+1}^t O_k \cdot T$$

### 2.3 机器人定位的数学模型

**状态空间**：42个网格位置，状态变量 $X_t \in \{1, 2, ..., 42\}$

**观测模型**：
$$P(E_t = e \mid X_t = x) = \prod_{d=1}^4 P(E_t^d = e^d \mid X_t = x)$$

其中 $e^d$ 表示第d个方向的障碍物信息（N, E, S, W）

**传感器误差模型**：
$$P(E_t^d = \text{correct} \mid X_t) = 1 - \epsilon$$
$$P(E_t^d = \text{incorrect} \mid X_t) = \epsilon$$

## 3. 核心数学理论总结

### 3.1 条件独立性的关键作用

**一阶马尔可夫假设**：
$$P(X_t \mid X_{1:t-1}) = P(X_t \mid X_{t-1})$$

**观测独立性**：
$$P(E_t \mid X_{1:t}, E_{1:t-1}) = P(E_t \mid X_t)$$

### 3.2 概率归一化的数学处理

对于任意非归一化概率向量 $v = ⟨v_1, v_2, ..., v_n⟩$，归一化公式为：
$$\text{Normalize}(v) = \left\langle \frac{v_1}{\sum v_i}, \frac{v_2}{\sum v_i}, ..., \frac{v_n}{\sum v_i} \right\rangle$$


# L25-L27 自动化规划学习笔记  
**Artificial Intelligence Foundation - Automated Planning Study Notes**  

---

## 一、搜索与规划的区别（Search vs. Planning）  
### 1.1 核心差异（Core Differences）  
- **搜索（Search）**  
  - 状态空间搜索：通过枚举所有可能路径寻找目标（e.g. BFS, DFS）。  
  - *Search in state space: Enumerate all possible paths to reach the goal (e.g., BFS, DFS).*  
  - 问题表示为**状态转移图**（State Transition Graph），每个节点是完整状态，边是动作。  
    *Problems are represented as a state transition graph where each node is a full state and edges are actions.*  

- **规划（Planning）**  
  - **逻辑命题表示**：用谓词逻辑描述状态、动作和目标（e.g. STRIPS, PDDL）。  
    *Logical representation: Use predicate logic to describe states, actions, and goals (e.g., STRIPS, PDDL).*  
  - 关注**部分可观察性**和**动作分解**（e.g. 买书问题只需直接执行Buy(1234567890)）。  
    *Focuses on partial observability and action decomposition (e.g., in the book-buying problem, only Buy(1234567890) is needed).*  

**例子**：  
- **搜索问题**：超市购物时穷举所有可能路径，包括无关动作（如买宠物狗）。  
  *Search problem: Enumerate all possible paths in a supermarket, including irrelevant actions (e.g., buying a pet dog).*  
- **规划问题**：直接推理出“买书”动作满足目标，无需遍历无关路径。  
  *Planning problem: Directly infer that the "Buy" action satisfies the goal without exploring irrelevant paths.*  

---

## 二、规划问题表示（Planning Problem Representation）  
### 2.1 STRIPS 表示法  
**定义**（Definition）：  
STRIPS（Stanford Research Institute Problem Solver）通过以下三要素定义规划问题：  
1. **状态**（State）：正文字合取（e.g., `At(P1, JFK) ∧ At(C1, SFO)`）。  
   *A conjunction of positive literals (e.g., `At(P1, JFK) ∧ At(C1, SFO)`).*  
2. **动作**（Action）：包含**前提条件**（Precondition）和**效果**（Effect）。  
   *Actions include preconditions and effects.*  
3. **目标**（Goal）：部分指定的状态（e.g., `At(C1, JFK) ∧ At(C2, SFO)`）。  
   *A partially specified state (e.g., `At(C1, JFK) ∧ At(C2, SFO)`).*  

**动作公式**（Action Schema）：  
```lisp
Action(Fly(p, from, to),  
       PRECOND: At(p, from) ∧ Plane(p) ∧ Airport(from) ∧ Airport(to),  
       EFFECT: ¬At(p, from) ∧ At(p, to))
```

**语义**（Semantics）：  
- **适用性**：当前状态满足前提条件时，动作可执行。  
  *Applicable if the current state satisfies the preconditions.*  
- **状态转换**：  
  - 添加效果中的正文字（Add positive literals）。  
  - 删除效果中的负文字（Remove negative literals）。  
  - **STRIPS假设**：未提及的条件保持不变（解决帧问题）。  
    *STRIPS assumption: Unmentioned conditions remain unchanged (solves the frame problem).*  

**例子**：  
- **初始状态**（Initial State）：  
  ```lisp
  At(C1, SFO) ∧ At(C2, JFK) ∧ At(P1, SFO) ∧ At(P2, JFK)
  ```  
- **目标状态**（Goal State）：  
  ```lisp
  At(C1, JFK) ∧ At(C2, SFO)
  ```  
- **动作序列**（Plan）：  
  1. `Load(C1, P1, SFO)`  
  2. `Fly(P1, SFO, JFK)`  
  3. `Unload(C1, P1, JFK)`  
  4. `Load(C2, P2, JFK)`  
  5. `Fly(P2, JFK, SFO)`  
  6. `Unload(C2, P2, SFO)`  

---

### 2.2 PDDL 表示法  
**定义**（Definition）：  
PDDL（Planning Domain Definition Language）是经典规划的标准语言，分为两个文件：  
1. **Domain 文件**：定义谓词、常量和动作。  
   *Defines predicates, constants, and actions.*  
2. **Problem 文件**：定义对象、初始状态和目标。  
   *Defines objects, initial state, and goal.*  

**示例**（Example - Gripper Task）：  
```lisp
; Domain 文件  
(define (domain gripper)  
  (:predicates (room ?r) (ball ?b) (at-robby ?r) (at ?b ?r) (free ?g) (carry ?o ?g))  
  (:action move  
    :parameters (?from ?to)  
    :precondition (and (room ?from) (room ?to) (at-robby ?from))  
    :effect (and (at-robby ?to) (not (at-robby ?from))))  
  (:action pick  
    :parameters (?obj ?room ?gripper)  
    :precondition (and (ball ?obj) (room ?room) (at ?obj ?room) (at-robby ?room) (free ?gripper))  
    :effect (and (carry ?obj ?gripper) (not (at ?obj ?room)) (not (free ?gripper))))  
  (:action drop  
    :parameters (?obj ?room ?gripper)  
    :precondition (and (ball ?obj) (room ?room) (carry ?obj ?gripper) (at-robby ?room))  
    :effect (and (at ?obj ?room) (free ?gripper) (not (carry ?obj ?gripper))))  
)

; Problem 文件  
(define (problem gripper-four-balls)  
  (:domain gripper)  
  (:objects rooma roomb ball1 ball2 ball3 ball4 left right)  
  (:init (room rooma) (room roomb) (ball ball1) (ball ball2) (ball ball3) (ball ball4)  
         (gripper left) (gripper right) (at-robby rooma) (free left) (free right)  
         (at ball1 rooma) (at ball2 rooma) (at ball3 rooma) (at ball4 rooma))  
  (:goal (and (at ball1 roomb) (at ball2 roomb) (at ball3 roomb) (at ball4 roomb)))  
)
```

---

## 三、前向与后向搜索（Forward and Backward Search）  
### 3.1 前向搜索（Progression Planning）  
- **过程**：从初始状态出发，逐步应用动作直到满足目标。  
  *Start from the initial state and apply actions until the goal is met.*  
- **优点**：直观，适合状态空间较小的问题。  
  *Advantages: Intuitive, suitable for small state spaces.*  
- **缺点**：状态空间爆炸，需高效启发式函数（如A*算法）。  
  *Disadvantages: State space explosion, requires efficient heuristics (e.g., A*).*  

**启发式函数**（Heuristic Function）：  
- **忽略前提条件**（Ignoring Precondition Heuristic）：  
  - 假设所有前提条件已满足，计算最少动作数。  
    *Assume all preconditions are satisfied, calculate the minimal number of actions.*  
- **曼哈顿距离**（Manhattan Distance）：  
  - 适用于块世界问题，计算物体位置与目标的差值。  
    *Used in the Blocks World, calculates the difference between object positions and goals.*  

---

### 3.2 后向搜索（Regression Planning）  
- **过程**：从目标状态反向推导，仅保留相关动作。  
  *Start from the goal state and regress to the initial state, keeping only relevant actions.*  
- **关键步骤**（Key Steps）：  
  1. **目标回归**（Goal Regression）：  
     - 对目标中的每个子目标，找到可实现的动作。  
     *For each subgoal in the goal, find the actions that achieve it.*  
  2. **预处理状态**（Predecessor State）：  
     - 根据动作的前提条件生成前驱状态。  
     *Generate predecessor states based on action preconditions.*  

**例子**（Air Cargo Transportation）：  
- **目标**：`At(C1, JFK) ∧ At(C2, SFO)`  
- **后向搜索步骤**：  
  1. 选择 `Unload(C1, P1, JFK)` 实现 `At(C1, JFK)`。  
  2. 需要前提 `In(C1, P1) ∧ At(P1, JFK)`。  
  3. 反推 `Load(C1, P1, SFO)` 和 `Fly(P1, SFO, JFK)`。  

---

## 四、规划复杂度分析（Complexity of Classical Planning）  
### 4.1 复杂度类（Complexity Classes）  
| 复杂度类 | 描述 |  
|----------|------|  
| **NP-Hard** | 非确定性多项式时间难问题（e.g., Plan-Existence）。 |  
| **PSPACE-Complete** | 多项式空间完全问题（e.g., Plan-Length with ADL）。 |  
| **NEXPTIME** | 非确定性指数时间问题（e.g., 多层条件效果规划）。 |  

### 4.2 条件对复杂度的影响（Impact of Features）  
| 条件 | 复杂度 |  
|------|--------|  
| 允许负效果（Negative Effects） | NP-Hard → PSPACE-Complete |  
| 条件效果（Conditional Effects） | PSPACE-Complete → NEXPTIME |  
| 函数符号（Function Symbols） | 经典规划 → 非经典规划（需更复杂模型）。 |  

---

## 五、规划图（Planning Graph）  
**定义**（Definition）：  
规划图是一种分层数据结构，用于高效计算启发式函数（如**层级状态评估**）。  
*A hierarchical data structure for efficiently computing heuristics (e.g., level-based state evaluation).*  

**构造过程**（Construction Steps）：  
1. **初始层**（Level 0）：包含初始状态的所有命题。  
2. **动作层**（Action Level）：添加所有适用动作。  
3. **命题层**（Proposition Level）：由动作效果生成新命题。  
4. **互斥关系**（Mutual Exclusion）：标记冲突动作或命题。  

**应用**（Application）：  
- **最大层级数**（Max Level）：估计最短计划长度。  
  *The maximum level number estimates the minimal plan length.*  
- **互斥解析**（Mutex Resolution）：剪枝不可行路径。  

---

## 六、总结（Summary）  
1. **搜索 vs. 规划**：规划利用逻辑表示减少冗余搜索。  
   *Planning reduces redundant search by using logical representations.*  
2. **表示语言**：STRIPS/PDDL 是经典规划的核心工具。  
   *STRIPS/PDDL are core tools for classical planning.*  
3. **搜索策略**：前向搜索适合小规模问题，后向搜索更高效。  
   *Forward search suits small problems; backward search is more efficient.*  
4. **复杂度**：规划问题的复杂度受前提条件、效果等条件影响。  
   *Complexity depends on preconditions, effects, etc.*  

--- 

**附录**（Appendix）：  
- **块世界（Blocks World）PDDL 示例**：  
  ```lisp
  (define (domain blocks)  
    (:predicates (on ?a ?b) (block ?b) (clear ?b))  
    (:action move  
      :parameters (?b ?x ?y)  
      :precondition (and (on ?b ?x) (clear ?y) (clear ?b) (block ?b) (block ?y))  
      :effect (and (on ?b ?y) (clear ?x) (not (on ?b ?x)) (not (clear ?y))))  
    (:action movetotable  
      :parameters (?b ?x)  
      :precondition (and (on ?b ?x) (clear ?b) (block ?b))  
      :effect (and (on ?b table) (clear ?x) (not (on ?b ?x))))  
  )
  ```


  