# Cline Migration Guide

This document outlines how to adapt the `kinwong-ds/skills` repository for use with **Cline** (the VSCode AI coding agent), replacing the current Claude-specific setup.

## Overview

This skills repository was originally built for Claude Code and the `.claude-plugin` system. Cline uses a different configuration and invocation model. This guide covers:

1. **Technical differences** between Claude Code and Cline
2. **Repository structure changes** needed
3. **Configuration migration** requirements
4. **Usage patterns** in Cline
5. **Step-by-step migration checklist**

---

## Part 1: Technical Differences

### Claude Code Setup (Current)

| Aspect | Claude Code |
|--------|-------------|
| **Config location** | `.claude-plugin/plugin.json` |
| **Skill definition** | `SKILL.md` + structured frontmatter (YAML) |
| **Invocation** | `/skill-name` commands in chat |
| **Skill discovery** | Automatic via plugin registry |
| **Context delivery** | Prompt injection + skill content |
| **Session state** | Maintained in Claude Code UI |

### Cline Setup (Target)

| Aspect | Cline |
|--------|-------|
| **Config location** | `.cline/cline.json` OR custom MCP server |
| **Skill definition** | Markdown files + frontmatter (similar) |
| **Invocation** | Tool calls via MCP protocol or direct prompting |
| **Skill discovery** | Manual configuration in settings |
| **Context delivery** | Environment variables + file system access |
| **Session state** | Maintained in Cline UI |

---

## Part 2: Repository Structure Changes

### Current Structure (Claude Code)

```
skills/
├── .claude-plugin/
│   └── plugin.json                  # Defines available skills
├── README.md                        # Main reference
├── skills/
│   ├── engineering/
│   │   ├── diagnose/
│   │   │   └── SKILL.md            # Skill definition
│   │   ├── grill-with-docs/
│   │   │   └── SKILL.md
│   │   └── ...
│   ├── productivity/
│   │   ├── grill-me/
│   │   │   └── SKILL.md
│   │   └── ...
│   ├── misc/
│   │   └── ...
│   ├── in-progress/
│   │   └── ...
│   ├── personal/
│   │   └── ...
│   └── deprecated/
│       └── ...
```

### Proposed Structure (Cline Compatible)

```
skills/
├── .cline/
│   └── cline.json                  # NEW: Cline configuration
├── .claude-plugin/                 # KEEP: For backward compatibility
│   └── plugin.json
├── README.md                       # UPDATED: Add Cline usage section
├── CLINE_MIGRATION.md              # NEW: This file
├── skills/
│   ├── engineering/
│   │   ├── README.md               # Lists skills with links to SKILL.md
│   │   ├── diagnose/
│   │   │   └── SKILL.md            # NO CHANGES to skill content
│   │   └── ...
│   └── ...
├── tools/                          # NEW: Optional custom tools for Cline
│   └── mcp-server.ts              # NEW: Optional MCP server for auto-discovery
```

---

## Part 3: Configuration for Cline

### Option A: Direct File Configuration (Recommended for Start)

Create `.cline/cline.json`:

