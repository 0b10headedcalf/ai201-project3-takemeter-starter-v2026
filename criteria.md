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

## 1.

<!-- Your criterion. It must name a number. -->

On the held-out test set, the classifier must correctly identify at least 70%
of comments whose human-assigned label is `question` (recall ≥ 0.70).

**Why this target:**

I chose 70% because questions identify editors seeking input, and I want the
classifier to find most of those comments.


---

## 2.

<!-- Your criterion. -->

Overall accuracy on the held-out posts must be at least 75%.


**Why this target:**

I chose 75% because the classifier should correctly sort a useful majority of
comments across the three categories, despite overlap between their purposes.


---

## 3.

<!-- Your criterion. -->

Two human annotators must agree on at least 60% of a shared set of 20 posts
that they label independently using this project's taxonomy.


**Why this target:**

I chose 60% as a minimum level of agreement because a comment can both suggest
an edit and ask for input; human agreement checks whether the written boundary
rule can be applied consistently.


---

## 4.

<!-- Your criterion. -->

The `discussion` label must have an F1 score of at least 0.50 on the held-out test
set.

**Why this target:**

I chose 0.50 because discussion comments can be short replies that depend on
the surrounding thread, which the text-only classifier does not receive, but I
still want the model to identify them with some reliability.


---

## 5.

<!-- Your criterion. -->

Of the held-out comments the classifier predicts as `suggestion`, at least 50%
must actually have the human-assigned label `suggestion` (precision ≥ 0.50).

**Why this target:**

I chose 50% because a list of predicted suggestions should contain genuine
actionable proposals often enough to help editors find potential improvements.


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
