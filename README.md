# TakeMeter: Classifying Stance in Polarized Discourse

## Overview

TakeMeter is a stance classification system designed to categorize comments and posts from Reddit's r/worldnews discussing the Israel-Palestine conflict. The project explores the tension between automated classification and the genuine ambiguity inherent in polarized discourse, comparing a zero-shot baseline (Groq LLM) with a fine-tuned DistilBERT model.

This dataset sits at the intersection of high emotion and genuine ambiguity. A classifier good enough to save time for online moderators and researchers, but not good enough to be weaponized against one perspective is the goal.

---

## Community: Reddit discussions on the Israel-Palestine conflict

This community was chosen because it represents one of the most polarized and emotionally charged topics on the platform. This discourse is fundamentally fractured, as users bring irreconcilable worldviews, personal stakes, and conflicting historical narratives to the same thread.

This community is an excellent fit for a classification task because the discourse is highly varied in several key ways:

- **Ideological diversity:** Users don't just disagree on facts. They express their voices from many different moral and political viewpoints. A comment might be pro-Israel *and* fact-based, or pro-Palestine *and* emotionally reactive.

- **Rhetorical variation:** The same basic claim can be made as rigorous analysis, as emotional assertion, or as a genuine question. This variation is what makes the labels challenging and meaningful.

- **High stakes:** Because people care deeply, they rarely phone it in. Even low-effort comments reveal something about stance and strategy. This discourse is alive in ways that casual subreddits aren't.

- **Contextual ambiguity:** It's very hard to classify some comments without knowing community norms. A sarcastic comment might read like a mere curiosity to an outsider. A seemingly neutral recount of history might be interpreted as support depending on what's omitted. This ambiguity is *the* interesting part, where subjectivity lives.

---

## Label Taxonomy
### `with israel`
**Description**: the post/comment expresses support for or alignment with Israel's position. The commenter presents arguments in favor of Israeli policies, actions, or existence, often with supporting context or counter-arguments to opposing views.

**Example 1**:
>The IDF on Friday denied allegations by Hamas that Israeli forces had fired on civilians awaiting aid in Gaza City, saying that soldiers had not used their weapons at any stage of the incident and pointed to Palestinian gunmen as the cause of the casualties.\
The incident took place late on Thursday after the military allowed 31 aid trucks to pass into the Gaza Strip heading toward Gaza City in the territory's north. According to unverified Palestinian reports, at least 40 people waiting for the convoy have been killed in the incident.


**Example 2**:
> The Arab monarchies will always side those that are fighting with the forces that want to topple them.\
Case in point: Yemen. During the North Yemen Civil War, the arab monarchies supported the Shia theocratic monarchy while Nasser's Egypt support the Republicans. Then you had the British being pushed out of South Yemen by the between the Communists, where the Arab monarchies now suported with the Republicans bcz the communists were a bigger threat to them.\
Edit: confused one yemeni civil war with the other.

### `with palestine`
**Description**: the post/comment expresses support for or alignment with Palestine's position. The commenter advocates for Palestinian rights, criticizes Israeli policies, or presents historical and political arguments favoring Palestinian interests.

**Example 1**:
>It's idiotic for Hamas to do this when Netanyahu and a hard right coalition are in power\
For one thing it's going to result in a full force counter attack that will kill hundreds of not thousands of Palestinians\
For another thing it will likely result in a surge in support for Netanyahu who will use it as mandate to further push the policies that harm Palestinians.

**Example 2**:
> To quote Netanyahu, 'A PM Up to His Neck in Investigations Has No Mandate' (said back when former Israeli PM Ehud Olmert was being investigated back in 2008 over corruption).\
There's a lot to be said that his inaction over ministers that have said and *continue to say* terrible things was what the ICJ correctly focused on. But of course, to quote Netanyahu once again in the week after October 7th, "what will my government be in three months?" Turns out, three months later and it's still the same clowns.

### `neutral`
**Description**: the post/comment takes a balanced or non-committal stance on the Israel-Palestine conflict. The comment acknowledges both sides, expresses uncertainty, calls for nuance, or focuses on humanitarian concerns without strongly endorsing either side's political position.

**Example 1**:
>As an Israeli. Sorry to say, at this point, both Palestinian ( hamas + Fatah) & Israeli sides elected such losers ..  it's like everyone is trying toes up the lives of 20 million people.\
Peace is in the hands of messianic psychopaths

**Example 2**:
> Reminder: Your beef isn't with Israel or Palestine, it's with a small handful of officials in charge who have done the unforgivable for power at the cost of lives on both sides. Sarah the Jewish girl working at the Tel Aviv Gap Store and Ahmed the Shia Butcher are not the reason things suck in the middle east right now.