```json
{
  "version": "1.0",
  "skillsRepository": {
    "name": "kinwong-ds-skills",
    "description": "Engineering and productivity skills",
    "type": "file-based"
  },
  "skills": {
    "engineering": [
      {
        "id": "diagnose",
        "path": "skills/engineering/diagnose/SKILL.md",
        "enabled": true,
        "category": "debugging"
      },
      {
        "id": "grill-with-docs",
        "path": "skills/engineering/grill-with-docs/SKILL.md",
        "enabled": true,
        "category": "planning"
      },
      {
        "id": "triage",
        "path": "skills/engineering/triage/SKILL.md",
        "enabled": true,
        "category": "workflow"
      },
      {
        "id": "improve-codebase-architecture",
        "path": "skills/engineering/improve-codebase-architecture/SKILL.md",
        "enabled": true,
        "category": "architecture"
      },
      {
        "id": "setup-matt-pocock-skills",
        "path": "skills/engineering/setup-matt-pocock-skills/SKILL.md",
        "enabled": true,
        "category": "setup"
      },
      {
        "id": "tdd",
        "path": "skills/engineering/tdd/SKILL.md",
        "enabled": true,
        "category": "testing"
      },
      {
        "id": "to-issues",
        "path": "skills/engineering/to-issues/SKILL.md",
        "enabled": true,
        "category": "planning"
      },
      {
        "id": "to-prd",
        "path": "skills/engineering/to-prd/SKILL.md",
        "enabled": true,
        "category": "planning"
      },
      {
        "id": "zoom-out",
        "path": "skills/engineering/zoom-out/SKILL.md",
        "enabled": true,
        "category": "understanding"
      },
      {
        "id": "prototype",
        "path": "skills/engineering/prototype/SKILL.md",
        "enabled": true,
        "category": "exploration"
      }
    ],
    "productivity": [
      {
        "id": "caveman",
        "path": "skills/productivity/caveman/SKILL.md",
        "enabled": true,
        "category": "communication"
      },
      {
        "id": "grill-me",
        "path": "skills/productivity/grill-me/SKILL.md",
        "enabled": true,
        "category": "planning"
      },
      {
        "id": "handoff",
        "path": "skills/productivity/handoff/SKILL.md",
        "enabled": true,
        "category": "collaboration"
      },
      {
        "id": "teach",
        "path": "skills/productivity/teach/SKILL.md",
        "enabled": true,
        "category": "learning"
      },
      {
        "id": "write-a-skill",
        "path": "skills/productivity/write-a-skill/SKILL.md",
        "enabled": true,
        "category": "meta"
      }
    ],
    "misc": [
      {
        "id": "git-guardrails-claude-code",
        "path": "skills/misc/git-guardrails-claude-code/SKILL.md",
        "enabled": false,
        "category": "safety",
        "notes": "Claude Code specific, may need adaptation"
      },
      {
        "id": "migrate-to-shoehorn",
        "path": "skills/misc/migrate-to-shoehorn/SKILL.md",
        "enabled": true,
        "category": "refactoring"
      },
      {
        "id": "scaffold-exercises",
        "path": "skills/misc/scaffold-exercises/SKILL.md",
        "enabled": true,
        "category": "scaffolding"
      },
      {
        "id": "setup-pre-commit",
        "path": "skills/misc/setup-pre-commit/SKILL.md",
        "enabled": true,
        "category": "setup"
      }
    ]
  },
  "settings": {
    "autoDiscoverSkills": false,
    "loadFromGit": false,
    "cacheTTL": 3600,
    "enableCLICommands": true,
    "allowCustomPrompts": true
  }
}
```

### Option B: MCP Server (Advanced - Auto-Discovery)

For more advanced integration, create a Model Context Protocol (MCP) server that auto-discovers skills:

**File: `tools/mcp-server.ts`**

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import * as fs from "fs";
import * as path from "path";

const server = new Server({
  name: "kinwong-skills-mcp",
  version: "1.0.0",
  capabilities: {
    tools: {}
  }
});

interface Skill {
  id: string;
  name: string;
  description: string;
  path: string;
  category: string;
  enabled: boolean;
}

// Auto-discover skills from filesystem
function discoverSkills(): Skill[] {
  const skills: Skill[] = [];
  const skillsDir = path.join(process.cwd(), "skills");
  const categories = ["engineering", "productivity", "misc"];

  for (const category of categories) {
    const categoryPath = path.join(skillsDir, category);
    
    if (fs.existsSync(categoryPath)) {
      const skillDirs = fs.readdirSync(categoryPath).filter(f => {
        const fullPath = path.join(categoryPath, f);
        return fs.statSync(fullPath).isDirectory();
      });

      for (const skillDir of skillDirs) {
        const skillPath = path.join(categoryPath, skillDir, "SKILL.md");
        
        if (fs.existsSync(skillPath)) {
          const content = fs.readFileSync(skillPath, "utf-8");
          const frontmatter = parseFrontmatter(content);
          
          skills.push({
            id: skillDir,
            name: frontmatter.name || skillDir,
            description: frontmatter.description || "",
            path: skillPath,
            category: category,
            enabled: !["deprecated", "in-progress", "personal"].includes(category)
          });
        }
      }
    }
  }

  return skills;
}

function parseFrontmatter(content: string): Record<string, string> {
  const match = content.match(/^---\n([\s\S]*?)\n---/);
  if (!match) return {};
  
  const fm: Record<string, string> = {};
  const lines = match[1].split("\n");
  
  for (const line of lines) {
    const [key, value] = line.split(":").map(s => s.trim());
    if (key && value) {
      fm[key] = value;
    }
  }
  
  return fm;
}

// Tool: Get skill details
server.setRequestHandler("tools/call", async (request) => {
  const { name, arguments: args } = request.params;
  
  if (name === "get_skill") {
    const skills = discoverSkills();
    const skill = skills.find(s => s.id === args.skill_id);
    
    if (!skill) {
      return { content: [{ type: "text", text: `Skill '${args.skill_id}' not found` }] };
    }
    
    const skillContent = fs.readFileSync(skill.path, "utf-8");
    return {
      content: [{
        type: "text",
        text: `Skill: ${skill.name}\nCategory: ${skill.category}\n\n${skillContent}`
      }]
    };
  }
  
  if (name === "list_skills") {
    const skills = discoverSkills();
    const grouped = skills.reduce((acc, skill) => {
      if (!acc[skill.category]) acc[skill.category] = [];
      acc[skill.category].push({ id: skill.id, name: skill.name, enabled: skill.enabled });
      return acc;
    }, {} as Record<string, typeof skills>);
    
    return {
      content: [{ type: "text", text: JSON.stringify(grouped, null, 2) }]
    };
  }
  
  return { content: [{ type: "text", text: "Unknown tool" }] };
});

