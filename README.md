# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

For this project, I explored discussions from r/solotravel and r/travel on Reddit. I selected these communities because they include different perspectives on travel, ranging from destination reviews and trip planning to safety concerns, loneliness, and personal growth.

While reviewing the posts, I noticed three recurring distinctions. First, some travelers share firsthand experiences, while others ask questions or seek recommendations. Second, some posts provide practical advice about transportation, budgeting, accommodation, and itineraries, while others focus on personal opinions or emotional experiences. Finally, some users support their perspectives with specific examples, including destinations, costs, and challenges, while others provide limited context.

Posts about Oslo, Malta, and Italy particularly stood out because the authors discussed both positive and negative aspects of their trips while providing useful details. These observations helped me identify potential categories for classifying travel discussions based on their primary purpose and the type of information shared.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `advice_seeking`

**Definition:** Posts where travelers primarily ask for recommendations, guidance, or help making travel-related decisions.

**Example 1:**
> "Is this itinerary too packed?" — A traveler asks whether their multi-country itinerary is realistic and seeks suggestions for improving it.

Source: https://www.reddit.com/r/solotravel/comments/1l38rup/

**Example 2:**
> "Southeast Asia onward tickets" — A traveler asks about onward-ticket requirements when entering different countries.

Source: https://www.reddit.com/r/solotravel/comments/1b0rv5h/

### `personal_experience`

**Definition:** Posts where travelers primarily share firsthand experiences, emotions, challenges, or personal reflections.

**Example 1:**
> "First solo trip at 56" — A traveler reflects on completing a five-day solo trip and their expectations of traveling independently.

Source: https://www.reddit.com/r/solotravel/comments/1bad46z/

**Example 2:**
> "Loneliness while solo travelling" — A traveler describes feeling excluded during a hostel group activity.

Source: https://www.reddit.com/r/solotravel/comments/1n5sb2y/

### `practical_guidance`

**Definition:** Posts where travelers primarily share actionable tips, recommendations, or lessons that others can apply when planning or taking trips.

**Example 1:**
> "Things I learned from my first solo trip" — A traveler shares packing recommendations and practical travel tips.

Source: https://www.reddit.com/r/solotravel/comments/1l0ebb6/

**Example 2:**
> "Safety tip from a solo female traveler" — A traveler uses a personal accommodation experience to offer safety advice.

Source: https://www.reddit.com/r/solotravel/comments/1dazyyo/

### The hardest boundary

**Which two labels:** `personal_experience` and `practical_guidance`

**The decision rule I used every time:**

If a post primarily describes what happened during a trip or how the traveler felt, I label it `personal_experience`, even if it briefly mentions a recommendation. If the main purpose is to provide actionable advice, specific suggestions, or lessons other travelers can apply, I label it `practical_guidance`, even when the advice comes from a personal experience.

**AI Boundary Test:**

I tested my decision rule using a hypothetical post about a traveler who experienced loneliness during their first solo trip and recommended staying in social hostels. I initially classified it as `personal_experience` because the main focus was the traveler's emotions and firsthand experience.

The AI agreed with my classification but pointed out that the recommendation could also fit `practical_guidance`. This helped me confirm that the primary purpose of the post should determine its label rather than the presence of individual pieces of advice.

**Final Decision:** I kept `personal_experience` because the post primarily describes an emotional experience, while the recommendation is secondary.
---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
I collected 200 real Reddit posts from r/solotravel and r/travel. The first 29 posts were collected manually, while the remaining 171 were sourced from the Arctic Shift Reddit archive.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I manually labeled the first 29 posts using my label taxonomy. For the remaining 171 posts, I used Claude to suggest labels and short explanations, then selected and approved the posts for my dataset. I also checked for duplicates, missing text, and label imbalance.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
|  advice_seeking| 134 | 67% |
|  personal_experience| 57 | 28.5% |
|  practical_guidance|  9| 4.5% |
| **Total** | 200  | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->



**1. My Comprehensive Montenegro Visit Guide**

