# Case Study: Turning a 30-Year Family Business From Founder-Operated to System-Operated

**Company:** Walker Diseño y Decoración Ltda. — Santiago, Chile
**My role:** Product/systems lead (unpaid, family engagement)
**Status:** In progress — diagnosis and roadmap complete, first two phases underway

A note on the numbers below: this is a real, ongoing engagement with my family's
business, and several figures (revenue, exact conversion rates, cost structure) are
commercially sensitive. Where I've used numbers, they're rounded or expressed as
ranges/relative changes rather than exact figures. The diagnosis, the reasoning, and
the roadmap are real.

---

## The business

Walker Diseño y Decoración Ltda. installs safety netting for balconies and windows
(a common, often legally required safeguard in Santiago apartments with young
children or pets), along with adjacent services — mosquito screens, wood shutters,
flooring, perimeter fencing. My mother, Vanessa Walker, founded it in 1995. My
father, Carlos, coordinates operations. Several thousand installations are on record
across three decades, likely an undercount for reasons that turn out to be central
to the diagnosis.

I stepped in because the business is a real, working case: real customers, real
revenue, real operational failure modes — the same shape of problem I've solved in
product roles, just with a WhatsApp thread and a legacy Visual Basic database
instead of a modern stack.

## The problem, stated simply

**Close rate on inbound leads fell from roughly 60–70% in the mid-2010s to roughly
10–15% in 2023–2025 — while lead volume roughly tripled.**

That combination is the whole diagnosis in one sentence: this was never a demand
problem. More people wanted the service than ever. The business was simply
converting a much smaller share of them, and doing so slower than before. Something
structural had broken between "someone wants a quote" and "someone pays for the
job."

## Digging in: two businesses wearing one brand

The first surprising finding was that "the business" wasn't one operation — it was
two, run independently by my two parents, sharing a brand, a price list, and a bank
account, but nothing else:

- **My father's channel** runs on email, a website quote form, and in-person visits,
  logged into a legacy database that's treated as the single source of truth.
  Response is measured in hours; nearly all of this channel's leads make it into the
  system.
- **My mother's channel** runs entirely on WhatsApp, from her phone, with no formal
  system at all — pricing, scheduling, and follow-up all live in her head and her
  chat history. She closes deals in minutes, not hours. But an estimated **19 out of
  20 of her leads and the revenue behind them never reach the shared database at
  all** — meaning any report built from "the data" was, until this project,
  systematically blind to a large and disproportionately high-converting share of
  the actual business.

Once I broke conversion out by channel instead of by the flawed aggregate, a second
counterintuitive fact emerged: **WhatsApp — the channel with zero formal tooling —
converts dramatically better than the polished web quoting tool** (roughly
75–80% vs. under 10%). The website was functioning as a showcase, not a closing
mechanism; the actual sales motion was happening in a channel the business had
never invested in and couldn't see.

That reframes the whole project. The instinct when conversion drops is usually "fix
the website" or "spend more on ads." The data said: the highest-leverage channel is
already working — it's just invisible, unsupported, and about to hit a scaling
wall because it depends entirely on one person's attention.

## Root-causing the WhatsApp channel specifically

