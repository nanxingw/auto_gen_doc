# **Teams 模块文档**

---

## **1. 模块概述**

`teams` 模块定义了多代理协作的框架，支持多种团队模型（如轮询式、选择器式、接力式），实现代理间的高效任务协作和消息交换。

**总体介绍：**
1. **`BaseGroupChat`**：定义团队聊天的基础类，负责多代理协作的核心逻辑。
2. **`BaseGroupChatManager`**：管理团队消息流、发言者选择和终止条件。
3. **`RoundRobinGroupChat`**：基于轮询逻辑的团队模型，每个参与者依次发言。
4. **`SelectorGroupChat`**：基于模型或自定义逻辑选择发言者的团队模型。
5. **`Swarm`**：基于 `HandoffMessage` 实现接力选择发言者的团队模型。
6. **`ChatAgentContainer`**：适配代理到团队框架的辅助工具。
7. **`SequentialRoutedAgent`**：保证消息按顺序处理的辅助组件。
8. **事件类型**：如 `GroupChatStart`、`GroupChatAgentResponse` 和 `GroupChatTermination`，用于支持消息流转和任务控制。

---

## **2. 核心基类**

### **2.1. BaseGroupChat**
- **描述**：团队聊天的基础类，定义了多代理协作的核心逻辑。
- **主要字段**：
  - **`_participants: List[ChatAgent]`**：参与者代理。
  - **`_termination_condition: TerminationCondition`**：终止条件。
- **主要方法**：
  - **`run`**：运行团队任务，返回最终结果。
  - **`run_stream`**：运行任务并生成消息流。
  - **`reset`**：重置团队状态。
  - **`save_state / load_state`**：保存/加载团队状态。

### **2.2. BaseGroupChatManager**
- **描述**：管理团队的消息流转、发言者选择和终止条件。
- **主要字段**：
  - **`_message_thread: List[AgentMessage]`**：消息线程。
  - **`_termination_condition: TerminationCondition`**：终止条件。
- **主要方法**：
  - **`select_speaker`**：选择下一发言者。
  - **`validate_group_state`**：验证团队状态。

---

## **3. 特定团队实现**

### **3.1. RoundRobinGroupChat**
- **描述**：参与者按顺序轮流发言。
- **依赖**：`RoundRobinGroupChatManager`
- **主要方法**：
  - **`select_speaker`**：根据轮询逻辑选择下一发言者。

### **3.2. SelectorGroupChat**
- **描述**：基于模型或自定义逻辑选择下一发言者。
- **依赖**：`SelectorGroupChatManager`
- **主要字段**：
  - **`_selector_prompt: str`**：选择发言者的提示模板。
  - **`_allow_repeated_speaker: bool`**：是否允许连续发言。
- **主要方法**：
  - **`select_speaker`**：使用模型或选择器函数选择发言者。

### **3.3. Swarm**
- **描述**：通过 `HandoffMessage` 接力选择下一发言者。
- **依赖**：`SwarmGroupChatManager`
- **主要方法**：
  - **`select_speaker`**：根据最近的 `HandoffMessage` 确定发言者。

---

## **4. 辅助组件**

### **4.1. ChatAgentContainer**
- **描述**：封装代理以适配团队框架。
- **主要方法**：
  - **`handle_start / handle_reset`**：处理启动和重置事件。
  - **`save_state / load_state`**：保存/加载代理状态。

### **4.2. SequentialRoutedAgent**
- **描述**：确保消息按顺序处理。
- **主要字段**：
  - **`_fifo_lock: FIFOLock`**：保证消息顺序。

---

## **5. 事件类型**

### **5.1. GroupChatStart**
- **描述**：启动团队任务的消息。
- **字段**：
  - **`messages: List[ChatMessage] | None`**

### **5.2. GroupChatAgentResponse**
- **描述**：代理的响应消息。
- **字段**：
  - **`agent_response: Response`**

### **5.3. GroupChatTermination**
- **描述**：团队终止消息。
- **字段**：
  - **`message: StopMessage`**

---

## **6. 模块总结**
- **灵活的团队模型**：支持多种发言者选择逻辑（轮询、模型选择、接力）。
- **核心功能组件**：通过 `BaseGroupChat` 和 `BaseGroupChatManager` 提供统一接口。
- **丰富的事件支持**：通过标准化消息类型实现消息流转。

`teams` 模块为多代理协作任务提供了高效、可扩展的实现框架。

