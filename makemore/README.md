## makemore

Character-level MLP name generator. 2-layer feedforward net over a fixed 5-char context window. Predicts next character (or `.`) and samples names autoregressively.

| Area         | Details                                                                 |
|--------------|-------------------------------------------------------------------------|
| Task         | Next-char prediction on names.txt (27 classes: a–z + `.`)              |
| Context      | `block_size=5` — fixed window, no recurrence                            |
| Architecture | Embed(30) → flatten → Linear(150→200) → tanh → Linear(200→27)          |
| Scale        | ~36k parameters                                                        |
| Data         | 80/10/10 split, shuffle seed 42                                        |
| Training     | 200k SGD steps, batch 128, linear LR decay 0.1 → 1e-8                  |
| Loss         | Cross-entropy ≈ 2.0 train / 2.1 test                                   |
| Inference    | Softmax sampling, terminate on `.`                                     |

**Scope**: Captures local letter patterns and name-like structure. No long-range modeling (hard 5-char limit). Useful baseline, not SOTA generation.