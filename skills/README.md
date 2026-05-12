# Jota Skills

  A collection of reusable skills for AI coding agents.

  ## What is a Skill?

  A skill is a Markdown file that teaches an AI agent a repeatable technique, pattern, or workflow. Instead of explaining the same process every time, you write it once and
   the agent loads it on demand.

  Skills follow the [AgentSkills specification](https://agentskills.io/specification).

  ## Structure

  skills/
    daily-news/
      SKILL.md
    your-skill/
      SKILL.md

  Each `SKILL.md` has a YAML frontmatter with `name` and `description`, followed by the instructions.

  ## How to Use

  ### Claude Code

  Place skills in `~/.claude/skills/` or clone this repo there directly:

  ```bash
  git clone https://github.com/your-username/skills ~/.claude/skills
  ```

  Invoke with the Skill tool or via slash command: /daily-news

  Gemini CLI

  Skills are auto-discovered via GEMINI.md. Clone to ~/.agents/skills/ and reference in your GEMINI.md.

  OpenAI / Codex

  Clone to ~/.agents/skills/. Use the skill tool as documented in references/codex-tools.md.

  Skills

  ┌───────────────────────┬────────────────────────────────────────────────────────────────────────────────────────┐
  │         Skill         │                                      Description                                       │
  ├───────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
  │ ./daily-news/SKILL.md │ Fetch today's top news from multiple sources in parallel and synthesize a top-5 digest │
  └───────────────────────┴────────────────────────────────────────────────────────────────────────────────────────┘
