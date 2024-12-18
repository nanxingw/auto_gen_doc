# **Base 模块文档**

---

## **1. 模块概述**

`Base` 模块提供了项目的核心基础类和通用功能。所有其他模块的代理、任务、终止条件等均直接或间接依赖于此模块。

**主要功能：**
- 提供核心基类（如 `TaskRunner`、`ChatAgent`）。
- 定义代理与团队任务执行的基础逻辑。
- 提供终止条件和交接工具的实现。

**依赖关系：**
- 依赖 `ChatMessage` 和 `AgentMessage` 作为消息模型。
- 依赖异步任务管理工具如 `CancellationToken`。

---

## **2. 类：ChatAgent**

### **描述**
`ChatAgent` 是一个协议类，定义了代理的通用接口，具体功能由实现类继承实现。

### **依赖关系**
- **继承**：`TaskRunner`（协议类）。
- **依赖**：`CancellationToken`、`ChatMessage`、`AgentMessage`。

### **属性**
1. **`name (str)`**：唯一标识代理名称。
2. **`description (str)`**：代理的描述，通常用于决策代理的用途。
3. **`produced_message_types (List[type[ChatMessage]])`**：代理生成的消息类型。

### **方法**
1. **`on_messages`**
   - **参数**：`messages: Sequence[ChatMessage]`, `cancellation_token: CancellationToken`
   - **返回值**：`Response`
   - **作用**：处理传入的消息并返回一个响应。

2. **`on_messages_stream`**
   - **参数**：同上。
   - **返回值**：`AsyncGenerator[AgentMessage | Response, None]`
   - **作用**：返回一个消息流和最终响应。

3. **`on_reset`**
   - **参数**：`cancellation_token: CancellationToken`
   - **作用**：重置代理到初始状态。

4. **`save_state`**
   - **返回值**：`Mapping[str, Any]`
   - **作用**：保存当前代理状态。

5. **`load_state`**
   - **参数**：`state: Mapping[str, Any]`
   - **作用**：从保存的状态恢复代理。

---

## **3. 类：TaskRunner**

### **描述**
`TaskRunner` 是任务运行的核心协议类，定义了任务执行的接口。

### **依赖关系**
- **依赖**：`TaskResult`、`CancellationToken`、`ChatMessage`。

### **方法**
1. **`run`**
   - **参数**：
     - `task: str | ChatMessage | List[ChatMessage] | None = None`
     - `cancellation_token: CancellationToken | None = None`
   - **返回值**：`TaskResult`
   - **作用**：执行任务并返回结果。

2. **`run_stream`**
   - **参数**：同上。
   - **返回值**：`AsyncGenerator[AgentMessage | TaskResult, None]`
   - **作用**：执行任务并生成消息流，最后返回任务结果。

---

## **4. 类：Team**

### **描述**
`Team` 继承了 `TaskRunner`，用于管理多代理团队任务的执行。

### **依赖关系**
- **继承**：`TaskRunner`（协议类）。

### **方法**
1. **`reset`**
   - **作用**：重置团队和所有成员到初始状态。

2. **`save_state`**
   - **返回值**：`Mapping[str, Any]`
   - **作用**：保存当前团队状态。

3. **`load_state`**
   - **参数**：`state: Mapping[str, Any]`
   - **作用**：从保存的状态恢复团队。

---

## **5. 类：Response**

### **描述**
`Response` 表示代理生成的响应，包含主要的消息和内部消息记录。

### **依赖关系**
- **依赖**：`ChatMessage`、`AgentMessage`。

### **属性**
1. **`chat_message (ChatMessage)`**：由代理生成的聊天消息。
2. **`inner_messages (List[AgentMessage] | None)`**：代理生成的内部消息。

---

## **6. 类：TaskResult**

### **描述**
`TaskResult` 用于保存任务执行的结果。

### **依赖关系**
- **依赖**：`AgentMessage`。

### **属性**
1. **`messages (Sequence[AgentMessage])`**：任务生成的消息。
2. **`stop_reason (str | None)`**：任务终止的原因。

---

## **7. 类：Handoff**

### **描述**
`Handoff` 提供代理之间的交接工具。

### **依赖关系**
- **继承**：`BaseModel`（来自 Pydantic）。

### **属性**
1. **`target (str)`**：目标代理名称。
2. **`description (str)`**：交接描述。
3. **`name (str)`**：交接名称，默认为 `transfer_to_{target}`。
4. **`message (str)`**：传递给目标代理的信息。

### **方法**
1. **`set_defaults`**
   - **参数**：`values: Dict[str, Any]`
   - **返回值**：`Dict[str, Any]`
   - **作用**：自动设置默认值（如 `description`、`name` 和 `message`）。

2. **`handoff_tool`**
   - **返回值**：`Tool`
   - **作用**：创建一个交接工具。

---

## **8. 类：TerminationCondition**

### **描述**
`TerminationCondition` 是所有终止条件的抽象基类。

### **依赖关系**
- **继承**：`ABC`（抽象类）。

### **方法**
1. **`terminated`**
   - **返回值**：`bool`
   - **作用**：检查终止条件是否已满足。

2. **`__call__`**
   - **参数**：`messages: Sequence[AgentMessage]`
   - **返回值**：`StopMessage | None`
   - **作用**：根据消息判断是否终止。

3. **`reset`**
   - **作用**：重置终止条件。

4. **`__and__` / `__or__`**
   - **返回值**：组合后的终止条件实例。

---

## **9. 类：_AndTerminationCondition 和 _OrTerminationCondition**

### **描述**
组合多个终止条件的子类。

### **依赖关系**
- **继承**：`TerminationCondition`。

### **方法**
1. **`__call__`**
   - **参数**：`messages: Sequence[AgentMessage]`
   - **返回值**：`StopMessage | None`
   - **作用**：检查组合条件是否满足。

2. **`reset`**
   - **作用**：重置所有组合条件。

---

## **10. 类：TerminatedException**

### **描述**
用于表示终止条件已被触发的异常。

### **依赖关系**
- **继承**：`BaseException`。

---

## **11. 类间的关系总结**

1. **`ChatAgent`** 和 **`Team`** 都继承 `TaskRunner`，实现了任务运行的接口。
2. **`TaskResult`** 用于保存任务执行的结果，依赖 `AgentMessage`。
3. **`TerminationCondition`** 及其子类 `_AndTerminationCondition` 和 `_OrTerminationCondition` 用于定义终止逻辑。
4. **`Handoff`** 提供代理间的交接工具和消息，支持目标代理的任务传递。

---

## **12. 依赖图示意**
- **核心类**：`TaskRunner`、`ChatAgent` 和 `Team`。
- **结果类**：`Response` 和 `TaskResult`。
- **终止类**：`TerminationCondition` 及其子类。
- **工具类**：`Handoff` 提供代理间的传递机制。

