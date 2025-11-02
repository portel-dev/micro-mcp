# MicroMCP

**Single-file MCPs with zero configuration**

MicroMCP is the simplest way to create MCP (Model Context Protocol) servers. Write a single TypeScript file with async methods, and you have a fully functional MCP server - no decorators, no base classes, no configuration needed.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-blue.svg)](https://www.typescriptlang.org/)
[![MCP](https://img.shields.io/badge/MCP-Compatible-green.svg)](https://modelcontextprotocol.io/)

## 📋 Prerequisites

**You need [NCP](https://github.com/anthropics/ncp) to use MicroMCPs.**

NCP is the CLI tool for managing and running MCP servers. It provides:
- **Discovery**: Find MCPs via `ncp find`
- **Execution**: Run tools via `ncp run`
- **Management**: Install and configure MCPs
- **MicroMCP Support**: Built-in support for `.micro.ts` files

### Install NCP

```bash
npm install -g @anthropic-ai/ncp
```

Once NCP is installed, you can use any MicroMCP immediately without additional setup.

## 🚀 Quick Start

Create a file `greet.micro.ts`:

```typescript
/**
 * Greeting MicroMCP - Simple greeting service
 *
 * @version 1.0.0
 * @author Your Name
 * @license MIT
 */

export class Greet {
  async hello(params: { name: string }) {
    return {
      message: `Hello, ${params.name}!`
    };
  }
}
```

That's it! You now have an MCP server with a `hello` tool.

Use it:
```bash
ncp run greet:hello --params '{"name":"World"}'
# Output: { "message": "Hello, World!" }
```

## ✨ Features

- **📝 Single File** - One `.micro.ts` file = One MCP server
- **🔍 Auto-Discovery** - Methods automatically become tools
- **📚 JSDoc → Descriptions** - Comments become tool documentation
- **🎯 TypeScript → Schemas** - Types become parameter validation
- **📦 Self-Managed Dependencies** - Declare deps in comments
- **⚡ Zero Config** - No decorators, no base classes, no setup
- **🔄 Auto-Sync** - Automatically indexed in [mcps.portel.dev](https://mcps.portel.dev)

## 📚 Available MicroMCPs

### Calculator (`calculator.micro.ts`)
Basic arithmetic operations.

**Tools**: `add`, `subtract`, `multiply`, `divide`, `power`

```bash
ncp run calculator:add --params '{"a":10,"b":5}'
# Output: { "result": 15, "operation": "addition" }
```

### String (`string.micro.ts`)
Comprehensive string manipulation utilities.

**Tools**: `uppercase`, `lowercase`, `slugify`, `reverse`, `wordCount`, `split`, `replace`, `titleCase`, `substring`, `trim`

```bash
ncp run string:slugify --params '{"text":"Hello World 2024!"}'
# Output: { "result": "hello-world-2024" }
```

### Workflow (`workflow.micro.ts`)
Intelligent task orchestration for scheduling and automation.

**Tools**: `list`, `get`, `create`, `validate`

```bash
ncp run workflow:list
# Lists available predefined workflows
```

## 🎯 Philosophy

MicroMCPs follow the **convention over configuration** principle:

1. **File name** determines MCP name: `calculator.micro.ts` → `calculator`
2. **Class methods** become tools: `async add()` → `calculator:add`
3. **JSDoc comments** provide descriptions
4. **TypeScript types** define schemas
5. **Everything else is automatic**

## 📖 Documentation

- **[GUIDE.md](./GUIDE.md)** - Complete developer guide
- **[Examples](.)** - All `.micro.ts` files in this repository
- **[MCP Specification](https://spec.modelcontextprotocol.io/)** - Official MCP docs

## 🛠️ Creating Your Own MicroMCP

### 1. Create the file

```typescript
/**
 * Weather MicroMCP - Get weather information
 *
 * @version 1.0.0
 * @author Your Name
 * @license MIT
 */

export class Weather {
  /**
   * Get current weather
   * @param city City name
   * @param units Temperature units (metric/imperial)
   */
  async current(params: { city: string; units?: string }) {
    // Your implementation
    return {
      temperature: 72,
      conditions: "Sunny",
      city: params.city
    };
  }
}
```

### 2. Test locally

```bash
# Place in NCP's examples directory
cp weather.micro.ts ~/.ncp/internal/

# Use it
ncp run weather:current --params '{"city":"San Francisco"}'
```

### 3. Publish

Submit a pull request to this repository. Your MicroMCP will be automatically:
- ✅ Validated for correct format
- ✅ Indexed in the registry
- ✅ Discoverable via `ncp find weather`
- ✅ Available to all NCP users

## 🌐 Registry Integration

All MicroMCPs in this repository are automatically synced to [mcps.portel.dev](https://mcps.portel.dev) every hour.

Discover MicroMCPs:
```bash
ncp find calculator
ncp find string
ncp find workflow
```

Each MicroMCP appears with a **(Micro)** label in search results.

## 🤝 Contributing

We welcome contributions! To add your MicroMCP:

1. **Fork** this repository
2. **Create** your `.micro.ts` file
3. **Include** JSDoc with `@version`, `@author`, `@license`
4. **Test** locally with NCP
5. **Submit** a pull request

See [GUIDE.md](./GUIDE.md) for detailed instructions.

### Requirements

- Valid TypeScript syntax
- JSDoc class comment with metadata
- At least one public async method
- Clear, descriptive tool names
- Proper parameter typing

## 📜 License

This project is licensed under the MIT License - see each individual MicroMCP file for its specific license.

## 🔗 Links

- **[NCP](https://github.com/anthropics/ncp)** - **Required** CLI for managing and running MCPs
- **[mcps.portel.dev](https://mcps.portel.dev)** - MCP Registry (auto-syncs MicroMCPs)
- **[MCP Specification](https://spec.modelcontextprotocol.io/)** - Official protocol docs
- **[Portel](https://portel.dev)** - MCP platform

## 💬 Support

- **Issues**: [GitHub Issues](https://github.com/portel-dev/micro-mcp/issues)
- **Discussions**: [GitHub Discussions](https://github.com/portel-dev/micro-mcp/discussions)

---

**Made with ❤️ by the Portel team**

[Discover more MCPs →](https://mcps.portel.dev)
