# **Agents 模块文档**

---

## **1. 模块概述**

`agents` 模块定义了多种类型的代理（Agent），每种代理具有不同的功能和应用场景。所有代理类直接或间接继承了 `BaseChatAgent`，并实现了处理消息的核心方法。

**总体介绍：**
1. **`BaseChatAgent`**：所有代理的基类，定义了代理的通用接口和核心功能。
2. **`AssistantAgent`**：通用助手代理类，支持工具调用和消息处理。
3. **`CodeExecutorAgent`**：执行代码的代理类，从消息中提取代码块并运行。
4. **`CodingAssistantAgent`**：专注于代码生成的代理（已废弃）。
5. **`SocietyOfMindAgent`**：维护一个内部团队，协作完成任务的代理。
6. **`ToolUseAssistantAgent`**：提供工具使用功能的代理（已废弃）。
7. **`UserProxyAgent`**：模拟人类用户的代理，通过输入函数交互。

**主要功能：**
- 提供一系列代理类，用于与用户或其他代理协作。
- 支持消息处理、工具调用、代码执行、团队协作等任务。

**依赖关系：**
- 依赖 `base` 模块中的核心类 `BaseChatAgent` 和 `Response`。
- 依赖 `messages` 模块中的消息类 `ChatMessage`、`TextMessage`、`HandoffMessage` 等。

---

## **2. 类：BaseChatAgent**

### **描述**
`BaseChatAgent` 是所有代理的基类，定义了代理的通用接口和核心功能。

### **依赖关系**
- **父类**：`ChatAgent`（协议类，定义接口）。
- **依赖**：
  - `ChatMessage`：消息类型。
  - `Response`：响应类型。
  - `CancellationToken`：用于取消异步任务。

### **方法**
1. **`on_messages`**
   - **参数**：`messages: Sequence[ChatMessage]`, `cancellation_token: CancellationToken`
   - **返回值**：`Response`
   - **作用**：处理传入的消息并返回响应。

2. **`on_messages_stream`**
   - **参数**：同上。
   - **返回值**：`AsyncGenerator[AgentMessage | Response, None]`
   - **作用**：生成消息流。

3. **`run`**
   - **参数**：`task: str | ChatMessage | List[ChatMessage] | None`, `cancellation_token: CancellationToken | None`
   - **返回值**：`TaskResult`
   - **作用**：运行任务并返回结果。

4. **`save_state / load_state`**
   - **作用**：保存或加载代理状态。

---

## **3. 类：AssistantAgent**

### **描述**
`AssistantAgent` 是一个通用的助手代理类，提供工具使用和消息处理能力。

### **依赖关系**
- **父类**：`BaseChatAgent`
- **依赖**：
  - `Response`：响应类型。
  - `Tool` 和 `FunctionTool`：工具调用机制。
  - `HandoffMessage`：支持代理间的交接。

### **方法**
1. **`on_messages`**
   - **参数**：`messages: Sequence[ChatMessage]`, `cancellation_token: CancellationToken`
   - **返回值**：`Response`
   - **作用**：根据消息生成响应，并支持工具调用和交接。

2. **`on_messages_stream`**
   - **参数**：同上。
   - **返回值**：`AsyncGenerator[AgentMessage | Response, None]`
   - **作用**：处理消息流，支持工具执行和交接。

3. **`_execute_tool_call`**
   - **参数**：`tool_call: FunctionCall`, `cancellation_token: CancellationToken`
   - **返回值**：`FunctionExecutionResult`
   - **作用**：执行工具调用并返回结果。

4. **`save_state / load_state`**
   - **作用**：保存或恢复代理的上下文和消息历史。

---

## **4. 类：CodeExecutorAgent**

### **描述**
`CodeExecutorAgent` 是一个专门用于执行代码的代理类，支持从消息中提取代码块并运行。

### **依赖关系**
- **父类**：`BaseChatAgent`
- **依赖**：
  - `CodeExecutor`：代码执行器，用于实际运行代码。
  - `TextMessage`：消息类型。

### **方法**
1. **`on_messages`**
   - **参数**：`messages: Sequence[ChatMessage]`, `cancellation_token: CancellationToken`
   - **返回值**：`Response`
   - **作用**：从消息中提取代码块并执行，返回执行结果。

2. **`on_reset`**
   - **参数**：`cancellation_token: CancellationToken`
   - **作用**：重置代理状态（代码执行器无状态）。

---

## **5. 类：CodingAssistantAgent**

### **描述**
`CodingAssistantAgent` 是一个专注于代码生成和任务解决的代理类，目前已被标记为 **废弃**。

### **依赖关系**
- **父类**：`AssistantAgent`
- **依赖**：
  - `Response`：响应类型。
  - `TextMessage`：消息类型。

### **方法**
与 `AssistantAgent` 相同，增加了对代码生成的支持。

---

## **6. 类：SocietyOfMindAgent**

### **描述**
`SocietyOfMindAgent` 是一个基于团队协作的代理，内部维护一个团队（`Team`）来协作完成任务。

### **依赖关系**
- **父类**：`BaseChatAgent`
- **依赖**：
  - `Team`：内部团队。
  - `Response`：响应类型。

### **方法**
1. **`on_messages`**
   - **参数**：`messages: Sequence[ChatMessage]`, `cancellation_token: CancellationToken`
   - **返回值**：`Response`
   - **作用**：运行内部团队的任务，并基于团队结果生成响应。

2. **`on_messages_stream`**
   - **参数**：同上。
   - **返回值**：`AsyncGenerator[AgentMessage | Response, None]`
   - **作用**：生成消息流。

3. **`save_state / load_state`**
   - **作用**：保存或加载团队的状态。

---

## **7. 类：ToolUseAssistantAgent**

### **描述**
`ToolUseAssistantAgent` 提供工具使用能力，目前已被标记为 **废弃**，建议使用 `AssistantAgent` 替代。

### **依赖关系**
- **父类**：`AssistantAgent`
- **依赖**：
  - `Tool`：工具调用机制。

---

## **8. 类：UserProxyAgent**

### **描述**
`UserProxyAgent` 通过输入函数（`input_func`）模拟人类用户，用于团队协作或代理间交互。

### **依赖关系**
- **父类**：`BaseChatAgent`
- **依赖**：
  - `TextMessage` 和 `HandoffMessage`：消息类型。

### **方法**
1. **`on_messages`**
   - **参数**：`messages: Sequence[ChatMessage]`, `cancellation_token: CancellationToken`
   - **返回值**：`Response`
   - **作用**：通过输入函数处理用户消息。

2. **`_get_input`**
   - **参数**：`prompt: str`, `cancellation_token: Optional[CancellationToken]`
   - **返回值**：用户输入字符串。

3. **`on_reset`**
   - **参数**：`cancellation_token: Optional[CancellationToken]`
   - **作用**：重置代理状态。

---

## **依赖关系总结**

1. **核心基类**：`BaseChatAgent`，其他代理类继承自此类。
2. **协作代理**：`SocietyOfMindAgent` 和 `UserProxyAgent`，用于团队或人类交互。
3. **功能性代理**：`AssistantAgent`、`CodeExecutorAgent`，提供工具调用和代码执行功能。

