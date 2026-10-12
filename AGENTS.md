# Marketplace maintenance rules

These instructions apply when editing this repository. `plugins/astro/rules/astro-project.md` is guidance for Astro projects that install the plugin; it is not the maintenance policy for this marketplace.

## Shared skills

- Group each technology under `plugins/<technology>/`. Keep one canonical `skills/<skill-name>/SKILL.md` per workflow for Claude Code and Codex. Do not copy a skill into separate host directories.
- Use portable Agent Skills frontmatter in shared skills: a folder-matching `name` and a concise `description` that says when to use it. Add another standard field only when needed and supported by both hosts. Keep Claude-only invocation, tool, or context fields out of shared frontmatter.
- Write workflows in host-neutral language. When a workflow depends on a tool such as Penpot MCP or Playwright, state the capability needed and what to do when it is unavailable; do not assume an exact tool identifier exists in both hosts.
- Keep relative links and supporting files within the plugin directory so installed copies work. Put conditional detail in `references/`, repeatable deterministic work in `scripts/`, and output templates in `assets/` only when useful.
- Preserve a skill's public name and task scope when editing it. If a rename or changed trigger is necessary, treat it as a breaking change and update all agent references, catalogs, documentation, and migration notes.

## Host-specific files

- Claude Code agents live in `plugins/<technology>/agents/*.md`; Codex agent templates live in `plugins/<technology>/codex/agents/*.toml`. Keep their purposes and report contracts aligned, while using each host's supported syntax. Codex templates require copying into a target project's `.codex/agents/`; plugin installation alone does not install them there.
- Keep `plugins/<technology>/plugin.json` as the portable manifest and `.claude-plugin/plugin.json` as the Claude Code manifest. Keep `name`, `version`, and description consistent. The root `.agents/plugins/marketplace.json` and `.claude-plugin/marketplace.json` are separate catalogs pointing to the same plugin folder.
- Keep project rules in `plugins/<technology>/rules/`; document how a target project imports or merges them into `AGENTS.md` and `CLAUDE.md`. Do not mistake Codex `.codex/rules/*.rules` command permission files for prose project instructions.

## Changes and releases

- For every skill edit, check its frontmatter, links, referenced agents, and both client paths. For new agents, update the shared QA report contract and coordinator if they participate in release reviews. Validate JSON, TOML, YAML frontmatter, and relative paths; run `claude plugin validate` when Claude Code is available and test installation or discovery in each available client.
- Version each technology plugin as one unit, not each skill. Until `1.0.0`, use a patch bump for corrections and a minor bump for new capabilities or changed workflows; document incompatible changes prominently. After `1.0.0`, use SemVer major, minor, and patch. Keep the two plugin manifest versions identical, and do not duplicate the version in marketplace entries.
- Bump the version only for a release, after review and checks. Tag the release commit as `<plugin-name>-v<version>` (for example, `moduo-astro-v0.2.0`) and record user-visible changes. Check published tags and releases before choosing a new version; never reuse a published version or move a published tag.
- A marketplace source pinned to a Git ref freezes the whole catalog at that ref. With this repository's relative plugin paths, individual plugins cannot be pinned independently; use a Git-backed per-plugin source or separate catalog if that becomes necessary.
