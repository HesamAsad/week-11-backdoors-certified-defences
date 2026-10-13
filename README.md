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

## Changes to the notebook code

The page shows the workshop notebook with small fixes, and every number on it comes from a CPU run of that code:

- cell 12 previews the trigger actually used (3 × 3, bottom left);
- cell 15 reuses `./data/MNIST/raw` instead of downloading a second copy;
- cell 21 removes the true zeros from the triggered test set, so the ASR counts only digits the trigger changes;
- cell 22 prints CA and ASR (98.5% and 98.9% after 5 epochs);
- cell 25 uses `max_iter` for t-SNE;
- cells 27–29 compute the k-NN distance score over all 60 000 training images (AUC 0.83);
- cell 30 raises a `ValueError`;
- cells 32–33 score 1200 poisoned and 3000 clean images with cognitive distillation (AUC 0.91, large masks flag poisons).

Keys: `]` / `[` next and previous section, `P` present mode, `L` light/dark.
