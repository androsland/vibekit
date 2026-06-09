# Agent SOP: [Agent / Automation Name]

> One page. If it doesn't fit, the agent is too complex to ship as-is — narrow its scope.

**Status:** [prototype / staged rollout / production]
**Last updated:** [date]

---

**Purpose**
[1–2 sentences: what it does and why it exists.]

**Allowed tools / systems**
- [System or API it may call]
- [System or API it may call]

**Data classes touched**
- Reads: [public / internal / confidential / restricted-PII — be specific]
- Writes: [...]

**Read vs. write permissions** (per system)

| System | May read | May write | Must NEVER write |
|---|---|---|---|
| [system] | [what] | [what] | [what] |

**Human approval gates**
- [Action that stops for a human] → approved by: [named role/person]
- [Irreversible action: money / contract / external promise / personnel data] → approved by: [...]

**Logging expectations**
- What is recorded: [inputs / outputs / decisions / errors / triggers]
- Where: [destination]

**Failure & rollback**
- On failure: [what happens — fallback, alert, who's notified]
- To turn off / roll back: [exact steps]
- What breaks when turned off: [...]

**Named owner**
[A specific human. Not a team.]

**Review cadence**
- Routine: [e.g. monthly for first quarter, then quarterly]
- Off-cadence trigger: [e.g. any customer-facing incident → immediate review]

---

*Tape this next to wherever the agent's dashboard lives. It's the first thing the
on-call person reads when something goes wrong.*
