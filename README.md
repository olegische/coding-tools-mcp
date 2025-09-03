# Dev Tools MCP Server

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MCP Version](https://img.shields.io/badge/MCP-1.12.4+-green.svg)](https://modelcontextprotocol.io/)

**Dev Tools MCP Server** is a standalone server that provides a powerful toolkit for software development tasks through the Model Context Protocol (MCP). Built on battle-tested tools from the Trae Agent project, it's repackaged as a lightweight, independent, and easy-to-use service.

## 📋 Table of Contents

- [🚀 Key Features](#-key-features)
- [🛠️ Available Tools](#️-available-tools)
- [🏗️ Architecture](#️-architecture)
- [⚡ Quick Start](#-quick-start)
- [📦 Installation](#-installation)
- [⚙️ Configuration](#️-configuration)
- [🚀 Usage](#-usage)
- [🔧 Troubleshooting](#-troubleshooting)
- [🛡️ Security](#️-security)
- [📊 Performance & Limitations](#-performance--limitations)
- [🧪 Development](#-development)
- [📚 Documentation](#-documentation)
- [🤝 Contributing](#-contributing)
- [❓ FAQ](#-faq)
- [📄 License](#-license)
- [🙏 Acknowledgments](#-acknowledgments)

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

#### `json_editor` (optional)
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

#### `sequential_thinking` (optional)
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

## ⚡ Quick Start

Get up and running in 3 simple steps:

1. **Install and start the server:**
   ```bash
   git clone https://github.com/olegische/dev-tools-mcp.git
   cd dev-tools-mcp
   uv sync
   source .venv/bin/activate
   dev-tools-mcp
   ```

2. **Connect your MCP client** (e.g., Claude Desktop, Cline, or any MCP-compatible client)

3. **Start exploring:**
   ```python
   # Basic file operations
   await file_system(subcommand="pwd")
   await file_system(subcommand="ls", path=".")
   
   # Lock directory for editing
   await file_system(subcommand="lock_cwd")
   
   # Edit files
   await file_editor(command="view", path="README.md")
   ```

That's it! You're ready to use all the development tools through your MCP client.

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
| `FEATURE_SEQUENTIAL_THINKING_ENABLED` | Enable Sequential Thinking tool | `false` |
| `FEATURE_JSON_EDITOR_ENABLED` | Enable JSON Editor tool | `false` |
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

**Enable Sequential Thinking tool:**
```bash
export FEATURE_SEQUENTIAL_THINKING_ENABLED=true
dev-tools-mcp
```

**Enable JSON Editor tool:**
```bash
export FEATURE_JSON_EDITOR_ENABLED=true
dev-tools-mcp
```

## 🚀 Usage

### Starting the server
```bash
dev-tools-mcp
```

### Client Connection
Any MCP-compatible client can connect to the server and use available tools.

#### 🔌 Compatible Clients
- **[Claude Desktop](https://claude.ai/desktop)** - Official Anthropic client
- **[Cline](https://github.com/Anthropic/cline)** - VS Code extension
- **[Continue](https://continue.dev/)** - Open-source VS Code extension
- **[MCP Inspector](https://github.com/modelcontextprotocol/inspector)** - Debug tool
- Any custom client implementing MCP protocol

#### 🔗 Connection Examples
**Claude Desktop configuration:**
```json
{
  "mcpServers": {
    "dev-tools": {
      "command": "dev-tools-mcp",
      "args": [],
      "env": {
        "MCP_TRANSPORT": "stdio"
      }
    }
  }
}
```

**HTTP/SSE connection:**
```bash
# Start server with HTTP transport
export MCP_TRANSPORT=http
export MCP_PORT=8660
dev-tools-mcp

# Connect from client to http://localhost:8660
```

### Usage Examples

#### 🔍 File System Exploration
```python
# Discovery Phase - Safe exploration
await file_system(subcommand="pwd")
await file_system(subcommand="ls", path=".")
await file_system(subcommand="cd", path="src")
await file_system(subcommand="lock_cwd")  # Transition to Edit Phase
```

#### ✏️ File Editing Workflows
```python
# Edit Phase - Full editing capabilities
await file_editor(command="view", path="main.py")
await file_editor(command="create", path="new_file.py", file_text="# New file")
await file_editor(command="str_replace", path="main.py", old_str="old", new_str="new")
await file_editor(command="insert", path="main.py", line_number=10, text="new_line")
```

#### 🔧 Advanced File Operations
```python
# Create a new Python module
await file_editor(command="create", path="utils/helpers.py", 
                 file_text="""def format_name(name: str) -> str:
    return name.strip().title()

def validate_email(email: str) -> bool:
    return '@' in email and '.' in email.split('@')[1]
""")

# Update existing file
await file_editor(command="str_replace", path="main.py", 
                 old_str="import os", new_str="import os\nimport sys")
```

#### 📚 Git Workflow Examples
```python
# Check repository status
await git(command="status", path=".")

# View changes
await git(command="diff", path=".", base_commit="HEAD~1")

# Stage and commit changes
await git(command="add", path=".", add_path="src/")
await git(command="commit", path=".", message="Add new feature")

# Restore files
await git(command="restore", path=".", restore_path="src/old_file.py")
```

#### 🔍 Code Search Examples
```python
# Search for functions (requires CKG enabled)
await code_search(command="search_function", query="calculate_total")

# Search for classes
await code_search(command="search_class", query="UserManager")

# Search for class methods
await code_search(command="search_class_method", query="UserManager.get_user")
```

#### 💻 Command Execution Examples
```python
# Run tests
await bash(command="python -m pytest tests/ -v")

# Install dependencies
await bash(command="pip install -r requirements.txt")

# Build project
await bash(command="make build")

# Check system info
await bash(command="uname -a")
```

#### 🧠 Sequential Thinking (Optional)
```python
# Enable structured thinking for complex tasks
await sequential_thinking(command="start", task="Refactor user authentication system")

# Add reasoning steps
await sequential_thinking(command="add_step", 
                         step="Analyze current auth implementation")

# Branch and revise
await sequential_thinking(command="branch", 
                         branch_name="alternative_approach")

# Complete the task
await sequential_thinking(command="complete", 
                         conclusion="Implemented OAuth2 with JWT tokens")
```

#### 📄 JSON Editing Examples
```python
# View JSON configuration
await json_editor(command="view", path="config.json")

# Update configuration values
await json_editor(command="set", path="config.json", 
                 jsonpath="$.database.host", value="localhost")

# Add new configuration
await json_editor(command="add", path="config.json", 
                 jsonpath="$.features", value={"new_feature": True})

# Remove configuration
await json_editor(command="remove", path="config.json", 
                 jsonpath="$.old_setting")
```

#### 🔄 Complete Development Workflow
```python
# 1. Explore the project
await file_system(subcommand="pwd")
await file_system(subcommand="ls", path=".")
await file_system(subcommand="cd", path="src")

# 2. Lock directory for editing
await file_system(subcommand="lock_cwd")

# 3. Check git status
await git(command="status", path=".")

# 4. Make changes
await file_editor(command="view", path="main.py")
await file_editor(command="str_replace", path="main.py", 
                 old_str="TODO: implement feature", 
                 new_str="def new_feature(): pass")

# 5. Test changes
await bash(command="python -m pytest tests/test_main.py")

# 6. Commit changes
await git(command="add", path=".", add_path="src/main.py")
await git(command="commit", path=".", message="Implement new feature")

# 7. Return to exploration mode
await file_system(subcommand="unlock_cwd")
```

## 🔧 Troubleshooting

### Common Issues

**Q: Server fails to start with "Module not found" error**
```bash
# Solution: Ensure virtual environment is activated and dependencies are installed
source .venv/bin/activate
uv sync
```

**Q: Permission denied when trying to edit files**
```bash
# Solution: Make sure you've locked the directory first
await file_system(subcommand="lock_cwd")
# Then all file operations will work with relative paths
```

**Q: Git operations fail with "not a git repository"**
```bash
# Solution: Initialize git repository or navigate to a git repository
git init
# or
cd /path/to/your/git/repo
```

**Q: CKG search returns no results**
```bash
# Solution: Ensure CKG is enabled and properly configured
export FEATURE_CKG_ENABLED=true
# Restart the server after enabling features
```

**Q: Server connection issues with HTTP/SSE transport**
```bash
# Check if port is available and not blocked by firewall
netstat -tulpn | grep :8660
# Try different port if needed
export MCP_PORT=9000
```

**Q: JSON Editor tool not available**
```bash
# Solution: Enable the JSON Editor feature
export FEATURE_JSON_EDITOR_ENABLED=true
dev-tools-mcp
```

### Debug Mode

Enable debug logging for troubleshooting:
```bash
export LOG_LEVEL=DEBUG
dev-tools-mcp
```

### Getting Help

- Check the [Issues](https://github.com/olegische/dev-tools-mcp/issues) page
- Review the [Documentation](docs/) for detailed guides
- Enable debug logging and check the output for error details

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

## 🛡️ Security

### Important Security Considerations

**⚠️ This server provides powerful system access - use with caution!**

The Dev Tools MCP Server can execute arbitrary shell commands and modify files on your system. Here are essential security guidelines:

### 🔒 Best Practices

1. **Run in isolated environments:**
   ```bash
   # Use Docker or virtual machines for untrusted code
   docker run -it --rm -v $(pwd):/workspace dev-tools-mcp
   ```

2. **Limit network access:**
   ```bash
   # For HTTP/SSE transport, bind to localhost only
   export MCP_HOST=127.0.0.1
   export MCP_PORT=8660
   ```

3. **Use read-only mode when possible:**
   ```python
   # Stay in Discovery Phase for safe exploration
   # Only use lock_cwd() when you need to make changes
   await file_system(subcommand="pwd")  # Safe
   # await file_system(subcommand="lock_cwd")  # Only when needed
   ```

4. **Validate all inputs:**
   - Never trust external input without validation
   - Use relative paths when possible
   - Avoid executing commands from untrusted sources

### 🚨 Security Warnings

- **Shell Command Execution**: The `bash` tool can run any system command
- **File System Access**: Full read/write access to the locked directory
- **Git Operations**: Can modify repository history and push changes
- **Network Access**: HTTP/SSE transport exposes server over network

### 🔐 Recommended Configuration

For production or shared environments:

```bash
# Restrict to localhost only
export MCP_HOST=127.0.0.1
export MCP_TRANSPORT=stdio  # Most secure transport

# Enable logging for audit trails
export LOG_LEVEL=INFO

# Disable optional features if not needed
export FEATURE_CKG_ENABLED=false
export FEATURE_SEQUENTIAL_THINKING_ENABLED=false
```

### 🛡️ Network Security

When using HTTP/SSE transport:
- Use HTTPS in production
- Implement authentication if needed
- Use firewall rules to restrict access
- Consider VPN for remote access

## 📊 Performance & Limitations

### ⚡ Performance Characteristics

**File Operations:**
- File reading: ~1-10ms for typical files (< 1MB)
- File writing: ~5-50ms depending on file size
- Directory traversal: ~10-100ms for large directories

**Git Operations:**
- Status check: ~50-200ms
- Diff operations: ~100-500ms
- Commit operations: ~200-1000ms

**Search Operations:**
- CKG search: ~100-2000ms (depends on codebase size)
- File content search: ~50-500ms

### 📏 Known Limitations

**File System:**
- Maximum file size: ~100MB (configurable)
- Maximum directory depth: 100 levels
- Path length limit: 4096 characters (OS dependent)

**Git Operations:**
- Large repository performance may be slower
- Binary file diffs are not supported
- Submodule operations are limited

**Search & CKG:**
- CKG indexing can be memory intensive for large codebases
- Search results are limited to 1000 items
- Real-time indexing is not supported

**Network & Transport:**
- HTTP transport: ~10-50ms latency overhead
- SSE transport: ~5-20ms latency overhead
- stdio transport: ~1-5ms latency (fastest)

### 🚀 Performance Optimization Tips

1. **Use appropriate transport:**
   ```bash
   # For best performance, use stdio
   export MCP_TRANSPORT=stdio
   ```

2. **Optimize CKG usage:**
   ```bash
   # Only enable CKG for large codebases where search is needed
   export FEATURE_CKG_ENABLED=true
   ```

3. **Batch operations:**
   ```python
   # Instead of multiple small operations, batch them
   await file_editor(command="str_replace", path="file.py", 
                    old_str="old1", new_str="new1")
   await file_editor(command="str_replace", path="file.py", 
                    old_str="old2", new_str="new2")
   ```

4. **Use relative paths:**
   ```python
   # Faster than absolute paths
   await file_system(subcommand="lock_cwd")
   await file_editor(command="view", path="src/main.py")  # Relative
   ```

### 🔧 Resource Requirements

**Minimum Requirements:**
- RAM: 512MB
- CPU: 1 core
- Disk: 100MB free space

**Recommended for large projects:**
- RAM: 2GB+
- CPU: 2+ cores
- Disk: 1GB+ free space

**Memory Usage:**
- Base server: ~50-100MB
- With CKG enabled: +100-500MB (depends on codebase)
- With all features: ~200-800MB

## 📚 Documentation

### 📖 Additional Documentation
Additional documentation is available in the `docs/` directory:
- [MCP Server Architecture](docs/mcp_server_design.md)
- [CKG Implementation Details](docs/ckg_implementation_details.md)
- [CKG Improvement Strategy](docs/ckg_improvement_strategy.md)
- [Refactoring Plan](docs/refactoring_plan_ru.md)

### 📋 Version Information
- **Current Version**: 0.1.0
- **Python Support**: 3.12+
- **MCP Protocol**: 1.12.4+
- **License**: MIT

### 🔄 Release Notes
This is the initial release of Dev Tools MCP Server. Key features include:
- Complete file system operations
- Git integration
- Code search with CKG support
- JSON editing capabilities
- Sequential thinking tools
- Multiple transport protocols (stdio, HTTP, SSE)

For detailed version history and upcoming features, check the [GitHub Releases](https://github.com/olegische/dev-tools-mcp/releases) page.

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### 🚀 Getting Started
1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/your-username/dev-tools-mcp.git
   cd dev-tools-mcp
   ```
3. **Set up development environment**:
   ```bash
   make install-dev
   pre-commit install
   ```

### 🔧 Development Workflow
1. **Create a feature branch**:
   ```bash
   git checkout -b feature/amazing-feature
   ```
2. **Make your changes** and test them:
   ```bash
   make test
   make pre-commit
   ```
3. **Commit your changes**:
   ```bash
   git commit -m 'Add amazing feature'
   ```
4. **Push to your fork**:
   ```bash
   git push origin feature/amazing-feature
   ```
5. **Open a Pull Request** on GitHub

### 📋 Contribution Guidelines
- **Code Style**: Follow existing code style and use `make fix-format`
- **Testing**: Add tests for new features and ensure all tests pass
- **Documentation**: Update README and docs for new features
- **Commit Messages**: Use clear, descriptive commit messages
- **Issues**: Check existing issues before creating new ones

### 🐛 Reporting Issues
When reporting issues, please include:
- **Environment**: OS, Python version, MCP client
- **Steps to reproduce**: Clear steps to reproduce the issue
- **Expected behavior**: What you expected to happen
- **Actual behavior**: What actually happened
- **Logs**: Relevant error messages or logs

### 💡 Feature Requests
We welcome feature requests! Please:
- Check existing issues first
- Provide clear use cases
- Explain the expected behavior
- Consider contributing the implementation yourself

## ❓ FAQ

### General Questions

**Q: What is MCP?**
A: MCP (Model Context Protocol) is a standard for connecting AI assistants to external tools and data sources. It allows AI models to interact with your development environment safely and efficiently.

**Q: How is this different from other development tools?**
A: Dev Tools MCP Server provides a unified interface for common development tasks through the MCP protocol, making it easy to integrate with AI assistants and other MCP-compatible tools.

**Q: Can I use this with any AI assistant?**
A: Yes, as long as the AI assistant supports MCP protocol. Popular clients include Claude Desktop, Cline, and Continue.

### Technical Questions

**Q: Is it safe to run shell commands through this server?**
A: The server can execute arbitrary shell commands, so use it responsibly. Always run in isolated environments for untrusted code and follow the security guidelines in the Security section.

**Q: What's the difference between Discovery and Edit phases?**
A: Discovery phase is read-only and safe for exploration. Edit phase allows file modifications but requires locking a directory first. This two-phase approach prevents accidental modifications.

**Q: Can I use this in production?**
A: While technically possible, we recommend using it in development environments. For production use, ensure proper security measures and consider the security implications.

**Q: How do I enable optional features like CKG?**
A: Set the appropriate environment variables before starting the server:
```bash
export FEATURE_CKG_ENABLED=true
export FEATURE_JSON_EDITOR_ENABLED=true
dev-tools-mcp
```

### Troubleshooting

**Q: The server won't start - what should I check?**
A: Ensure Python 3.12+ is installed, virtual environment is activated, and all dependencies are installed with `uv sync`.

**Q: My MCP client can't connect - help!**
A: Check that the server is running, the correct transport is configured, and the port is available. Try stdio transport first as it's the most reliable.

**Q: File operations are failing - why?**
A: Make sure you're in Edit phase by calling `file_system.lock_cwd()` first, and use relative paths within the locked directory.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

The project is based on tools from [Trae Agent](https://github.com/bytecodealliance/trae-agent) and adapted for use as a standalone MCP server.

---

**Dev Tools MCP Server** - a powerful tool for development automation, providing professional capabilities through a standardized MCP interface.
