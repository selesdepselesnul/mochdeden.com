---
name: resume-update
description: Update wording in the resume (index.html) for Summary, Professional Value, job descriptions, or job responsibilities. Focuses on text only, not style or structure.
argument-hint: "target: [summary|professional-value|job-description|job-responsibilities] company: [name] instruction: [what to change]"
---

# Resume Wording Update

Update the wording of a specific section in `/home/morrisseymarr/Playground/mochdeden.com/index.html`.

## What You Can Edit

| Target | Section in index.html |
|---|---|
| `summary` | The `<p>` inside the Summary section |
| `professional-value` | The `<p>` tags inside the Professional Value section |
| `job-description` | The company description `<p>` for a given company |
| `job-responsibilities` | The `<ul>` bullet points for a given company |

## Writing Rules

Apply these rules to all output text:

- No em dashes or double dashes. Use a comma, period, or rewrite the sentence.
- Natural, human tone. Not stiff, not AI-sounding.
- Concise. Fewer words over more.
- No filler phrases like "certainly", "of course", "I'd be happy to".
- Preserve all HTML tags and structure. Only change the visible text content.

## Input Format

Structured:
```
target: [summary | professional-value | job-description | job-responsibilities]
company: [Company name — required for job-description and job-responsibilities]
instruction: [new content or description of what to change]
```

Plain text is also accepted. Infer the target from context.

## Behavior

1. Read the current `index.html`
2. Find the section matching the target (and company, if provided)
3. Show the current wording
4. Write the updated wording following the writing rules
5. Apply the edit using the Edit tool, touching only the text inside the HTML tags

## Examples

**Update Summary:**
```
target: summary
instruction: Software engineer since 2015. Led teams since 2021, focused on shipping reliable products.
```

**Update Professional Value:**
```
target: professional-value
instruction: Emphasize that I care about team growth, not 10x engineers. AI helps us reach 100x as a team.
```

**Update a company description:**
```
target: job-description
company: Linguise
instruction: AI powered translation SaaS
```

**Add a responsibility:**
```
target: job-responsibilities
company: Linguise
instruction: add: Help the team adopt AI tooling through agentic engineering practices.
```

**Replace a responsibility:**
```
target: job-responsibilities
company: Ravenstack
instruction: replace "Supported and mentored junior engineers on technical challenges." with something more natural
```

## Arguments

$ARGUMENTS
