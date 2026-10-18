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

## 1. Overall accuracy

The classifier reaches at least **0.65 overall accuracy** on the held-out test set.

**Why this target:** Three labels means a random guesser hits about 0.33. A target of 0.65 is roughly double that — enough to say the model learned something real rather than learning to guess the most common label. It’s ambitious without being delusional for a small fine-tuned model on ~200 examples.

---

## 2. Per-label F1 — analysis

The F1 score for the `analysis` label is at least **0.55** on the held-out test set.

**Why this target:** `analysis` is the hardest label to collect (genuine craft discussion is rarer on r/popheads than hype or gossip), the hardest to apply consistently (the line between citing a specific fact and just sounding specific is narrow), and the hardest to distinguish from `hot_take`. A lower floor than the other two labels is honest. I also expect `analysis` to be my smallest label by count, which means about 8 examples in the test split — a jumpy number that could move a lot between seeds, so 0.55 needs to hold across all three.

---

## 3. Class balance

No single label exceeds **65%** of the rows in `labels.csv`.

**Why this target:** The assignment’s hard cap is 70%. I’m targeting 65% to build in a buffer — if my collection drifts to 66%, I want to catch and fix it before hitting the cap where the model just learns to guess the dominant label. A model trained on a heavily skewed dataset isn’t learning three distinctions; it’s learning one.

---

## 4. Per-label F1 — reaction

The F1 score for the `reaction` label is at least **0.70** on the held-out test set.

**Why this target:** `reaction` has the most distinctive surface features of the three labels — event nouns (tour, collab, chart, #1), all-caps excitement, strings of exclamation marks. A model should find this pattern reliably from relatively few examples. Setting a higher floor here than for `analysis` reflects that the labels are genuinely unequal in difficulty. This target also needs to hold across all three seeds — if it hits 0.72, 0.68, 0.71, that’s a miss on seed 7.

---

## 5. Macro F1

The macro F1 across all three labels is at least **0.60** on the held-out test set.

**Why this target:** Macro F1 averages across labels equally regardless of count — so a model that nails `reaction` and ignores `analysis` won’t pass this criterion even if its overall accuracy looks fine. It’s a check that the model learned all three distinctions rather than just the easiest one. 0.60 is the floor I’d need to call this classifier genuinely useful rather than a demo. Like criterion 1, this must hold across all three seeds.

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
