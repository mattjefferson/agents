# AI Writing Patterns to Avoid

Distilled from Wikipedia's AI Cleanup project. Use for final review of any AI-assisted text.

## The Core Problem

LLMs regress to statistical mean: specific facts become generic claims, concrete details become vague significance. The subject gets simultaneously less specific and more exaggerated.

## Language Patterns

### Inflated Significance

LLMs puff up importance with formulaic phrases. Delete or replace with specific facts.

**Words to cut:**
- stands/serves as a testament/reminder
- plays a vital/significant/crucial role
- underscores/highlights its importance/significance
- reflects broader
- symbolizing its ongoing
- enduring/lasting impact
- key turning point
- indelible mark
- deeply rooted
- profound heritage
- steadfast dedication

**Example (bad):**
> Berry Hill today stands as a symbol of community resilience, ecological renewal, and historical continuity.

**Fix:** State what actually happened. If nothing notable happened, cut the sentence.

### Superficial Analysis

Trailing -ing phrases that claim significance without evidence.

**Words to cut:**
- ensuring...
- highlighting...
- emphasizing...
- reflecting...
- underscoring...
- showcasing...
- aligns with...
- contributing to...

**The tell:** Abstract subjects doing human actions. A person can highlight something; a fact cannot.

**Example (bad):**
> The Federation was internationally recognized, highlighting Pakistan's entry into the global pickleball community.

**Fix:** Delete everything after the comma. The fact speaks for itself.

### Promotional Language

**Words to cut:**
- rich/vibrant tapestry
- artistic/cultural/literary landscape
- boasts a
- continues to captivate
- groundbreaking
- intricate
- stunning natural beauty
- enduring/lasting legacy
- nestled
- in the heart of

**Example (bad):**
> Nestled within the breathtaking region, the town stands as a vibrant hub with rich cultural heritage.

**Fix:** "The town is in [region]." Then cite specific heritage if notable.

### Hedging and Disclaimers

**Phrases to cut:**
- it's important to note/remember/consider
- may vary
- based on available information
- while specific details are limited

These phrases add nothing. State facts or don't.

## Structural Patterns

### Formulaic Conclusions

LLMs add "In summary" or "In conclusion" sections. Cut them. If the writing is clear, readers don't need a recap.

### "Despite... Challenges" Formula

Almost always AI-generated:

> Despite its [positive words], [subject] faces challenges... Despite these challenges, [subject] continues to thrive.

**Fix:** If challenges are real and notable, describe them specifically. Otherwise cut.

### Rule of Three Padding

LLMs overuse triads to seem comprehensive:

> global SEO professionals, marketing experts, and growth hackers

**Fix:** Pick the most accurate term. One specific noun beats three vague ones.

### Weasel Attribution

Vague attribution to unnamed authorities:

- Industry reports suggest...
- Observers have cited...
- Some critics argue...

**Fix:** Name the source or cut the claim.

### Elegant Variation

LLMs avoid repeating words by using synonyms that obscure meaning:

> the constraints of socialist realism... the challenging climate of Soviet artistic constraints... the confines of state-imposed artistic norms

**Fix:** Use the same term consistently when referring to the same thing.

### False Ranges

"From X to Y" where X and Y aren't on a coherent scale:

> from problem-solving and tool-making to scientific discovery

**Fix:** Use "including" or list items directly.

## Formatting Tells

### Title Case in Headings
LLMs capitalize all main words. Wikipedia and most style guides use sentence case.

### Excessive Boldface
Bolding every key term or concept. Use sparingly.

### Inline-Header Lists
Bullet points with bold headers followed by colons:

> **Key Point:** Description here...

This is slide-deck style, not prose.

### Em-Dash Overuse
LLMs use em-dashes where commas or parentheses work better. One or two per piece is plenty.

### Curly Quotes
LLMs use "curly quotes" and 'smart apostrophes'. Not definitive alone, but combined with other patterns it's a signal.

## Technical Artifacts

These are dead giveaways of unreviewed AI output:

- `citeturn0search0` or similar markup
- `utm_source=chatgpt.com` in URLs
- `[placeholder text here]` unfilled templates
- "As an AI language model..."
- "I hope this helps!"
- "Would you like me to..."
- Markdown syntax (`**bold**`, `## headings`) in non-markdown contexts

## What's NOT a Tell

- Perfect grammar (skilled humans write well too)
- Academic vocabulary (not the same as AI vocabulary)
- Long sentences (LLMs tend toward short, punchy sentences)
- Letter-like formality (existed before LLMs)

## Quick Audit Process

1. **Scan for red-flag phrases** — Ctrl+F the word list above
2. **Check sentence endings** — Look for trailing -ing analysis phrases
3. **Verify claims** — Are significance statements backed by facts?
4. **Read aloud** — Does it sound like a press release or a human?
5. **Cut ruthlessly** — When in doubt, delete the flourish
