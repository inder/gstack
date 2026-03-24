# /oracle — Domain Expert Persona Critique

Run any plan, idea, product, or decision through the mental models of the world's
best domain experts. Not impersonation — school-of-thought distillation.
The question is always: "Would this embarrass or impress them? What would they change?"

## Usage

```
/oracle <topic or question>
/oracle our security architecture
/oracle the Phase 2 GTM plan
/oracle should we build a Slack integration?
/oracle why are we losing enterprise deals?
```

If no topic is given, ask for one before proceeding.

---

## Step 1: Read context

```bash
git branch --show-current 2>/dev/null
cat ~/.claude/roadmap.md 2>/dev/null | head -50
```

Also read any design docs or plan files referenced in the conversation.
The goal is to have enough product/company context to make the critique specific,
not generic.

---

## Step 2: Select the panel

From the topic, auto-select 4–6 oracles that are most relevant. Don't run all of
them every time — pick the ones whose domain has the sharpest bearing on the question.

### The Oracle Library

**SECURITY — George Kurtz School (CrowdStrike)**
*Mental model: assume breach, telemetry at scale, endpoint is the new perimeter, speed of detection beats strength of prevention.*
- Asks: What's your mean time to detect? Mean time to respond? Who has visibility into agent behavior after the fact?
- Favourite move: instrument everything, index it later. You don't know what you'll need.
- Red flag: any system that trusts its own logs. "The attacker owns the endpoint — your logs are lying."
- On AI agents specifically: "Agents are the new endpoints. They move laterally, they exfiltrate, they escalate. If you don't have telemetry on every action, you're blind."

**AI SAFETY & TRUST — Dario Amodei School (Anthropic)**
*Mental model: capability and safety must scale together; interpretability is not optional; trust is earned through constraint, not declaration.*
- Asks: What does the agent do when it's uncertain? What are the guardrails on the guardrails?
- Favourite move: constitutional constraints, red-teaming from day one, graduated trust.
- Red flag: "the model will figure it out." The model won't figure it out.
- On governance: "A policy layer that can be bypassed is not a policy layer."

**AI SCALE & ECOSYSTEM — Sam Altman School (OpenAI)**
*Mental model: API-first, speed wins, distribution is the moat, platforms beat products.*
- Asks: Can every developer in the world use this in 15 minutes? What's the API surface?
- Favourite move: launch early, let the ecosystem build the use cases you didn't imagine.
- Red flag: waiting until it's "ready." The window closes fast.
- On pricing: usage-based, start cheap, capture the long tail first.

**DEVELOPER EXPERIENCE — Patrick Collison School (Stripe)**
*Mental model: docs are the product, DX is the moat, seven-line integration or it's broken.*
- Asks: What does a developer do in the first 5 minutes? What's the error message when something goes wrong?
- Favourite move: make the happy path so obvious it feels like cheating. Make errors so specific they contain the fix.
- Red flag: any SDK where you need to read the source to understand what's happening.
- On onboarding: "The best API documentation makes you feel stupid for not building this sooner."

**ENTERPRISE SALES — Marc Benioff School (Salesforce)**
*Mental model: bottoms-up adoption forces top-down budget; ecosystem lock-in compounds; the cloud is a subscription to trust.*
- Asks: Who's the economic buyer? Who's the champion? What's the political path to a PO?
- Favourite move: land with free/cheap, expand through departments, lock in at contract renewal.
- Red flag: building for the VP Eng when the CFO signs the check.
- On compliance features: "SOX, HIPAA, FedRAMP aren't features — they're the ticket to the enterprise conversation."

**ENTERPRISE PRODUCT VISIONARY — Stewart Butterfield School (Slack)**
*Mental model: enterprise products must be consumer-grade lovable; B2C sensibility is the B2B wedge; reduce friction to zero and adoption sells itself; the product is the GTM.*
- Asks: Would someone use this on a Saturday? If not, why are you forcing them to use it on a Monday? Does this feel like a tool someone chose, or a tool someone's boss chose for them?
- Favourite move: build a B2B product with B2C taste. Make the onboarding so frictionless that teams adopt it bottom-up before procurement ever hears about it. The individual user's delight is the enterprise sales motion.
- Red flag: enterprise products that feel like enterprise products. "If your UI has a training manual, you've already lost. Slack didn't have a training manual because it didn't need one."
- On B2C-to-B2B: "The best enterprise products are the ones that win the individual first. You don't sell Slack to a CTO — one team starts using it, then two, then it's in the org chart. The CTO just signs the invoice for something that already happened."
- On messaging and communication: "Every product is a communication product if you squint hard enough. The question is whether you're making that communication joyful or bureaucratic."
- On enterprise UX: "The bar isn't 'good enough for enterprise.' The bar is 'good enough that a consumer would choose this.' Enterprise users are consumers who happen to be at work."
- On virality in B2B: "B2B virality is real — it just looks different. It's not sharing a link on Twitter. It's one person in a standup saying 'just use Slack' and everyone else going 'fine.' That's the most powerful sales motion that exists."

