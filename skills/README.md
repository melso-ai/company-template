# Company-wide skills

Skills in this folder are available to Tasks in every Area. Skills in `areas/company/skills/` apply only to the Company Area. Put a reusable company-wide procedure here when that is the intended scope.

Create a readable skill folder, such as `summarize-document/`, with a `SKILL.md` file. The file needs YAML frontmatter containing a name and description, followed by the procedure. This complete example is documentation only:

```markdown
---
name: Summarize a document
description: Summarize a company document with its key facts, open questions, and source references.
---

# Summarize a document

Read the document requested by the Task and identify its source and revision.
Summarize its key facts, decisions, and open questions. Keep unsupported
assumptions separate and link to the original document. Save a summary only
when requested by the Task.
```

Keep supporting text under `references/` and optional scripts under `scripts/` inside the skill directory. Reference those files from `SKILL.md`. A script is not executed by company validation and does not gain account access by being present.

Skill identity includes its scope. Use the qualified skill identity when similarly named global, Area, built-in, or plugin skills exist. All authored skill content for a run comes from its pinned company revision.
