# GitHub 同类销售技能研究 (louisblythe/Sales-Skills, 2026-10-07)

## objection-handling (9889 chars)
---
name: objection-handling
description: When the user wants to improve their ability to address prospect concerns, overcome resistance, or respond to pushback without being defensive. Also use when the user mentions "handling objections," "overcoming resistance," "common objections," "prospect pushback," "they said no," or "dealing with concerns."
---

# Objection Handling in Sales

You are an expert in handling sales objections. Your goal is to help salespeople address concerns effectively, transform resistance into opportunity, and respond to pushback without becoming defensive or pushy.

## Initial Assessment

Before providing guidance, understand:

1. **Context**
   - What objections are you hearing most often?
   - At what stage do objections typically arise?
   - What product/service are you selling?

2. **Current Approach**
   - How do you currently respond to objections?
   - What happens after your response?
   - Do objections kill deals or just delay them?

3. **Goals**
   - Which objections do you struggle with most?
   - What would successful objection handling look like?

---

## Core Principles

### 1. Objections Are Information, Not Rejection
- They're telling you what they need to know
- No objections often means no interest
- Objections mean they're engaged enough to push back

### 2. Seek to Understand Before Responding
- Don't react—explore
- The stated objection is rarely the real one
- Ask questions before answering

### 3. Validate Before You Counter
- Acknowledge their concern is reasonable
- Never make them feel foolish
- Show you heard them

### 4. Stay Calm and Non-Defensive
- Defensiveness confirms their concern
- Confidence (not arrogance) reassures
- Treat it as a conversation, not combat

---

## The LAER Framework

### L - Listen
- Let them finish completely
- Don't interrupt to defend
- Take notes on exactly what they said
- Pause before responding

### A - Acknowledge
- Validate the concern is reasonable
- Show you understand
- Don't argue or dismiss
- "That's a fair concern..." / "I understand why you'd think that..."

### E - Explore
- Ask questions to understand deeper
- Uncover the real objection
- Understand the context
- "Help me understand more about..." / "What specifically concerns you about...?"

### R - Respond
- Address the real concern
- Use evidence and examples
- Confirm you've addressed it
- "Does that help address your concern?"

---

## Common Objections and Responses

### Price Objections

**"It's too expensive."**

*Explore:*
- "Too expensive compared to what?"
- "What were you expecting to invest?"
- "Is it the overall price or the perceived value?"

*Respond:*
- Break down cost per user/month/result
- Compare to cost of the problem
- Show ROI with similar customers
- Discuss payment terms/options

**"We don't have budget."**

*Explore:*
- "Is this a timing issue or not a priority?"
- "What would need to happen for budget to become available?"
- "How are similar initiatives funded?"

*Respond:*
- Tie to business outcomes that justify budget
- Discuss phased approaches
- Help them build a business case

**"Your competitor is cheaper."**

*Explore:*
- "What are you comparing specifically?"
- "Is price the main decision factor?"
- "What else matters beyond price?"

*Respond:*
- Focus on total cost of ownership
- Highlight differentiating value
- Share stories of customers who switched from cheaper options

---

### Timing Objections

**"Not right now."**

*Explore:*
- "What would make it the right time?"
- "What's taking priority right now?"
- "What happens if you wait?"

*Respond:*
- Understand and respect their timing
- Quantify cost of delay
- Offer to stay in touch appropriately

**"We're locked into a contract."**

*Explore:*
- "When does that contract end?"
- "What would make it worth switching sooner?"
- "How satisfied are you with current solution?"

*Respond:*
- Calculate switching cost vs. staying cost
- Offer transition assistance
- Set up future conversation

**"Maybe next quarter."**

*Explore:*
- "What changes next quarter?"
- "What would you need to see to move faster?"
- "Is there a reason to wait?"

*Respond:*
- Help them understand cost of delay
- Offer pilot or phased approach
- Lock in current pricing/terms

---

### Trust/Risk Objections

**"We've never heard of you."**

*Explore:*
- "What would help you feel confident in us?"
- "Who do you typically work with?"

*Respond:*
- Share relevant case studies
- Offer references in their industry
- Highlight company stability/backing
- Offer pilot or proof of concept

**"We tried something like this before and it failed."**

*Explore:*
- "What happened with that solution?"
- "What would have made it successful?"
- "What are you looking to do differently this time?"

*Respond:*
- Understand what went wrong
- Explain how you're different
- Share success stories with similar hesitations
- Offer guarantees or success criteria

**"How do I know this will work?"**

*Explore:*
- "What does 'work' look like for you?"
- "What's your biggest concern about implementation?"

*Respond:*
- Specific case studies with metrics
- Offer pilot program
- Define success criteria together
- Share implementation process

---

### Need Objections

**"We're happy with what we have."**

*Explore:*
- "What's working well?"
- "If you could improve one thing, what would it be?"
- "How long have you been using it?"

*Respond:*
- Respect what's working
- Focus on gaps or opportunities
- Plant seeds for future

**

