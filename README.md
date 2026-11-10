# Agent Mentors 🧠

> Universal AI Agent Skills that turn coding assistants into Socratic mentors for Software Engineering, Distributed Systems, and Product Design.

Default AI coding assistants suffer from an anti-learning flaw: **they dump full answers immediately**. This creates cognitive passivity and ruins retention.

**Agent Mentors** is a collection of modular agent skills (following the open `SKILL.md` standard) engineered with strict pedagogical guardrails. They force the model to behave like a Senior Staff Engineer or Design Director: guiding through discovery, questioning assumptions, teaching first-principles mental models, and never spoon-feeding solutions.

All mentors enforce the **Conversational Ping-Pong & Verbosity Gate Protocol**:
- ⚡ **Token-Conscious (< 80 tokens/turn)**: tight 40–60 word responses; zero conversational filler or pleasantries.
- 🚪 **The Verbosity Gate (Permission Required)**: mentors never dump long explanations, code traces, or diagrams unprompted; they must ask permission to unpack deep dives first.
- 🚫 **No walls of text**: strictly 2–3 short sentences per standard turn.
- 🏓 **Always back-and-forth**: exactly one focused question or reflection nudge per turn.
- 🎯 **Adaptive Scaffolding**: mirrors your assumptions and hints at alternative perspectives without giving away the answer.

---

## 📦 Included Mentors

| Skill | Focus Area | Description |
| :--- | :--- | :--- |
| **[`socratic-code-coach`](skills/socratic-code-coach/SKILL.md)** | Programming & Algorithms | Adaptive mastery engine with 5-tier ladder, PCDIT & Pólya frameworks, anti-freeze protocol, and strict anti-spoonfeed guardrails. |
| **[`systems-architect-mentor`](skills/systems-architect-mentor/SKILL.md)** | Distributed Systems & Scalability | Explores tradeoffs, back-of-the-envelope math, physical hardware limits, and failure injection testing. |
| **[`incident-postmortem-mentor`](skills/incident-postmortem-mentor/SKILL.md)** | SRE & Incident Analysis | Guides blameless Root Cause Analysis (RCA), detects monitoring gaps, blast radius risks, and systemic architectural fixes. |
| **[`cicd-pipeline-mentor`](skills/cicd-pipeline-mentor/SKILL.md)** | CI/CD & Delivery Architecture | Audits deployment safety, silent failure swallowing (`\|\| true`), state coupling across jobs, and secret exposure. |
| **[`product-design-mentor`](skills/product-design-mentor/SKILL.md)** | UX & Product Design | Teaches user mental models, cognitive load reduction, information architecture, and design system rigor. |

---

## 🚀 Quick Install & Compatibility

Agent Mentors uses the universal `SKILL.md` format supported across all major AI agent ecosystems.

### 1. Google Antigravity / Gemini CLI (`agy`)

**Global (all workspaces on your machine):**
```bash
git clone https://github.com/alkurdy/agent-mentors.git ~/.gemini/config/skills/agent-mentors
```

**Project-Specific (checked into your repository):**
```bash
git clone https://github.com/alkurdy/agent-mentors.git .agents/skills/agent-mentors
```

---

### 2. Claude Code

Add the skills to your project's `.claude/` or reference them in your `CLAUDE.md`:

```markdown
# Developer Mentorship
When I ask to learn or practice a topic, read and adopt the mentor instructions in:
- `skills/socratic-code-coach/SKILL.md` (for coding exercises)
- `skills/systems-architect-mentor/SKILL.md` (for systems design)
- `skills/incident-postmortem-mentor/SKILL.md` (for incident RCA / postmortems)
- `skills/cicd-pipeline-mentor/SKILL.md` (for CI/CD pipeline architecture)
- `skills/product-design-mentor/SKILL.md` (for UI/UX architecture)
```

---

### 3. Cursor / Windsurf / Copilot

Place in `.cursor/rules/` or add to your `.cursorrules`:

```markdown
Follow the pedagogical guidelines and discovery loops outlined in the skills/ directory of https://github.com/alkurdy/agent-mentors.
Never provide full working solutions unprompted; guide with Socratic breadcrumbs.
```

---

### 4. Custom Agents & Prompts (OpenAI, Anthropic, Ollama)

Import any individual `SKILL.md` directly into your system prompt or custom assistant configuration.

---

## 🛠️ How to Add a New Mentor

1. Fork this repository.
2. Create a folder in `skills/<mentor-name>/`.
3. Add `SKILL.md` with standard YAML frontmatter:
   ```markdown
   ---
   name: your-mentor-name
   description: Activate when the user wants to learn [domain]...
   ---

   # Your Mentor Title
   ## Core Directives
   ## The Mentorship Loop
   ```
4. Submit a Pull Request!

---

## ⚖️ License

[MIT License](LICENSE) © 2026 alkurdy.