- **The post:** A traveler describes a 10-day trip to Montenegro, sharing transportation costs, beach recommendations, and travel warnings.
- **Could have been:** `personal_experience` or `practical_guidance`.
- **I chose, because:** I chose `practical_guidance` because although the author shares firsthand experiences, the post primarily provides actionable recommendations that other travelers can use when planning their trips.

**2. CDG Airport Naivety**

- **The post:** A traveler shares an experience navigating long airport queues in Paris and nearly missing a connecting train.
- **Could have been:** `personal_experience` or `practical_guidance`.
- **I chose, because:** I chose `personal_experience` because the post focuses on the traveler's frustration, challenges, and personal experience. Although the author recommends allowing additional time for connections, the advice is secondary to the story.

**3. Fatigue After Only One Week**

- **The post:** A traveler describes experiencing exhaustion and anxiety shortly after beginning a long trip and questions whether they should change their approach.
- **Could have been:** `advice_seeking` or `personal_experience`.
- **I chose, because:** I chose `advice_seeking` because the traveler uses their personal experience to explain their situation but ultimately seeks guidance on what to do next. The main purpose is asking for advice rather than simply reflecting on the experience.
---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->
I used Claude Code to help develop and run a data collection workflow for my TakeMeter project. My goal was to gather Reddit posts related to solo travel that I could use to build a text classification dataset. With Claude Code's assistance, I collected over 3,000 posts. However, I did not use all the collected data. I reviewed and filtered the posts to remove irrelevant examples and focused on building a dataset of 200 posts across my three categories: advice_seeking, personal_experience, and practical_guidance. This experience showed me that collecting a large amount of data does not necessarily mean the data will be balanced or suitable for training.
### Moment 1

- **What I asked for:** I used Claude Code to help develop a Python script to collect and organize real Reddit travel posts for my dataset.
- **What came back:** Claude generated scripts to retrieve Reddit posts from the Arctic Shift archive, filter unsuitable entries, and check for duplicates.
- **What I changed:** I adjusted my approach when Reddit API credentials became a challenge, switching to a publicly available archive while preserving my original manually labeled posts.

### Moment 2

- **What I asked for:** I asked Claude to help classify additional Reddit posts using my three label definitions and decision rules.
- **What came back:** Claude suggested labels and explanations. However, the initial selection contained too many `advice_seeking` posts, exceeding the assignment's 70% limit.
- **What I changed:** I expanded the candidate pool to include more personal experiences and practical guidance, excluded irrelevant or promotional posts, and selected a more balanced dataset.

### Pre-labelling disclosure

I manually labeled my first 29 Reddit posts before using AI assistance. Claude suggested labels and explanations for the remaining 171 posts. I used my taxonomy and decision rules to review the selected posts and identify ambiguous cases. The final dataset contains 200 posts, with no duplicates and no label exceeding 70%.


I fine-tuned a DistilBERT classifier on 200 labeled Reddit travel posts using three categories: `advice_seeking`, `personal_experience`, and `practical_guidance`. Training used 3 epochs, a learning rate of 2e-5, a batch size of 16, and random seed 42. The dataset was divided into 139 training, 31 validation, and 30 test examples. Training was performed on a CPU.

The model achieved **66.67% test accuracy** and a **macro F1-score of 0.267**. Although it correctly classified all 20 advice-seeking posts, it failed to identify the 9 personal-experience posts and 1 practical-guidance post. Every test prediction was assigned to `advice_seeking`, demonstrating how class imbalance can produce misleading overall accuracy.
<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.833 | 0.667 | -0.167 |
| Macro F1 | 0.870 | 0.267 | -0.603 |
| F1 — `advice_seeking` | 0.872 | 0.800 | -0.072 |
| F1 — `personal_experience` | 0.737 | 0.000 | -0.737 |
| F1 — `practical_guidance` | 1.000 | 0.000 | -1.000 |

**What I predicted before I looked:**

I did not record a prediction before viewing my trained model's results, so I cannot claim to have made one beforehand. I initially expected fine-tuning to help the model recognize patterns specific to travel-related discussions.

**What the gap actually means:**

