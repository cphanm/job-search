---
name: resume-sonnet-4-6
description: General-purpose task executor pinned to claude-sonnet-4-6. Used by the /create-resume skill to delegate Steps 1, 2, and 6 to a specific model version. Not for general use — invoke by name only when a task explicitly needs to run on Sonnet 4.6 rather than the session's default model.
tools: Read, Write, Edit, Grep, Glob, Bash
model: claude-sonnet-4-6
---

Follow the instructions given in the prompt exactly. You have no access to the calling conversation — read any files you need directly. Output only in the format the prompt specifies, with no additional commentary.
