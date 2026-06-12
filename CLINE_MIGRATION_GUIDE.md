# Migrating from Claude Code to Cline

This guide explains how to use the Matt Pocock skills collection with Cline instead of Claude Code, and what to watch out for.

## Why These Skills Work with Cline

These skills are fundamentally **agent-agnostic**. They:

- Focus on **engineering practices** (TDD, grilling, etc.), not Claude-specific APIs
- Use **standard inputs** (files, GitHub issues, conversation) that all agents can access
- Encode **decision-making patterns** that apply across models and tools
- Avoid hard dependencies on Claude Code-specific features

## What's Different

### Claude Code Assumptions

The original skills reference Claude Code in a few ways:

1. **References to "Claude"** in documentation → Apply to any agent
2. **GitHub integration via Claude Code UI** → Use Cline's native GitHub support
3. **File operations through Claude's file system** → Work the same way in Cline
4. **Mentions of "agent" or "you"** → Still apply to Cline

### What You Need to Do

#### ✅ No Changes Needed
- TDD workflow (write tests, fix code, refactor)
- Grilling process (asking clarifying questions)
- Architecture reviews using CONTEXT.md
- GitHub issue creation and management
- Creating PRDs and breaking them into issues

#### ⚠️ Minor Adjustments
- **File creation syntax**: Should work identically, but test first
- **GitHub API calls**: Verify Cline's API token is configured
- **Shell command execution**: May need adaptation for your environment
- **Custom tool calls**: Check what Cline supports vs. Claude Code

#### 🔄 Potentially Breaking
- **Claude-specific tool references**: Look for "claude" or "claude-code" in skill markdown
- **Assumption about model capabilities**: Some older skills assume Claude's reasoning abilities
- **IDE integrations**: Claude Code has VS Code integration; Cline may differ

## Migration Checklist

### Step 1: Setup (5 minutes)
- [ ] Clone this repo to your machine
- [ ] Read `CLINE_SETUP.md`
- [ ] Install/enable Cline in your IDE

### Step 2: Configuration (10 minutes)
- [ ] Set up GitHub credentials in Cline
- [ ] Choose one skill to test (recommend `/tdd` or `/grill-me`)
- [ ] Copy the skill's `SKILL.md` content into Cline custom instructions

### Step 3: Test Run (20 minutes)
- [ ] Open a non-critical test repository
- [ ] Create a simple feature branch
- [ ] Run the test skill with a small task
- [ ] Verify it works as expected

### Step 4: Full Rollout (ongoing)
- [ ] Add more skills to your Cline instructions
- [ ] Set up `CONTEXT.md` in your project repos
- [ ] Create project-specific documentation
- [ ] Run `/setup-matt-pocock-skills` if using GitHub issues

## Common Issues & Solutions

### Issue: `/command` doesn't trigger the skill

**Solution:**
- Cline may not recognize the slash-syntax the same way Claude Code does
- Instead of `/diagnose`, try including the full skill prompt in custom instructions
- Check Cline's documentation for its custom instruction syntax

### Issue: GitHub operations fail

**Solution:**
- Verify Cline has a valid GitHub PAT (Personal Access Token)
- Check that your PAT has `repo`, `issues`, and `pull_requests` scopes
- Test with `gh auth status` if you have GitHub CLI installed

### Issue: File creation doesn't work

**Solution:**
- Make sure the target directory exists
- Verify Cline has write permissions
- Try creating a simple test file first to debug
- Check the exact error message — Cline will tell you what went wrong

### Issue: Skills work but produce verbose output

**Solution:**
- This is expected! Many of these skills are designed for detailed feedback
- Use `/caveman` to compress output and reduce token usage
- Customize the skill to match your preferences

### Issue: Skill references "Claude" or "Claude Code"

**Solution:**
- **Read-only**: No changes needed, apply to Cline instead
- **Functional**: May need minor edits to match Cline's interface
- **Fork the skill**: Copy it to your own version and adapt as needed

## Skill-by-Skill Migration Notes

### Engineering Skills

#### `/diagnose` — Debugging workflow
- **Status**: ✅ Fully compatible
- **Notes**: Works identically in Cline; just follow the workflow

#### `/grill-with-docs` — Planning & documentation
- **Status**: ✅ Fully compatible
- **Notes**: Writes CONTEXT.md and ADRs; works with any agent

#### `/tdd` — Test-driven development
- **Status**: ✅ Fully compatible
- **Notes**: Language-agnostic; works with any test framework

#### `/to-prd` → `/to-issues` — Feature planning
- **Status**: ✅ Fully compatible
- **Notes**: Creates GitHub issues; just needs GitHub auth in Cline

#### `/improve-codebase-architecture` — Architecture review
- **Status**: ✅ Fully compatible
- **Notes**: Requires CONTEXT.md; reads existing code structure

