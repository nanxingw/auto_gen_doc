# **Task 模块文档**

---

## **1. 模块概述**

`task` 模块提供了任务控制相关的功能，主要包括终止条件的别名定义和一个用于处理消息流的异步控制台方法。

**总体介绍：**
1. **`ExternalTermination`**：外部触发终止条件。
2. **`HandoffTermination`**：基于 `HandoffMessage` 的交接终止条件。
3. **`MaxMessageTermination`**：当消息数量超过指定阈值时触发终止。
4. **`SourceMatchTermination`**：当消息来源匹配指定源时触发终止。
5. **`StopMessageTermination`**：检测到 `StopMessage` 时停止任务。
6. **`TextMentionTermination`**：当消息中提到指定文本时触发终止。
7. **`TimeoutTermination`**：任务超时未完成时触发终止。
8. **`TokenUsageTermination`**：消息的 token 数量超过限制时触发终止。
9. **`Console`**：一个异步方法，用于消息流的实时渲染和任务结果输出。

---

## **2. 终止条件类（Deprecated 别名）**

### **说明**
`task` 模块中的终止条件类是对 `conditions` 模块中终止类的别名映射。这些类已被标记为 **废弃**，将在未来版本 (`0.4.0`) 中移除。

---

### **2.1. ExternalTermination**
- **描述**：外部触发的终止条件。
- **代码路径**：`autogen_agentchat.conditions.ExternalTermination`
- **方法**：
  - **`set()`**：手动触发终止。
  - **`reset()`**：重置终止状态。

---

### **2.2. HandoffTermination**
- **描述**：基于 `HandoffMessage` 的交接终止条件。
- **代码路径**：`autogen_agentchat.conditions.HandoffTermination`
- **方法**：
  - **`__call__()`**：检查消息是否为交接消息。
  - **`reset()`**：重置终止状态。

---

### **2.3. MaxMessageTermination**
- **描述**：基于最大消息数量的终止条件。
- **代码路径**：`autogen_agentchat.conditions.MaxMessageTermination`
- **字段**：
  - **`max_messages: int`**：最大消息数量。
- **方法**：
  - **`__call__()`**：判断消息数量是否达到阈值。
  - **`reset()`**：重置计数。

---

### **2.4. SourceMatchTermination**
- **描述**：当消息来源匹配指定源时触发终止。
- **代码路径**：`autogen_agentchat.conditions.SourceMatchTermination`
- **字段**：
  - **`sources: List[str]`**：目标消息来源。
- **方法**：
  - **`__call__()`**：检查消息来源。
  - **`reset()`**：重置状态。

---

### **2.5. StopMessageTermination**
- **描述**：检测 `StopMessage` 是否出现，用于停止会话。
- **代码路径**：`autogen_agentchat.conditions.StopMessageTermination`
- **方法**：
  - **`__call__()`**：检测消息是否为 `StopMessage`。
  - **`reset()`**：重置状态。

---

### **2.6. TextMentionTermination**
- **描述**：当消息中提到特定文本时触发终止。
- **代码路径**：`autogen_agentchat.conditions.TextMentionTermination`
- **字段**：
  - **`text: str`**：触发终止的目标文本。
- **方法**：
  - **`__call__()`**：检查消息是否包含指定文本。
  - **`reset()`**：重置状态。

---

### **2.7. TimeoutTermination**
- **描述**：基于超时时间的终止条件。
- **代码路径**：`autogen_agentchat.conditions.TimeoutTermination`
- **字段**：
  - **`timeout_seconds: float`**：超时时间（秒）。
- **方法**：
  - **`__call__()`**：检查是否超时。
  - **`reset()`**：重置状态。

---

### **2.8. TokenUsageTermination**
- **描述**：基于消息的 token 数量的终止条件。
- **代码路径**：`autogen_agentchat.conditions.TokenUsageTermination`
- **字段**：
  - **`max_total_token: int`**：最大总 token 数量。
- **方法**：
  - **`__call__()`**：统计并检查 token 数量是否超出阈值。
  - **`reset()`**：重置状态。

---

## **3. 函数：Console**

### **描述**
`Console` 是一个异步方法，用于处理消息流。它通过终端显示消息内容，并在会话结束时返回任务结果或响应。

### **代码路径**
- **来源**：`autogen_agentchat.ui.Console`

### **参数**
- **`stream: AsyncGenerator[AgentMessage | T, None]`**  
  - 描述：异步生成器，产生消息流。
  - 类型：`AsyncGenerator`
- **`no_inline_images: bool = False`**  
  - 描述：是否禁用行内图像显示。
  - 默认值：`False`

### **返回值**
- **`TaskResult | Response`**
  - **`TaskResult`**：任务执行结果。
  - **`Response`**：代理响应。

### **注意**
- 该函数已被标记为 **废弃**，将在 `0.4.0` 版本中移除。

---

## **4. 模块导出**

`task` 模块通过 `__all__` 导出以下类和函数：
- **终止条件类**：
  - `ExternalTermination`
  - `HandoffTermination`
  - `MaxMessageTermination`
  - `SourceMatchTermination`
  - `StopMessageTermination`
  - `TextMentionTermination`
  - `TimeoutTermination`
  - `TokenUsageTermination`
- **异步函数**：
  - `Console`

---

## **5. 模块总结**

**`task` 模块的主要作用：**
1. 提供终止条件类的别名，确保与旧版本兼容。
2. 提供一个异步 `Console` 方法用于消息流的处理。

### **未来更新提示：**
- 终止条件类和 `Console` 函数已被标记为 **废弃**，用户应迁移到 `conditions` 模块和 `ui.Console`。

