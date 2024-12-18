# Agents Directory

The `agents` directory contains various implementations of AI assistant agents designed to provide different functionalities. Below is an overview of the subdirectories, their classes, and dependencies within this directory.

## Directory Structure

```
agents/
├── file_surfer/
│   ├── __init__.py
│   ├── _file_surfer.py
│   ├── _markdown_file_browser.py
│   └── _tool_definitions.py
├── magentic_one/
│   ├── __init__.py
│   └── _magentic_one_coder_agent.py
├── openai/
│   ├── __init__.py
│   └── _openai_assistant_agent.py
└── web_surfer/
    ├── __init__.py
    ├── _events.py
    ├── _multimodal_web_surfer.py
    ├── _prompts.py
    ├── _set_of_mark.py
    ├── _tool_definitions.py
    ├── _types.py
    ├── _utils.py
    ├── page_script.js
    └── playwright_controller.py
```

## Subdirectories

### `file_surfer`

The `file_surfer` subdirectory contains the implementation of the `FileSurfer` agent, which acts as a local file previewer. It can open and read a variety of common file types and navigate the local file hierarchy.

#### Files

- `__init__.py`: Initializes the `file_surfer` package.
- `_file_surfer.py`: Contains the `FileSurfer` class implementation.
- `_markdown_file_browser.py`: Contains the `MarkdownFileBrowser` class for browsing files in Markdown format.
- `_tool_definitions.py`: Defines various tools used by the `FileSurfer` agent.

#### Classes

- **FileSurfer**: An agent that can handle local files, providing functionalities such as opening, reading, and navigating files.
- **MarkdownFileBrowser**: A class for browsing files in Markdown format, providing functionalities such as opening files, paging, and searching within files.

#### Dependencies

- `aiofiles`
- `PIL`
- `markitdown`

### `magentic_one`

The `magentic_one` subdirectory contains the implementation of the `MagenticOneCoderAgent`, which is a specialized AI assistant agent designed to provide coding assistance using a language model.

#### Files

- `__init__.py`: Initializes the `magentic_one` package.
- `_magentic_one_coder_agent.py`: Contains the `MagenticOneCoderAgent` class implementation.

#### Classes

- **MagenticOneCoderAgent**: An agent that provides coding assistance using an LLM model client. It can generate Python and shell scripts to solve various tasks.

#### Dependencies

- `autogen_agentchat`
- `autogen_core`

### `openai`

The `openai` subdirectory contains the implementation of the `OpenAIAssistantAgent`, which uses the OpenAI Assistant API to generate responses. This agent can handle tasks such as code interpretation, file handling, and custom function calling.

#### Files

- `__init__.py`: Initializes the `openai` package.
- `_openai_assistant_agent.py`: Contains the `OpenAIAssistantAgent` class implementation.

#### Classes

- **OpenAIAssistantAgent**: An agent that uses the OpenAI Assistant API to create AI assistants with capabilities like code interpretation, file handling, and custom function calling.

#### Dependencies

- `aiofiles`
- `openai`
- `autogen_core`
- `autogen_agentchat`

### `web_surfer`

The `web_surfer` subdirectory contains the implementation of the `MultimodalWebSurfer` agent, which is a multimodal agent designed to interact with web pages using Playwright.

#### Files

- `__init__.py`: Initializes the `web_surfer` package.
- `_events.py`: Contains the definition of the `WebSurferEvent` class.
- `_multimodal_web_surfer.py`: Contains the `MultimodalWebSurfer` class implementation.
- `_prompts.py`: Contains various prompt templates used by the `MultimodalWebSurfer` agent.
- `_set_of_mark.py`: Contains the implementation of functions to add set-of-mark annotations to screenshots.
- `_tool_definitions.py`: Defines various tools used by the `MultimodalWebSurfer` agent.
- `_types.py`: Contains type definitions used by the `MultimodalWebSurfer` agent.
- `_utils.py`: Contains utility functions used by the `MultimodalWebSurfer` agent.
- `page_script.js`: Contains JavaScript code that is injected into web pages to assist with interactions.
- `playwright_controller.py`: Contains the implementation of the `PlaywrightController` class.

#### Classes

- **MultimodalWebSurfer**: A multimodal agent that acts as a web surfer, capable of performing web searches, visiting web pages, and interacting with content using Playwright.
- **WebSurferEvent**: A dataclass that represents events occurring within the `MultimodalWebSurfer` agent.
- **PlaywrightController**: A helper class to allow Playwright to interact with web pages to perform actions such as clicking, filling, and scrolling.

#### Dependencies

- `aiofiles`
- `PIL`
- `playwright`
- `autogen_agentchat`
- `autogen_core`

## Conclusion

The `agents` directory provides various implementations of AI assistant agents, each designed to handle specific tasks and functionalities. By leveraging different models and tools, these agents can assist users with coding, file browsing, web surfing, and more.