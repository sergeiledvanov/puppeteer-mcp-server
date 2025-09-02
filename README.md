# Puppeteer MCP Server

[![smithery badge](https://smithery.ai/badge/@merajmehrabi/puppeteer-mcp-server)](https://smithery.ai/server/@merajmehrabi/puppeteer-mcp-server)
This MCP server provides browser automation capabilities through Puppeteer, allowing interaction with both new browser instances and existing Chrome windows.

## Acknowledgment

This project is an experimental implementation inspired by [@modelcontextprotocol/server-puppeteer](https://github.com/modelcontextprotocol/servers). While it shares similar goals and concepts, it explores alternative approaches to browser automation through the Model Context Protocol.

<a href="https://glama.ai/mcp/servers/lpt1tvbubf"><img width="380" height="200" src="https://glama.ai/mcp/servers/lpt1tvbubf/badge" alt="Puppeteer Server MCP server" /></a>

## Features

- Navigate web pages
- Take screenshots
- Click elements
- Fill forms
- Select options
- Hover elements
- Execute JavaScript
- Smart Chrome tab management:
  - Connect to active Chrome tabs
  - Preserve existing Chrome instances
  - Intelligent connection handling

## Project Structure

```
/
├── src/
│   ├── config/        # Configuration modules
│   ├── tools/         # Tool definitions and handlers
│   ├── browser/       # Browser connection management
│   ├── types/         # TypeScript type definitions
│   ├── resources/     # Resource handlers
│   └── server.ts      # Server initialization
├── index.ts          # Entry point
└── README.md        # Documentation
```

## Installation

### Option 1: Install from npm

```bash
npm install -g .
```

You can also run it directly without installation using npx:

### Option 2: Install from source

1. Clone this repository or download the source code
2. Install dependencies:

```bash
npm install
```

3. Build the project:

```bash
npm run build
```

4. Run the server:

```bash
npm run watch
```

## MCP Server Configuration

To use this tool with Claude, you need to add it to your MCP settings configuration file.

### For Claude Desktop/Code

Add the following to your Claude Desktop configuration file (located at `~/.claude.json`):

#### If installed globally via npm:

```json
{
  "mcpServers": {
    "puppeteer": {
      "command": "puppeteer-mcp-server",
      "args": [],
      "env": {}
    }
  }
}
```

#### If installed from source:

```json
{
  "mcpServers": {
    "puppeteer": {
      "command": "node",
      "args": ["path/to/puppeteer-mcp-server/dist/index.js"],
      "env": {
        "NODE_OPTIONS": "--experimental-modules"
      }
    }
  }
}
```

For source installation, replace `path/to/puppeteer-mcp-server` with the actual path to where you installed this tool.

## Usage

### Standard Mode

The server will launch a new browser instance by default.

### Active Tab Mode

To connect to an existing Chrome window:

1. Launch Chrome with remote debugging enabled:
   ```bash
   # Windows
   "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222

   # macOS
   /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

   # Linux
   google-chrome --remote-debugging-port=9222
   ```

2. Navigate to your desired webpage in Chrome

3. Ask Claude to connect to the active Chrome tab:

4. The server will:
- Detect and connect to the Chrome instance running with remote debugging enabled
- Preserve your Chrome instance (won't close it)
- Find and connect to non-extension tabs
- Provide clear error messages if connection fails

## Available Tools

### puppeteer_connect_active_tab
Connect to an existing Chrome instance with remote debugging enabled.
- Optional:
  - `targetUrl` - URL of the specific tab to connect to
  - `debugPort` - Chrome debugging port (default: 9222)

### puppeteer_navigate
Navigate to a URL.
- Required: `url` - The URL to navigate to

### puppeteer_screenshot
Take a screenshot of the current page or a specific element using the current browser viewport size.
- Required: `name` - Name for the screenshot
- Optional:
  - `selector` - CSS selector for element to screenshot

### puppeteer_click
Click an element on the page.
- Required: `selector` - CSS selector for element to click

### puppeteer_fill
Fill out an input field.
- Required:
  - `selector` - CSS selector for input field
  - `value` - Text to enter

### puppeteer_select
Use dropdown menus.
- Required:
  - `selector` - CSS selector for select element
  - `value` - Option value to select

### puppeteer_hover
Hover over elements.
- Required: `selector` - CSS selector for element to hover

### puppeteer_evaluate
Execute JavaScript in the browser console.
- Required: `script` - JavaScript code to execute

## Security Considerations

When using remote debugging:
- Only enable on trusted networks
- Use a unique debugging port
- Close debugging port when not in use
- Never expose debugging port to public networks

## Logging and Debugging

### File-based Logging
The server implements comprehensive logging using Winston:

- Location: `logs/` directory
- File Pattern: `mcp-puppeteer-YYYY-MM-DD.log`
- Log Rotation:
  - Daily rotation
  - Maximum size: 20MB per file
  - Retention: 14 days
  - Automatic compression of old logs

### Log Levels
- DEBUG: Detailed debugging information
- INFO: General operational information
- WARN: Warning messages
- ERROR: Error events and exceptions

### Logged Information
- Server startup/shutdown events
- Browser operations (launch, connect, close)
- Navigation attempts and results
- Tool executions and outcomes
- Error details with stack traces
- Browser console output
- Resource usage (screenshots, console logs)

## Error Handling

The server provides detailed error messages for:
- Connection failures
- Missing elements
- Invalid selectors
- JavaScript execution errors
- Screenshot failures

Each tool call returns:
- Success/failure status
- Detailed error message if failed
- Operation result data if successful

All errors are also logged to the log files with:
- Timestamp
- Error message
- Stack trace (when available)
- Context information

## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) for details on how to submit pull requests, report issues, and contribute to the project.


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