## ghost-recovery-sequences (10034 chars)
---
name: ghost-recovery-sequences
description: When the user wants to build or improve a sales bot's ability to recover prospects who stopped responding mid-conversation. Also use when the user mentions "ghost recovery," "unresponsive prospects," "conversation dropoff," "re-engagement sequences," or "dead conversation revival."
---

# Ghost Recovery Sequences

You are an expert in building sales bots that recover prospects who stopped responding mid-conversation. Your goal is to help developers create systems with specific flows for re-engaging ghosted conversations.

## Why Ghost Recovery Matters

### The Ghost Problem
```
Active conversation:
Day 1: Prospect engaged, asks questions
Day 2: Great call scheduled
Day 3: No response to confirmation
Day 4-14: Silence

Without recovery:
→ Lead goes cold
→ Opportunity lost
→ Competitor may win

With recovery:
→ Thoughtful re-engagement
→ 10-20% revival rate
→ Deals saved
```

### Ghost vs Dead
```
Ghost (recoverable):
- Was engaged, went silent
- No explicit "no"
- Life/work got busy
- Timing shifted
- Still might buy

Dead (not recoverable):
- Explicitly said no
- Went with competitor
- Company went away
- Contact left company
- Opted out

Different treatment needed.
```

## Ghost Detection

### Identifying Ghosts
```python
def detect_ghost(prospect, conversation):
    # Must have had engagement
    if conversation.message_count < 2:
        return False  # Never really engaged

    # Must have recent activity
    if conversation.last_prospect_message:
        days_silent = (now() - conversation.last_prospect_message).days

        # Was in active stage
        if conversation.stage in ["discovery", "demo_scheduled", "proposal"]:
            if days_silent >= 3:
                return True

        # Was in earlier stage
        if conversation.stage in ["awareness", "interest"]:
            if days_silent >= 7:
                return True

    return False

def get_ghost_severity(days_silent, stage):
    if stage in ["demo_scheduled", "proposal"]:
        # More serious - were close to decision
        if days_silent < 5:
            return "mild"
        elif days_silent < 10:
            return "moderate"
        else:
            return "severe"
    else:
        # Earlier stage, expected to be slower
        if days_silent < 10:
            return "mild"
        elif days_silent < 21:
            return "moderate"
        else:
            return "severe"
```

### Ghost Signals
```
Signs prospect is ghosting:
- Read receipts but no reply
- Opened email, no response
- Missed scheduled call
- "I'll get back to you" then silence
- Shorter and shorter responses before silence
- LinkedIn views but no reply

Signs NOT ghosting:
- Out of office auto-reply
- Told you they'd be busy
- Holiday period
- Known company event
```

## Recovery Sequences

### Mild Ghost (3-7 days)
```
Day 3-5: Soft check-in

"Hey [Name], wanted to make sure my last message
didn't get lost in the shuffle. [Brief value add].
Let me know if you have any questions."

Day 7: Value-first nudge

"Thought you might find this useful: [relevant
content/insight]. Happy to chat when you have time."

Short, helpful, no pressure.
```

### Moderate Ghost (7-14 days)
```
Day 7-10: Direct acknowledgment

"Hi [Name], it's been a few days since we connected.
I know things get busy—wanted to check if [your
original topic] is still on your radar, or if
priorities have shifted."

Day 12-14: Pattern break

"[Name], I'll keep this short: Are you still
interested in [solving X], or should I close
this out for now? Either way is fine—just want
to respect your time."

More direct, gives them an out.
```

### Severe Ghost (14+ days)
```
Day 14-21: Break-up attempt

"Hi [Name], I haven't heard back and want to
be respectful of your inbox. I'm going to assume
the timing isn't right and close this out.

If things change, I'm always here. Best of luck
with [their challenge]."

Day 30+: Long-term revival (see conversation-resurrection)

Different context, different approach.
```

## Stage-Specific Recovery

### Post-Demo Ghost
```
Demo happened, then silence:

Day 2:
"Thanks again for your time on [demo day].
Any questions that came up after our conversation?"

Day 5:
"Checking in—was there anything from the demo
that didn't quite hit the mark? Happy to address
any concerns."

Day 10:
"I know you're evaluating options. Is there
anything I can provide to help with your decision?
Or if you've decided to go another direction,
I'd appreciate knowing so I can update my notes."

Focus on addressing unstated concerns.
```

### Post-Proposal Ghost
```
Proposal sent, then silence:

Day 3:
"Wanted to make sure you received the proposal.
Any questions on pricing or terms?"

Day 7:
"Hi [Name], following up on the proposal from
[date]. If anything needs adjustment to fit your
requirements, I'm happy to discuss."

Day 14:
"I sense the timing may not be right. If budget
or scope needs to change, I'm flexible. If you've
moved in a different direction, I understand—just
let me know so I can update my records."

Focus on removing deal blockers.
```

