# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall Classification Accuracy

The classifier should achieve at least **70% accuracy** on the held-out test set across all three random seeds.

**Why this target:** My dataset includes three categories: `advice_seeking`, `personal_experience`, and `practical_guidance`. Since some travel posts contain both personal stories and recommendations, I expect occasional confusion between categories. A 70% accuracy target would demonstrate that the model can identify the primary purpose of a post substantially better than random guessing.

---

## 2. Per-Label F1 Score

Each of the three labels should achieve an **F1-score of at least 0.55** on the held-out test set across all three seeds.

**Why this target:** Posts describing personal experiences may overlap with practical guidance, especially when travelers share lessons learned. With only 200 posts and a relatively small test set, scores for less common labels may fluctuate. A minimum F1-score of 0.55 sets a meaningful target while accounting for these challenges.

---

## 3. Dataset Balance

No single label should represent more than **60% of the 200 labeled posts**, and each label should contain at least **40 examples**.

**Why this target:** Travel communities contain many personal stories, which could cause `personal_experience` to dominate the dataset. Requiring at least 40 examples per category helps ensure that the classifier has enough examples of advice-seeking and practical guidance posts to learn their differences.

**Dataset observation:** After collecting and labeling the posts, I found that `advice_seeking` was the dominant category at 67%, while `practical_guidance` represented only 4.5%. This imbalance may affect the classifier's ability to distinguish less-represented categories, particularly across different training seeds.
---

## 4. Labeling Consistency

A second review of **20 randomly selected posts** should agree with the original labels on at least **17 out of 20 posts (85%)**, using the documented labeling rules.

**Why this target:** Some posts include multiple purposes, such as describing loneliness while recommending social hostels. Reviewing posts again will help determine whether my decision rule consistently distinguishes personal experiences from practical guidance.

---

## 5. Confidence Reliability

Among predictions with a confidence score of **0.75 or higher**, at least **75% should be correctly classified** on the held-out test set across all three seeds.

**Why this target:** The classifier may struggle with posts that combine questions, experiences, and recommendations. When it assigns a high confidence score, I want that prediction to be reasonably trustworthy rather than confidently assigning the wrong category.






---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
