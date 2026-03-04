# Skill: meeting-processor

**Process meeting transcripts into structured organizational records**

## Purpose

Transforms raw meeting transcripts or recordings into structured meeting notes following Organizational OS conventions, extracts action items, updates project pages, and writes entries to organizational memory.

## Skill File Location

`skills/meeting-processor/SKILL.md` in any organizational workspace.

## When to Use

- After receiving a meeting transcript (Granola, Otter.ai, Google Meet auto-transcript)
- When a team member pastes or sends meeting notes to be formatted
- After receiving an audio file of a meeting (with transcription skill available)
- On a schedule to process pending transcripts from a shared inbox

## Inputs

| Input | Format | Required |
|-------|--------|----------|
| Raw transcript | Text | Yes (or audio) |
| Meeting title | String | No (inferred from transcript) |
| Date | YYYY-MM-DD | No (inferred) |
| Attendees | List | No (inferred) |
| Related projects | List | No (inferred) |

## Outputs

| Output | Location | Format |
|--------|----------|--------|
| Meeting note | `packages/operations/meetings/YYMMDD [Title].md` | Markdown |
| Memory entry | `memory/YYYY-MM-DD.md` | Markdown |
| Action items | Project pages in `packages/operations/projects/` | Checkboxes |
| Heartbeat update | `HEARTBEAT.md` | Checkboxes |

## Meeting Note Structure

```markdown
---
categories: [Meetings]
projects:
  - "[[YYMMDD Project Name]]"
date: YYYY-MM-DD
attendees: [Name1, Name2, Name3]
type: sync | council | standup | workshop | retrospective
---
# Meeting Title

## Key Decisions
- Decision 1 (context if needed)

## Action Items
- [ ] Task description (Owner — due: YYYY-MM-DD)

## Discussion Summary
Brief narrative of main topics. Not a transcript — synthesized insights.

## Next Steps / Follow-ups
- Next meeting date (if set)
- Specific follow-ups not yet assigned
```

## Extraction Guidelines

### Decisions
Look for language like:
- "we decided", "we agreed", "confirmed", "going with", "approved"
- "it was decided", "consensus on", "final call"

### Action Items
Look for:
- "will [do X]", "[Name] to [do X]", "by [date]", "before [event]"
- Existing `- [ ]` markers
- "next step", "follow up on", "make sure to"

### Participants
Look for names in speaker labels, greetings, or @mentions.

## Terminology Standards

Apply org-specific terminology from `SOUL.md`. Common standards:
- Preserve proper names exactly as they appear
- Use organization name as in `IDENTITY.md`
- Apply language conventions from `SOUL.md` voice section

## Error Handling

- If transcript is unclear or truncated: note gaps explicitly, do not infer
- If attendees can't be determined: list as "Partial — see transcript"
- If projects can't be determined: leave `projects:` empty, add note

## Integration

This skill works with:
- **schema-generator** — trigger schema regen after adding meeting notes
- **knowledge-curator** — meeting summaries feed knowledge commons
- **heartbeat-monitor** — new action items go to HEARTBEAT.md

## References

See also:
- `packages/operations/meetings/templates/` — meeting templates
- `docs/02-standards/workspace-file-system.md` — file conventions
- Zettelkasten `AGENTS.md` — meeting notes terminology rules