// Initialize tools
server.setRequestHandler("tools/list", async () => {
  const skills = discoverSkills();
  
  return {
    tools: [
      {
        name: "get_skill",
        description: "Get the full content and details of a skill",
        inputSchema: {
          type: "object",
          properties: {
            skill_id: {
              type: "string",
              description: `One of: ${skills.map(s => s.id).join(", ")}`
            }
          },
          required: ["skill_id"]
        }
      },
      {
        name: "list_skills",
        description: "List all available skills grouped by category",
        inputSchema: { type: "object", properties: {} }
      }
    ]
  };
});

async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("Kinwong Skills MCP Server running");
}

main().catch(console.error);
```

---

## Part 4: Usage in Cline

### Invoking Skills in Cline

#### Method 1: Direct Reference (Simplest)

```
@kinwong-skills /diagnose

Help me debug this performance issue...
[Cline loads and applies the diagnose skill]
```

#### Method 2: Tool Call (Via MCP)

Cline automatically surfaces tools from the MCP server:

```
[User describes problem]
Cline sees "diagnose" tool available
Cline calls get_skill("diagnose") → loads SKILL.md content
Cline applies skill guidance to reasoning
```

#### Method 3: Environment Variable

Add to Cline environment:

```bash
export KINWONG_SKILLS_PATH="/path/to/skills/repo"
export KINWONG_SKILLS_ENABLED="diagnose,grill-with-docs,tdd,grill-me"
```

Cline checks these and auto-loads relevant skills.

---

## Part 5: Skill Content - No Changes Required

**Good news:** The `SKILL.md` files themselves need **zero changes**. They work as-is:

```yaml
---
name: diagnose
description: Disciplined diagnosis loop for hard bugs...
---

# Diagnose

[Rest of markdown works identically]
```

Cline reads the same frontmatter format and ingests the same markdown guidance.

---

## Part 6: Step-by-Step Migration Checklist

### Phase 1: Preparation (No Public Changes)

- [ ] Clone the repo locally
- [ ] Read this entire migration guide
- [ ] Back up `.claude-plugin/plugin.json`
- [ ] Test current setup in Claude Code to confirm baseline

### Phase 2: Add Cline Configuration

- [ ] Create `.cline/` directory
- [ ] Copy `.cline/cline.json` template (from Part 3, Option A)
- [ ] Test `.cline/cline.json` structure is valid JSON
- [ ] Verify all skill paths exist and point to real `SKILL.md` files

### Phase 3: Add MCP Server (Optional)

- [ ] Create `tools/` directory
- [ ] Add `tools/mcp-server.ts` from Part 3, Option B
- [ ] Create `package.json` in `tools/` for dependencies:
  ```json
  {
    "name": "@kinwong/skills-mcp",
    "version": "1.0.0",
    "main": "mcp-server.ts",
    "type": "module",
    "dependencies": {
      "@modelcontextprotocol/sdk": "^1.0.0"
    }
  }
  ```
- [ ] Test MCP server runs: `npx tsx tools/mcp-server.ts`
- [ ] Verify `list_skills` output is complete

### Phase 4: Update Documentation

- [ ] Add "Cline Setup" section to main `README.md`
- [ ] Create `CLINE_SETUP.md` with quick-start for Cline users
- [ ] Update `.gitignore` to exclude `.cline/private.json` (if added later)
- [ ] Verify all skill links still work in markdown

### Phase 5: Create Cline-Specific Setup Skill

- [ ] Duplicate `skills/engineering/setup-matt-pocock-skills/` → `skills/engineering/setup-cline-skills/`
- [ ] Update the new skill's content to reference `.cline/cline.json` instead of `.claude-plugin/`
- [ ] Include Cline-specific configuration steps (env vars, paths, etc.)

### Phase 6: Testing

- [ ] Install skills repo in a test Cline project
- [ ] Load `.cline/cline.json` settings in Cline
- [ ] Run `/grill-me` skill → verify it works identically
- [ ] Run `/diagnose` skill → verify it works identically
- [ ] Test all 15 active skills (mark any Claude Code-specific ones as disabled)

### Phase 7: Document Deprecations

- [ ] Flag `git-guardrails-claude-code` as "Claude Code only" in `.cline/cline.json` (set `enabled: false`)
- [ ] Add comment: `"notes": "Uses Claude Code hooks - use native Cline settings instead"`
- [ ] Update skill's `SKILL.md` with deprecation notice

### Phase 8: Publish Changes

- [ ] Commit all changes
  ```bash
  git add .cline/ tools/ CLINE_MIGRATION.md CLINE_SETUP.md
  git commit -m "Add Cline support alongside Claude Code setup"
  ```
- [ ] Push to GitHub
- [ ] Update repo description: "Skills for Claude Code and Cline"
- [ ] Create GitHub release noting Cline support

---

## Part 7: Handling Claude Code-Specific Skills

### Skills That Need Adaptation

| Skill | Issue | Solution |
|-------|-------|----------|
| `git-guardrails-claude-code` | Uses Claude Code hooks | Disable in Cline; create Cline-native version or use native git protections |
| `setup-matt-pocock-skills` | Assumes Claude Code plugin system | Create `setup-cline-skills` variant that guides `.cline/cline.json` setup |

### Skills That Work As-Is

All others (14/16) work unchanged:
- ✅ `diagnose` — Pure instruction, no agent-specific hooks
- ✅ `grill-with-docs` — Pure conversation flow
- ✅ `tdd` — Pure instruction
- ✅ `grill-me` — Pure conversation
- ✅ `teach` — Pure instruction
- etc.

---

## Part 8: User Onboarding (README Section)

Add this section to main `README.md`:

### Using with Cline

1. **Install dependencies** (if using MCP):
   ```bash
   cd tools && npm install
   ```

2. **Configure Cline**:
   - Copy `.cline/cline.json` to your Cline settings directory
   - OR set environment variables:
     ```bash
     export KINWONG_SKILLS_PATH="/path/to/skills"
     export KINWONG_SKILLS_ENABLED="diagnose,grill-with-docs,tdd,grill-me"
     ```

3. **Invoke a skill** in Cline:
   ```
   @kinwong-skills /diagnose
   
   I'm seeing a memory leak in the auth module...
   ```

4. **Full setup** (if customizing):
   ```bash
   /setup-cline-skills
   ```

---

## Part 9: Maintaining Dual Support

### Backward Compatibility

Keep `.claude-plugin/plugin.json` unchanged. Users of Claude Code will continue to work.

### Version Strategy

```json
{
  "version": "2.0.0",
  "supportedClients": {
    "claude-code": "1.0.0+",
    "cline": "0.2.0+"
  }
}
```

### Release Notes Template

```markdown
## v2.0.0 - Added Cline Support

