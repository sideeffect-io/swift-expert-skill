# SwiftUI Expert Skill

A portable SwiftUI skill for AI coding agents that support the Agent Skills format. It focuses on current SwiftUI APIs, Observation-based data flow, view composition, navigation and presentation, tabs and forms, deeplinks, lists and scrolling, performance and profiling, accessibility, theming, previews, animations, Swift and concurrency hygiene, optional iOS 26 Liquid Glass, on-demand macOS guidance, and micro references for long-tail SwiftUI patterns such as sheets, programmatic scroll, image loading, list identity, and macOS-specific UI.

The repository contains one reusable skill folder: `swiftui-expert-skill/`.

## Scope

- Latest-first SwiftUI guidance for Apple platforms.
- Architecture-agnostic: it does not require MVVM, a router framework, or a specific project layout.
- Covers component patterns such as tabs, forms, grids, placeholders, previews, haptics, theming, and app-level deeplinks without prescribing an app architecture.
- macOS-only APIs stay in dedicated optional references so the default trigger cost stays low.

## Installation

### Manual

Copy or symlink [`swiftui-expert-skill`](./swiftui-expert-skill) into the skills directory used by your agent.

Useful references:

- Agent Skills format: [agentskills.io/home](https://agentskills.io/home)
- Codex skills docs: [developers.openai.com/codex/skills](https://developers.openai.com/codex/skills/)
- Claude Code skills docs: [platform.claude.com/docs/en/agents-and-tools/agent-skills/overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

### npx

Once this repository is published, install it from GitHub with:

```bash
npx skills add https://github.com/<owner>/<repo> --skill swiftui-expert-skill
```

### Codex

Install the skill folder into either:

- Project scope: `.codex/skills/`
- User scope: `~/.codex/skills/`

See the official Codex skills documentation: [developers.openai.com/codex/skills](https://developers.openai.com/codex/skills/).

The root folder should look like:

```text
.codex/
  skills/
    swiftui-expert-skill/
      SKILL.md
      references/
```

### Claude Code

Install the skill folder into either:

- Project scope: `.claude/skills/`
- User scope: `~/.claude/skills/`

See the official Claude Code skills documentation: [platform.claude.com/docs/en/agents-and-tools/agent-skills/overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

The root folder should look like:

```text
.claude/
  skills/
    swiftui-expert-skill/
      SKILL.md
      references/
```

## Usage

Explicit invocation examples:

- Codex: `$swiftui-expert review this SwiftUI feature for state, navigation, and performance issues`
- Claude Code: `/swiftui-expert modernize this SwiftUI screen and keep behavior unchanged`

Natural-language examples:

- `Use the SwiftUI Expert skill to refactor this view hierarchy without changing behavior.`
- `Use the SwiftUI Expert skill to review this project for outdated SwiftUI APIs and accessibility issues.`
- `Use the SwiftUI Expert skill to build a searchable sheet-based flow with modern Observation.`
- `Use the SwiftUI Expert skill to diagnose janky scrolling in this list and tell me what to profile.`
- `Use the SwiftUI Expert skill to wire typed tabs and deep links for this SwiftUI app without changing its architecture.`
- `Use the SwiftUI Expert skill to review this code for SwiftUI issues, concurrency hazards, and localization or persistence hygiene problems.`
- `Use the SwiftUI Expert skill to add Liquid Glass to this iOS 26 toolbar with fallbacks.`
- `Use the SwiftUI Expert skill to review this macOS MenuBarExtra and Settings scene setup.`

## References

This skill was merged and rewritten from:

- [AvdLee/SwiftUI-Agent-Skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)
- [twostraws/SwiftUI-Agent-Skill](https://github.com/twostraws/SwiftUI-Agent-Skill)
- [Dimillian/Skills](https://github.com/Dimillian/Skills)

Accuracy was checked against current Apple documentation using Cupertino MCP, Sosumi MCP, and official Apple docs, including SwiftUI, Observation, and WebKit for SwiftUI references.
