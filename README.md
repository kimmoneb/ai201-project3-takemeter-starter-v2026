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

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

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



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



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
