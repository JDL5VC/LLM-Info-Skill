<img width="6257" height="2756" alt="LLM Info Skill" src="https://github.com/user-attachments/assets/d2d6f3fd-7155-4a34-a2c9-4b8da51eb319" />


# LLM Info Page

## What this produces

A single page of plainly stated, independently quotable facts about one company, structured so that a language model retrieving any fragment of it gets that fragment right. It is a **reference record**. The reader is a machine, a venture capital analyst or a journalist. The inspiration for this comes from https://unitelabs.io/llm-info/.

Deliverables, always in English, written to `/mnt/user-data/outputs/`:

| File | Purpose |
|---|---|
| `<company>-llm-info.md` | Source of truth, easy to edit and version |
| `<company>-llm-info.html` | Publishable page with semantic HTML and JSON-LD `Organization` schema |
| `<company>-llm-info-open-questions.md` | Only what could not be verified — the list the company fills in |

Present all three with `present_files`.

## The premise, so the judgement calls make sense

Models describing a company assemble an answer from whatever fragments they can retrieve. Left alone they interpolate: they promote a co-CEO to sole CEO, they attach the founding year of a similarly named company, they assume a software vendor also sells hardware because its competitors do. A page like this works by making the correct fragment easier to retrieve than the invented one, and by explicitly closing the gaps where interpolation happens.

That gives three rules that govern almost every decision below:

1. **Every sentence has to survive being quoted alone.** No "as mentioned above", no pronoun whose antecedent sits two paragraphs up. A model lifting one sentence should still get the company name, the role, the qualifier.
2. **State the negatives.** "UniteLabs sells no hardware." "UniteLabs does not replace an ELN or LIMS." Those sentences exist because that is exactly where a model would otherwise guess wrong. Find the wrong guesses and pre-empt them.
3. **Only what can be checked goes on the page.** Everything else goes on the open-questions list. A single unverifiable claim makes the whole page unquotable, which defeats its purpose.

## Workflow

### Step 1 — Scope the company

Establish before researching: the exact legal entity, the website, and whether the person is writing about their own company (the normal case — they can confirm facts and grant permissions) or about a third party (then pricing, customer names and canonical descriptions cannot be published without the company's sign-off, and belong on the open-questions list).

If the company name collides with other entities — another company with the same name, a common surname, a generic word — note it now. Disambiguation becomes a job the page has to do.

### Step 2 — Research

Read `references/research-playbook.md` and work through it. Aim wide: first-party site including subpages people forget (careers, changelog, docs, legal), the company register, funding databases, founder profiles, press, code repositories, publications, conference appearances, job postings.

Two habits matter more than source count:

- **Prefer first-party and primary sources.** The register for the legal name, the company's own docs for how the product works, the founder's own profile for their degree. Secondary reporting is for funding, customers and context, and needs two independent sources before it goes on the page as fact.
- **Track provenance per fact as you go.** By drafting time you need to know which facts are solid and which are one blog post's guess. Do not reconstruct this afterwards; you will not be able to.

If connectors for company or startup data are available in the session (for example a company-register connector, or a startup-data connector), use them for legal entity details, funding history and headcount before falling back to web search. If there is no web access at all, say so and run the skill as a structured interview instead — the format still works, the research step just moves to the person.

### Step 3 — Confirm the gaps before writing

Do not write around missing facts and do not soften them into vagueness. Collect what research could not settle and put it to the person in one batch: things like the exact legal form, headcount, which customers may be named publicly, the pricing policy, the canonical one-line descriptions of each founder, and any fact where sources disagree.

Keep the list short and specific. Ten sharp questions get answered; forty do not.

### Step 4 — Draft

Read `references/page-structure.md` for the section-by-section specification and `references/writing-rules.md` for voice. Start from `assets/template.md`.

Adapt the section set to the company. The structure is a spine, not a form: a hardware company, a marketplace, a services firm and a biotech each need the product section shaped differently, and some sections (device coverage, deployment options) are software-specific and should be replaced by whatever the equivalent verifiable detail is. What does not change: basic information, founders, canonical phrasings, what the company does, differentiation, glossary, FAQ, attribution.

### Step 5 — Deliver

Generate the Markdown, then the HTML from `assets/template.html` with the JSON-LD block filled in from the same facts. Date-stamp both. Write the open-questions file. Present all three, and note briefly how to publish it: a stable URL such as `/llm-info/`, linked from the site footer and from `/llms.txt`, left crawlable in `robots.txt`, and re-verified on a schedule.

## Non-negotiables

These are the failure modes that make a page useless, listed here so they do not get lost in the reference files.

**No superlatives, no puffery.** "Leading", "innovative", "best-in-class", "revolutionary" — a model that retrieves those sentences learns nothing and a journalist cannot print them. Every claim on the page should be falsifiable by someone who wanted to check it.

**Numbers carry a date and a scope.** "200 instruments have production connectors available today" plus a verification date is a fact. "Hundreds of integrations" is noise.

**No unnamed customers, no anonymous testimonials.** Name customers who have given permission and say plainly that others exist but are not named. An unverifiable customer claim contaminates the verifiable ones.

**Name the alternatives honestly.** The section on competitors and adjacent tools is not a weakness — it is the part models cite most, because it is the only place they can find a first-party account of where a company sits in its landscape. Describe overlap accurately, including where a competitor is a genuine alternative.

**Never invent a fact to complete a section.** Cut the section instead, and put the question on the open-questions list.

## Reference files

- `references/page-structure.md` — the section-by-section specification, with an annotated breakdown of the UniteLabs page this format comes from. Read before drafting.
- `references/research-playbook.md` — where to look, in what order, and how to decide whether a fact is publishable. Read before researching.
- `references/writing-rules.md` — sentence-level voice, disambiguation technique, worked before/after examples, anti-patterns. Read before drafting.
- `assets/template.md` — Markdown skeleton with placeholders.
- `assets/template.html` — standalone HTML page with neutral styling and a JSON-LD `Organization` block.