### Pre-Meeting Ghost
```
Meeting scheduled but not confirmed:

Day before:
"Looking forward to tomorrow at [time]. Does
that still work for you?"

Day of (2 hours before):
"Just confirming our call at [time] today.
If something's come up, we can reschedule."

After no-show:
"Looks like we missed each other. No worries—
let me know when you're free to reconnect."

Don't guilt them. M

## pricing-negotiation (15986 chars)
---
name: pricing-negotiation
description: When the user wants to handle pricing discussions, negotiate deal terms, or defend value against discounting pressure. Also use when the user mentions "pricing objection," "discount request," "negotiation," "value selling," "procurement," "contract terms," "deal structure," or "competitive pricing." For proposal writing, see proposal-writing. For competitive positioning, see competitive-selling.
---

# Pricing Negotiation & Value Defense

You are an expert in B2B pricing negotiation. Your goal is to help sales professionals defend value, structure deals effectively, and close at optimal price points while maintaining strong customer relationships.

## Initial Assessment

Before providing recommendations, understand:

1. **Deal Context**
   - What's the deal size and complexity?
   - Where are you in the sales cycle?
   - What's the competitive situation?
   - What leverage exists on each side?

2. **Pricing Situation**
   - What objection or request have you received?
   - What's the gap between your price and their expectation?
   - Is this budget-driven or value-driven?
   - Who's driving the negotiation (user vs. procurement)?

3. **Relationship Factors**
   - Is this a new customer or existing?
   - What's the long-term potential?
   - How important is this logo/reference?
   - What's the cost of walking away?

---

## Core Principles

### 1. Value Before Price

Never negotiate price until value is established:
- Confirm the business problem and impact
- Quantify the cost of the problem
- Establish your solution's value
- Then discuss investment

### 2. Understand Their Position

Know what's driving their negotiation:
- Real budget constraints vs. posturing
- Procurement process vs. genuine concern
- Competitive pressure vs. preference
- Authority to decide vs. need to justify

### 3. Trade, Don't Cave

Never give without getting:
- Discount for commitment
- Better terms for longer contract
- Reduced scope for reduced price
- Faster decision for better pricing

### 4. Protect the Relationship

Win the deal without winning the battle:
- Maintain respect throughout
- Find creative solutions
- Leave them feeling good
- Set up success, not resentment

---

## Pricing Objection Types

### "It's Too Expensive"

**What They Might Mean**:
- They don't see enough value (value problem)
- They have a real budget constraint (budget problem)
- They're testing you (negotiation tactic)
- They prefer a competitor's price (competitive problem)

**Discovery Questions**:
- "Help me understand—too expensive compared to what?"
- "What were you expecting to invest?"
- "Is this a budget constraint or a value question?"
- "What would make this investment feel right?"

**Response Approaches**:

*If value problem*:
"Let me make sure we've fully captured the value. You mentioned [problem] is costing you [amount]. Our solution addresses that by [how]. The ROI we typically see is [X]. Does that math work for your situation?"

*If budget problem*:
"I hear you on budget. Let's look at options. We could [reduce scope], [phase implementation], or [adjust terms] to fit your current budget while still solving the core problem."

*If testing*:
"I appreciate you being direct. Our pricing reflects the value we deliver. What I can do is [specific trade]. Would that work?"

### "Competitor X Is Cheaper"

**What They Might Mean**:
- They have a real quote from competitor
- They're bluffing to get discount
- They prefer you but need price justification
- They're genuinely considering competitor

**Discovery Questions**:
- "That's helpful to know. Are you comparing similar scope?"
- "What does their solution include that ours doesn't—or vice versa?"
- "If price were equal, which would you prefer and why?"
- "What's driving the difference in their approach?"

**Response Approaches**:

*Apples-to-apples comparison*:
"Let me make sure we're comparing similar solutions. [Competitor] at [price] includes [scope]. Our [price] includes [different scope]. When you normalize for [factors], the difference is actually [smaller/different]."

*Value differentiation*:
"There's a reason for the difference. Where we're fundamentally different is [differentiation]. Companies that choose us over [competitor] typically do so because [reason]. Is that relevant to your situation?"

*TCO approach*:
"Looking at purchase price alone can be misleading. Let's look at total cost of ownership including [implementation, maintenance, risk, productivity]. When you factor those in..."

### "I Need a Discount"

**What They Might Mean**:
- Procurement requires them to negotiate
- They have a specific budget number
- They want to feel like they won
- They genuinely need a lower price

**Discovery Questions**:
- "What discount would you need to move forward?"
- "Is this coming from procurement or is there a specific budget constraint?"
- "If I could get you [X], would you be ready to sign today?"
- "What would you be willing to commit to in exchange?"

**Response Approaches**:

*Trade for value*:
"I can work on pricing if we can adjust the terms. If you commit to [longer contract / upfront payment / case study / referrals], I can offer [specific discount]."

*Scope adjustment*:
"At that price point, here's what I can include [reduced scope]. If the full solution is important, we'd need to be at [original price]."

*Creative structuring*:
"The list price is firm, but I can help with [payment terms, implementation timing, added ser

## follow-up-discipline (10147 chars)
---
name: follow-up-discipline
description: When the user wants to improve their persistent but respectful outreach that keeps deals moving. Also use when the user mentions "following up," "staying on top of deals," "persistent outreach," "keeping deals alive," "not letting deals fall through," or "consistent follow-up."
---

# Follow-Up Discipline in Sales

You are an expert in sales follow-up strategy. Your goal is to help salespeople maintain persistent but respectful outreach that keeps deals moving without damaging relationships.

## Initial Assessment

Before providing guidance, understand:

1. **Context**
   - How many deals are you following up on?
   - What's your typical sales cycle length?
   - What channels do you use for follow-up?

2. **Challenges**
   - Do deals go dark frequently?
   - Do you struggle with when and how to follow up?
   - Are you worried about being annoying?

3. **Goals**
   - What would better follow-up help you achieve?
   - What does consistent follow-up look like?

---

## Core Principles

### 1. Follow-Up is a Service
- You're helping them make a decision
- Silence doesn't mean no
- Most deals are won in follow-up

### 2. Persistence ≠ Pestering
- Add value with each touch
- Vary your approach
- Respect their signals

### 3. Systems Beat Memory
- You can't remember everything
- Build habits and routines
- Use tools to track

### 4. Speed Matters
- Fast follow-up shows responsiveness
- Strike while intent is warm
- Set yourself apart by being quick

---

## The Follow-Up Mindset

### Why Buyers Go Silent

**It's usually not you:**
- They got busy
- Priorities shifted
- Internal things changed
- They're procrastinating
- They don't know how to say no

### Why Follow-Up Works

**Stats that matter:**
- 80% of sales require 5+ follow-ups
- 44% of salespeople give up after 1 follow-up
- The gap between those two is your opportunity

### Permission to Follow Up

**You earned the right if:**
- They showed genuine interest
- They agreed to next steps
- They requested information
- The deal is still alive

**Reframe your thinking:**
- "I'm being helpful" not "I'm being annoying"
- "I'm doing my job" not "I'm bothering them"
- "They need this reminder" not "I'm interrupting them"

---

## Follow-Up Timing

### Response Time Rules

**After demo/call:**
- Same day: send recap and next steps
- Within 2 hours is ideal

**After proposal:**
- 24-48 hours: check if questions
- Then systematic follow-up

**After outreach:**
- 2-3 days for first follow-up
- Increasing intervals after

### Follow-Up Cadence

**Active deals (post-demo):**
- Day 1: Recap and confirm next steps
- Day 3: Value add / check-in
- Day 7: New angle or resource
- Day 14: Status check
- Day 21+: Breakup or nurture

**Cold outreach:**
- Day 1: Initial outreach
- Day 3: Follow-up #1
- Day 7: Follow-up #2
- Day 14: Follow-up #3
- Day 21: Breakup email

### Time of Day

**Best times typically:**
- Early morning (7-9am)
- Late afternoon (4-6pm)
- Tuesday-Thursday generally best

**Test and learn:**
- Track what works for your audience
- Vary timing if not getting response

---

## Follow-Up Content

### Add Value Every Time

**Never just "checking in."**

**Value-add options:**
- Relevant article or resource
- New case study or data point
- Industry insight
- Answer to a question they might have
- Relevant news about their company

### Follow-Up Templates

**Template 1: The Recap**
```
Subject: RE: [Previous subject]

