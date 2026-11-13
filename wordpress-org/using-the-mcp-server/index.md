# Using the WordPress.org MCP Server

WordPress.org provides an MCP (Model Context Protocol) server that lets AI-powered development tools help you prepare, validate, and submit plugins to the Plugin Directory. If you use tools like Claude, Cursor, or VS Code with AI capabilities, you can connect them to WordPress.org so they have direct access to plugin guidelines, readme validation, submission status, and more.

This is a tool to assist your workflow, not a replacement for the review process. All plugins submitted through the MCP server go through the same review as plugins submitted through the web form. All [Plugin Guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/) apply in full.

## What is MCP?

[Model Context Protocol](https://modelcontextprotocol.io/) (MCP) is an open standard that lets AI assistants connect to external services. Instead of copying and pasting between your AI tool and WordPress.org, an MCP connection allows your AI assistant to read plugin guidelines, validate your readme, check your submission status, and submit your plugin, all within your existing development environment.

## Prerequisites

- A [WordPress.org account](https://login.wordpress.org).
- An MCP-compatible client (such as [Claude Desktop](https://claude.ai/download), [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Cursor](https://www.cursor.com/), or [VS Code](https://code.visualstudio.com/)).
- [Node.js](https://nodejs.org/) (version 18 or later).

## Setting Up Your Connection

### Quick setup

Run this command in your terminal:

```
npx -y @wporg/mcp
```

This opens your browser to authorize the connection and create an application password, then automatically detects and configures your installed MCP clients (currently Claude Desktop, Claude Code, Cursor, and VS Code).

If your client isn't supported yet, please [let us know](https://meta.trac.wordpress.org/newticket) and use the manual setup below.

### Manual setup

If you prefer to configure the connection yourself, or if your client isn't yet supported by the quick setup, follow these steps.

#### Step 1: Authorize the connection

Visit the authorization page to get started:

[Authorize WordPress.org MCP →](https://login.wordpress.org/?action=authorize_application&app_id=c4c73a54-96d7-47b9-9bdc-1a66b9b04505)

If you're not already logged in, you'll be prompted to log in to your WordPress.org account first. Once logged in, you'll see an authorization screen for "WordPress.org MCP." Click **Approve** to generate your connection credentials.

[tip]Only one MCP application password exists per account at a time. If you authorize again later, your previous password will be revoked and a new one will be created.[/tip]

#### Step 2: Copy your configuration

After approving, you'll see a JSON configuration block that looks like this:

```json
{
  "mcpServers": {
    "wporg-mcp-server": {
      "command": "npx",
      "args": ["-y", "@automattic/mcp-wordpress-remote@^0.2"],
      "env": {
        "WP_API_URL": "https://wordpress.org/wp-json/mcp/wporg",
        "WP_API_USERNAME": "your-username",
        "WP_API_PASSWORD": "xxxx xxxx xxxx xxxx"
      }
    }
  }
}
```

Click **Copy** to copy this configuration to your clipboard. Your application password is shown only once. If you lose it, you'll need to authorize again to generate a new one.

#### Step 3: Add to your MCP client

Where you paste the configuration depends on which client you use:

- **Claude Desktop**: Open Settings, then Developer, then Edit Config. Paste the configuration into your `claude_desktop_config.json` file.
- **Claude Code**: Add the configuration to a `.mcp.json` file in your project directory.
- **Cursor**: Go to Settings, then Tools and MCP, then Add Custom MCP.
- **VS Code**: Save as `.vscode/mcp.json`. Use `servers` as the top-level key instead of `mcpServers`.

Once saved, restart your client or reload the MCP configuration. Your AI assistant should now have access to the WordPress.org Plugin Directory tools.

## What's Available

The MCP server provides three types of capabilities: tools your AI assistant can execute, reference resources it can read, and guided prompts for common workflows.

### Tools

Tools are actions your AI assistant can perform on your behalf:

- **Validate Readme**: Checks your `readme.txt` or `readme.md` for errors, warnings, and suggestions before submission.
- **Get Plugin Status**: Retrieves the current review status and any reviewer feedback for a plugin you've submitted.
- **Submit Plugin**: Submits a new plugin for review or updates an existing submission that is still under review.

[info]The Submit Plugin tool is for new submissions and for updating plugins that are under review. Once a plugin is approved, updates are published via [Subversion](https://developer.wordpress.org/plugins/wordpress-org/how-to-use-subversion/).[/info]

### Resources

Resources are reference documents your AI assistant can read to give you accurate, up-to-date guidance:

- **Plugin Guidelines**: The [Detailed Plugin Guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/) that all plugins must follow.
- **Plugin Developer FAQ**: Answers to [frequently asked questions](https://developer.wordpress.org/plugins/wordpress-org/plugin-developer-faq/) about the plugin directory.
- **Plugin Readmes**: How your [readme.txt works](https://developer.wordpress.org/plugins/wordpress-org/how-your-readme-txt-works/), including required and optional fields.
- **Plugin Headers**: Required and optional [plugin header fields](https://developer.wordpress.org/plugins/the-basics/header-requirements/).
- **Plugin Check Guide**: How to run [Plugin Check (PCP)](https://wordpress.org/plugins/plugin-check/) locally before submitting.
- **Reserved Slugs**: Plugin name slugs that are reserved or trademarked and cannot be used.

### Prompts

Prompts are guided workflows that walk you and your AI assistant through multi-step processes:

- **Prepare Plugin for Submission**: A step-by-step checklist covering guidelines review, header verification, readme validation, Plugin Check, and submission readiness.
- **Run Plugin Check**: Instructions for installing and running Plugin Check with the same flags used by WordPress.org's automated review.
- **Address Review Feedback**: A guided workflow for fetching reviewer feedback, understanding the requested changes, applying fixes, and resubmitting.

## Your Responsibilities

Using the MCP server does not change your responsibilities as a plugin developer. Whether you write every line of code by hand or use AI assistance, the following applies:

You are responsible for all code in your plugin. The Plugin Review Team evaluates every submission the same way, regardless of how the code was produced. If your plugin is rejected, you are expected to understand the issues and make the necessary changes.

The [Detailed Plugin Guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/) are not relaxed for AI-assisted submissions. Your plugin must be original, secure, performant, and distributed under a [GPL-compatible license](https://www.gnu.org/licenses/license-list.html#GPLCompatibleLicenses). Trademark and copyright law must be respected.

### Review what AI produces

AI tools can introduce security vulnerabilities, license violations, unnecessary external service calls, or code that doesn't match your intent. You must review every change your AI assistant makes before submitting your plugin. Running [Plugin Check](https://wordpress.org/plugins/plugin-check/) locally is a good way to catch common issues, and a prerequisite before submitting a plugin for review.

[alert]"The AI did it" is not a valid response to a review rejection. You must understand and be able to explain the code in your plugin.[/alert]

## Example Workflows

Here are some common ways developers use the MCP server. In each case, you're talking to your AI assistant in your development tool. The assistant handles the MCP server communication for you.

### Prepare a plugin for submission

Ask your AI assistant to help you get your plugin ready:

```
Help me prepare my plugin for submission to WordPress.org.
```

Your assistant will walk through a multi-step checklist: reviewing your plugin against the guidelines, verifying your plugin headers, validating your readme, running Plugin Check, checking for reserved slugs, and producing a submission readiness report.

### Check your review status

After submitting, you can check on your plugin's status:

```
What's the review status of my plugin "my-awesome-plugin"?
```

Your assistant will retrieve the current status, including any feedback from the review team.

### Address reviewer feedback

If your plugin needs changes, your assistant can help you work through the feedback:

```
Help me address the review feedback for "my-awesome-plugin".
```

Your assistant will fetch the reviewer's comments, help you understand what's being asked, suggest fixes, and help you re-validate and resubmit.

## Troubleshooting

### Authentication errors

If you see authentication errors, your application password may have expired or been revoked. Visit the [authorization page](https://login.wordpress.org/?action=authorize_application&app_id=c4c73a54-96d7-47b9-9bdc-1a66b9b04505) again to generate a new one. Going through the authorization flow again will automatically replace your existing application password. Update your client configuration with the new credentials.

### Removing access

To revoke your MCP connection, go to your [Account & Security](https://profiles.wordpress.org/me/profile/edit/group/3/) settings on profiles.wordpress.org and open the Application Passwords section. From there you can find and delete the application password created for the MCP connection.
