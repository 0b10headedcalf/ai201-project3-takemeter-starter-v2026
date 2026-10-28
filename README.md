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
This project classifies individual comments from Wikipedia's WikiProject Video
games discussion community, where editors collaborate on video-game articles,
sources, and organization. It sorts each comment by its main purpose: asking a
`question`, offering a `suggestion`, or contributing to the `discussion`. The
classifier uses the comment text, not images or the full conversation. Its
initial dataset comes from a single discussion archive, so it may not represent
the entire community.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `question`

**Definition:** A comment explicitly requests information, clarification, help,
or input from other editors about a Wikipedia-related issue.

**Example 1:**
> [ProtoDrake asks about a guideline for game engines.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-ProtoDrake-20221012164200-Respiciens-20221012103500)

**Example 2:**
> [Blaze Wolf asks for suggestions for a Did You Know hook.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-Blaze_Wolf-20221010131600-DYK_for_Need_for_Speed_Unbound)

### `suggestion`

**Definition:** A comment recommends a specific action or change that an editor
could take concerning a Wikipedia article, source, or process.

**Example 1:**
> [PerryPerryD recommends gathering more article information before a nomination.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-PerryPerryD-20221010144300-Blaze_Wolf-20221010131600)

**Example 2:**
> [Yeeno proposes a hook about the game's leaked title.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-Yeeno-20221010212600-Blaze_Wolf-20221010131600)

### `discussion`

**Definition:** A comment shares an opinion, explanation, observation, status
update, or agreement without explicitly requesting input or recommending a
specific action.

**Example 1:**
> [Blaze Wolf explains why there is little criticism available yet.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-Blaze_Wolf-20221010144500-PerryPerryD-20221010144300)

**Example 2:**
> [Masem explains the convention for including game engines in infoboxes.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-Masem-20221012180400-ProtoDrake-20221012164200)

### The hardest boundary

**Which two labels:** `question` and `suggestion`

**Borderline comment:** [ProtoDrake raises a sourcing issue and asks whether a
guideline applies.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-ProtoDrake-20221012164200-Respiciens-20221012103500)

**The decision rule:** If a comment explicitly asks others for information,
help, or input, label it `question` even if it also mentions a possible action;
otherwise, use `suggestion` for a specific recommended action and `discussion`
for the remaining comments. A rhetorical question that does not seek an answer
does not by itself make a comment a `question`.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Individual comments on [Wikipedia's WikiProject
Video games discussion archive 165](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165),
collected using the MediaWiki API. The CSV's `note` field retains each comment's
permalink (which identifies its author) and the source page revision. Wikipedia
text is licensed under [CC BY-SA](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use);
preserve attribution and check the applicable license requirements when sharing.
This initial sample comes from one archive and includes related replies, so it
may not represent the whole community and random splitting may place comments
from the same discussion in different splits.

**How I labelled them:** The first 20 comments were labeled independently and
marked `cold`. The remaining 180 were pre-labeled by AI and are marked
`AI pre-labeled; human review pending`; each must still be read and corrected by
the student. Source information is retained in each note.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `question` | 47 | 23.5% |
| `suggestion` | 55 | 27.5% |
| `discussion` | 98 | 49% |
| **Total** | 200 | 100% |

These counts include 180 provisional AI labels and will change if labels are
corrected. The AI flagged several automated announcements and a formatting
artifact in their notes for special review.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

These are AI-proposed hard cases and choices; they still need my review.

**1.** [ProtoDrake discusses sourcing and asks about the guideline.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-ProtoDrake-20221012164200-Respiciens-20221012103500)

Could be `question` or `discussion`. The provisional choice is `question`:
although the comment explains a concern, it also explicitly asks for clarification.

**2.** [Blaze Wolf describes an unsourced Pokémon gameplay article and says they are unsure what to do.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-Blaze_Wolf-20221013154000-Gameplay_of_Pokémon)

Could be `question` or `discussion`. The provisional choice is `discussion`:
the comment expresses uncertainty but does not explicitly ask for help.

**3.** [Alyo discusses whether the gameplay article has enough sourcing and proposes a different focus.](https://en.wikipedia.org/wiki/Wikipedia_talk:WikiProject_Video_games/Archive_165#c-Alyo-20221013155100-Blaze_Wolf-20221013154000)

Could be `suggestion` or `discussion`. The provisional choice is `suggestion`:
the comment proposes focusing more on how gameplay has evolved, not just an assessment of the article.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`.

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, maximum 128 tokens,
seed 42; CPU, PyTorch 2.14.1+cu130.

**Anything I changed from the defaults, and why:** The label map and input file
were set in memory to the Wikipedia labels and `labels.csv`; the notebook itself
was not changed. Training hyperparameters were unchanged. Results were written
to a separate run directory to preserve existing root-level results.

**Split sizes:** 139 train / 31 validation / 30 test. The test set has 7
`question`, 8 `suggestion`, and 15 `discussion` examples. With only 7 test
questions, that label's metrics can vary substantially between splits. This run
is provisional because human review of 180 AI-labeled comments is pending.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Small edits and guidance to the Jupyter notebook and how to set up the kernel properly.
- *What came back:* The requested changes.
- *What I changed:* I was able to adapt the notebook and set up the project to use my dataset.

**Moment 2**

- *What I asked for:* Help collecting and labeling 200 comments and documenting
  difficult label boundaries.
- *What came back:* AI created a collector and source-linked dataset,
  pre-labeled 180 comments, and proposed three hard cases with explanations.
- *What I changed:* I independently labeled the first 20 comments and kept the
  AI-generated labels and hard-case choices marked as provisional. I still need
  to read and correct every pre-labeled comment myself.

**Pre-labelling disclosure:** 180 rows were pre-labeled by AI after the first
20 were labeled cold. Human review and correction of the pre-labels is pending.

**Moment 3**

- *What I asked for:* Help compare the three-seed results, interpret the confusion matrix and staff agreement, and test one improvement.
- *What came back:* Codex independently labeled the staff set, ran the agreement script, identified the model's repeated `discussion` predictions, and suggested testing more epochs. It ran the six-epoch comparison and drafted the run logs and diagnoses.
- *What I changed:* The notebook now uses six epochs and an `after` run label, and this report records the resulting scores and their limits. The staff comparison does not check consistency under my project taxonomy, and the 180 AI-pre-labeled rows still need human review.


<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

|Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.600 | 0.500 | -0.100 |
| Macro F1 | 0.552 | 0.222 | -0.330 |
| F1 — `discussion` | 0.667 | 0.667 | +0.000 |
| F1 — `question` | 0.625 | 0.000 | -0.625 |
| F1 — `suggestion` | 0.364 | 0.000 | -0.364 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

     I predict that it will be better at classifying 'discussion' posts due to the density of their labels within the dataset.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

     Looks like there was no difference in the 'discussion' posts, the fine tuning didn't seem to do to much! I will try to change this to ensure better accuracy.

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. `question` recall | ≥ 0.70 | 0.000 | 0.000 | 0.000 | MISSED |
| 2. Overall accuracy | ≥ 0.75 | 0.500 | 0.500 | 0.500 | MISSED |
| 3. Human annotator agreement | ≥ 0.60 on 20 shared posts using project taxonomy | Not measured | Not measured | Not measured | NOT MEASURED |
| 4. `discussion` F1 | ≥ 0.50 | 0.667 | 0.667 | 0.667 | MET |
| 5. `suggestion` precision | ≥ 0.50 | 0.000 | 0.000 | 0.000 | MISSED |


| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.500 | 0.500 | 0.000 |
| Macro F1 | 0.222 | 0.222 | 0.222 | 0.000 |
| F1 — `question` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `suggestion` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `discussion` | 0.667 | 0.667 | 0.667 | 0.000 |

Accuracy spread was **0 percentage points**, below threshold. The identical scores reflect the model choosing `discussion` for every test post, so stable accuracy here does not mean it distinguishes the labels well.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | question | suggestion | discussion |
|---|---|---|---|
| **question** | 0 | 0 | 7 |
| **suggestion** | 0 | 0 | 8 |
| **discussion** | 0 | 0 | 15 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

In seed 42, 8 real `suggestion` posts were called `discussion`, while no real `discussion` posts were called `suggestion`; the model favored `discussion` rather than recognizing suggested actions.


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | `question` recall | ≥ 0.70 | MISSED | 0.000 in all three seeds. |
| 2 | Overall accuracy | ≥ 0.75 | MISSED | 0.500 in all three seeds. |
| 3 | Human agreement using project taxonomy | ≥ 0.60 on 20 shared posts | NOT MEASURED | The staff comparison uses a different taxonomy and set, so it cannot establish this criterion. |
| 4 | `discussion` F1 | ≥ 0.50 | MET | 0.667 in all three seeds. |
| 5 | `suggestion` precision | ≥ 0.50 | MISSED | Reported as 0.000 in all three seeds; no suggestions were predicted. |

**Diagnoses**

1. **Question recall:** All seven test questions were predicted as `discussion` in every seed. The model collapsed to the majority label; limited training examples and the 15-of-30 test-set prevalence of `discussion` are plausible contributors, but the results do not establish the cause. The baseline identified 5 of the 7 questions, so the text is not demonstrably beyond reach. The staff agreement was 30/30, with no evidence of inconsistent application on that separate taxonomy; it cannot verify the question/discussion boundary in the project labels.

2. **Overall accuracy:** The same collapse explains the 0.500 accuracy: all 15 `discussion` examples were correct and all 15 other examples were wrong in each seed. The baseline achieved 0.600 on the same test set, suggesting the fine-tuned model did not learn useful distinctions in this run. Whether limited examples, training behavior, or unreviewed labels caused that collapse remains unresolved. The staff agreement gives no observed labeling inconsistency on its own set, but does not validate the project's 180 AI-pre-labeled rows, whose human review is pending.

3. **Human agreement:** This criterion has not been tested. The 30/30 staff result used staff definitions on running posts, not two annotators independently applying this project's taxonomy to 20 shared posts. A separate project-taxonomy comparison is needed; neither MET nor MISSED is supported yet.

5. **Suggestion precision:** The model made zero `suggestion` predictions in every seed, so precision has a zero denominator and the evaluation reports 0.000. It cannot produce a useful suggestion list under this run. All eight test suggestions went to `discussion`; too few examples or difficulty distinguishing suggestions from discussion are plausible, but the matrix alone cannot distinguish these causes. The staff agreement found no disagreements, and its different labels do not test this boundary; review of the project labels and a targeted agreement check are needed before attributing the failure to labeling.


---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

**Disagreements:** None. Piles (a), (b), and (c) are all empty.

**What this result tells me:** The set was relatively easy to separate in this pass: the first ten posts use specific evidence to support an argument, the next ten make general claims without load-bearing support, and the last ten report immediate personal experiences. Some individual boundaries could still invite different readings, so one perfect comparison on 30 posts does not establish that the rules are universally tight or that my own 200 labels are consistent. It shows no observed inconsistency against staff on this set.


---

## The Improvement

**What I changed:** Increased training from 3 to 6 epochs. All other settings, the 200 rows, split procedure, and seeds 42, 7, and 2024 stayed the same. Section 6 uses `RUN_LABEL = "after"`; results are in `results_three_seeds_after.json`.

**Which diagnosis pointed at it:** The before model predicted `discussion` for every test example across all seeds, while the baseline identified 5 of 7 questions on seed 42. That suggested the fine-tuning might not have learned the distinctions; extra epochs test that possibility, without establishing why the collapse occurred.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. `question` recall | ≥ 0.70 | 0.000 | 0.000 | 0.000 | MISSED |
| 2. Overall accuracy | ≥ 0.75 | 0.500 | 0.500 | 0.533 | MISSED |
| 3. Human annotator agreement | ≥ 0.60 on 20 shared posts using project taxonomy | Not measured | Not measured | Not measured | NOT MEASURED |
| 4. `discussion` F1 | ≥ 0.50 | 0.667 | 0.667 | 0.682 | MET |
| 5. `suggestion` precision | ≥ 0.50 | 0.000 | 0.000 | 1.000 | MISSED |

For seeds 42 and 7, no `suggestion` predictions were made: precision is undefined and the evaluation reports it as zero. Seed 2024's precision of 1.000 reflects only one prediction, which was correct. Human agreement is independent of training seeds and the staff-taxonomy check does not measure the project-taxonomy criterion.

**Did it help, and how do I know:** Only slightly on seed 2024: accuracy rose from 0.500 to 0.533 and one of eight suggestions was identified, raising suggestion F1 from 0.000 to 0.222. Seeds 42 and 7 still predicted `discussion` for every post, and all seven questions were missed in every seed. No criterion changed verdict, so six epochs did not produce a reliable improvement across trials. The 30-post test splits and pending human review of 180 pre-labeled examples limit the conclusion.


---

## What's Still Broken

- **Question recall (criterion 1):** Review the question labels and the question/discussion boundary, then add carefully labeled examples if needed and evaluate a new run. All seven test questions were missed in every after seed. I stopped after the single planned epoch change so another data change would not be mixed into its measurement; human review of the pre-labeled data remains pending.
- **Overall accuracy (criterion 2):** Investigate the persistent majority-label predictions, including the training data and label quality, before testing another targeted change. Accuracy is 0.500, 0.500, and 0.533 against a 0.75 target. I stopped after the controlled rerun because the cause is not established and another intervention would need its own comparison.
- **Suggestion precision (criterion 5):** Review actionable-suggestion examples and their boundary with discussion, then add carefully labeled examples or test a targeted training change. Two seeds made no suggestion predictions and the third made only one correct prediction. I stopped after the one change; a single prediction is too little evidence of a reliable solution.
- **Human agreement (criterion 3):** This is unmeasured, rather than a demonstrated miss. Have two people independently label at least 20 shared project comments using the project taxonomy and compare them; the 30/30 staff check used different definitions. I stopped because that independent second annotation has not been supplied.

**The gap between what I meant my labels to capture and what the model learned:**

The labels were meant to distinguish requests for input, actionable proposals, and other discussion. After six epochs, the confusion matrices show every question sent to `discussion` in all seeds and all but one suggestion sent there, so the model mostly learned to predict the broad discussion category.


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
