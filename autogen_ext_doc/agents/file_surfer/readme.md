# FileSurfer Agent

## Overview

The `FileSurfer` agent is a local file previewer that can open and read a variety of common file types and navigate the local file hierarchy. It is designed to assist users in browsing and interacting with files and directories using a text-based interface.

## Features

- **Open Path**: Open a local file or directory at a specified path and return the current viewport content.
- **Page Up**: Scroll the viewport up one page-length in the current file and return the new viewport content.
- **Page Down**: Scroll the viewport down one page-length in the current file and return the new viewport content.
- **Find on Page (Ctrl+F)**: Scroll the viewport to the first occurrence of the search string, equivalent to Ctrl+F.
- **Find Next**: Scroll the viewport to the next occurrence of the search string.

## Usage

### Initialization

To initialize the `FileSurfer` agent, you need to provide the agent's name and a `ChatCompletionClient` model client that supports tool use.

```python
from autogen_core.models import ChatCompletionClient
from ._file_surfer import FileSurfer

model_client = ChatCompletionClient(model="your-model")
file_surfer = FileSurfer(name="FileSurfer", model_client=model_client)