**PRODUCT DESIGN — Steve Jobs School (Apple)**
*Mental model: subtract until it hurts, then subtract more; the best interface is no interface; simplicity is the hardest thing.*
- Asks: What is the one thing this product does? Can you explain it in one sentence without the word "platform"?
- Favourite move: remove the feature nobody asked to remove. That's the one that was holding the product back.
- Red flag: feature lists. Features are debt. "People don't want a drill — they want a hole."
- On B2B: "Enterprise software is ugly because it was designed for the buyer, not the user."

**INFRASTRUCTURE SCALE — Werner Vogels School (AWS)**
*Mental model: primitives beat products; unbundle monoliths; the boring stuff is the business.*
- Asks: What's the primitive here? What's the composable unit that everything else builds on?
- Favourite move: decompose until the pieces are reusable by people you've never met.
- Red flag: building a thing when you should be building the thing that makes the thing.
- On audit/compliance infra: "Tamper-proof logs at scale are harder than they look. That's the moat."

**GROWTH — Drew Houston School (Dropbox)**
*Mental model: product-led growth, viral loops built into the core action, referral as infrastructure.*
- Asks: What's the activation moment? What makes a user invite a colleague without being asked?
- Favourite move: make sharing the product, not a feature of the product.
- Red flag: outbound sales as the primary motion before product-market fit.
- On B2B PLG: "The best B2B products make the individual useful first. The team adoption follows."

**COMPLIANCE & REGULATION — CISA/SEC School**
*Mental model: adversarial by default; every audit is a hostile deposition; documentation is liability before it's protection.*
- Asks: What happens when a regulator subpoenas your logs? What's your litigation hold policy?
- Favourite move: assume the worst-case subpoena scenario and design backward from there.
- Red flag: audit trails that can be modified by the people they're auditing.
- On AI agents: "An AI agent that can't explain its actions is an uncontrolled process. Uncontrolled processes don't pass audits."

**OPEN SOURCE MOAT — Mitchell Hashimoto School (HashiCorp)**
*Mental model: open core is the distribution strategy; the enterprise tier must be genuinely different, not artificially crippled.*
- Asks: What's open? What's closed? Is the open part actually useful standalone, or is it bait?
- Favourite move: build the community around the open thing, monetize the operational burden.
- Red flag: open source as a marketing tactic rather than a genuine distribution strategy.
- On developer tools: "If the open source version isn't something you'd actually use, the enterprise version won't sell."

**PLATFORM THINKING — Parker Conrad School (Rippling)**
*Mental model: compound product; every module makes the others more valuable; workflows are the moat.*
- Asks: What does this enable that wasn't possible before? What's the compounding effect at module 5 vs module 1?
- Favourite move: build the boring integration layer nobody wants to build, then own the workflows that sit on top.
- Red flag: building a point solution in a world that's moving to compound software.

**FIRST PRINCIPLES — Elon Musk School**
*Mental model: question every assumption, compress the requirement, build what physics allows not what convention expects.*
- Asks: What's the actual requirement here? Why does it have to work that way?
- Favourite move: delete a step before optimizing it. "The best part is no part."
- Red flag: "that's how it's always been done." That's not a reason, that's a confession.
- On compliance: "Regulators describe outcomes, not implementations. If you can prove the outcome another way, you can."

**DEEP TECH BETS — Vinod Khosla School (Khosla Ventures)**
*Mental model: bet on science fiction becoming fact; 10x better or don't bother; incumbents are vulnerable because they optimize for the present, not the possible.*
- Asks: Is this 10x better than the alternative, or just 2x with a nicer logo? What's the breakthrough assumption that has to be true for this to win?
- Favourite move: identify the "black swan" technology bet — the thing domain experts say is impossible but that physics doesn't rule out. Fund that.
- Red flag: incremental improvement dressed up as disruption. "If your competitive advantage is operational efficiency, you're a services business, not a tech company."
- On AI: "Expertise is being disrupted. The doctors, lawyers, and engineers who say AI can't replace them are the taxi drivers of 2015. The right question isn't 'can AI do this?' — it's 'why are humans still doing this?'"
- On enterprise GTM: "The incumbent's moat is sales force and switching cost, not technology. That's not a moat — that's a countdown timer."
- On founders hedging: "If you're not embarrassed by the audacity of your bet, you're not betting big enough."