The baseline outperformed my fine-tuned model, achieving 83.3% accuracy and a macro F1-score of 0.870 compared to 66.7% accuracy and 0.267 macro F1. My trained model classified every test post as `advice_seeking`, failing to recognize the other two categories. This suggests that class imbalance and limited examples may have affected its ability to distinguish between labels. The results show that fine-tuning did not improve performance in this experiment and that a general pretrained model using clear label definitions can perform better on this task.


---


## Run Log — Before

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall Classification Accuracy | ≥ 70% | 66.7% | 66.7% | 66.7% | MISSED |
| 2. Per-Label F1 Score | ≥ 0.55 for each label | 0.800 / 0.000 / 0.000* | 0.800 / 0.000 / 0.000* | 0.800 / 0.000 / 0.000* | MISSED |
| 3. Dataset Balance | No label > 60%; ≥ 40 per label | 67% largest; 9 smallest | Same dataset | Same dataset | MISSED |
| 4. Labeling Consistency | ≥ 17/20 (85%) | Not measured | Not measured | Not measured | NOT YET TESTED |
| 5. Confidence Reliability | ≥ 75% accuracy when confidence ≥ 0.75 | Not measured | Not measured | Not measured | NOT YET TESTED |

*Per-label F1 values are shown in this order: `advice_seeking`, `personal_experience`, `practical_guidance`. These values come from the original test evaluation and should be verified against the three-seed results.*|



### Confusion matrix

| true \ predicted | advice_seeking | personal_experience | practical_guidance |
|---|---|---|---|
| **advice_seeking** | 20 | 0 | 0 |
| **personal_experience** | 9 | 0 | 0 |
| **practical_guidance** | 1 | 0 | 0 |

**My biggest off-diagonal number, and what it means:**

The largest confusion occurred when the model incorrectly classified all 9 `personal_experience` posts as `advice_seeking`, while no `advice_seeking` posts were classified as `personal_experience`. This suggests the model learned to favor the majority category rather than distinguish between posts requesting advice and those primarily sharing experiences. The imbalance in my training dataset may have contributed to this behavior.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

|## Verdicts and Diagnoses

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall classification accuracy | ≥ 70% across all three seeds | **MISSED** | Accuracy was 66.7% for seeds 42, 7, and 2024, falling below my 70% target. |
| 2 | Per-label F1 score | ≥ 0.55 for every label across all seeds | **MISSED** | `advice_seeking` achieved 0.800, but `personal_experience` and `practical_guidance` both scored 0.000 across all seeds. |
| 3 | Dataset balance | No label above 60%; at least 40 examples per label | **MISSED** | `advice_seeking` represented 67% of the dataset, while `practical_guidance` had only 9 examples. |
| 4 | Labeling consistency | ≥ 17/20 (85%) agreement | **NOT YET MEASURED** | I still need to complete the second review of 20 randomly selected posts to determine whether the labels were applied consistently. |
| 5 | Confidence reliability | ≥ 75% accuracy for predictions with confidence ≥ 0.75 | **NOT YET MEASURED** | My current results do not report accuracy specifically for predictions with confidence scores of at least 0.75. |

**Diagnoses**

**Criterion 1 — Overall accuracy:** The model consistently achieved 66.7% accuracy across all three seeds, but this result is misleading because it predicted `advice_seeking` for every test post. The model appears to have learned the majority-class pattern rather than the distinctions between the three categories.

**Criterion 2 — Per-label F1:** The confusion matrix shows that all 9 `personal_experience` posts and the single `practical_guidance` post were incorrectly classified as `advice_seeking`. This suggests that the model struggled to distinguish posts sharing experiences or recommendations from posts requesting advice.

**Criterion 3 — Dataset balance:** My dataset contains 134 `advice_seeking` posts, 57 `personal_experience` posts, and only 9 `practical_guidance` posts. This imbalance likely contributed to the model favoring `advice_seeking`. Increasing representation of the minority categories could help the model learn their differences.

**Criterion 4 — Labeling consistency:** My labels matched all 30 staff labels, resulting in 100% agreement with no disagreements. Although this suggests that I was able to follow the staff's labeling rules, it does not necessarily mean my original dataset is consistent. I still need to review 20 posts from my TakeMeter dataset to determine whether I applied my own labeling rules consistently.

