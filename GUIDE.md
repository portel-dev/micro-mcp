# MicroMCPs

**Single-file MCPs with zero configuration**

MicroMCPs are lightweight, self-contained MCP servers defined in a single TypeScript file. No decorators, no base classes required - just pure TypeScript classes with async methods.

## What is a MicroMCP?

A MicroMCP is the simplest way to create an MCP server:
- **One file** = One MCP server
- **Class methods** = MCP tools (automatically discovered)
- **JSDoc comments** = Tool descriptions
- **TypeScript types** = Parameter schemas (auto-extracted)
- **Dependencies** = Self-managed (declared in comments)

## Quick Start

### Creating a MicroMCP

```typescript
/**
 * My Custom MicroMCP - Does amazing things
 *
 * @version 1.0.0
 * @author Your Name
 * @license MIT
 */

export class MyCustomMCP {
  /**
   * Greet someone
   * @param name Person's name
   * @param language Language code (en, es, fr)
   */
  async greet(params: { name: string; language?: string }) {
    const greetings = {
      en: `Hello, ${params.name}!`,
      es: `¡Hola, ${params.name}!`,
      fr: `Bonjour, ${params.name}!`
    };

    return {
      message: greetings[params.language || 'en'] || greetings.en
    };
  }
}
```

### File Naming Convention

MicroMCP files use the `.micro.ts` extension:
- `calculator.micro.ts` → MCP name: "calculator"
- `string.micro.ts` → MCP name: "string"
- `my-helper.micro.ts` → MCP name: "my-helper"

### Version Support

Add version metadata via JSDoc:

```typescript
/**
 * Calculator MicroMCP - Basic arithmetic operations
 *
 * @version 1.0.0
 * @author Portel
 * @license MIT
 */
```

## How It Works

1. **Discovery**: NCP scans for `.micro.ts` files
2. **Compilation**: TypeScript is compiled at runtime (cached)
3. **Schema Extraction**: Parameter types → JSON Schema
4. **Tool Registration**: Each async method becomes an MCP tool
5. **Execution**: Methods called with validated parameters

## Directory Structure

```
micromcps/
├── README.md                       # This file
├── calculator.micro.ts             # Arithmetic operations
├── calculator.micro.schema.json    # Pre-extracted schemas (optional)
├── string.micro.ts                 # String manipulation
├── string.micro.schema.json
├── workflow.micro.ts               # Task orchestration
└── workflow.micro.schema.json
```

## Example MicroMCPs

### Calculator (calculator.micro.ts)

Basic arithmetic operations: add, subtract, multiply, divide, power.

```bash
# Use it
ncp run calculator:add --params '{"a":10,"b":5}'
```

### String (string.micro.ts)

Text manipulation: uppercase, lowercase, slugify, reverse, wordCount, split, replace, titleCase, substring, trim.

```bash
# Use it
ncp run string:uppercase --params '{"text":"hello world"}'
```

### Workflow (workflow.micro.ts)

Intelligent task orchestration with predefined workflows and custom workflow creation.

```bash
# List workflows
ncp run workflow:list

# Get a workflow
ncp run workflow:get --params '{"workflowName":"daily-standup"}'
```

## Advanced Features

### Managing Dependencies

Declare dependencies in JSDoc comments:

```typescript
/**
 * My MicroMCP with dependencies
 *
 * @dependencies axios@^1.6.0, lodash@^4.17.21
 */

import axios from 'axios';
import _ from 'lodash';

export class MyMCP {
  async fetchData(params: { url: string }) {
    const response = await axios.get(params.url);
    return _.pick(response.data, ['id', 'name']);
  }
}
```

Dependencies are automatically installed to a per-MCP cache directory.

### Lifecycle Hooks (Optional)

```typescript
export class MyMCP {
  async onInitialize() {
    // Called once when MCP loads
    console.log('MCP initialized');
  }

  async onShutdown() {
    // Called when MCP unloads
    console.log('MCP shutting down');
  }

  async myTool(params: { input: string }) {
    return { output: params.input.toUpperCase() };
  }
}
```

### Using MicroMCP Base Class (Optional)

While not required, you can extend `MicroMCP` for additional utilities:

```typescript
import { MicroMCP } from '../src/internal-mcps/base-micro.js';

export class MyMCP extends MicroMCP {
  async myTool(params: { input: string }) {
    // Your implementation
  }
}
```

## Publishing to Registry

MicroMCPs in this repository are automatically indexed by mcps.portel.dev and discoverable via:

```bash
ncp find calculator
ncp find string
```

### Requirements for Registry Listing

1. **File**: Must end with `.micro.ts`
2. **Version**: Include `@version` in JSDoc
3. **Description**: Clear class-level JSDoc comment
4. **Author**: Include `@author` in JSDoc
5. **License**: Include `@license` in JSDoc (default: MIT)

## Testing

Test your MicroMCP locally before publishing:

```bash
# 1. Place your .micro.ts file in src/internal-mcps/examples/
cp my-custom.micro.ts src/internal-mcps/examples/

# 2. Rebuild
npm run build

# 3. Test it
ncp find my-custom
ncp run my-custom:myTool --params '{"input":"test"}'
```

## Schema Pre-extraction

For production deployments, schemas can be pre-extracted at build time:

```bash
# Extract schemas
npm run extract-schemas

# This creates .micro.schema.json files alongside .micro.ts files
```

Pre-extracted schemas:
- Faster loading (no runtime TypeScript parsing)
- Included in DXT packages
- Automatically used when available

## Contributing

1. Create your MicroMCP in `micromcps/`
2. Add JSDoc with `@version`, `@author`, `@license`
3. Test locally with `ncp run`
4. Submit a pull request

## Learn More

- [NCP Documentation](https://github.com/anthropics/ncp)
- [MCP Protocol Specification](https://spec.modelcontextprotocol.io/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

## License

All MicroMCPs in this directory are licensed under MIT unless otherwise specified.
