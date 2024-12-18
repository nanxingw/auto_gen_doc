# **UI 模块文档**

---

## **1. 模块概述**

`ui` 模块提供工具，用于在终端控制台中渲染和打印代理消息流。其功能包括：
- **消息流的实时渲染**：便于用户实时了解任务进展。
- **任务摘要信息显示**：展示任务的消息流、token 消耗和耗时等。
- **多模态支持**：处理文本和图片等多模态消息。

**总体介绍：**
1. **`Console`**：核心工具，用于渲染消息流并输出任务结果或响应。
2. **辅助函数**：
   - **`_message_to_str`**：将消息对象转换为字符串形式。
   - **`_image_to_iterm`**：将图片转换为 `iTerm2` 图片协议格式。
   - **`_is_running_in_iterm`**：检测是否运行在 `iTerm2` 环境。
   - **`_is_output_a_tty`**：检测标准输出是否为 TTY。

---

## **2. 核心工具**

### **Console**
- **描述**：渲染消息流并在终端输出，返回最终的任务结果或响应。
- **参数**：
  - **`stream: AsyncGenerator[AgentMessage | TaskResult | Response, None]`**  
    异步生成器，表示消息流或任务结果流。
  - **`no_inline_images: bool = False`**  
    是否禁用 `iTerm2` 的行内图片渲染（默认允许）。
- **返回值**：
  - **`TaskResult` 或 `Response`**：表示最后处理的任务结果或响应。
- **主要功能**：
  - 渲染消息流到控制台。
  - 输出消息的源头、内容及消耗的 `token` 信息。
  - 支持 `iTerm2` 的图片内嵌协议。
- **异常**：
  - **`ValueError`**：如果流中未处理任何结果会抛出异常。

---

## **3. 辅助函数**

### **_message_to_str**
- **描述**：将消息对象转换为字符串表示。
- **参数**：
  - **`message: AgentMessage`**：要转换的消息。
  - **`render_image_iterm: bool = False`**：是否以 `iTerm2` 的图片渲染协议显示图片。
- **返回值**：字符串形式的消息内容（包含文本或图片占位符）。

---

### **_image_to_iterm**
- **描述**：将图片转换为 `iTerm2` 的内嵌图片协议格式。
- **参数**：
  - **`image: Image`**：要渲染的图片。
- **返回值**：图片的 `iTerm2` 协议格式字符串。

---

### **_is_running_in_iterm**
- **描述**：检测当前是否运行在 `iTerm2` 环境中。
- **返回值**：布尔值，指示是否为 `iTerm2`。

---

### **_is_output_a_tty**
- **描述**：检测标准输出是否是 TTY。
- **返回值**：布尔值，指示标准输出是否为 TTY。

---

## **4. 模块导出**

`ui` 模块通过 `__all__` 导出以下内容：
- **`Console`**

---

## **5. 模块总结**

- **实时渲染**：`Console` 提供实时消息流渲染功能，便于用户直观了解任务进度。
- **图片支持**：支持多模态消息中的图片渲染（适用于 `iTerm2`）。
- **任务结果输出**：在控制台输出任务的摘要信息，包括消息数量、token 消耗和耗时。

通过 `ui` 模块，用户可以方便地在终端中实时查看代理的任务进展和结果。