### What's New
- Skills now work with both Claude Code and Cline
- New `.cline/cline.json` configuration
- Optional MCP server for auto-discovery

### For Claude Code Users
No changes. Continue using `.claude-plugin` as before.

### For Cline Users
See [CLINE_SETUP.md](./CLINE_SETUP.md) for quick start.

### Breaking Changes
None.
```

---

## Part 10: Future Enhancements

1. **Skills Registry**: Host skills on a registry so Cline can discover `kinwong-ds/skills` automatically
2. **Skill Marketplace**: Create UI for browsing and installing individual skills
3. **Custom Prompts**: Allow skills to define custom system prompts for Cline
4. **Skill Versioning**: Support multiple versions of the same skill
5. **Dependency Management**: Let skills declare dependencies on other skills or tools

---

## FAQ

### Q: Do I have to use the MCP server?

**A:** No. Start with `.cline/cline.json` alone. The MCP server is optional for auto-discovery later.

### Q: Will Claude Code break if I add Cline config?

**A:** No. `.cline/` and `.claude-plugin/` are independent. Both can coexist.

### Q: What if a skill breaks in Cline?

**A:** Check:
1. Skill's `.cline/cline.json` entry is valid JSON
2. `path` points to existing `SKILL.md`
3. SKILL.md frontmatter is valid YAML
4. No Claude Code-specific hooks in the skill content

### Q: Can I test this locally before publishing?

**A:** Yes:
```bash
git clone https://github.com/kinwong-ds/skills
cd skills
# Test in local Cline project
export KINWONG_SKILLS_PATH="$(pwd)"
```

### Q: How do I adapt a skill for Cline?

**A:** Most skills need zero changes. Only adapt if:
- Skill uses Claude Code hooks (e.g., `git-guardrails-claude-code`)
- Skill references `.claude-plugin` system
- Skill requires features not available in Cline

Otherwise, the markdown guidance works identically.

---

## Summary

| Step | Time | Status |
|------|------|--------|
| Create `.cline/cline.json` | 30 min | Ready |
| Add documentation | 1 hr | Ready |
| Create MCP server | 1 hr | Optional |
| Test in Cline | 30 min | Ready |
| Publish | 15 min | Ready |
| **Total** | **~3 hrs** | **Straightforward** |

The good news: **99% of your skills need zero changes.** This is a configuration and discovery problem, not a content problem.

---

## Resources

- [Cline Documentation](https://github.com/cline/cline)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [Original Skills README](./README.md)
- [SKILL.md Format Guide](./skills/engineering/diagnose/SKILL.md)