Reading a sample of real conversation threads (not shared here — customer
conversations aren't mine to publish) surfaced five recurring failure patterns, each
independently fixable:

1. **Ambiguous pricing** — customers often couldn't tell what they were being
   quoted for.
2. **Fragmented information** — quotes arrived split across several separate
   messages, breaking the conversational momentum that makes WhatsApp convert so
   well in the first place.
3. **No active close** — the channel would deliver information and wait, rather
   than asking a direct question that moves the conversation to a decision.
4. **No differentiation story** — nothing addressed why a customer should pay a
   premium (the business runs roughly 20–25% above informal, unlicensed
   competitors) rather than choosing the cheaper option.
5. **No follow-up on silence** — a lead that went quiet for about 48 hours was
   treated as dead, with no re-engagement attempt, in a channel that otherwise
   closes in minutes.

None of these require new headcount or new demand. They're a systems and
scripting problem — which is what made this tractable as a phased build rather
than a "hire more people" ask a family business genuinely couldn't afford.

## The roadmap: founder-operated → system-operated

The North Star I proposed, and that both my parents signed off on, was blunt:
**move from a business that only runs because two specific people are personally
handling every conversation, to one where the founders supervise quality and
relationships while a system handles the repeatable parts.**

I structured this as six phases, each scoped to be affordable for a business this
size (most add no new tooling cost at all; the later phases run in the tens of
dollars per month):

| Phase | Focus | Adds |
|---|---|---|
| **M0 — Diagnosis** | Understand the real business before changing anything | The analysis above: true revenue picture, channel conversion, root causes |
| **M1 — Capture the invisible channel** | Get my mother's operation into a shared system without disrupting how she actually works | Lightweight capture, retention outreach to lapsed long-term customers |
| **M2 — Test infrastructure** | A real website and a real leads database, deliberately unpolished | Structured lead storage, still human-run |
| **M3 — Automation appears** | Remove manual toil from the parts that don't need a human | Automated payment reconciliation, real-time technician scheduling, WhatsApp Business API |
| **M4 — Scale my mother's voice** | Let the WhatsApp channel handle volume neither parent can personally sustain | An AI quoting agent, designed to sound like her, not like a bot |
| **M5 — Full visibility** | Founders supervise a running system rather than operating it | Live dashboard, automated renewal outreach |

I'm currently past the diagnosis phase and into early execution on capturing the
second channel and standing up the test infrastructure.

## Designing the automation layer as a product problem, not a chatbot

The part of this I'd most want a product/AI-systems interviewer to look at is how
I scoped the eventual automation (M3–M4), because I treated it explicitly as a
state machine with accountable handoffs, not a single do-everything chatbot:

```
LEAD_RECEIVED → QUOTE_DELIVERED → SCHEDULED → COMPLETED → PAID
```

Each transition is owned by one narrowly-scoped agent with a defined input,
output, and — critically — a defined failure mode that escalates to a human rather
than guessing:

| Agent | Owns the transition | Escalates to a human when |
|---|---|---|
| Quoting | Lead received → quote delivered | It can't confidently price the job |
| Scheduling | Quote accepted → job scheduled | No technician is available in the requested window |
| Customer success | Job scheduled → job completed | The customer doesn't confirm satisfaction |
| Finance monitor | Job completed → payment reconciled | A payment doesn't match the database record |

Two design decisions I'd call out specifically:

- **Every conversation gets a single ID that survives every handoff**, logged
  against every agent action (agent name, tool called, latency, whether it
  escalated). Without this, "the AI agent" becomes a black box the moment
  something goes wrong — which is exactly the failure mode a family business
  running on trust can't tolerate.
- **The social-media assistant is scoped deliberately small on purpose.** It
  drafts a content calendar and ready-to-post copy from real finished jobs and
  recent reviews — it does not auto-post, schedule, or generate images. That's a
  conscious choice to keep a human in the loop on anything public-facing, rather
  than defaulting to "more automation is better."

Before any agent goes into production, I set a non-negotiable bar: a labeled test
set of real conversations, explicit accuracy targets (e.g., quotes within roughly
5% of the correct price), and a defined human review cadence — my mother reviewing
voice and tone, me reviewing logic and escalation behavior.

## Where this stands

The diagnosis and roadmap are done and agreed with both founders. Execution is
underway on getting my mother's channel into a shared system and standing up the
first version of the website and lead database, without yet touching the
automation layer — deliberately, since the earlier phases need to be solid before
anything is allowed to run unattended.

## Why I'm including this

Most of my product experience is enterprise SaaS. This project is the opposite
end of the spectrum — a pre-digital, two-person, word-of-mouth operation — and I
think it's a better test of product instinct than a polished B2B example would be,
for one reason: there was no existing data team, no existing systems, and no
existing agreement on what the actual business even was. The first and hardest
deliverable wasn't a roadmap — it was convincing two people who've run this
business successfully for thirty years that the invisible half of their revenue
was real, using their own numbers.
