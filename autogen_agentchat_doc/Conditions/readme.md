# **Conditions 模块文档**

---

## **1. 模块概述**

`conditions` 模块提供了多种终止条件（Termination Conditions），用于控制多代理（multi-agent）团队的行为。这些终止条件定义了任务或会话的结束标准，可以基于消息数量、内容、时间、外部触发等进行判断。

**总体介绍：**
1. **`TerminationCondition`**：所有终止条件的基类，提供公共接口。
2. **`StopMessageTermination`**：当接收到 `StopMessage` 时触发终止。
3. **`MaxMessageTermination`**：消息数量超过设定阈值时触发终止。
4. **`TextMentionTermination`**：当消息中提到特定文本时触发终止。
5. **`TokenUsageTermination`**：消息的 token 数量超过限制时触发终止。
6. **`HandoffTermination`**：接收到匹配目标的 `HandoffMessage` 时触发终止。
7. **`TimeoutTermination`**：任务超时未完成时触发终止。
8. **`ExternalTermination`**：通过外部调用触发终止。
9. **`SourceMatchTermination`**：指定来源的代理响应时触发终止。

---

## **2. 基类：TerminationCondition**

### **描述**
所有终止条件的基类，定义了公共接口和属性。

### **属性**
- **`terminated: bool`**  
  - 描述：终止条件是否满足。  
  - 类型：`bool`

### **方法**
- **`__call__(messages: Sequence[AgentMessage]) -> StopMessage | None`**  
  - 描述：检查是否满足终止条件。  
  - 参数：`messages` - 消息序列。  
  - 返回值：`StopMessage`（终止消息）或 `None`。

- **`reset() -> None`**  
  - 描述：重置终止条件状态。

---

## **3. 类：StopMessageTermination**

### **描述**
当接收到 `StopMessage` 时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`
- **依赖**：
  - `StopMessage`：检测消息是否为终止消息。

### **方法**
- **`__call__`**：检查消息中是否包含 `StopMessage`。  
- **`reset`**：重置终止状态。

---

## **4. 类：MaxMessageTermination**

### **描述**
在消息数量超过设定的最大阈值时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`

### **字段**
- **`max_messages: int`**  
  - 描述：允许的最大消息数量。  
  - 类型：`int`

### **方法**
- **`__call__`**：统计消息数量，判断是否超出最大限制。  
- **`reset`**：重置消息计数。

---

## **5. 类：TextMentionTermination**

### **描述**
当消息中提到特定文本时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`
- **依赖**：
  - `TextMessage` 和 `MultiModalMessage`：检查文本或内容是否匹配。

### **字段**
- **`text: str`**  
  - 描述：需要匹配的目标文本。  
  - 类型：`str`

### **方法**
- **`__call__`**：检查消息内容是否包含目标文本。  
- **`reset`**：重置终止状态。

---

## **6. 类：TokenUsageTermination**

### **描述**
在消息的 token 数量超过阈值时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`

### **字段**
- **`max_total_token: int`**  
  - 描述：允许的最大总 token 数量。  
- **`max_prompt_token: int`**  
  - 描述：最大提示 token 数量。  
- **`max_completion_token: int`**  
  - 描述：最大补全 token 数量。

### **方法**
- **`__call__`**：统计消息的 token 数量并判断是否超出限制。  
- **`reset`**：重置 token 计数。

---

## **7. 类：HandoffTermination**

### **描述**
当接收到一个 `HandoffMessage` 且目标匹配时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`
- **依赖**：
  - `HandoffMessage`：用于匹配目标名称。

### **字段**
- **`target: str`**  
  - 描述：交接目标的名称。

### **方法**
- **`__call__`**：检查消息中是否存在目标匹配的 `HandoffMessage`。  
- **`reset`**：重置终止状态。

---

## **8. 类：TimeoutTermination**

### **描述**
在设定的时间限制内未完成任务时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`

### **字段**
- **`timeout_seconds: float`**  
  - 描述：超时时间（秒）。  
  - 类型：`float`

### **方法**
- **`__call__`**：检查是否超时。  
- **`reset`**：重置超时状态。

---

## **9. 类：ExternalTermination**

### **描述**
外部调用 `set` 方法来手动触发终止条件。

### **依赖关系**
- **父类**：`TerminationCondition`

### **方法**
- **`set`**：手动设置终止标志。  
- **`__call__`**：检查终止标志。  
- **`reset`**：重置终止状态。

---

## **10. 类：SourceMatchTermination**

### **描述**
当指定来源的代理响应时触发终止。

### **依赖关系**
- **父类**：`TerminationCondition`
- **依赖**：
  - 消息的 `source` 字段。

### **字段**
- **`sources: List[str]`**  
  - 描述：触发终止的来源代理名称列表。

### **方法**
- **`__call__`**：检查消息来源是否在目标列表中。  
- **`reset`**：重置终止状态。

---

## **11. 模块导出**

`conditions` 模块通过 `__all__` 导出以下终止条件类：
- `ExternalTermination`
- `HandoffTermination`
- `MaxMessageTermination`
- `SourceMatchTermination`
- `StopMessageTermination`
- `TextMentionTermination`
- `TimeoutTermination`
- `TokenUsageTermination`

---

## **12. 依赖关系总结**

- **消息类型**：
  - `StopMessage`、`TextMessage`、`MultiModalMessage` 和 `HandoffMessage` 用于触发不同的终止条件。
- **外部依赖**：
  - 基类 `TerminationCondition` 通过统一接口提供了通用的终止控制机制。

