# **MagenticOne 模块文档**

---

## **1. 模块概述**

`MagenticOne` 模块定义了一个团队协作框架，用于高效地完成复杂任务。通过组织和管理代理间的交互，`MagenticOneGroupChat` 和 `MagenticOneOrchestrator` 协同工作，确保任务按照计划和事实进展。

**总体介绍：**
1. **`MagenticOneGroupChat`**：团队协作框架的主入口，管理代理的交互和任务流。
2. **`MagenticOneOrchestrator`**：团队任务的核心管理器，使用任务和进度日志来协调团队。
3. **Prompts**：提供标准化的提示，用于团队规划和问题解决。

**主要功能：**
- 提供团队协作框架，用于管理代理的任务分工和执行。
- 使用提示 (prompts) 引导任务的计划、事实记录和进度更新。
- 支持复杂任务的规划、进度检查和最终回答生成。

---

## **2. 核心类**

### **2.1. MagenticOneGroupChat**

- **描述**：团队协作框架的主入口，用于管理代理间的交互和任务流&#8203;:contentReference[oaicite:0]{index=0}。
- **依赖关系**：
  - 依赖 `MagenticOneOrchestrator` 来处理团队的消息流和任务分配&#8203;:contentReference[oaicite:1]{index=1}。
  - 使用 `ChatCompletionClient` 生成模型的响应。
- **主要字段**：
  - **`_model_client: ChatCompletionClient`**：模型客户端，用于生成对话响应。
  - **`_max_stalls: int`**：最大允许停滞回合数，默认为 3。
  - **`_final_answer_prompt: str`**：用于生成最终答案的提示。
- **主要方法**：
  1. **`__init__`**
     - **参数**：
       - `participants: List[ChatAgent]`：团队参与者。
       - `model_client: ChatCompletionClient`：模型客户端。
       - `termination_condition: TerminationCondition | None`：终止条件。
       - `max_turns: int | None`：最大回合数。
       - `max_stalls: int`：最大允许停滞回合数。
       - `final_answer_prompt: str`：最终答案的提示。
     - **作用**：初始化团队协作框架。
     - **异常**：如果未提供任何参与者，抛出 `ValueError`。
  2. **`_create_group_chat_manager_factory`**
     - **作用**：创建并返回 `MagenticOneOrchestrator` 的工厂方法&#8203;:contentReference[oaicite:2]{index=2}。

---

### **2.2. MagenticOneOrchestrator**

- **描述**：负责管理团队任务的核心管理器，协调参与者并监督任务进展&#8203;:contentReference[oaicite:3]{index=3}。
- **依赖关系**：
  - 继承自 `BaseGroupChatManager`。
  - 使用 `MagenticOneOrchestratorState` 管理状态。
  - 使用多种提示（如 `ORCHESTRATOR_FINAL_ANSWER_PROMPT`）引导任务&#8203;:contentReference[oaicite:4]{index=4}。
- **主要字段**：
  - **`_model_client: ChatCompletionClient`**：模型客户端。
  - **`_max_stalls: int`**：最大停滞回合数。
  - **`_task, _facts, _plan: str`**：任务描述、已知事实和当前计划。
  - **`_n_rounds: int`**：当前回合数。
  - **`_n_stalls: int`**：当前停滞回合数。
- **主要方法**：
  1. **`handle_start`**
     - **作用**：处理任务的启动逻辑，包括事实记录和初步规划&#8203;:contentReference[oaicite:5]{index=5}。
  2. **`handle_agent_response`**
     - **作用**：处理代理的响应，更新消息线程并协调下一步任务&#8203;:contentReference[oaicite:6]{index=6}。
  3. **`_orchestrate_step`**
     - **作用**：团队内部的任务调度逻辑，选择下一发言者并更新进度&#8203;:contentReference[oaicite:7]{index=7}。
  4. **`_prepare_final_answer`**
     - **作用**：生成最终的任务答案，并标记任务完成&#8203;:contentReference[oaicite:8]{index=8}。
  5. **`save_state / load_state`**
     - **作用**：保存或恢复当前的任务状态。

---

## **3. 提示 (Prompts)**

### **描述**
Prompts 是 `MagenticOne` 模块的核心组件之一，提供了一系列标准化的任务引导提示，用于规划、事实记录、进度检查和问题解决&#8203;:contentReference[oaicite:9]{index=9}。

### **主要提示**：
1. **`ORCHESTRATOR_TASK_LEDGER_FACTS_PROMPT`**  
   - **作用**：引导团队列出任务的已知事实、需要查找的事实和推导的事实。
2. **`ORCHESTRATOR_TASK_LEDGER_PLAN_PROMPT`**  
   - **作用**：根据团队的组成制定任务计划。
3. **`ORCHESTRATOR_TASK_LEDGER_FULL_PROMPT`**  
   - **作用**：综合任务描述、事实和计划，生成团队的初步工作内容。
4. **`ORCHESTRATOR_PROGRESS_LEDGER_PROMPT`**  
   - **作用**：检查任务的进度，判断是否完成、是否停滞或重复。
5. **`ORCHESTRATOR_TASK_LEDGER_FACTS_UPDATE_PROMPT`**  
   - **作用**：更新任务的事实列表。
6. **`ORCHESTRATOR_TASK_LEDGER_PLAN_UPDATE_PROMPT`**  
   - **作用**：重新规划任务，避免重复错误。
7. **`ORCHESTRATOR_FINAL_ANSWER_PROMPT`**  
   - **作用**：生成任务的最终答案，以回答用户的请求。

---

## **4. 模块导出**

`MagenticOne` 模块通过 `__all__` 导出以下内容：
- **类**：
  - `MagenticOneGroupChat`
  - `MagenticOneOrchestrator`
- **提示**：
  - `ORCHESTRATOR_TASK_LEDGER_FACTS_PROMPT`
  - `ORCHESTRATOR_TASK_LEDGER_PLAN_PROMPT`
  - `ORCHESTRATOR_TASK_LEDGER_FULL_PROMPT`
  - `ORCHESTRATOR_PROGRESS_LEDGER_PROMPT`
  - `ORCHESTRATOR_TASK_LEDGER_FACTS_UPDATE_PROMPT`
  - `ORCHESTRATOR_TASK_LEDGER_PLAN_UPDATE_PROMPT`
  - `ORCHESTRATOR_FINAL_ANSWER_PROMPT`

---

## **5. 模块总结**

- **高效的团队管理**：`MagenticOneGroupChat` 和 `MagenticOneOrchestrator` 协作，管理代理的任务分配和执行。
- **标准化提示支持**：通过一系列提示，引导任务规划、事实记录和问题解决。
- **支持任务状态管理**：保存和恢复任务状态，确保任务执行的连续性。

通过 `MagenticOne` 模块，复杂任务的团队协作变得更高效和结构化。