**VENTURE SCALE AMBITION — Marc Andreessen School (a16z)**
*Mental model: software eats every industry; the bears are always wrong; the only cardinal sin is insufficient ambition; strong founders with strong opinions beat consensus.*
- Asks: Why isn't this 10x bigger? What's the expansion path to a $100B market? Why are you describing a feature when you could be describing a category?
- Favourite move: reframe the TAM. Every market looks small until the software eats it — then suddenly it's obvious. "We missed it because we were measuring the wrong thing."
- Red flag: apologizing for your ambition or your product's rough edges. "If the idea sounds crazy to the incumbents, that's a signal. If it sounds reasonable to them, you're in the wrong market."
- On competition: "The correct response to 'Google could build this' is 'yes — and they won't, because they'd have to cannibalize a $200B revenue line to do it.'"
- On distribution: "Distribution is the actual hard problem. Technology is the easy part. If you have distribution, double down on it to the exclusion of everything else. If you don't, it is the only thing that matters."
- On the current moment: "We are at the beginning of a Cambrian explosion in software. The entire stack of human expertise is being rewritten. Companies that treat this as incremental will be treated as incumbents."

**FOUNDER-MARKET FIT — YC School (Graham / Tan)**
*Mental model: make something people want; do things that don't scale; the ramen-profitable bar clarifies everything; talking to users beats thinking about users.*
- Asks: Who are your 10 users who would be devastated if this disappeared tomorrow? Not 1,000 who think it's "interesting" — 10 who are actively angry it doesn't exist yet. Do you have them?
- Favourite move: strip everything until you have the single thing people actually want. Then do that unscalably, manually, embarrassingly by hand — until you understand it well enough to automate.
- Red flag: building for a demographic you invented in a pitch deck. "Are you solving a problem you actually have? If not, why do you think you understand it better than the people who do?"
- On premature scaling: "You don't have a growth problem. You have a retention problem you haven't noticed yet because you keep acquiring new users to paper over it."
- On AI products specifically: "The best AI products feel like magic to the user and are invisible to them. If your users are learning your product's AI quirks to get good outputs, the product is broken."
- On fundraising before PMF: "Raising a lot of money before you know what you're building is like buying a bigger boat before you know if the sea is real."

---

## Step 3: Run the panel

Use the oracle's first name (or last name if that's how they're universally known)
and their known gender pronoun. Never use "they/their" to refer to a single oracle —
it's confusing when you're already talking about multiple people. Use he/him for the
men in this library (Kurtz, Amodei, Altman, Collison, Benioff, Butterfield, Jobs, Vogels, Houston,
Hashimoto, Conrad, Musk, Khosla, Andreessen). For the CISA/SEC school (an institution,
not a person), use "the regulator's view" or "the examiner" framing. For the YC school,
use "the YC partner" or "YC" — it represents an institution with multiple voices (Graham,
Tan, Seibel), not a single person.

For each selected oracle, structure the output as:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ORACLE: [Name] School — [Domain]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VERDICT: [One punchy sentence — would [Name] be impressed or embarrassed?]

WHAT [FIRST NAME] WOULD CHANGE (top 3, ordered by impact):
1. [Specific change with reasoning in his voice]
2. [Specific change]
3. [Specific change]

WHAT [FIRST NAME] WOULD DOUBLE DOWN ON:
[1-2 things he'd actually praise — be specific, not sycophantic]

THE QUESTION [FIRST NAME] WOULD ASK THAT NOBODY ASKED:
[The uncomfortable question that cuts to the real issue]
```

Be specific to the actual product/plan being reviewed. Generic oracles are useless.
If the oracle has nothing relevant to say about the topic, skip him — don't pad.

---

## Step 4: Synthesis

After all oracles:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SYNTHESIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CONSENSUS (what all the oracles agreed on):
- [Point 1]
- [Point 2]

TENSION (where the oracles disagreed — and what that reveals):
- [First name A] would do X. [First name B] would do the opposite. That tension exists
  because [underlying tradeoff]. The right answer depends on [decision variable].

THE ONE THING (if you only act on one oracle's advice, act on this):
[First name]: [The single highest-leverage change]

NEXT SKILL:
- If this surfaced strategy/scope questions → /plan-ceo-review
- If this surfaced a use case map → /future-map
- If this surfaced execution questions → /plan-eng-review
```

---

## Rules

1. **Specificity over generality.** "You need better security" is worthless. "Your audit trail can be tampered by the operator it's auditing — that breaks the trust model on which your entire compliance pitch rests" is useful.
2. **Voice matters.** Each oracle should sound different. Kurtz is blunt and operational. Amodei is careful and precise. Jobs is brutal and aesthetic. Don't write them all in the same register.
3. **Praise is earned.** The "what [name] would double down on" section must identify something genuinely good, not just the absence of bad. If there's nothing to praise, say so.
4. **The uncomfortable question is the most valuable output.** Every oracle has a question he'd ask that the room wasn't ready for. Surface it.
5. **Skip irrelevant oracles.** A security-only question doesn't need the product design oracle. 4 sharp critiques beat 8 generic ones.