**Criterion 5 — Confidence reliability:** I need to examine predictions with confidence scores of at least 0.75 to determine whether the model's confidence reflects its actual correctness.

---

## Agreement Report

Agreement rate: 30/30 = 100%

Disagreements: None (0).


**What the pattern in my disagreements tells me:**

My labels matched all 30 staff labels, resulting in a 100% agreement rate. There were no disagreements to adjudicate. This shows that the labels I submitted were consistent with the staff's taxonomy and decision rules. However, this does not necessarily mean my original TakeMeter dataset is free from labeling inconsistencies, since it uses different categories and definitions.
**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->


There were no disagreements between my labels and the staff labels. All 30 posts matched, so there were no cases that required further review or adjudication under the staff's taxonomy. While this suggests that I understood and applied the staff's decision rules consistently, I recognize that my original TakeMeter dataset uses different categories. Therefore, I cannot assume that the same level of consistency applies to my own dataset without reviewing those labels separately.

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**
Achieving 100% agreement suggests that I understood and applied the staff's taxonomy and decision rules consistently. Although there were no disagreements to analyze, this exercise helped me recognize the importance of having clear labeling guidelines, especially when posts could fit multiple categories. However, my TakeMeter project uses a different taxonomy, so I would still need to evaluate whether my original 200 posts were labeled consistently.


---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**
## The Improvement

**What I changed:** I increased the number of training epochs from 3 to 5 while keeping the dataset, learning rate, and three random seeds unchanged.

**Which diagnosis pointed at it:** My model consistently predicted `advice_seeking` for every test post. I wanted to test whether additional training would help it learn the differences between the three categories.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| Overall accuracy | ≥ 70% | 66.7% | 66.7% | 66.7% | MISSED |
| Per-label F1 | ≥ 0.55 for each label | 0.800 / 0.000 / 0.000 | 0.800 / 0.000 / 0.000 | 0.800 / 0.000 / 0.000 | MISSED |
| Dataset balance | No label above 60%; ≥ 40 per label | Unchanged | Unchanged | Unchanged | MISSED |
| Labeling consistency | ≥ 17/20 (85%) | Not measured | Not measured | Not measured | NOT YET MEASURED |
| Confidence reliability | ≥ 75% accuracy when confidence ≥ 0.75 | Not measured | Not measured | Not measured | NOT YET MEASURED |

*Per-label F1 values are ordered: advice_seeking / personal_experience / practical_guidance.*

**Did it help, and how do I know?**

Increasing the number of training epochs did not improve my model's performance. Accuracy remained at 66.7%, and macro F1 stayed at 0.267 across all three seeds. The model continued predicting `advice_seeking` for every test post, suggesting that additional training alone was not enough to address the class imbalance or help the model distinguish between categories. This showed me that improving the quality and balance of the training data may be more important than simply training the model longer.




---

## What's Still Broken

## What's Still Broken

My model still falls short of several acceptance criteria. Overall accuracy remained at 66.7%, below my 70% target, and the F1 scores for `personal_experience` and `practical_guidance` remained at 0.000. Increasing training from 3 to 5 epochs did not improve these results. If I continued working on the model, I would focus on collecting more examples of the underrepresented categories and reviewing ambiguous labels rather than increasing training time again.

Dataset balance also remains an issue. Of my original 200 posts, 134 were labeled `advice_seeking`, 57 were `personal_experience`, and only 9 were `practical_guidance`. I would prioritize adding carefully labeled examples of `practical_guidance` and evaluating whether the model can distinguish these posts from advice requests. I stopped after testing one training change because the milestone required one measurable improvement, and collecting and validating additional posts would require more time.

I also still need to complete the separate 20-post labeling consistency check and calculate accuracy specifically for predictions with confidence scores of at least 0.75. My staff agreement exercise achieved 30/30 agreement, but that does not replace checking consistency within my own travel-post dataset.

**The gap between my labels and what the model learned:**

I designed my labels to distinguish between people asking for advice, sharing personal travel experiences, and providing practical guidance. However, the confusion matrix shows that the model learned to predict `advice_seeking` for every test post rather than recognizing the differences between these categories. This reinforced how important dataset representation and label quality are when training a classifier.



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