#### `/zoom-out` — Getting broader context
- **Status**: ✅ Fully compatible
- **Notes**: Simple instruction to explain code in system context

#### `/prototype` — Rapid prototyping
- **Status**: ✅ Fully compatible
- **Notes**: Create throwaway code; works in any environment

### Productivity Skills

#### `/grill-me` — Detailed planning interview
- **Status**: ✅ Fully compatible
- **Notes**: Pure conversation; works identically in Cline

#### `/caveman` — Compressed communication
- **Status**: ✅ Fully compatible
- **Notes**: Token-saving mode; useful for long sessions

#### `/handoff` — Pass work to another agent
- **Status**: ✅ Fully compatible
- **Notes**: Creates summary document; works with any agent

#### `/teach` — Learning skill
- **Status**: ✅ Fully compatible
- **Notes**: Creates workspace; teaches new concepts

#### `/write-a-skill` — Create new skills
- **Status**: ✅ Fully compatible
- **Notes**: Help Cline create new custom instructions

### Misc Skills

#### `/git-guardrails-claude-code` — Git safety hooks
- **Status**: ⚠️ Claude Code specific
- **Notes**: Setup can be done manually; not directly compatible

#### `/scaffold-exercises` — Create exercise structures
- **Status**: ✅ Fully compatible
- **Notes**: File creation; works in Cline

## Best Practices for Cline

### 1. Start Small
- Use one skill on a small, non-critical project first
- Get comfortable with the workflow
- Then expand to your main projects

### 2. Customize Your Context
- Create a `CONTEXT.md` in each project that uses these skills
- Define your domain language (key terms, concepts)
- This makes all agents significantly more effective

### 3. Chain Skills Together
- **Feature work**: `/grill-with-docs` → `/to-prd` → `/to-issues` → `/tdd`
- **Debugging**: `/diagnose` to find the root cause
- **Architecture**: `/zoom-out` → `/improve-codebase-architecture`

### 4. Adapt and Extend
- These are **templates**, not locked-in processes
- Modify skills to match your team's workflow
- Create new skills for your specific needs

### 5. Use Compression When Needed
- Long conversations? Use `/caveman` for token efficiency
- Handing off to another agent? Use `/handoff`
- Need a summary? Let the skill create documentation

## Testing Checklist for Each Skill

When testing a skill with Cline:

```markdown
- [ ] Skill prompt loads correctly
- [ ] Can reference the skill by name/command
- [ ] Produces expected output format
- [ ] File creation works (if applicable)
- [ ] GitHub integration works (if applicable)
- [ ] Time taken is reasonable
- [ ] Output quality matches original Claude Code
```

## Troubleshooting Reference

| Problem | Check | Solution |
|---------|-------|----------|
| Skill not found | Custom instructions enabled? | Paste full SKILL.md content |
| GitHub fails | PAT token valid? | `gh auth status` or re-auth in Cline |
| File creation fails | Directory exists? Permissions ok? | Create test file manually first |
| Verbose output | Expected for some skills | Use `/caveman` to compress, or customize prompt |
| Different behavior | Model differences | Claude vs Cline may reason differently; adjust prompts as needed |

## Performance Expectations

### Expected Behavior

- **`/grill-with-docs`**: 5-10 minutes of conversation, creates documentation
- **`/tdd`**: Creates test, fails it, fixes it, refactors — cycles through code changes
- **`/diagnose`**: 3-5 repro/instrument cycles before identifying root cause
- **`/to-prd` → `/to-issues`**: Creates 8-15 granular issues from a plan

### Cline-Specific Notes

- **Faster execution**: Cline may be faster at file operations than Claude Code
- **Different reasoning**: Output quality depends on your Cline model
- **Token tracking**: Monitor usage if on a metered plan

## When to Modify Skills

You might need to adapt a skill if:

1. **References to Claude**: Replace with "the agent" or "Cline"
2. **Claude Code UI elements**: Map to Cline's interface
3. **Specific tool requirements**: Add/remove based on Cline's capabilities
4. **Team process**: Customize prompts to match your workflow

Example modification:
```markdown
BEFORE:
"Claude Code will now..."

AFTER:
"Cline will now..."
```

## Next Steps

1. ✅ Read `CLINE_SETUP.md` for hands-on setup
2. ✅ Choose your first skill (start with `/tdd` or `/grill-with-docs`)
3. ✅ Test in a small project
4. ✅ Customize `CONTEXT.md` for your projects
5. ✅ Build new skills for your specific workflow

---

**Questions?** Check the skill's `SKILL.md` file or the main `README.md` for original documentation and philosophy.

**Found an issue?** Test the skill independently, compare to the original Claude Code behavior, and consider whether it's a Cline difference or a skill bug.
