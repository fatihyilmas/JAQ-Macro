# Security

## MCP connection security

- **Copy AI quick start** includes a bearer token. Enter it in the private MCP configuration of a client you trust. Keep tokens, API keys, passwords, license keys and private backups out of public issues, shared prompts and workflow steps.
- If an MCP key is exposed, revoke it in JAQ Macro’s account area, create a replacement and update your client configuration.
- Connected assistants can read project metadata and queue permitted edits. Review incoming changes in JAQ Macro before running them. MCP authoring keeps local image files on the device. The separate provider-backed OCR feature sends the selected image region to the configured provider when used.

## Reporting a security issue

Use the authenticated **Support** area in JAQ Macro for private reports. Include the app version, the affected feature, the observed behavior and sanitized reproduction steps. Remove credentials, personal details and private workflow data from logs and screenshots before attaching them.

## Official downloads

Download the Windows ZIP from the release page. JAQ Macro verifies update descriptors against its pinned publisher key and uses the signed file list and hashes to check the package. [Official GitHub Releases](https://github.com/fatihyilmas/JAQ-Macro/releases) · [Update verification](docs/UPDATES.md)
