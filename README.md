# Moduo AI Marketplace

A marketplace of skills and agents grouped by technology. Each technology lives under `plugins/<technology>/`. Claude Code and Codex share the same `SKILL.md` files, while agents and marketplace catalogs use host-specific formats.

## Structure and compatibility

| Element | Source of truth | Claude Code | Codex |
| --- | --- | --- | --- |
| Skills | `plugins/astro/skills/<name>/SKILL.md` | Loads the plugin's skills | Loads the same plugin skills |
| Agents | Technology-specific reviewers | `plugins/astro/agents/*.md` | Templates in `plugins/astro/codex/agents/*.toml`; copy them to the target project's `.codex/agents/` |
| Project rules | `plugins/astro/rules/astro-project.md` | Merge them into `CLAUDE.md` or import the project's `AGENTS.md` | Merge them into the project's `AGENTS.md` |
| Identity and version | `plugins/astro/plugin.json` | Keep `plugins/astro/.claude-plugin/plugin.json` in sync | Uses the portable manifest |
| Marketplace catalog | One entry per plugin | `.claude-plugin/marketplace.json` | `.agents/plugins/marketplace.json` |

The [maintenance rules](./AGENTS.md), shared with Claude through [CLAUDE.md](./CLAUDE.md), explain how to change a skill without breaking either client. Shared frontmatter uses `name` and `description`; Claude-only options stay out of shared skills. Both clients document this `SKILL.md` format. [Claude Code](https://code.claude.com/docs/en/skills), [OpenAI Docs](https://developers.openai.com/plugins/build/skills).

### Astro plugin

[`plugins/astro`](./plugins/astro/) covers design tokens, UI, tests, functional and responsive QA, accessibility, performance, localized content, CMS integrations, and SEO. Its review agents use a [shared report format](./plugins/astro/references/qa-report.md); `astro-qa-coordinator` consolidates findings and coverage gaps. See [research and attribution](./plugins/astro/SOURCES.md).

The Astro project rules are a template for target projects. Installing the plugin does not automatically add rules to a project's `AGENTS.md` or `CLAUDE.md`, or copy Codex agent templates into that project.

## Changing a skill

1. Edit the canonical `SKILL.md` under `plugins/<technology>/skills/`. Keep its `name` equal to the folder name and write a `description` that clearly says when to use it. Keep references, scripts, and assets inside the same plugin.
2. Keep instructions independent of the client. If a feature is specific to Claude Code or Codex, document that variant in its host-specific file and provide a clear alternative in the shared skill.
3. If a reviewer's behavior changes, update the Claude agent, the Codex template, and, where relevant, the [report contract](./plugins/astro/references/qa-report.md) and coordinator.
4. Validate JSON, TOML, frontmatter, and relative links. When Claude Code is available, run `claude plugin validate ./plugins/astro` and `claude plugin validate .`, then check installation or loading in both clients. Claude's validation checks manifests and catalogs, while installation can reveal additional issues. [Claude Code](https://code.claude.com/docs/en/plugin-marketplaces).

## Versioning and publishing

**Versioning unit:** one plugin per technology. Astro skills evolve together under `moduo-astro`; individual `SKILL.md` files do not have separate version numbers. The catalogs do not have their own semantic version: a Git tag identifies a snapshot of the entire marketplace. The plugin manifests currently declare `0.2.0`; consult the repository's GitHub Releases and tags for published history.

| Change | Policy before `1.0.0` | Policy after `1.0.0` |
| --- | --- | --- |
| Editorial or instruction fix without a workflow change | Patch | Patch |
| New skill, agent, or workflow; compatible extension | Minor | Minor |
| Incompatible name, path, or contract change | Minor with documented migration | Major |

For each release, set **the same version** in `plugins/<technology>/plugin.json` and `plugins/<technology>/.claude-plugin/plugin.json`, record user-visible changes, and tag the commit as `<plugin>-v<version>`; for example, `moduo-astro-v0.2.0`. Do not repeat `version` in either marketplace catalog entry. Claude Code prioritizes the version in its plugin manifest and does not deliver a remote edit under the same version number; plugins loaded in place from a local path behave differently. [Claude Code: versions and updates](https://code.claude.com/docs/en/plugins/host-marketplace), [OpenAI Docs: packaging and versioning](https://developers.openai.com/plugins/build/plugins).

Git tags can pin **the entire marketplace** to a known state. Claude accepts `moduo-tech/moduo-ai-marketplace#<ref>`; Codex accepts `codex plugin marketplace add moduo-tech/moduo-ai-marketplace --ref <ref>`. Codex can refresh a Git source with `codex plugin marketplace upgrade`. With the current relative paths, the ref pins the whole catalog. Independent plugin pinning would require a separate Git-backed source per plugin or separate catalogs. [Claude Code: refs](https://code.claude.com/docs/en/plugins/host-marketplace), [OpenAI Docs: marketplaces](https://developers.openai.com/plugins/build/plugins).

### Release procedure

1. **Review the scope.** Identify changed skills, agents, and rules. Update both variants of any affected agent and the catalogs if you add a plugin. Record user-visible changes and any required migration.
2. **Choose the plugin version.** Apply the policy above and set the exact same new value in `plugins/astro/plugin.json` and `plugins/astro/.claude-plugin/plugin.json`. Check published tags and releases before selecting it; never reuse a published version. Do not add a version to each skill or to `marketplace.json` entries.
3. **Validate before publishing.** Check JSON, TOML, YAML frontmatter, relative links, and matching names; run `git diff --check`. With Claude Code installed, run `claude plugin validate ./plugins/astro` and `claude plugin validate .`. Confirm Codex discovers the plugin from the local catalog and that a skill loads in each client. To test a Codex agent, copy its template into a test project's `.codex/agents/`. Note any unavailable client checks in the release notes.
4. **Create the release commit.** Include both manifests and the release notes in a reviewed commit. Merge and push it to the release branch. Then create an annotated, immutable tag on that same commit and push the tag. For example, once the branch is pushed:

   ```text
   git tag -a moduo-astro-v0.2.0 -m "moduo-astro 0.2.0"
   git push origin moduo-astro-v0.2.0
   ```

5. **Publish and verify.** Create a GitHub Release from the tag, including changes, migrations, and outstanding checks. Update the marketplace in Claude Code or run `codex plugin marketplace upgrade` to refresh Codex's Git source; install or update `moduo-astro` and verify a skill in a new session. Never reuse a version number or move a published tag. [Claude Code: updates](https://code.claude.com/docs/en/plugins/host-marketplace), [OpenAI Docs: Git sources](https://developers.openai.com/plugins/build/plugins).

## Test installation

After publishing the repository, Claude Code can register the catalog with `claude plugin marketplace add moduo-tech/moduo-ai-marketplace` and install `moduo-astro@moduo-marketplace`. Codex can register it with `codex plugin marketplace add moduo-tech/moduo-ai-marketplace` and install `moduo-astro` from its plugin browser. For local tests, both clients accept a path to this repository; check the commands available in the installed client version. [Claude Code](https://code.claude.com/docs/en/plugin-marketplaces), [OpenAI Docs](https://developers.openai.com/plugins/build/plugins).
