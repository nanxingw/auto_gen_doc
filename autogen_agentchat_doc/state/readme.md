# **State 模块文档**

---

## **1. 模块概述**

`state` 模块负责管理代理、团队和终止条件的状态数据。所有状态类基于 `BaseState`，通过标准化的字段和方法实现状态的保存与恢复。

**总体介绍：**
1. **`BaseState`**：所有状态类的基类，提供统一的接口。
2. **`AssistantAgentState`**：存储助手代理（Assistant Agent）的状态。
3. **`SocietyOfMindAgentState`**：存储 `SocietyOfMindAgent` 的团队状态。
4. **`TeamState`**：存储整个团队的状态，包括每个代理的状态。
5. **`ChatAgentContainerState`**：存储代理容器的状态和消息缓冲区。
6. **`BaseGroupChatManagerState`**：所有团队管理器状态的基础类。
7. **`RoundRobinManagerState`**：存储轮询式团队的管理器状态。
8. **`SelectorManagerState`**：存储选择器式团队的管理器状态。
9. **`SwarmManagerState`**：存储接力式团队的管理器状态。
10. **`MagenticOneOrchestratorState`**：存储 `MagenticOne` 团队管理器的状态。

---

## **2. 核心基类**

### **BaseState**
- **描述**：所有可保存状态的基类。
- **主要字段**：
  - **`type: str`**：状态类型标识，默认值为 `"BaseState"`。
  - **`version: str`**：状态版本号，默认值为 `"1.0.0"`。

---

## **3. 代理相关状态**

### **AssistantAgentState**
- **描述**：存储助手代理（Assistant Agent）的状态。
- **主要字段**：
  - **`llm_messages: List[LLMMessage]`**：助手代理处理的 LLM 消息记录。
  - **`type: str`**：默认值为 `"AssistantAgentState"`。

### **SocietyOfMindAgentState**
- **描述**：存储 `SocietyOfMindAgent` 的内部团队状态。
- **主要字段**：
  - **`inner_team_state: Mapping[str, Any]`**：存储内部团队的状态。
  - **`type: str`**：默认值为 `"SocietyOfMindAgentState"`。

---

## **4. 团队相关状态**

### **TeamState**
- **描述**：存储整个团队的状态。
- **主要字段**：
  - **`agent_states: Mapping[str, Any]`**：每个代理的状态。
  - **`team_id: str`**：团队唯一标识符。
  - **`type: str`**：默认值为 `"TeamState"`。

### **ChatAgentContainerState**
- **描述**：存储代理容器的状态。
- **主要字段**：
  - **`agent_state: Mapping[str, Any]`**：代理的状态。
  - **`message_buffer: List[ChatMessage]`**：消息缓冲区。
  - **`type: str`**：默认值为 `"ChatAgentContainerState"`。

---

## **5. 团队管理器状态**

### **BaseGroupChatManagerState**
- **描述**：所有团队管理器状态的基础类。
- **主要字段**：
  - **`message_thread: List[AgentMessage]`**：消息线程记录。
  - **`current_turn: int`**：当前回合计数。
  - **`type: str`**：默认值为 `"BaseGroupChatManagerState"`。

### **RoundRobinManagerState**
- **描述**：轮询式团队（`RoundRobinGroupChat`）的管理器状态。
- **主要字段**：
  - **`next_speaker_index: int`**：下一发言者的索引。
  - **`type: str`**：默认值为 `"RoundRobinManagerState"`。

### **SelectorManagerState**
- **描述**：选择器式团队（`SelectorGroupChat`）的管理器状态。
- **主要字段**：
  - **`previous_speaker: Optional[str]`**：上一次发言者。
  - **`type: str`**：默认值为 `"SelectorManagerState"`。

### **SwarmManagerState**
- **描述**：接力式团队（`Swarm`）的管理器状态。
- **主要字段**：
  - **`current_speaker: str`**：当前发言者。
  - **`type: str`**：默认值为 `"SwarmManagerState"`。

### **MagenticOneOrchestratorState**
- **描述**：`MagenticOne` 团队管理器的状态。
- **主要字段**：
  - **`task: str`**：当前任务描述。
  - **`facts: str`**：已知事实。
  - **`plan: str`**：当前计划。
  - **`n_rounds: int`**：已完成回合数。
  - **`n_stalls: int`**：连续停滞计数。
  - **`type: str`**：默认值为 `"MagenticOneOrchestratorState"`。

---

## **6. 状态类导出**

`state` 模块通过 `__all__` 导出以下状态类：
- **基础类**：
  - `BaseState`
- **代理状态**：
  - `AssistantAgentState`
  - `SocietyOfMindAgentState`
- **团队状态**：
  - `TeamState`
  - `ChatAgentContainerState`
- **管理器状态**：
  - `BaseGroupChatManagerState`
  - `RoundRobinManagerState`
  - `SelectorManagerState`
  - `SwarmManagerState`
  - `MagenticOneOrchestratorState`

---

## **7. 模块总结**
- **灵活的状态管理**：所有状态类基于 `BaseState`，提供一致的接口。
- **支持团队与代理的状态存储**：团队管理器和代理都有独立的状态类。
- **多样的团队管理器**：支持轮询、选择器和接力模式的状态管理。

通过 `state` 模块，代理和团队可以在复杂任务执行中保持状态一致性，支持任务中断与恢复。

