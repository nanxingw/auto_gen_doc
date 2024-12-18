# **Messages 模块文档**

## **1. 模块概述**
`messages` 模块定义了在代理之间通信时使用的各种消息类型。这些消息类型是代理的核心交互手段，所有消息类型都继承自 `BaseMessage`。

**主要功能：**
- 规范消息格式。
- 支持不同类型的通信内容（文本、多模态内容、工具调用等）。
- 提供一致的消息接口，方便代理间的协作。

---

## **2. 类：BaseMessage**
### **描述**
`BaseMessage` 是所有消息类型的基类，定义了消息的通用字段和属性。

### **字段**
- **`source: str`**  
  描述：发送消息的代理名称。  
  类型：`str`

- **`models_usage: RequestUsage | None`**  
  描述：生成此消息时模型的使用情况。  
  类型：`RequestUsage | None`

- **`model_config`**  
  描述：允许任意类型字段（使用 Pydantic 的配置）。  
  类型：`ConfigDict`

---

## **3. 类：TextMessage**
### **描述**
`TextMessage` 表示一个纯文本消息，最常用的消息类型。

### **字段**
- **继承自 `BaseMessage`**
- **`content: str`**  
  描述：消息的文本内容。  
  类型：`str`

- **`type: Literal["TextMessage"]`**  
  描述：消息的类型标识。  
  默认值：`"TextMessage"`

---

## **4. 类：MultiModalMessage**
### **描述**
`MultiModalMessage` 表示包含多种媒体形式（如文本、图像）的消息。

### **字段**
- **继承自 `BaseMessage`**
- **`content: List[str | Image]`**  
  描述：消息的内容，可以是字符串或图像。  
  类型：`List[str | Image]`

- **`type: Literal["MultiModalMessage"]`**  
  描述：消息的类型标识。  
  默认值：`"MultiModalMessage"`

---

## **5. 类：StopMessage**
### **描述**
`StopMessage` 表示请求终止会话的消息，用于中断代理间的交互。

### **字段**
- **继承自 `BaseMessage`**
- **`content: str`**  
  描述：终止请求的内容。  
  类型：`str`

- **`type: Literal["StopMessage"]`**  
  描述：消息的类型标识。  
  默认值：`"StopMessage"`

---

## **6. 类：HandoffMessage**
### **描述**
`HandoffMessage` 表示请求将会话交接给另一个代理的消息。

### **字段**
- **继承自 `BaseMessage`**
- **`target: str`**  
  描述：交接目标代理的名称。  
  类型：`str`

- **`content: str`**  
  描述：发送给目标代理的交接内容。  
  类型：`str`

- **`type: Literal["HandoffMessage"]`**  
  描述：消息的类型标识。  
  默认值：`"HandoffMessage"`

---

## **7. 类：ToolCallMessage**
### **描述**
`ToolCallMessage` 表示调用工具的消息，用于代理请求外部工具的协助。

### **字段**
- **继承自 `BaseMessage`**
- **`content: List[FunctionCall]`**  
  描述：工具调用的具体内容。  
  类型：`List[FunctionCall]`

- **`type: Literal["ToolCallMessage"]`**  
  描述：消息的类型标识。  
  默认值：`"ToolCallMessage"`

---

## **8. 类：ToolCallResultMessage**
### **描述**
`ToolCallResultMessage` 表示工具调用结果的消息，用于反馈工具调用的结果。

### **字段**
- **继承自 `BaseMessage`**
- **`content: List[FunctionExecutionResult]`**  
  描述：工具调用的执行结果。  
  类型：`List[FunctionExecutionResult]`

- **`type: Literal["ToolCallResultMessage"]`**  
  描述：消息的类型标识。  
  默认值：`"ToolCallResultMessage"`

---

## **9. 类型别名**
### **ChatMessage**
- **定义**：`Annotated[TextMessage | MultiModalMessage | StopMessage | HandoffMessage, Field(discriminator="type")]`  
- **描述**：代理间通信的常用消息类型的集合。

### **AgentMessage**
- **定义**：`Annotated[TextMessage | MultiModalMessage | StopMessage | HandoffMessage | ToolCallMessage | ToolCallResultMessage, Field(discriminator="type")]`  
- **描述**：包含所有支持的消息类型。

---

## **10. 模块导出**
`messages` 模块通过 `__all__` 导出以下内容：
- `BaseMessage`
- `TextMessage`
- `MultiModalMessage`
- `StopMessage`
- `HandoffMessage`
- `ToolCallMessage`
- `ToolCallResultMessage`
- `ChatMessage`
- `AgentMessage`

---

## **依赖关系总结**
1. **工具支持**：  
   - `ToolCallMessage` 和 `ToolCallResultMessage` 依赖于工具调用功能（`FunctionCall` 和 `FunctionExecutionResult`）。
   
2. **多媒体支持**：  
   - `MultiModalMessage` 依赖 `Image` 类型支持图像内容。
   
3. **会话控制**：  
   - `StopMessage` 和 `HandoffMessage` 用于控制代理间的交互流程。

---

## **模块使用场景**
- **消息传递**：  
  - `TextMessage` 和 `MultiModalMessage` 是代理之间的主要通信形式。
  
- **工具调用**：  
  - `ToolCallMessage` 和 `ToolCallResultMessage` 用于代理与外部工具的交互。
  
- **会话控制**：  
  - `StopMessage` 和 `HandoffMessage` 控制会话的终止和交接。

通过统一的消息格式，`messages` 模块为代理间的高效通信提供了基础支持。