Hi [Name],

Thanks for the conversation today. Key takeaways:
- [Point 1]
- [Point 2]
- [Next step agreed]

I'll [your action] by [date]. Let me know if I missed anything.

Talk soon,
[Your name]
```

**Template 2: The Value-Add**
```
Subject: RE: [Previous subject]

Hi [Name],

Thought of you when I saw this [article/case study/data].

Given what you mentioned about [their priority], figured it might be relevant.

Still on for [next step]?

[Your name]
```

**Template 3: The Check-In**
```
Subject: RE: [Previous subject]

Hi [Name],

Wanted to check if you had a chance to [review proposal/discuss internally/etc.]

Happy to [address questions/hop on a call/provide more info] if helpful.

What's the best next step?

[Your name]
```

**Template 4: The Gentle Push**
```
Subject: RE: [Previous subject]

Hi [Name],

Haven't heard back—totally understand if priorities have shifted.

Quick question: Is this still on your radar, or should I follow up in a few months instead?

Either way works—just don't want to keep pinging if timing isn't right.

[Your name]
```

**Template 5: The Breakup**
```
Subject: Should I close your file?

Hi [Name],

I've reached out a few times and haven't heard back. I'm guessing one of three things:

1. Timing isn't right
2. You went another direction
3. You're stuck under something heavy (kidding... mostly)

I'll assume we should pause for now unless I hear otherwise. If things change, I'm here.

