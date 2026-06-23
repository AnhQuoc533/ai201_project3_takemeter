# TakeMeter
Online communities run on opinions. NBA subreddits, music theory Discord servers, reality TV recap forums, anime discussion boards — these are spaces where people share takes constantly, and the quality of those takes varies enormously. Some are insightful. Some are hyperbole. Some are just noise. But what makes a good take is genuinely hard to define — it's specific to the community, the context, and the moment.

## Community: Reddit discussions on the Israel-Palestine conflict
I chose this community because it represents one of the most polarized and emotionally charged topics on the platform. This discourse is fundamentally fractured, as users bring irreconcilable worldviews, personal stakes, and conflicting historical narratives to the same thread.

This community is an excellent fit for a classification task because the discourse is highly varied in many ways:

* **Ideological diversity:** Users don't just disagree on facts. They express their voices from many different moral and political viewpoints. A comment might be pro-Israel *and* fact-based, or pro-Palestine *and* emotionally reactive.

* **Rhetorical variation:** The same basic claim can be made as rigorous analysis, as emotional assertion, or as a genuine question. This variation is what makes the labels challenging and meaningful.

* **High stakes:** Because people care deeply, they rarely phone it in. Even low-effort comments reveal something about stance and strategy. This discourse is alive in ways that casual subreddits aren't.

* **Contextual ambiguity:** It's very hard to classify some comments without knowing community norms. A sarcastic comment might read like a mere curiosity to an outsider. A seemingly neutral recount of history might be interpreted as support depending on what's omitted. This ambiguity is *the* interesting part, where subjectivity lives.

## Labels
### with israel
**Description**: the post/comment expresses support for or alignment with Israel's position. The commenter presents arguments in favor of Israeli policies, actions, or existence, often with supporting context or counter-arguments to opposing views.

**Example 1**:
>The IDF on Friday denied allegations by Hamas that Israeli forces had fired on civilians awaiting aid in Gaza City, saying that soldiers had not used their weapons at any stage of the incident and pointed to Palestinian gunmen as the cause of the casualties.\
The incident took place late on Thursday after the military allowed 31 aid trucks to pass into the Gaza Strip heading toward Gaza City in the territory's north. According to unverified Palestinian reports, at least 40 people waiting for the convoy have been killed in the incident.


**Example 2**:
> The Arab monarchies will always side those that are fighting with the forces that want to topple them.\
Case in point: Yemen. During the North Yemen Civil War, the arab monarchies supported the Shia theocratic monarchy while Nasser's Egypt support the Republicans. Then you had the British being pushed out of South Yemen by the between the Communists, where the Arab monarchies now suported with the Republicans bcz the communists were a bigger threat to them.\
Edit: confused one yemeni civil war with the other.

### with palestine
**Description**: the post/comment expresses support for or alignment with Palestine's position. The commenter advocates for Palestinian rights, criticizes Israeli policies, or presents historical and political arguments favoring Palestinian interests.

**Example 1**:
>It's idiotic for Hamas to do this when Netanyahu and a hard right coalition are in power\
For one thing it's going to result in a full force counter attack that will kill hundreds of not thousands of Palestinians\
For another thing it will likely result in a surge in support for Netanyahu who will use it as mandate to further push the policies that harm Palestinians.

**Example 2**:
> To quote Netanyahu, 'A PM Up to His Neck in Investigations Has No Mandate' (said back when former Israeli PM Ehud Olmert was being investigated back in 2008 over corruption).\
There's a lot to be said that his inaction over ministers that have said and *continue to say* terrible things was what the ICJ correctly focused on. But of course, to quote Netanyahu once again in the week after October 7th, "what will my government be in three months?" Turns out, three months later and it's still the same clowns.

### neutral
**Description**: the post/comment takes a balanced or non-committal stance on the Israel-Palestine conflict. The comment acknowledges both sides, expresses uncertainty, calls for nuance, or focuses on humanitarian concerns without strongly endorsing either side's political position.

**Example 1**:
>As an Israeli. Sorry to say, at this point, both Palestinian ( hamas + Fatah) & Israeli sides elected such losers ..  it's like everyone is trying toes up the lives of 20 million people.\
Peace is in the hands of messianic psychopaths

**Example 2**:
> Reminder: Your beef isn't with Israel or Palestine, it's with a small handful of officials in charge who have done the unforgivable for power at the cost of lives on both sides. Sarah the Jewish girl working at the Tel Aviv Gap Store and Ahmed the Shia Butcher are not the reason things suck in the middle east right now.

### inquisitive
**Description**: the post/comment poses genuine questions or seeks clarification about the conflict. The OP is asking for information, requesting explanation of positions, or exploring aspects of the topic without making strong claims or taking a clear stance.

**Example 1**:
>So Israel is required to provide Gaza with electricity, water, and food? And Israel is somehow accountable for the Egypt-Palestine border?\
I don't understand.

**Example 2**:
> Canada's parliament passed a non-binding motion late Monday that called for a halt to Israeli arms sales and urged the international community to work toward a two-state solution to resolve the conflict between Israel and the Palestinians, in line with government policy.\
> Would a 2 state solution work? What do the people living there want?

