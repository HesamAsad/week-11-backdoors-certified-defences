# COMP90073 Workshop Week 11: Threat Models, Certified Defences and Backdoor Attacks

An interactive workbook for the Week 11 tutorial. It is grounded in COMP90073 lecture 19 (training-time attacks: poisoning and backdoors), lecture 20 (defences against test-time attacks: adversarial training, IBP, randomised smoothing, preprocessing, guardrails), lecture 21 (poisoning defences and the public good) and the Week 11 tutorial answers and notebook.

Live site: [hesamasad.github.io/week-11-backdoors-certified-defences](https://hesamasad.github.io/week-11-backdoors-certified-defences/). You can also open [`index.html`](index.html) locally in a browser. The only external requests are Google Fonts, KaTeX and highlight.js. A Light / Dark / Auto switch sits in the sidebar.

Each question starts with the prerequisites it needs, explained inline with a figure you can play with. A simpler warm-up exercise with self-checking answer boxes follows, then a live version of the question, then the official answer as a step-by-step reveal with cheat notes.

## What's inside

- **Prologue (A1–A8).** The three lectures in order, with live figures:
  - a logistic regression you can poison by label flipping or clean-label moves;
  - interval bound propagation through slide 17's network;
  - partition-and-vote poisoning bounds;
  - a text-watermark z-test.
- **Q1–Q3.** Threat models, randomised smoothing and adversarial CAPTCHAs (details in the table below).
- **Q4, the notebook.** Every code cell verbatim, with Copy buttons and the notebook's saved plots. We re-ran it offline with the same seeds; clean accuracy is 98.5% and ASR 98.9% (the notebook reports 98.6% and 98.8%). The trained LeNet is exported, and its JavaScript forward pass matches PyTorch to 4 × 10⁻⁶. On the page you can:
  - draw or pick a digit and move the trigger;
  - see ASR by trigger position, size and intensity;
  - explore the t-SNE of the features;
  - compare SLOF with k-NN scores;
  - view the cognitive-distillation masks and ROC curves.
- **Wrap-up.** A summary of key results and 24 references, each checked on dblp or the publisher's page.

## Question-by-question sequence

| Question | Prerequisites (inline) | Interactive check |
|---|---|---|
| 1 · White-box versus black-box threat models | components of a threat model; query-based gradient estimates and transfer | one evasion attack under white-box, black-box (query budget) and transfer access, with success rates over 53 start points |
| 2 · Limits of randomised smoothing; an operational system | the smoothed classifier; \(R = \sigma\Phi^{-1}(p_A)\) and its one-dimensional derivation; Hoeffding margin and abstention; \(\ell_2\) versus \(\ell_\infty\) balls | smoothing a curved classifier with a draggable input; radius against vote share; routing inputs to review by certified radius |
| 3 · Adversarial CAPTCHAs | transferability; \(P_{\text{solve}} = p^k\) | digit CAPTCHAs attacked against a LeNet solver and read by an unseen MLP solver (PyTorch, precomputed) |
| 4 · Backdoor notebook | correct ASR; k-distance and SLOF; cognitive distillation | the backdoored LeNet live in the browser, plus the re-run's curves, t-SNE, ROC curves and masks |

## Warm-up exercises

| Before | Warm-up | Source |
|---|---|---|
| Q1 | query cost of a finite-difference gradient on MNIST (1568 per gradient; 15 680 for 10 PGD steps) | lecture 18; written for this workbook |
| Q2 | Hoeffding margin \(t \approx 0.0059\) and \(R \approx 0.312\); the safe guess with \(n = 100\) (0.714); \(\epsilon\sqrt d = 2.8\) | lecture 20, slides 22–25 |
| Q3 | CAPTCHA solve rates \(0.98^4 \approx 0.922\) and \(0.6^4 \approx 0.13\) | lecture 21, slide 30 |
| Q4 | 1200 poisoned images; the spurious ASR of a clean model (0.098); corrected ASR (0.9863) | notebook cells 16 and 22 |

## Notes for the tutorial

- **The ASR counts true zeros as successes.** Cell 21 relabels every triggered test image as 0, including the 980 real zeros, so the epoch-1 "ASR" of 10.39% is mostly those zeros (corrected: under 1%).
- **Cell 12 previews a different trigger** (5 × 5, bottom right) from the one used (3 × 3, bottom left). The learned backdoor is position-specific: 99.7% at the trained corner, 2.3% on average elsewhere.
- **`slof` is a k-NN mean distance, not the lecture's SLOF ratio.** Its docstring lists arguments that don't exist, and cell 28 scores within unshuffled batches of 256. Re-run AUCs:
  - 0.838 per batch;
  - 0.831 over all samples;
  - 0.617 for the true SLOF ratio.
- **Cognitive distillation's score direction is reversed relative to lecture 21.** With `gamma=0.05`, clean masks collapse while poisoned masks keep the trigger (mean L1 norm 18.1 vs 3.6). Flagging large masks gives AUC 0.915; the lecture's small-mask rule gives 0.085.
- **The backdoor is learned in epoch 2.** In epoch 1, poisoned samples keep a high loss. Their loss falls below the clean loss only from epoch 2 on, which matters for anti-backdoor learning.
- **Cell 30's error check is broken.** `raise('images should be normalized')` raises a `TypeError` instead.
- **Subclassing MNIST re-downloads the dataset** into `./data/InMemoryPoisonedMNIST/raw`.
- **Small issues:**
  - `batch_size = 128` is unused;
  - t-SNE's `n_iter` is now `max_iter`;
  - the t-SNE compares different clean and poisoned images rather than matched pairs;
  - cell 31 was never executed;
  - cell 34 ends with leftover chat-assistant text.

Keys: `]` / `[` next and previous section, `P` present mode, `L` light/dark.
