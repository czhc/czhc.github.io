---
title: "Baseline Prompt for Generating Incident Reports"
description: "Using AI to create precise and actionable incident reports for your engineering team"
categories: [sre]
tags: [site-reliability, service-outages, incident-reports, aiops]
last_modified_at: "2025-04-17"
published: true
---

### Your engineering team hates service outages, but they probably hate writing Incident Reports even more. 

An incident might last for minutes, hours, or even days. Writing a meaningful report afterward? That’s an additional tax on the process. But it is a necessary task.


A good incident report should:

- Use precise, succinct language to minimize reading time
- Highlight key components: service-level indicators, failure points, and affected business KPIs
- Be readable by business, operations, and customer success teams to enable clear calls to action. 

### Story Time

A friend and mentor (s/o Wilson) recently shared a useful insight: while AI is powerful, it is not as accessible to ESL (English as a Second Language) users because it sometimes requires the right keywords in English to produce quality results.

I hope this benefits some ESL teams, small teams, or anyone who needs a point of reference.

---
### Baseline Prompt for Generating Incident Reports

```
### INSTRUCTION
You are drafting a technical incident report based on the discussion above. Use only the information provided. Do not make assumptions or invent information. If any detail is missing, ask for clarification before generating the report.

### FORMAT
Follow this structure enclosed in "~~~~~" strictly. Start each item as a Title.

~~~~~
* Title: [YYYYMMDD] [15 word description of the incident cause and impact]
* Summary: A succinct executive summary of what went wrong and whether the issue is recovered. Keep this under 30 words. 
* Impact: Any measurable indicators of the incident including error %, affected data groups and rows, or service-level indicators. Prioritize Service Level Objectives (Uptime, Apdex, Error %) or operational metrics (Mean Time to Resolve, Lead Time to Change, Build Time) before other indicators. Prioritize output metrics without providing an exhaustive list of metrics discussed. Focus only on measurable indicators. 
- Timeline: A bullet list of events, formatted as: [HH:mm]: [@person] performed [@action].
- For cross-day events, use [MM/DD HH:mm]
- Use active voice, e.g., "@alice restarted service X"
- Avoid system descriptions like “System triggered alarm”
* Root cause: Technical explanation of what went wrong.
* Resolution: A technical explanation of what action was taken and how the patch was verified. Use a bullet list of changes. Use one line for each change. Specify: [action taken] [what was recovered] [how the patch was confirmed]. Do not include code snippets here. Link to them in Appendix. 
* Discussion: Specific topics or talking points that include new information, unresolved questions about the root cause, observability, or resolution, etc., that warrant separate or further discussion. Format your points as a numbered list. Use a Q&A format only. Do not print the topic or summary heading before each Q&A.
* Next Steps: A list of specific follow-ups, including patch work or open questions, due within 30 days of the incident.
* Appendix: A bullet list of external references, including third-party source material, documentation, internal and external links, and conversations.
* Drafted by: GPT model and Reviewed By (leave as TBD).
~~~~~

### ADDITIONAL INFORMATION 
Refer to the attached conversations, logs, or files. Extract only relevant information—e.g., names, services, metrics, root causes. Condense verbose threads into key facts.

### TONE
Keep it succinct, technical, and free of filler or emojis. Emphasize jargon or acronyms understood by software engineers, SREs, PMs, and support engineers. Use bold formatting sparingly to highlight only urgent action items or key metrics.

### WARNING
Before drafting, confirm with the user:
1. That all necessary information has been provided.
2. That any missing timestamps, metrics, or root cause details are clarified.
 
Do not generate output until confirmation is received.
```

----

I'll update newer versions in the future here: [[czhc/shared-prompts]](https://github.com/czhc/shared-prompts/blob/b54796e763b03f96ed5802e10930742b44c9c9ec/sre/incident-response.txt)

### Disclaimers

1. Prompting, like coding, is an art. It’s contextual and subjective. This is just a baseline.
2. Prompting should be collaborative. I recommend teams and organizations to maintain their PROMPT file similarly to their README files. **Collaborate, test, version-control, and fork as needed.**
3. Test, test, test your prompts against different models, although summarizers generally work well with generalized LLMs.
4. There are no silver bullets in integrating AI. Review your output. Be patient and tune your prompts so that the long-term benefit is exponential.


Let me know what you think! I use AI for a lot of things _(except vibe-coding, ironically)_ and I'd love to hear how you’d tune this prompt or prompt differently.
