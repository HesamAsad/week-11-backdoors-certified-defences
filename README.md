# COMP90073 Workshop Week 11: Threat Models, Certified Defences and Backdoor Attacks

An interactive workbook for the Week 11 tutorial. It is grounded in COMP90073 lecture 19 (training-time attacks: poisoning and backdoors), lecture 20 (defences against test-time attacks: adversarial training, IBP, randomised smoothing, preprocessing, guardrails), lecture 21 (poisoning defences and the public good) and the Week 11 tutorial answers and notebook.

Live site: [hesamasad.github.io/week-11-backdoors-certified-defences](https://hesamasad.github.io/week-11-backdoors-certified-defences/). You can also open [`index.html`](index.html) locally in a browser. The only external requests are Google Fonts, KaTeX and highlight.js. A Light / Dark / Auto switch sits in the sidebar.

Slide numbers are the printed slide numbers in the lecture footers (lectures 20 and 21 have animation builds, so they differ from PDF page numbers).

Each question starts with the prerequisites it needs, explained inline with a figure you can play with. A simpler warm-up exercise with self-checking answer boxes follows, then a live version of the question, then the official answer as a step-by-step reveal with cheat notes.

## What's inside

The page follows the four tutorial questions in order. Each question brings in the lecture material it needs at the point it is needed; everything else is in "Additional material" at the end.

| Question | What students do |
|---|---|
| Q1 · What can the attacker access? | a threat-model comparison table with the cost of one gradient at each access level; a query-cost exercise; one answer reveal |
| Q2 · What does a certificate guarantee? | one input judged empirically and with a certificate; a guided smoothing experiment (noisy inputs → votes → confidence bound → radius, with base and smoothed boundaries drawn) with four tasks; the ℓ2 versus ℓ∞ comparison; a review-capacity decision with clean inputs, attacks just across the boundary and attacks pushed further |
| Q3 · Can perturbations help people? | read a perturbed CAPTCHA yourself, then reveal what an attacked and an unseen solver read; the unseen solver's estimated CAPTCHA solve rate |
| Q4 · How does a backdoor work? | the same digit with and without the trigger (live LeNet) with presets for the trained trigger, wrong location, smaller and dimmer triggers, next to the ASR measured across 1000 images; training results; side-by-side confusion matrices; two t-SNE views; outlier detection and cognitive distillation results, with extensions |
| Additional material | the lecture recap (lectures 19–21) with optional figures, the notebook set-up cells, implementation notes, a summary table and references |

Every figure is labelled **Live model** (the trained network running in the browser), **Precomputed** (measured offline with PyTorch) or **Illustrative toy model**. Notebook code is collapsed by default and shown verbatim when opened.

Earlier layouts remain available: [`previous.html`](previous.html) (prologue first) and [`text.html`](text.html) (text prologue).

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
| Q2 | Hoeffding margin \(t \approx 0.0059\) and \(R \approx 0.312\); the safe guess with \(n = 100\) (0.714); \(\epsilon\sqrt d = 2.8\) | lecture 20, slide 22 |
| Q3 | CAPTCHA solve rates \(0.98^4 \approx 0.922\) and \(0.6^4 \approx 0.13\) | lecture 21, slide 24 |
| Q4 | 1200 poisoned images; the spurious ASR of a clean model (0.098); corrected ASR (0.9863) | notebook cells 16 and 22 |

## Notes for the tutorial

- **The ASR counts true zeros as successes.** Cell 21 relabels every triggered test image as 0, including the 980 real zeros, so the epoch-1 "ASR" of 10.39% is mostly those zeros (corrected: under 1%).
- **Cell 12 previews a different trigger** (5 × 5, bottom right) from the one used (3 × 3, bottom left). The learned backdoor is position-specific: 99.7% at the trained position, 79% on average within 2 pixels of it, 0.5% further away.
- **`slof` is a k-NN mean distance, not the lecture's SLOF ratio.** Its docstring lists arguments that don't exist, and cell 28 scores within unshuffled batches of 256. Re-run AUCs:
  - 0.838 per batch;
  - 0.831 over all samples;
  - 0.617 for the true SLOF ratio.
- **Cognitive distillation's score direction is reversed relative to lecture 21.** With `gamma=0.05`, most clean masks collapse while poisoned masks keep the trigger (mean L1 norm 18.1 vs 3.6). Flagging large masks gives AUC 0.915; the lecture's small-mask rule gives 0.085.
- **The backdoor is learned in epoch 2.** In epoch 1, poisoned samples keep a high loss. Their loss falls below the clean loss only from epoch 2 on, which matters for anti-backdoor learning.
- **Cell 30's error check is broken.** `raise('images should be normalized')` raises a `TypeError` instead.
- **Subclassing MNIST re-downloads the dataset** into `./data/InMemoryPoisonedMNIST/raw`.
- **Small issues:**
  - `batch_size = 128` is unused;
  - t-SNE's `n_iter` is now `max_iter`;
  - the t-SNE compares different clean and poisoned images rather than matched pairs;
  - cell 31 has no saved output or execution count (probably cleared);
  - cell 34 ends with leftover chat-assistant text.

Keys: `]` / `[` next and previous section, `P` present mode, `L` light/dark.
