---
name: linkedin-posts
description: "Generate LinkedIn posts from a script or from scratch, with a hook-validation and approval workflow. Use when creating LinkedIn posts, writing a post, or when user mentions linkedin post, linkedin posts, write a post, create a post."
license: MIT
metadata:
  version: 1.0.0
  author: Xavier Caffrey
  category: marketing
  domain: social-media
---

# LinkedIn Posts

Generate high-performing LinkedIn posts with validated hooks, approval workflows, and consistent quality.

---

## Workflow

### Step 1: Choose Mode

Ask the user:
- **Option A: From existing script** — ask which file to read (e.g., `script.md` or `research.md`)
- **Option B: From scratch** — ask for topic, key talking points, any specific data/stories/angles

### Step 2: Analyze Source Material

**Mode A (from script):** Extract key insights, contrarian takes, audience pain points, stories.

**Mode B (from scratch):** Use topic + key points. If thin, ask:
- "Any specific data points or numbers?"
- "A story or personal experience tied to this?"
- "Who's the target audience?"

### Step 3: Generate 3 Posts

Each post must:
1. Use a DIFFERENT hook style (practical, contrarian, story-based) — ALL must pass Hook Rules below
2. Be substantial — real depth, specific details, data when available
3. Conversational and authentic, not salesy
4. End with a CTA (vary across posts):
   - Question: "What's your experience with X?"
   - Video reference: "Full breakdown in the video (link in comments)"
   - Challenge: "I'd love for someone to tell me I'm wrong."

**Style:** Substantial long-form, opinion-driven, first-person. Separators: dashes, bullets, arrows (->). Minimal emoji. Short paragraphs.

**No false-contrast filler (NON-NEGOTIABLE):**
NEVER use the "isn't X / it's Y" or "not about X / it's about Y" pattern. This is lazy copy that wastes words negating something just to state the real point. Say what you mean directly.

Bad: "The goal isn't just to make my team more efficient. It's to make them more technical."
Good: "The goal is to make my team more efficient and technical."

Bad: "This isn't about automation. It's about leverage."
Good: "This is about leverage."

Bad: "I'm not saying AI will replace you. I'm saying AI will make you dangerous."
Good: "AI will make you dangerous."

If you catch yourself writing "isn't/not [something]. It's [the real point]" — delete the first sentence and lead with the real point. Every time.

### Hook Rules (NON-NEGOTIABLE)

Every hook MUST pass both tests:

**1. The Number Test** — at least one specific number ($76M, 90 days, 15-person team, 70-80%, etc.)

Bad: "AI SDRs are failing"
Good: "AI SDRs raised $200M+ and 70-80% of implementations fail by month 3"

**2. The Visualization Test** — reader can picture a specific scene. "Can I see this in my head?"

Bad: "Sales teams struggle with outbound"
Good: "I had coffee with a VP of Sales last week who's never heard of Clay"

**Both together:** "The biggest AI SDR company raised $76M from a16z. Then they got caught lying about their customers. The CEO just stepped down." (Numbers: $76M. Visual: the scandal, CEO stepping down.)

**Using personal context:**
- If the topic connects to the user's experience, ASK: "Does this relate to something you've experienced? Any story I should weave in?"
- Reference their expertise, match voice/tone, use their tools/stack in examples

### Step 4: Approval Loop

For each post, display content + hook validation, then ask: **"Approve / Edit / Regenerate / Skip?"**

```
Hook validation:
- Number: [the number used] ✓
- Visual: [the scene/image] ✓
```

Continue until all posts approved or skipped.

### Step 5: Save Output

Save approved posts as `linkedin-posts.md` in the user's project folder. Display all approved posts in chat.

---

## Reference Examples

### Practical/Educational
```
If you're running ABM, you should start scoring awareness.

Unlike other RevOps systems, reps LOVE this.

It answers one question: "Does this account actually know we exist?"

[Post continues with 5-stage framework and implementation details...]
```

### Contrarian/Opinion
```
Salesforce promised us an AI revolution with Agentforce. I'm looking around, and all I see is a whole lot of nothing.

The $300B giant is stumbling.

-> Technical glitches.
-> Slow adoption.
-> Executives quietly backpedaling on their "agentic AI" promises.

Meanwhile...
A 22-year-old with Clay and a $50 API key just automated more work than their entire Enterprise implementation.

[Post continues with contrarian take on enterprise vs modern stack...]
```

### Story/Data-Driven
```
I had coffee with a VP of Sales last week who's never heard of Clay.

Not "heard of it but doesn't use it." Never. Heard. Of. It.

This is a guy running a 15-person SDR team at a Series B SaaS company doing $20M ARR.

Smart operator. Reads all the sales blogs. Goes to conferences.

And when I mentioned waterfall enrichment, he looked at me like I was speaking Giberish.

[Post continues with data on adoption trends and analysis...]
```

---

## Output Format

Before saving, verify each hook passes both tests:
- [ ] Post 1: Number? ___ / Visual? ___
- [ ] Post 2: Number? ___ / Visual? ___
- [ ] Post 3: Number? ___ / Visual? ___

If any fails, rewrite that hook before proceeding.

```
# LinkedIn Posts — [Topic/Video Title]

---

## Post 1: [Hook Style]
**Status:** Ready for posting

[Full post content]

---

## Post 2: [Hook Style]
**Status:** Ready for posting

[Full post content]

---

## Post 3: [Hook Style]
**Status:** Ready for posting

[Full post content]
```

---

## Hook Templates (66 Proven Hooks)

Use these templates as inspiration. Customize with specific details from the source material.

