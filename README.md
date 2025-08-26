# Dev Tools MCP Server

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MCP Version](https://img.shields.io/badge/MCP-1.12.4+-green.svg)](https://modelcontextprotocol.io/)

**Dev Tools MCP Server** is a standalone server that provides a powerful toolkit for software development tasks through the Model Context Protocol (MCP). Built on battle-tested tools from the Trae Agent project, it's repackaged as a lightweight, independent, and easy-to-use service.

## 🚀 Key Features

The server provides a comprehensive set of development tools:

- **File System & Navigation** - explore and manage the file system
- **Code Editing** - powerful tools for working with files and JSON
- **Command Execution** - run shell commands in persistent sessions
- **Git Operations** - full Git support for version control
- **Code Search** - intelligent search using Code Knowledge Graph (CKG)
- **Meta Tools** - structured thinking and task management

## 🛠️ Available Tools

### 🔍 File System & Navigation

#### `file_system`
Basic file system operations:
- `pwd` - show current working directory
- `ls` - list files in directory
- `cd` - change directory
- `lock_cwd` - lock directory for editing
- `unlock_cwd` - unlock directory

#### `directory_explorer`
Advanced directory exploration:
- `list` - detailed file listing
- `tree` - tree structure visualization
- `search` - search through file contents

### ✏️ Code Editing

#### `file_editor`
Powerful file manipulation tool:
- `view` - view files (available in both phases)
- `create` - create new files
- `str_replace` - replace strings in files
- `insert` - insert text at specific lines

#### `json_editor`
Precise JSON file editing with JSONPath:
- `view` - view JSON data
- `set` - set values
- `add` - add new elements
- `remove` - remove elements

### 💻 Command Execution

#### `bash`
Execute shell commands in persistent sessions:
- Support for complex commands
- State preservation between calls
- Automatic session restart when needed

### 📚 Git Operations

#### `git`
Full Git operation support:
- `status` - repository status
- `diff` - view changes
- `add` - add files to index
- `commit` - create commits
- `restore` - restore files

### 🔍 Code Search

#### `code_search` (optional)
Intelligent search using Code Knowledge Graph:
- `search_function` - search for functions
- `search_class` - search for classes
- `search_class_method` - search for class methods

### 🧠 Meta Tools

#### `sequential_thinking`
Structured thinking for complex tasks:
- Record reasoning steps
- Branch and revise thoughts
- Track progress

## 🏗️ Architecture

The server is built on **FastMCP** and uses a two-phase workflow model:

### 🔍 Discovery Phase (Read-Only)
- Only navigation and viewing tools available
- Safe file system exploration
- Use `file_system.lock_cwd()` to transition to editing

### ✏️ Edit Phase (Read-Write)
- Full access to all editing tools
- All paths must be relative to locked directory
- Use `file_system.unlock_cwd()` to return to exploration

## 📦 Installation

### Requirements
- Python 3.12+
- [UV](https://docs.astral.sh/uv/) (fast Python package installer)

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/olegische/dev-tools-mcp.git
   cd dev-tools-mcp
   ```

2. **Create virtual environment and install dependencies:**
   ```bash
   uv sync
   ```

3. **Activate virtual environment:**
   ```bash
   source .venv/bin/activate
   ```

## ⚙️ Configuration

The server is configured through environment variables:

| Variable | Description | Default Values |
|----------|-------------|----------------|
| `MCP_TRANSPORT` | Transport mechanism | `"stdio"` |
| `MCP_HOST` | Host for server binding | `"0.0.0.0"` |
| `MCP_PORT` | Port for listening | `8660` |
| `FEATURE_CKG_ENABLED` | Enable Code Knowledge Graph | `false` |
| `LOG_LEVEL` | Logging level | `"INFO"` |

### Configuration Examples

**HTTP transport on port 9000:**
```bash
export MCP_TRANSPORT=http
export MCP_PORT=9000
dev-tools-mcp
```

**SSE transport with CKG enabled:**
```bash
export MCP_TRANSPORT=sse
export MCP_PORT=8660
export FEATURE_CKG_ENABLED=true
dev-tools-mcp
```

## 🚀 Usage

### Starting the server
```bash
dev-tools-mcp
```

### Client connection
Any MCP-compatible client can connect to the server and use available tools.

### Usage examples

**File system exploration:**
```python
# Discovery Phase
await file_system(subcommand="pwd")
await file_system(subcommand="ls", path=".")
await file_system(subcommand="cd", path="src")
await file_system(subcommand="lock_cwd")
```

**File editing:**
```python
# Edit Phase
await file_editor(command="view", path="main.py")
await file_editor(command="create", path="new_file.py", file_text="# New file")
await file_editor(command="str_replace", path="main.py", old_str="old", new_str="new")
```

**Git operations:**
```python
await git(command="status", path=".")
await git(command="diff", path=".", base_commit="HEAD~1")
await git(command="add", path=".", add_path="src/")
await git(command="commit", path=".", message="Update feature")
```

## 🧪 Development

### Development setup
```bash
make install-dev
```

### Running tests
```bash
make test
# or
uv run pytest
```

### Code quality checks
```bash
make pre-commit
```

### Code formatting
```bash
make fix-format
```

### Building
```bash
make build-wheel
```

## 📚 Documentation

Additional documentation is available in the `docs/` directory:
- [MCP Server Architecture](docs/mcp_server_design.md)
- [CKG Implementation Details](docs/ckg_implementation_details.md)
- [CKG Improvement Strategy](docs/ckg_improvement_strategy.md)
- [Refactoring Plan](docs/refactoring_plan_ru.md)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

The project is based on tools from [Trae Agent](https://github.com/bytecodealliance/trae-agent) and adapted for use as a standalone MCP server.

---

**Dev Tools MCP Server** - a powerful tool for development automation, providing professional capabilities through a standardized MCP interface.
