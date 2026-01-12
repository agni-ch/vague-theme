# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a VS Code theme extension - a port of the popular vague.nvim theme from Neovim. It provides four dark theme variants with muted colors designed for excellent readability.

## Theme Variants

The extension provides four theme variants, each as a separate JSON file:
- **Vague** (`themes/vague.json`) - The standard variant
- **Vague Dimmed** (`themes/vague-dimmed.json`) - Lower contrast variant
- **Vague Soft** (`themes/vague-soft.json`) - Softer color variant
- **Vague One Dark** (`themes/vague-one-dark.json`) - One Dark inspired variant

## Architecture

### Theme File Structure

Each theme JSON file (~960 lines) follows the VS Code theme schema with two main sections:

1. **`colors` object** (~510 lines): Defines UI element colors for the VS Code interface
   - Editor chrome (background, foreground, line highlights, selections, find matches)
   - UI components (activity bar, sidebar, tabs, panels, status bar, title bar)
   - Interactive elements (buttons, inputs, dropdowns, menus, lists)
   - Git decorations and diff colors
   - Terminal colors
   - Widget styling (hover, suggestions, notifications, peek view, breadcrumbs)
   - Debug UI elements

2. **`tokenColors` array** (~450 lines): Defines syntax highlighting using TextMate scopes
   - General language constructs (comments, strings, numbers, keywords)
   - Specific language support (JavaScript/TypeScript, Python, Go, Rust, HTML, CSS, JSON, Markdown, etc.)
   - Each entry maps TextMate scopes to color values

### Color Palette

The standard Vague variant uses this color palette (from vague.nvim):
- **Background tones**: `#141415` (main), `#1c1c24`, `#252530`, `#333738`
- **Foreground tones**: `#cdcdcd` (main text), `#90a0b5`, `#606079` (muted)
- **Accent colors**:
  - Blue: `#6e94b2`, `#90a0b5` (primary accent)
  - Pink/Red: `#bb9dbd`, `#d8647e`
  - Orange/Yellow: `#e8b589`, `#f3be7c`, `#e0a363`
  - Green: `#7fa563`, `#b4d4cf`
  - Cyan: `#9bb4bc`
  - Hint: `#7e98e8`

Each variant adjusts the background color while maintaining the same accent palette:
- **Vague**: `#141415` (standard, matches upstream vague.nvim)
- **Vague Dimmed**: `#1b1b1d` (lighter for reduced contrast)
- **Vague Soft**: `#16171b` (slightly lighter with more blue)
- **Vague One Dark**: `#282C34` (inspired by Atom's One Dark theme)

### Key Design Patterns

- **Independent variants**: Each theme variant is a complete standalone file. Color changes must be replicated across all four variants if they should apply globally.
- **Opacity for layers**: Many UI elements use hex color codes with alpha channels (e.g., `#40506520`) for semi-transparent overlays.
- **Semantic naming**: Color keys follow VS Code's semantic token naming (e.g., `editor.findMatchBackground`, `list.activeSelectionBackground`).
- **Bracket pair colorization**: Custom colors defined for six levels of nested brackets.

## Development Workflow

### Publishing to VS Code Marketplace

```bash
# Package the extension
npx @vscode/vsce package

# Publish (requires publisher account)
npx @vscode/vsce publish
```

### Local Development/Testing

1. Open this folder in VS Code
2. Press `F5` to launch Extension Development Host with the theme loaded
3. Select one of the Vague theme variants from Command Palette → "Color Theme"
4. Make changes to theme JSON files
5. Reload the Extension Development Host window to see changes

### Making Color Changes

When modifying colors:
- Decide if the change should apply to all variants or just one
- If all variants: Update the same color key in all four JSON files
- Test in Extension Development Host to verify visual appearance
- Pay attention to contrast ratios for accessibility

### Adding New Language Support

To add syntax highlighting for a new language:
1. Identify the TextMate scopes used by that language (use Developer Tools → Inspect Editor Tokens)
2. Add entries to the `tokenColors` array with appropriate scope selectors
3. Use colors from the variant's established palette for consistency
4. Test with real code samples in that language

## Important Notes

- No build process or compilation - theme files are used directly by VS Code
- No npm scripts defined - this is a pure theme extension with no dependencies
- Version updates require changing `version` field in `package.json`
- The `icon.png` file is the extension icon shown in the marketplace
