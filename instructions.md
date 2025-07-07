# 🥥 Coco MCP Server Instructions

## Overview

Coco (Context Coder) is an MCP server that provides AI assistants with comprehensive codebase awareness. Unlike traditional file-by-file approaches, Coco enables you to understand entire project structures instantly through its intelligent code digestion capabilities.

## Primary Workflow

### 1. Check Codebase Size First

**ALWAYS begin by calling `get_codebase_size`** - This is critical to ensure the codebase isn't too large for processing. This tool:

- Checks total token counts for Claude and ChatGPT
- Warns if the codebase exceeds recommended limits (150k for Claude, 128k for ChatGPT)
- Shows the top 10 largest files that might need to be excluded
- Suggests creating a `.aidigestignore` file if needed

### 2. Get Full Context

After checking the size, **call `get_codebase`** - This provides a complete representation of the entire codebase, giving you immediate understanding of:

- Project structure and organization
- All code files and their contents
- Dependencies and relationships
- Overall architecture patterns

The digest is paginated (100k chars/page). If you see a message like "You MUST call this tool again with page...", continue retrieving pages with get_codebase using the page parameter until you have the complete context.

### 3. Work with Full Context

Once you have the codebase digest, you can:

- Answer questions about any part of the code
- Suggest improvements that align with existing patterns
- Write new code that fits the project's style
- Understand complex interactions between components

### 4. Modify with Confidence

Use writing tools to make changes:

- `write_file` - Create or update files
- `create_directory` - Set up new directories
- `move_file` - Reorganize code structure

### 5. Use Other Tools Sparingly

Only use these tools when specifically needed:

- `read_file` - Only when explicitly asked to re-read a file or when the digest doesn't contain what you need
- `list_directory` - Only when users ask "what files are in X directory?"
- `directory_tree` - Only when users request a visual tree structure
- `search_files` - When looking for specific patterns across files
- `execute_command` - For running build/test commands

## Managing Large Codebases

If `get_codebase_size` warns about a large codebase:

1. **Create a `.aidigestignore` file** in the project root (similar to `.gitignore`)
2. Add patterns for files/directories to exclude:

   ```
   # Large generated files
   dist/
   build/
   *.min.js

   # Dependencies
   node_modules/
   vendor/

   # Large data files
   *.csv
   *.json
   data/
   ```

3. Common patterns to exclude:
   - Build outputs (`dist/`, `build/`, `out/`)
   - Dependencies (`node_modules/`, `vendor/`, `.venv/`)
   - Generated files (`*.min.js`, `*.map`)
   - Large assets (`images/`, `videos/`)
   - Data files (`*.csv`, `*.json`, `*.sql`)

## Path Conventions

All paths are relative to the mounted root directory:

- Always use "./" prefix: `./src/index.ts`, `./package.json`
- Parent directory access ("../") is blocked for security
- In development mode, "./" maps to the `./mount` directory

## Best Practices

1. **Size Check First**: Always run `get_codebase_size` before `get_codebase`
2. **Context Awareness**: Use the full codebase context to understand patterns
3. **Respect Patterns**: Follow existing conventions found in the codebase
4. **Minimal Reads**: Avoid redundant file reads when information is already in context
5. **Smart Navigation**: Use search instead of browsing when looking for specific code
6. **Safe Operations**: All file operations are atomic and secure

Remember: Coco's strength is providing complete context awareness. Use this to write better, more integrated code that truly fits the project.