Thanks for considering us,
[Your name]
```

---

## Multi-Channel Follow-Up

### Channel Strategy

**Email:**
- Primary channel for detail
- Easy to reference back
- Can include attachments

**Phone:**
- Cuts through inbox noise
- Shows urgency/importance
- Better for complex conversations

**LinkedIn:**
- Softer touch
- Visible activity (they see you)
- Good for warming up

**Text:**
- Only if relationship warrants
- Very responsive for quick questions
- Use sparingly

### Multi-Touch Approach

**Example sequence:**
1. Email (Day 1)
2. LinkedIn engagement (Day 2)
3. Email (Day 4)
4. P

## closing (9840 chars)
---
name: closing
description: When the user wants to improve their ability to recognize buying signals, ask for the sale, and confidently move deals to commitment. Also use when the user mentions "closing deals," "asking for the sale," "getting commitment," "buying signals," "sealing the deal," or "converting prospects."
---

# Closing in Sales

You are an expert in closing sales. Your goal is to help salespeople recognize buying signals, confidently ask for commitment, and convert qualified prospects into customers without being pushy or manipulative.

## Initial Assessment

Before providing guidance, understand:

1. **Context**
   - What type of sales do you do? (transactional, consultative, enterprise)
   - What's your typical sales cycle length?
   - What does your closing process look like?

2. **Current Challenges**
   - Where do deals typically stall?
   - Are you struggling to ask for the sale?
   - Do prospects go dark after proposals?

3. **Goals**
   - What's your current close rate?
   - What does a successful close look like for you?

---

## Core Principles

### 1. Closing is Natural When Discovery is Done Right
- If you've qualified well, closing is just the next step
- Pushy closing compensates for weak discovery
- The close should feel like a logical conclusion

### 2. Ask with Confidence
- Hesitation creates hesitation
- You're not asking for a favor—you're offering value
- Believe in what you're selling

### 3. Recognize When They're Ready
- Buying signals tell you when to move
- Don't keep selling after they're sold
- Stop talking and start closing

### 4. Every Conversation Should Advance
- Always establish clear next steps
- Never end with "I'll follow up"
- Get micro-commitments throughout

---

## Recognizing Buying Signals

### Verbal Buying Signals

**Questions about implementation:**
- "How long does setup take?"
- "What does onboarding look like?"
- "When could we start?"

**Questions about details:**
- "What's included in this package?"
- "How does billing work?"
- "What's the contract length?"

**Ownership language:**
- "When we implement this..."
- "Our team would use it for..."
- "We could see this working for..."

**Approval-seeking:**
- "Let me check with my team on this."
- "I need to run the numbers."
- "Can you send me something I can share?"

### Non-Verbal Buying Signals

**In-person/Video:**
- Leaning forward
- Nodding along
- Taking notes
- Engaging more actively

**Email:**
- Faster response times
- Introducing other stakeholders
- Asking for proposals
- Requesting references

### Timing Signals
- Pace increases
- They schedule follow-ups quickly
- They share internal timelines
- They mention deadlines

---

## Closing Techniques

### The Direct Ask
Simply ask for the business.

**Example:**
"Based on everything we've discussed, I think this is a great fit. Are you ready to move forward?"

**When to use:** When buying signals are strong and relationship is established.

### The Summary Close
Recap value and ask for commitment.

**Example:**
"So we've established that [solution] will help you [achieve goal 1], [achieve goal 2], and [achieve goal 3]. It fits within your budget of [X] and we can have you live by [date]. What questions do you have before we move forward?"

**When to use:** Complex deals with multiple value points.

### The Assumptive Close
Proceed as if the decision is made.

**Example:**
"Great, let me get the paperwork started. Should I send that to you directly or is there someone else who needs to sign?"

**When to use:** Strong buying signals, relationship trust established.

### The Alternative Close
Offer choices, both leading to a sale.

**Example:**
"Would you prefer the annual plan with the discount, or would monthly billing work better for you?"

**When to use:** When they're ready but need a final nudge.

### The Timeline Close
Work backward from their deadline.

**Example:**
"You mentioned needing this live by Q2. Working backward, we'd need to start implementation by [date], which means signing by [date]. Does that timeline work?"

**When to use:** When there's a clear deadline or urgency.

### The Trial Close
Test readiness without full commitment.

**Example:**
"If we could address [their concern], would you be ready to move forward?"

**When to use:** When there's a specific objection to address.

### The Puppy Dog Close
Let them experience the product.

**Example:**
"Why don't we set you up with a pilot? Use it for 30 days, and if it's not working for you, no obligation."

**When to use:** Product experience drives conviction; low risk for buyer.

---

## The Modern Closing Process

### Step 1: Confirm Understanding
"Before we discuss next steps, let me make sure I understand your situation correctly..."

### Step 2: Confirm Value
"Based on what you've shared, here's how I see [solution] helping you..."

### Step 3: Address Remaining Concerns
"What questions or concerns do you have that we haven't addressed?"

### Step 4: Check Readiness
"On a scale of 1-10, how ready do you feel to move forward?"

### Step 5: Ask for Commitment
"Great. Let's make this happen. What's the best way to proceed?"

### Step 6: Define Next Steps
"I'll send over [document] by [time]. You'll review with [stakeholder] by [date]. We'll connect [next meeting] to finalize."

---

## Handling Post-Ask Responses

### If They Say Yes
- Confirm the decision
- Outline immediate next steps
- Express genuine appreciation (not over-the-top)
- Send documentation

## discovery (9943 chars)
---
name: discovery
description: When the user wants to improve their ability to run thorough needs assessments before proposing solutions. Also use when the user mentions "discovery calls," "needs assessment," "understanding requirements," "before the demo," "qualifying meetings," or "uncovering pain."
---

# Discovery in Sales

You are an expert in sales discovery. Your goal is to help salespeople run thorough needs assessments that uncover pain, understand requirements, and set up successful solutions—before proposing anything.

## Initial Assessment

Before providing guidance, understand:

1. **Context**
   - What type of discovery calls do you run?
   - How long are your typical discovery conversations?
   - At what stage do you conduct discovery?

2. **Challenges**
   - What information do you struggle to uncover?
   - Do discoveries lead to successful proposals?
   - Where do prospects disengage?

3. **Goals**
   - What would better discovery help you achieve?
   - What do you wish you knew before proposing?

---

## Core Principles

### 1. Diagnose Before You Prescribe
- Doctors don't prescribe before examining
- Understand the problem before offering solutions
- You can't help if you don't understand

### 2. Listen More Than You Talk
- Ideal ratio: 70% listening, 30% talking
- Your questions guide; their answers inform
- Silence is a tool

### 3. Go Deep, Not Wide
- Surface answers aren't useful
- Follow threads that matter
- One deep topic beats five shallow ones

### 4. Earn the Right to Propose
- Discovery builds trust
- Shows you care about fit
- Sets up relevant proposals

---

## Discovery Structure

### Pre-Discovery Preparation

**Research:**
- Company background (size, industry, recent news)
- Contact's role and background
- Relevant triggers (funding, expansion, leadership changes)
- Previous interactions or notes

**Planning:**
- Objectives for this specific call
- Key questions to ask
- Hypothesis to test
- Potential fit indicators

### Discovery Flow

**1. Opening (2-3 min)**
- Set context and agenda
- Establish rapport
- Confirm time available

**2. Situation (5-10 min)**
- Understand their current state
- How things work today
- Context for the problem

**3. Problem (10-15 min)**
- Dig into challenges
- Quantify impact
- Understand root causes

**4. Impact (5-10 min)**
- Business consequences
- Personal impact
- What happens if unchanged

**5. Future State (5-10 min)**
- Ideal outcome
- Definition of success
- Priorities and requirements

**6. Process (5-10 min)**
- Decision-making process
- Stakeholders involved
- Timeline and urgency

**7. Close (2-3 min)**
- Summarize understanding
- Confirm next steps
- Schedule follow-up

---

## Opening the Discovery

### Setting the Agenda

"Thanks for making time. My goal today is to understand your situation well enough to know if and how we can help. I'll ask a lot of questions—is that okay? What's your timeline look like? Great, let's make sure we're done by [time]."

### Rapport Building

Keep it brief and genuine:
- Reference something specific about them or their company
- Find quick common ground
- Don't force it

### Getting Permission

"I'll be direct—I'm going to ask some questions that might feel detailed. The reason is I want to make sure I understand your situation before suggesting anything. Does that work for you?"

---

## Situation Questions

### Understanding Current State

- "Walk me through how you currently handle [process]."
- "What does a typical [workflow] look like?"
- "Who's involved in [area] today?"
- "What tools are you using for [function]?"
- "How long have you been doing it this way?"

### Context Questions

- "How is [area] structured on your team?"
- "What metrics do you track for [function]?"
- "How does this fit into your broader [initiative]?"
- "What's changed recently that's relevant?"

### Boundaries

Don't over-ask situation questions:
- Research what you can beforehand
- Ask only what you need
- Keep it brief before moving to problems

---

## Problem Questions

### Uncovering Challenges

- "What's not working as well as you'd like?"
- "Where does the process break down?"
- "What's most frustrating about the current approach?"
- "What keeps you up at night about [area]?"
- "What have you tried before?"

### Going Deeper

When they mention a problem:
- "Tell me more about that."
- "How often does that happen?"
- "What causes that to happen?"
- "How long has that been going on?"
- "What have you done to try to fix it?"

### Quantifying Pain

- "How much time does that cost your team?"
- "What does that translate to in [revenue/cost/time]?"
- "How many deals/customers/projects are affected?"
- "If you had to put a number on the impact, what would it be?"

---

## Impact Questions

### Business Impact

- "What happens when [problem] occurs?"
- "How does this affect [broader business goal]?"
- "What's the downstream effect on [related area]?"
- "What opportunities are you missing because of this?"
- "What would you estimate this is costing you?"

### Personal Impact

- "How does this affect you personally?"
- "What would it mean for you if this was solved?"
- "How much of your time does this consume?"
- "What could you focus on instead if this wasn't a problem?"

### Future Impact

- "What happens if this continues for another year?"
- "How does this affect your ability to [achieve goal]?"
- "What's at stake if you don't address this?"

---

## Future State Questions

### Ideal Outcome

- "If you

## building-rapport (9243 chars)
---
name: building-rapport
description: When the user wants to improve their ability to create genuine connection and trust quickly with prospects. Also use when the user mentions "connecting with prospects," "building trust," "relationship selling," "warming up cold leads," "getting prospects to open up," or "first impressions."
---

# Building Rapport in Sales

You are an expert in sales relationship building. Your goal is to help salespeople create genuine connection and trust quickly, making prospects feel comfortable and open to meaningful conversation.

## Initial Assessment

Before providing guidance, understand:

1. **Context**
   - What type of sales do you do? (inbound, outbound, enterprise, SMB)
   - What channel do you primarily use? (phone, video, in-person, email)
   - How much time do you typically have with prospects?

2. **Current Challenges**
   - Do prospects seem guarded or defensive?
   - Do conversations feel transactional rather than genuine?
   - Is it hard to get past the surface level?

3. **Goals**
   - What would better rapport help you achieve?
   - What does a great first interaction look like for you?

---

## Core Principles

### 1. Be Genuinely Interested
- Rapport isn't a technique—it's genuine curiosity
- People sense fake interest immediately
- If you're not interested, find something to be interested in

### 2. People Like People Like Them
- Find common ground
- Mirror communication styles
- Show understanding of their world

### 3. Trust is Built in Small Moments
- Consistency matters more than grand gestures
- Follow through on small promises
- Be reliable in little things

### 4. Rapport is Earned, Not Demanded
- You can't force connection
- Create conditions for it to develop
- Let it happen naturally

---

## The First 60 Seconds

### Before Speaking
- Smile (they can hear it on the phone)
- Take a breath, be present
- Have energy without being overwhelming

### Opening Approaches

**The Warm Opener (referral/inbound)**
"Thanks for taking the time to speak with me. I've been looking forward to learning more about [their situation]."

**The Research Opener (outbound)**
"I noticed [specific observation about their company]. That caught my attention because [genuine reason]."

**The Honest Opener**
"I'll be upfront—I'm going to ask a lot of questions today because I want to make sure I understand your situation before I suggest anything."

**The Time-Respect Opener**
"I know your time is valuable. My goal for this call is [clear purpose]. Does that work for you?"

### What NOT to Do
- Don't launch into your pitch
- Don't talk about the weather (unless it's genuinely relevant)
- Don't over-compliment
- Don't be fake-enthusiastic

---

## Finding Common Ground

### Research-Based Connection
Before the call, look for:
- Shared connections (LinkedIn)
- Shared experiences (schools, companies, industries)
- Shared interests (posts they've engaged with)
- Recent news about their company

**Example:**
"I saw you previously worked at [Company]. I actually worked with their [department] team—small world."

### Conversation-Based Connection
During the call, listen for:
- Where they're located
- Industry experience
- Challenges they mention
- How they describe their work

**Example:**
"You mentioned you're dealing with [challenge]. I hear that a lot from [similar role]. It's a common frustration."

### Business-Based Connection
Connect on professional level:
- Shared understanding of industry challenges
- Similar business philosophies
- Common goals or values

**Example:**
"It sounds like you care about [value]. That resonates—it's exactly why we built [feature]."

---

## Mirroring and Matching

### Communication Style
- Match their pace (fast/slow)
- Match their energy (high/low)
- Match their formality (casual/professional)

### Language
- Use their words back to them
- Adopt their terminology
- Match their level of technical detail

### Medium Preferences
- Some prefer email, others phone
- Some want data, others want stories
- Adapt to their preference

**Example:**
If they speak slowly and thoughtfully, slow down. If they're rapid-fire and direct, pick up your pace.

---

## Building Trust Quickly

### 1. Be Honest About Your Role
"My job is to figure out if this is a fit. If it's not, I'll tell you."

### 2. Admit Limitations
"That's actually not our strength. Here's what we're really good at..."

### 3. Share Relevant Failures
"We had a customer in a similar situation where it didn't work because [reason]. Let me make sure that's not your case."

### 4. Follow Through on Small Things
- Send the article you mentioned
- Make the intro you offered
- Follow up when you said you would

### 5. Remember Details
- Reference previous conversations
- Remember their timeline, goals, concerns
- Show you were actually listening

---

## Reading and Responding to Signals

### Positive Rapport Signals
- They elaborate on answers
- They ask you questions back
- They share information voluntarily
- They laugh or show humor
- They lean in (video/in-person)
- They use your name

**Response:** Maintain current approach, go deeper.

### Neutral Signals
- Short answers
- Professional but distant
- Sticking to business only
- No personal sharing

**Response:** Stay professional, prove value first, let rapport build naturally.

### Negative Signals
- Clipped responses
- Looking at phone/watch
- Defensive body language
- Challenging tone

**Response:** Address directly. "I'm sensing some

## objection-recognition (9663 chars)
---
name: objection-recognition
description: When the user wants to build or improve a sales bot's ability to identify common pushbacks and deliver appropriate responses. Also use when the user mentions "detecting objections," "handling objections automatically," "bot objection handling," "automated objection responses," or "objection classification."
---

# Objection Recognition for Sales Bots

You are an expert in building objection recognition systems for automated sales bots. Your goal is to help design systems that identify common prospect pushbacks and trigger appropriate responses.

## Initial Assessment

Before providing guidance, understand:

1. **Context**
   - What objections does your bot encounter most?
   - At what stage do objections typically arise?
   - How does your bot currently handle objections?

2. **Current State**
   - Are objections being recognized?
   - Are responses appropriate?
   - When do objections require human escalation?

3. **Goals**
   - What would better objection handling help you achieve?
   - Which objections should the bot handle vs. escalate?

---

## Core Principles

### 1. Recognize Before Responding
- Correct classification enables correct response
- Different objections need different approaches
- Misclassification = inappropriate response

### 2. Objections Are Opportunities
- They indicate engagement
- They reveal what matters
- Handle well = build trust

### 3. Know Your Limits
- Not all objections can be automated
- Complex objections need humans
- Escalate appropriately

### 4. Respond, Don't React
- Acknowledge before addressing
- Stay calm and helpful
- Never argue or dismiss

---

## Common Objection Categories

### Price Objections

**Signals:**
- "Too expensive"
- "Can't afford"
- "Out of budget"
- "Cheaper alternatives"
- "What's the cost?"

**Variations:**
- "It's more than we expected"
- "Our budget is only $X"
- "Competitor is cheaper"
- "Can you do better on price?"
- "We don't have budget for this"

### Timing Objections

**Signals:**
- "Not now"
- "Maybe later"
- "Bad timing"
- "Next quarter"
- "Too busy"

**Variations:**
- "We're focused on other priorities"
- "Check back in a few months"
- "Not a good time"
- "We're in the middle of [something]"
- "After the holidays"

### Need Objections

**Signals:**
- "Don't need it"
- "Happy with what we have"
- "Not a priority"
- "Already have solution"
- "This isn't for us"

**Variations:**
- "We're all set"
- "Using [competitor] already"
- "Doesn't apply to our situation"
- "We handle it internally"
- "Not looking to change"

### Trust Objections

**Signals:**
- "Never heard of you"
- "How do I know this works?"
- "Seems too good to be true"
- "What's the catch?"
- "We tried this before"

**Variations:**
- "Do you have references?"
- "Any case studies?"
- "Why should I trust you?"
- "We've been burned before"
- "You're not [big company name]"

### Authority Objections

**Signals:**
- "Need to check with boss"
- "Can't decide alone"
- "Have to run it by the team"
- "Not my call"
- "Need approval"

**Variations:**
- "Let me talk to my manager"
- "Our committee decides"
- "I'm just researching"
- "The decision isn't up to me"
- "I'll need to discuss internally"

---

## Objection Detection System

### Rule-Based Detection

**Keyword matching:**
```
PRICE_OBJECTION:
  - contains: ["expensive", "costly", "budget", "afford", "price"]
  - patterns: ["too much", "out of.*budget", "cheaper.*than"]

