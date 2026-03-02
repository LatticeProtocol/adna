# .obsidian/ — Obsidian Configuration

This directory contains Obsidian vault configuration. Most files are checked into git to ensure a consistent experience across clones.

> **Quick setup**: Run `./setup.sh` from the repo root to download all plugins and the theme automatically.

## Plugins (11 community plugins)

| Plugin | ID | Purpose |
|--------|----|---------|
| Dataview | `dataview` | Query and display structured data from frontmatter |
| Templater | `templater-obsidian` | Template engine with folder-based auto-triggers |
| Meta Bind | `obsidian-meta-bind-plugin` | Inline metadata editing |
| Homepage | `homepage` | Set a start page on vault open |
| Tasks | `obsidian-tasks-plugin` | Task tracking with queries |
| Style Settings | `obsidian-style-settings` | Customize CSS snippet options via GUI |
| Table Editor | `table-editor-obsidian` | Better markdown table editing |
| Omnisearch | `omnisearch` | Vault-wide fuzzy search |
| Terminal | `terminal` | Terminal access from Obsidian |
| Folder Notes | `folder-notes` | Index files for folder navigation |
| Notebook Navigator | `notebook-navigator` | Folder-based notebook navigation with icons |

## Theme

**Tokyo Night** — installed by `setup.sh`, or manually from Settings → Appearance → Themes.

**Font (optional)**: Install [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) for a polished look. The vault uses system default fonts out of the box — Space Grotesk can be set in Settings → Appearance → Font after installation.

**Accent**: Rebecca Purple (`#663399`) — configured in `appearance.json`.

## CSS Snippets (10)

All snippets are pre-configured and enabled in `appearance.json`. They integrate with Style Settings for GUI customization.

| Snippet | Purpose |
|---------|---------|
| `tokyo_night_purple.css` | Rebecca Purple accent override for Tokyo Night theme |
| `table_stripes.css` | Zebra stripes and hover highlight for tables |
| `mermaid_override.css` | Purple-tinted Mermaid diagrams on dashboard pages |
| `popover_size.css` | Larger link preview popovers |
| `code_blocks.css` | Accent-bordered code blocks, tinted inline code |
| `headings_styled.css` | Centered H1, bordered H2, colored H3+ |
| `links_enhanced.css` | External link arrows, dashed unresolved links |
| `scrollbar_custom.css` | Slim purple-tinted scrollbars |
| `modals_translucent.css` | Frosted glass effect on modals |
| `notebook_navigator.css` | Folder icon and notebook navigation styling |

## Per-Device Files

These files are excluded from git (see `.gitignore`) because they contain machine-specific state:

- `workspace.json` — window layout, open tabs
- `workspace-mobile.json` — mobile layout
- `graph.json` — graph view settings

## Templater Folder Mappings

Templater is configured to auto-apply templates when creating new files in specific directories. See `plugins/templater-obsidian/data.json` for the 10 folder→template mappings.