### Curiosity/Value Hooks
1. `I always show people this [method/diagram]`
2. `It's ridiculous that [blank]`
3. `There's a funny trick that people do in order to [blank]`
4. `A successful person doesn't have to [blank]`
5. `It's funny to me that [blank]`

### Urgency/Stakes Hooks
6. `If you don't watch this video you could lose [blank]`
7. `I have spent an absurd amount of time [blank], here is something that will [blank]`
8. `In the next 60 seconds I'm going to show you how to create a simple [tool/plan/system] that generates [blank]`

### Promise/Outcome Hooks
9. `What if I told you you could [blank] and [outcome]`
10. `If you have a business where your average product price is more than [£XX] then you have to be using this [tool/plan/system]`
11. `If you sell a [product/offer] here's a super simple [blank] you can use that made over [$XX] in [timeframe]`

### Trending/News Hooks
12. `Why is nobody talking about what [blank] just did`
13. `Every time you get a new [blank] send this [blank] to turn them into a [blank]`

### Comparison/Contrast Hooks
14. `Have you ever wondered how [blank] achieve [blank] while you struggle to [blank]`
15. `You know, I used to think that [blank] but then I realised...`

### Emotional/Relatable Hooks
16. `Ahhgggh, this is making me insane [explain]`
17. `I listened to 1000 podcasts on [blank], most advice is bad, some good, and some are dangerous. Here's what actually works`

### Framework/System Hooks
18. `Everybody says "[blank]" when trying [blank] but nobody gives you a solid framework to [get outcome]`
19. `If I had [X] days to make [$XX] using [blank] here's exactly how I'd do it`

### Surprising/Contrarian Hooks
20. `This is the weirdest [statistic/fact/result] I think I've ever seen when it comes to [blank]`
21. `I hate to break it to you but [blank]`
22. `If you're still doing [blank] in [year] you're literally wasting money/time`

### Results/Proof Hooks
23. `I grew to [$XX] per month and here's how I did it`
24. `No one cares about [blank] anymore. Here's what you should do going forward`

### Aspirational Hooks
25. `Imagine being able to [blank] and have this happen instantly`
26. `Here's one of the easiest ways to get [big desire]`

### Vulnerability/Honesty Hooks
27. `I messed up...`
28. `I'm sorry I lied, for years I've been telling people to [blank] but it's actually much more serious than that`

### Focus/Priority Hooks
29. `If you're not obsessively focused on these 3 things you can forget about [blank]`
30. `It's come to my attention that some people don't know how to [blank] here's how...`

### Solution Hooks
31. `This is how to get [result] from your [vehicle] without [hard thing]`
32. `Here's my favourite way to handle [statement your audience think]`

### Quick Tips Hooks
33. `Quick [insert topic] tip for you guys [say tip] [start explaining]`
34. `I [insert activity] in the last [X] days and here's 4 things I learned`

### Controversial/Opinion Hooks
35. `This is a controversial opinion - don't get mad at me but [say the thing]`
36. `If you see this [insert graphic] on an [agency/saas/fitness] website... run`

### Authority/Expertise Hooks
37. `I'm a [insert job title] and this is exactly how I [get result]`
38. `This [entrepreneur/business coach] will make [$XXX] without [hard thing] let me explain`

### Tool/Resource Hooks
39. `You're gonna thank me for this one - this tool makes it super easy for you to [blank]`
40. `Ok, normally I don't like [blank] however...`

### Story Hooks
41. `I remember I was in [past situation] and [something happened]`
42. `Yesterday I was [familiar place/action] and I noticed something weird [tell them something related to your business]`

### Demographic Hooks
43. `If you're between the ages of 25 and 35 [blank]`
44. `You're only one [blank] away from [outcome] and here's exactly how to [blank]`

### Identity Hooks
45. `99% of the time I am hardworking, polite [insert job title] but 1% of the time I will not accept [blank]`
46. `I am the worst [blank] of all time but this is day one me trying to [blank] follow along for more`

### Action Hooks
47. `Don't [action] until you've done this one thing`
48. `Never ever ever [blank]`
49. `Never [do this thing] until you've done this one thing`

### Observation Hooks
50. `Is it just me or is [opinion]`
51. `I don't know what's going on but... [realisation/opinion]`
52. `Let me get this straight, guys [realisation/opinion]`

### Promise Hooks
53. `What if I told you that [achieving goal] is easier than you think`
54. `If you hired me to help you [achieve result] in [X] days, this is exactly how I'd do it`
55. `Hey it's [your name], you're about to see a [degrading comment] [achieve this result] in [timeframe]`

### Common Belief Hooks
56. `Everyone always talks about [insert common belief/comment]`
57. `The #1 way to [achieve bad thing] is to [explain the bad thing]`
58. `Everyone tells you to [common saying] but they never show you how. Here's a 3-step framework that works for me.`

### Empathy Hooks
59. `If you're seeing this right now, I need you to understand this one thing... It's ok if...`

### Transformation Hooks
60. `6 months ago I was [successful thing] and then I realised that I was [downside] so I decided to [blank]`

### Competition Hooks
61. `The reason that people are not [buying from you/listening to you] they are buying from your competitors instead is because you are not doing this things and they are`

### Case Study Hooks
62. `Here's how one of our clients [name] generated [$XX] in [timeframe] using a [blank] strategy`

### Universal Hooks
63. `Every single person who [blank] should be [blank]`

### Direct Statement Hooks
64. `If you're [doing X], you should [do Y]`
65. `[Company/Tool] promised [X]. I'm looking around, and all I see is [Y].`
66. `I [did something] with [person/type] who's never heard of [thing you assumed everyone knew]`