---

## Data collection plan

### Source
**Platform:** Reddit ([r/worldnews](https://www.reddit.com/r/worldnews/))  
**Distribution:** 600 annotated samples, 200 per label  
**Collection Method:** Manual sampling from existing threads on the Israel-Palestine conflict

### Labeling Process
Labels were assigned by hand after careful review of each comment in context. The labeling process prioritized identifying the commenter's underlying position rather than surface-level claims, with special attention to sarcasm and satire. Then, Claude AI was utilized to stress-test the label results and amend any incorrectly classified samples.

## Hard Edge Cases

### Sarcasm
- **The ambiguity:** Sarcasm inverts the surface meaning, making it hard to know which underlying position to label.
- **Decision rule:** Identify sarcasm by heavy punctuation ("sure," "yeah right"), emoji shifts, or logical absurdity. Reverse the literal claim to find the true position. Example: "Nothing says self-defense like 100,000 casualties" expresses criticism (with palestine).
- **Implementation:** Classify the underlying position, not the surface statement. When uncertain, assume genuine (less presumptive).

### Satire
- **The ambiguity:** Satire mocks a style or logic rather than making a direct claim, so the position can be unclear.
- **Decision rule:** Look for the target of mockery, not the argument itself. Satire works by exaggerating something real. What's being exaggerated? If pro-Israel rhetoric is cranked to absurdity, the author is likely with palestine; if pro-Palestine victimhood is pushed to extremes, likely with israel.
- **Implementation:** Check for tell-tale satire markers like "modest proposal" framing or rhetorical escalation. When the target is unclear, default to neutral.

### Examples
#### Example 1:
> Israeli bombs are capable of levelling huge buildings within seconds, but apparently they can't make a meaningful hole in a car park. Seriously, have a look at the photos of the impact site. The 'crater' is more of a pothole, and buildings all around are damn near untouched aside from a few shaken roofing tiles and the occasional shrapnel hole. Whoever was actually *in* that car park at the time definitely had it bad, but there's no feasible way for five hundred people to even fit in there, especially not with all the (now burned) cars in it.

**True Label:** `with israel`

**Why it was hard:** At first glance, the comment appears to be discussing a bombing incident rather than expressing support for either side. Someone could reasonably classify it as `neutral` because it focuses on physical evidence and damage assessment.

**Decision:** The reason for this classification is because the argument was being used to support the Israeli explanation of the Al-Ahli hospital explosion and to cast doubt on accusations against Israel.

---

#### Example 2:
> WION (Indian news sources) reported today that it was an American JDAM that struck the hospital.\
Channel 4 (UK) reported today audio released by Israel is fake.\
Middle Eastern/Arabic news sources still point blame at Israel.\
For many people, this won't matter -- they've already dug deeper into their own bubble. Carl Sagan warned us that one day our society would become unable to distinguish between [what feels good and what's true](https://www.goodreads.com/quotes/632474-i-have-a-foreboding-of-an-america-in-my-children-s).

**True Label:** `with palestine`

**Why it was hard:** At the end of the comment, the author concluded with a philosophical observation about media consumption and echo chambers. This might mislead any naive reader into believing this person has a `neutral` point of view.

**Decision:** The reason for this classification is the highly selective presentation of evidence in this content. The text used three specific media reports from WION, Channel 4, and Arab sources that exclusively undermine the Israeli and Western intelligence accounts of the Al-Ahli hospital explosion.

---

#### Example 3:
> As much as I dislike Elon I do think X is one of the few platforms where most of REAL videos of what is happening right now is being posted and showing the true colours of what is going on.\
Unfortunately on Reddit many subs are getting banned and censored for posting similar information/videos of the war.

**True Label:** `with palestine`

**Why it was hard:** Without context or original post where this comment replied to, anyone can view this opinion as `neutral` or unrelated to the topic.

**Decision:** The reason for this classification is the use of words in this content. The OP described the Isreal-Palestine conflict as *war* and accused the systemic censorship of Reddit platform, both of which are claims most frequently leveled by pro-Palestinian activists


---

## Fine-Tuning Approach
**BaseModel:** DistilBERT (`distilbert-base-uncased`)

**Data Split:**
- Training: 420 examples (70%)
- Validation: 90 examples (15%)
- Test: 90 examples (15%)
- Stratified split to maintain label distribution in each set

**Tokenization:**
- Tokenizer: DistilBERT's default tokenizer
- Max sequence length: 256 tokens
- Truncation enabled for longer posts/comments

**Data Collation:**
- Used HuggingFace's DataCollatorWithPadding for dynamic padding
- Reduces padding waste and speeds up training

**Hyperparameters setup:**

| Hyperparameter | Value | Rationale |
| --- | --- | --- |
| num_train_epochs | 10 | Increased from 3 to 10 to allow the model more exposure to the training data. With 420 training examples (more than 200), more epochs were needed to reach convergence. |
| weight_decay | 0.05 | Increased from 0.01 to 0.05 to add stronger L2 regularization. This was intended to reduce overfitting, as the dataset is very small (less than 10 thounsands) and the training process is longer (more than 3 epochs). |
| learning_rate | 2e-5 | Kept at standard BERT fine-tuning rate. This is the established starting point for BERT-family models. |
| per_device_train_batch_size | 16 | This is optimal for T4 GPU memory and batch stability on small datasets. |
| warmup_steps | 50 | Provides a gentle learning rate ramp-up to stabilize early training. |

---

## Baseline Approach
**Model:** `llama-3.3-70b-versatile` via Groq API

**Grounded Prompt:**
>You are classifying posts and comments from a subreddit discussing the Israel-Palestine conflict. Assign each post to exactly one of the following categories:  
>
>LABEL: with israel  
DEFINITION: The post expresses support for or alignment with Israel's position. The commenter presents arguments in favor of Israeli policies, actions, or existence, often with supporting context or counter-arguments to opposing views.  
EXAMPLE: "The IDF on Friday denied allegations by Hamas that Israeli forces had fired on civilians awaiting aid in Gaza City, saying that soldiers had not used their weapons at any stage of the incident and pointed to Palestinian gunmen as the cause of the casualties. The incident took place late on Thursday after the military allowed 31 aid trucks to pass into the Gaza Strip heading toward Gaza City in the territory's north. According to unverified Palestinian reports, at least 40 people waiting for the convoy have been killed in the incident."
>
>LABEL: with palestine  
DEFINITION: The post expresses support for or alignment with Palestine's position. The commenter advocates for Palestinian rights, criticizes Israeli policies, or presents historical and political arguments favoring Palestinian interests.  
EXAMPLE: "It's idiotic for Hamas to do this when Netanyahu and a hard right coalition are in power. For one thing it's going to result in a full force counter attack that will kill hundreds of not thousands of Palestinians. For another thing it will likely result in a surge in support for Netanyahu who will use it as mandate to further push the policies that harm Palestinians."  
>
>LABEL: neutral  
DEFINITION: The post takes a balanced or non-committal stance on the Israel-Palestine conflict. The comment acknowledges both sides, expresses uncertainty, calls for nuance, or focuses on humanitarian concerns without strongly endorsing either side's political position.  
EXAMPLE: "Reminder: Your beef isn't with Israel or Palestine, it's with a small handful of officials in charge who have done the unforgivable for power at the cost of lives on both sides. Sarah the Jewish girl working at the Tel Aviv Gap Store and Ahmed the Shia Butcher are not the reason things suck in the middle east right now."  
>
>VALID LABELS (respond with exactly one):  
with israel  
with palestine  
neutral  
>
>INSTRUCTIONS:  
>- Analyze the post carefully, considering the underlying position, not just surface claims  
>- For sarcasm or satire, identify the true position being expressed  
>- Choose from the valid labels only and respond with ONLY the label name, nothing else  
>- Do not explain your reasoning  


**Results Collection:**
- Evaluated on the same 90-example test set as the fine-tuned model
- Temperature: 0 (deterministic output)
- All 90 responses were successfully parsed into valid labels

---

## Evaluation Report

### Overall Accuracy Comparison

| Model | Accuracy 
| --- | ---
| Zero-shot baseline (Groq) | 0.833 (83.3%) 
| Fine-tuned DistilBERT | 0.678 (67.8%) 
Fine-tuning regression: 0.156

The fine-tuned model underperformed the baseline by a significant margin. This counterintuitive result suggests that fine-tuning on a small, highly ambiguous dataset led to overfitting and loss of generalization. The baseline's zero-shot approach benefited from the scale and general knowledge of the 70B parameter model.

### Per-Class Metrics

#### Baseline Model (Groq)

| Label | Precision | Recall | F1-Score | Support |
| --- | --- | --- | --- | --- |
| with israel | 0.79 | 0.90 | 0.84 | 30 |
| with palestine | 0.92 | 0.80 | 0.86 | 30 |
| neutral | 0.80 | 0.80 | 0.80 | 30 |
| **Weighted Avg** | **0.84** | **0.83** | **0.83** | **90** |

The baseline model excels on with palestine (92% precision) and achieves strong recall on with israel (90%). Neutral achieves balanced performance (80% on all metrics).

#### Fine-Tuned Model (DistilBERT)

| Label | Precision | Recall | F1-Score | Support |
| --- | --- | --- | --- | --- |
| with israel | 0.67 | 1.00 | 0.80 | 30 |
| with palestine | 0.64 | 0.47 | 0.54 | 30 |
| neutral | 0.74 | 0.57 | 0.64 | 30 |
| **Weighted Avg** | **0.68** | **0.68** | **0.66** | **90 |

The fine-tuned model achieves perfect recall on with israel (1.00) but at the cost of low precision (0.67). It struggles significantly with with palestine (47% recall). The neutral class achieves moderate performance.

### Confusion Matrix

| True / Predicted | with israel | with palestine | neutral |
| --- | --- | --- | --- |
| **with israel** | 30 | 0 | 0 |
| **with palestine** | 10 | 14 | 6 |
| **neutral** | 5 | 8 | 17 |

**Key observations:**
- The model's "with israel" recall is perfect (30/30 correct), suggesting it learns this category well.
- The model confuses with palestine and neutral frequently: 10 with palestine examples are misclassified as with israel, and 6 are misclassified as neutral.
- The neutral class is confused with both other classes: 5 misclassified as with israel, 8 as with palestine.

### Wrong Prediction Analysis

#### Wrong Prediction #1
**Text:** "Literally had an irl friend say 'Who cares about Israelis' when I cited the Oct 7th massacre. We see eye to eye on 99% of every political and world issue, but they fiercely said it was AI-generated..."

**True Label:** `neutral`  
**Predicted Label:** `with israel` (confidence: 0.69)

**Analysis:** This comment is expressing frustration with a friend who dismissed Israeli victims. The commenter is reporting a conversation and their disagreement with that dismissal. The model classified it as with israel because the text centers Israeli casualties (Oct 7th massacre), but this misses the neutral framing of reporting interpersonal conflict. The comment doesn't advocate for either side; it's documenting disagreement. The model likely weighted the mention of Israeli victims too heavily, failing to recognize the meta-level of the comment (discussing a conversation, not taking a stance).

#### Wrong Prediction #2
**Text:** "I am a Muslim, I am a father, This is fucking disgusting and brought tears to my eyes. If you can do this to a child let alone another human you are so beyond evil. I don't know what to make of this..."

**True Label:** `with palestine`  
**Predicted Label:** `with israel` (confidence: 0.41)  

**Analysis:** This is one of the model's lowest-confidence errors, but it's revealing. The comment expresses outrage at a specific atrocity but doesn't name the actor or victim. Without context, the model cannot determine which side's actions are being condemned. The emotional language ("disgusting," "evil") is used in both pro-Israel and pro-Palestine discourse. The model may have picked up on generic humanitarian distress rather than the political alignment the comment implies when read against the broader thread context.

#### Wrong Prediction #3
**Text:** "I'll just add, Netanyahu is NOT Israel. You can be pro an independent, free Palestine, a strong, independent Israel, *and* anti-Netanyahu, anti-Hamas, anti-Iranian Ayatollahs and anti-Hezbollah. We should..."

**True Label:** `neutral`  
**Predicted Label:** `with palestine` (confidence: 0.49)

**Analysis:** This comment explicitly rejects binary thinking ("Netanyahu is NOT Israel") and lists support for both Palestinian independence and Israeli independence. However, the model classified it as with palestine because it leads with "anti-Netanyahu." The comment's true nature is a meta-political stance rejecting tribalism, but the model picked up on the explicit criticism of Netanyahu and mapped that to with palestine. The model struggles with comments that explicitly refuse to choose sides.

### Sample Classification Results

#### Correctly Classified Examples

**Example 1: with israel (predicted with confidence)**
- **Text:** "The Arab monarchies will always side those that are fighting with the forces that want to topple them. Case in point: Yemen. During the North Yemen Civil War, the arab monarchies supported the Shia theocratic monarchy..."
- **Predicted Label:** `with israel` (confidence: 0.92)
- **True Label:** `with israel`
- **Explanation:** This geopolitical analysis of Arab alliances is correctly identified as supporting the pro-Israel perspective by explaining Arab nations' pragmatic alignment. The model successfully captured the analytical, non-emotional framing of arguments in favor of Israeli positioning.

**Example 2: with palestine (correctly predicted)**
- **Text:** "It's idiotic for Hamas to do this when Netanyahu and a hard right coalition are in power. For one thing it's going to result in a full force counter attack that will kill hundreds of not thousands of Palestinians..."
- **Predicted Label:** `with palestine` (confidence: 0.78)
- **True Label:** `with palestine`
- **Explanation:** Despite criticizing Hamas, this comment is correctly classified as with palestine because it centers Palestinian vulnerability and harm from military response. The model learned to look beyond surface-level criticism to identify the underlying political alignment.

**Example 3: neutral (correctly identified as balanced)**
- **Text:** "Reminder: Your beef isn't with Israel or Palestine, it's with a small handful of officials in charge who have done the unforgivable for power at the cost of lives on both sides. Sarah the Jewish girl working at the Tel Aviv Gap Store and Ahmed the Shia Butcher are not the reason things suck..."
- **Predicted Label:** `neutral` (confidence: 0.81)
- **True Label:** `neutral`
- **Explanation:** The model successfully identified this as neutral by recognizing the explicit rejection of the Israel-Palestine binary and the emphasis on shared humanity across communities. The distinction between officials and civilians is a key neutral signifier.

---

## Reflection
The fine-tuned DistilBERT model learned a heavily skewed version of the task:
* **Perfect `with israel` identification:** The model achieved 100% recall on `with israel`, correctly identifying all 30 test examples. It learned clear linguistic markers of pro-Israel discourse.

* **Binary collapse toward with israel:** Rather than learning three distinct classes, the model appeared to collapse `with palestine` and `neutral` into a single anti-israel category. When uncertain, it defaulted to `with israel` (30/30 correct) rather than distributing errors across classes.

* **Confusion at boundaries:** The model struggled most where I intended it to show nuance. Comments that criticize both sides, acknowledge complexity, or refuse tribalism were misclassified.

I intended the model to learn:

* **Stance detection beyond surface claims:** Recognize that criticizing Hamas doesn't mean the comment is `with israel`; criticizing Netanyahu doesn't mean it's `with palestine`.

* **Sarcasm and satire inversion:** Reverse the literal claim to find the true position.

* **Neutral as a genuine category:** Recognize that calling for nuance, distinguishing civilians from officials, or focusing on humanitarian concerns expresses a distinct political position rather than being a failed attempt at one of the other categories.

* **Contextual ambiguity tolerance:** Accept that some comments are genuinely ambiguous and might reasonably belong to multiple categories depending on how a reader interprets unstated context.

The model's perfect recall on `with israel` but 47% recall on `with palestine` suggests a asymmetry in what it learned. The fine-tuning process may have led the model to conflate "not saying positive things about Israel" with "saying things that support Israel" due to the high dimensionality of the embedding space and the small dataset size.

This shows a real problem in polarized discourse: when a topic is highly charged, moderates positions or refusals to pick sides can be misread as opposition to whichever side one prefers.

---

## Spec Reflection

**One way the spec helped you during implementation:**  
The hard edge cases in the planning spec was very helful. Having thought through how to handle sarcasm and satire *before* labeling meant I could apply consistent rules. For example, the rule "reverse the literal claim to find the true position" for sarcasm gave me a clear decision procedure that I used for about 8-10 comments that involved ironic language. Without this upfront planning, I would have made inconsistent calls.

**One way your implementation diverged from the spec, and why:**  
I did not use AI to label the entire dataset. Instead, I manually labeled all of my samples. Then, I used Claude to *generate edge cases* for stress-testing (5 artificially created comments at the boundary between label pairs) and verified my own classifications against those. Using an LLM to label a dataset on a polarized topic where the baseline LLM would be one of my evaluation models seemed like a potential contamination risk. If I had Claude pre-label examples and then verified them, I might unconsciously bias my manual verification toward Claude's choices.

---

## AI Usage
**Instance 1:**
* *What I gave the AI:* Label definitions, hard edge case description, and a request to generate 5 artificial Reddit comments that sit at the boundary between pairs of my labels.
* *What it produced:* 5 Reddit comments that include sarcastic/satirical comments, a comment criticizing Hamas that acknowledged Palestinian suffering, and meta-commentary.
* *What I changed or overrode:* I rejected Claude's first response because the comments weren't ambiguous enough for me. So I asked it to make them more subtle, and the second reponse was better and as expected.


**Instance 2:**
* *What I gave the AI:* The confusion matrix of my fine-tuned model, my hyperparameters, the model name, and the dataset distribution
* *What it produced:* A detailed analysis on the performance result of my model including the bias pattern and where it got confused, as well as the reason for this performance and how to improve
* *What I changed or overrode:* I reject Claude's suggestion that the issue was primarily due to class imbalance in the data. The data *was* balanced, so I suggested that the issue might be in the *training procedure*