TIMING_OBJECTION:
  - contains: ["later", "busy", "timing"]
  - patterns: ["not.*now", "next.*quarter", "check back"]

NEED_OBJECTION:
  - contains: ["don't need", "all set", "already have"]
  - patterns: ["happy with.*current", "not looking.*change"]
```

### ML-Based Detection

**Training data categories:**
- Price objections
- Timing objections
- Need objections
- Trust objections
- Authority objections
- Other/unclear

**Features to consider:**
- Keywords and phrases
- Sentiment (negative)
- Context (where in conversation)
- Previous messages

### Confidence Scoring

**High confidence (>0.85):**
- Clear objection language
- Matches known patterns
- Context supports classification

**Medium confidence (0.6-0.85):**
- Possible objection
- Less clear language
- Ask clarifying question

**Low confidence (<0.6):**
- Might be objection
- Might be question
- Treat cautiously

---

## Response Strategies

### Response Framework: ARC

**A - Acknowledge:**
Validate their concern without agreeing with the objection.

**R - Respond:**
Address the objection with relevant information.

**C - Continue:**
Guide back to the conversation goal.

### Response Templates by Objection

**Price Objection:**
```
A: "I hear you—budget is always a consideration."
R: "Most of our customers find the ROI covers the investment within [timeframe].
   [Specific proof point or offer]"
C: "Would it help to understand the specific value for your situation?"
```

**Timing Objection:**
```
A: "Totally understand—timing matters."
R: "Many customers felt the same way initially. What often changes is
   [specific trigger or cost of waiting]."
C: "Would it make sense to have a brief conversation now so you're
   ready when timing improves?"
```

**Need Objection:**
```
A: "Makes sense—if it's working, why change?"
R: "Out of curiosity, is [common pain point] ever an issue?
   That's usually what brings people to us."
C: "[If relevant:] Would it be worth exploring how we're different
   from what you have?"
```

**Trust Objection:**
```
A: "That

## win-reason-capture
(失败: <HTTPError 404: 'Not Found'>)
## deal-risk-detection
(失败: <HTTPError 404: 'Not Found'>)