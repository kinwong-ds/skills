# Using These Skills with Cline

These skills are designed to work with **any AI coding agent**, including [Cline](https://github.com/cline/cline). They focus on engineering practices and workflow patterns rather than model-specific features.

## Quick Start with Cline

### 1. Get the Skills

Clone this repository:
```bash
git clone https://github.com/kinwong-ds/skills.git
cd skills
```

### 2. Configure Cline

#### Option A: Use as Custom Instructions (Recommended for getting started)

1. In Cline, open **Settings** → **Custom Instructions**
2. For each skill you want to use, copy the content from its `SKILL.md` file into custom instructions
3. Start using them with the command syntax (e.g., `/diagnose`, `/grill-with-docs`)

#### Option B: Point Cline to Skills Directory

Some versions of Cline support loading skills from a directory:
1. In Cline settings, look for "Skills Directory" or "Custom Skills Path"
2. Point it to your cloned `skills` repository
3. Cline should auto-discover the `.cline/cline.json` configuration

### 3. Run Initial Setup (Optional)

If you want to use the full setup experience:

1. Use `/setup-matt-pocock-skills` in Cline to configure:
   - Your issue tracker (GitHub, Linear, or local files)
   - Triage labels for your project  
   - Documentation locations

This creates project-specific configuration that enhances other skills.

## Recommended Skills by Use Case

### Starting a New Feature
1. **`/grill-with-docs`** — Align with Cline on requirements and document domain language
2. **`/to-prd`** — Turn conversation into a formal PRD
3. **`/to-issues`** — Break PRD into actionable GitHub issues

### Writing Code
1. **`/tdd`** — Test-driven development with red-green-refactor
2. **`/prototype`** — Build throwaway prototypes before committing
3. **`/zoom-out`** — Get broader context when navigating unfamiliar code

### Debugging
1. **`/diagnose`** — Structured debugging workflow: reproduce → minimize → hypothesize → instrument → fix
2. **`/triage`** — Categorize and prioritize issues

### Communication
1. **`/caveman`** — Ultra-compressed technical communication (~75% fewer tokens)
2. **`/handoff`** — Package current work for another agent to continue
3. **`/teach`** — Learn a new skill over multiple sessions

## Key Differences from Claude Code

Since these skills were built for Claude Code but work with Cline:

### What Works Exactly the Same
- **Workflow patterns** — The core approach to engineering (TDD, grilling, etc.)
- **Documentation structure** — CONTEXT.md, PRD templates, decision records
- **Interaction patterns** — Asking questions, breaking down complex tasks

### What Might Need Adaptation
- **GitHub API integration** — Verify Cline supports the same GitHub actions
- **File system operations** — Cline's file access may differ slightly
- **Tool availability** — Some skills reference Claude-specific tools; check Cline's available integrations

### Adaptation Tips
1. **Check Cline's supported tools** before running skills that create files
2. **Test in a non-critical repo first** to validate behavior
3. **Report issues** if specific skills don't work as expected
4. **Modify skills** to match your workflow — they're meant to be hacked on

## File Structure

```
skills/
├── .cline/
│   └── cline.json              # Cline configuration
├── skills/
│   ├── engineering/            # Code-focused skills
│   │   ├── diagnose/
│   │   ├── grill-with-docs/
│   │   ├── tdd/
│   │   └── ...
│   ├── productivity/           # General workflow skills
│   │   ├── grill-me/
│   │   ├── caveman/
│   │   └── ...
│   ├── misc/                   # Less common skills
│   └── deprecated/             # No longer used
├── CONTEXT.md                  # (Optional) Domain language for your repos
├── CLINE_SETUP.md             # This file
└── README.md                   # Original documentation
```

## Using Skills with Your Projects

### Per-Project Setup
For each project that uses these skills:

1. **Create `CONTEXT.md`** in your project root:
   ```markdown
   # Project Context
   
   ## Domain Language
   - **Materialization**: Converting a template into real files
   - **Cascade**: Process that propagates through dependent objects
   
   ## Key Modules
   - `src/core/` — Business logic
   - `src/api/` — External interfaces
   ```

2. **Point Cline** to this directory with the skills
3. **Reference `CONTEXT.md`** in `/grill-with-docs` conversations

## Troubleshooting

### Skill commands don't work
- Make sure you're using the correct syntax: `/skill-name` 
- Some versions of Cline may require "Run Skill:" prefix

### File creation doesn't work
- Check that Cline has write permissions in the target directory
- Verify the file path is relative to the project root

### Integration with GitHub/Linear fails
- Confirm Cline is authenticated with GitHub
- Check that your PAT/credentials are configured
- Some skills may need manual adaptation for Cline's GitHub integration

## Contributing & Customization

These skills are **meant to be forked and modified**. 

To create your own:
1. Copy a similar skill directory
2. Update the `SKILL.md` with your approach
3. Use `/write-a-skill` in Cline to help structure new skills
4. Test thoroughly in a sandbox project

## Resources

- **Original skills repo**: https://github.com/mattpocock/skills
- **Cline docs**: https://github.com/cline/cline
- **Skills philosophy**: See the main README.md for the engineering principles behind each skill

---

**Ready to go?** Pick one skill, read its `SKILL.md`, and try it with Cline. Start with `/grill-with-docs` or `/tdd` — they're the most popular.