## Hard edge cases
### Sarcasm contents
* **The ambiguity:** Sarcasm inverts the surface meaning, making it hard to know which underlying position to label. Does "Oh sure, Israel is totally just defending itself" express with palestine (by mocking Israel's claim) or should I label the literal statement? And when sarcasm is nested ("If you believe that, I have a bridge to sell you"), which layer am I classifying?
* **How I handle:**
  * Identify the sarcasm by looking for heavy punctuation ("sure," "yeah right"), emoji shifts (😂🙃), or obvious logical absurdity
  * Reverse the literal claim to find the true position. "Nothing says self-defense like 100,000 casualties" → actual position is "this is not legitimate self-defense"
  * Classify the underlying position, not the surface statement. The commenter's sarcasm is a rhetorical choice within that position, not the position itself
  * Flag high-confidence sarcasm in notes for calibration. If I'm uncertain whether it's sarcasm or genuine, assume genuine (less presumptive)
  * When sarcasm targets both sides equally, classify as neutral ("Sure, both sides are just saints"). This is rare in polarized discourse but honest.

### Satire contents
* **The ambiguity:** Satire mocks a style or logic rather than making a direct claim, so the position can be unclear. A mock-heroic defense of either side could be exposing that side's reasoning as absurd, or it could be that I'm misreading the intent. Does a satirical post arguing the "obviously correct" pro-Israel position mean the author is with israel or with palestine (using satire to mock)?
* **How I handle:**
  * Look for the target of mockery, not the argument itself. Satire works by exaggerating something real. What's being exaggerated? If it's pro-Israel rhetoric cranked to absurdity, the author is likely with palestine; if it's pro-Palestine victimhood narratives pushed to extremes, likely with israel
  * Check for tell-tale satire markers: "modest proposal" framing, rhetorical escalation beyond reason, inversion of expected emotion (celebrating what should be tragic), citations of absurd sources
  * When the target is unclear, read the comment history if available. Satirists usually aren't hiding their real position since they just use it rhetorically in this comment
  * Classify based on the position being mocked. If I can't determine which side is being satirized, default to neutral (satire targeting the discourse itself, not one side)

## Data collection plan
* **Target platform:** Reddit ([r/worldnews](https://www.reddit.com/r/worldnews/))
* **Volume:** 200 samples per label
* **Handling underrepresentation:**
  * Remove unuseful samples from overpopulated classes.
  * Use keyword sampling to pull more contents.
  * Remove the least populated class entirely.

## Evaluation metrics
| Metric | Why it matters |
| :--- | --- |
| Accuracy score | The most basic evaluation metric that gives me a quick look at model's performance
| Precision | Critical in this polarized spaces. False accusations of bias matter
| Recall | How many comments from a specific label does the model catch? If recall is 60%, I'm missing 40% of one perspective
| F1 | The harmonic mean of precision and recall that helps me detect imbalance
| Confusion matrix | Easy to see exactly where the algorithm is making mistakes and reveal systematic bias

## Definition of success
For a classifier to be genuinely useful in a polarized space, technical performance alone is insufficient. Success means the model reliably identifies stance while minimizing false accusations of bias, which carry real reputational and emotional stakes in this community.

Acceptance criteria:
* **Accuracy ≥ 72%**: Above random chance (25%) and realistic for the ambiguity inherent in this topic
* **Recall ≥ 65%**: Catches most comments without systematically silencing a viewpoint
* **Precision ≥ 70%**: Minimizes false accusations of bias, which carry emotional weight in polarized spaces
Neutral/inquisitive ≥ 60% — Acknowledges these labels are harder to predict
* **Per-class F1:**
  * *with israel:* ≥ 0.74 
  * *with palestine:* ≥ 0.74 
  * *neutral:* ≥ 0.64
  * *inquisitive:* ≥ 0.45 (rare, lower bar acceptable)

This dataset sits at the intersection of high emotion and genuine ambiguity. A classifier good enough to save time for online moderators and researchers, but not good enough to be weaponized against one perspective is the goal. The thresholds proposed are strict enough to demand real performance, yet lenient enough to accept the fuzziness of discourse analysis in a polarized space.

## AI Tool Plan
### Label stress-testing
- AI tool: Claude
- What I'll give it as input: Label definitions and edge case description
- What I expect it to produce: 5 artifically made, unlabeled posts/comments that sit at the boundary between any pair of labels
- How I'll verify: manually classify these AI-generated contents and ask the AI tool for correction and reasoning

### Annotation assistance
- AI tool: Claude
- What I'll give it as input: Label definitions, edge case description, and my dataset without labels
- What I expect it to produce: a csv file containing the original samples and their new labels
- How I'll verify: compare its results with my pre-labeled samples stored in a seperate csv file

### Failure analysis
- AI tool: Claude AI
- What I'll give it as input: List of wrong predictions and ask it to identify the bias patterns
- What I expect it to produce: a detailed analysis on the input predition results, including the bias patterns and the reason behind