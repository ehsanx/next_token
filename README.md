# next_token[ ] — LLM Prediction Game

An interactive, offline browser game that teaches how large language models work as **next-token predictors**. Players guess the most probable next word in a sentence, then see the model's actual probability distribution, Shannon entropy, and perplexity — with a collapsible statistical deep dive for each round.

Built for classroom use at the **University of British Columbia** as a demonstration tool for students learning about AI/LLM mechanics.

🔗 **[Play it here →](https://ehsanx.github.io/next_token)**

---

## What it teaches

Language models do not "understand" text. They assign a probability distribution over all possible next tokens given the preceding context:

> P(token | context) — learned from statistical regularities in large text corpora

This game makes that abstract idea tangible: students step into the model's shoes, and then see exactly where their human intuitions agree or diverge from corpus statistics.

---

## Features

### Six audience-specific tabs
Each tab contains 5 rounds tailored to a specific domain. Players choose the tab closest to their expertise:

| Tab | Domain focus |
|-----|-------------|
| 🔬 Epidemiologists | Hazard ratios, PAF, confounding, DAGs, recall bias |
| 🏥 Public Health | WHO definitions, herd immunity, SDH, prevention levels |
| 📐 Statisticians | Bayes' theorem, p-values, OLS, Bonferroni, bias-variance |
| 🐍 Python | `import pandas as pd`, sklearn idioms, matplotlib conventions |
| 📊 R | tidyverse pipelines, ggplot2 layers, `set.seed()`, `lm()` |
| 💬 Daily Life | Health googling, weather idioms, SMS templates, news language |

### Probability visualisation
After each guess, the interface shows:
- Animated probability bars for the top 5 predicted tokens
- Colour-coded score badge (100 / 80 / 60 / 40 / 20 pts by rank)
- Shannon entropy H (bits) and perplexity 2^H for the distribution
- Distribution shape descriptor (e.g., "Highly peaked — near-deterministic")

### Statistical deep dive (collapsible)
Each round includes an optional expanded panel showing:
- **Entropy decomposition table**: token | p(t) | −log₂ p(t) | contribution p·(−log₂ p)
- **Total H and perplexity** with interpretation
- **Temperature preview**: side-by-side distributions at T = 0.3 (focused) and T = 2.0 (diffuse)
- **Domain explanation**: a paragraph explaining the linguistic, statistical, or corpus-level reason behind the distribution

### Live temperature slider
A draggable slider reshapes the probability distribution in real time using softmax temperature scaling, updating entropy and all bar widths live. Demonstrates the tradeoff between determinism (T → 0) and creativity (T → ∞) without any API calls.

---

## Technical details

| Property | Value |
|----------|-------|
| Dependencies | None — zero npm, zero CDN |
| API calls | None — all distributions are pre-baked |
| File size | Single `.html` file, ~60 KB |
| Browser support | Any modern browser (Chrome, Firefox, Safari, Edge) |
| Internet required | Only for Google Fonts (optional; degrades gracefully) |
| Offline use | Yes — download and open locally |

The probability distributions are carefully researched approximations of what major LLMs (GPT-4, Claude, Llama) actually assign in these contexts, based on published analyses and direct observation. They are not fetched live.

---

## How scoring works

| Rank match | Points |
|-----------|--------|
| Your word = model's #1 prediction | 100 |
| Your word = model's #2 prediction | 80 |
| Your word = model's #3 prediction | 60 |
| Your word = model's #4 prediction | 40 |
| Your word = model's #5 prediction | 20 |
| Your word not in top 5 | 0 |

Partial matching is applied (prefix/suffix overlap). Maximum score per tab: **500 points**.

---

## Deploying to GitHub Pages

### First-time setup

1. Create a new repository (public) on GitHub
2. Clone it locally:
   ```bash
   git clone https://github.com/ehsanx/next_token.git
   cd next_token
   ```
3. Copy `next-token-tabbed.html` into the repo root. Optionally rename it `index.html` so the game loads at the root URL rather than requiring `/next-token-tabbed.html` in the path:
   ```bash
   cp /path/to/next-token-tabbed.html index.html
   ```
4. Add, commit, and push:
   ```bash
   git add .
   git commit -m "Initial deploy"
   git push origin main
   ```
5. In your repository on GitHub, go to **Settings → Pages → Source**, select **Deploy from a branch**, choose `main` and `/ (root)`, and click **Save**.
6. Wait ~60 seconds. Your site will be live at:
   ```
   ehsanx/next_token
   ```

### Updating

Just edit the HTML file, commit, and push — GitHub Pages redeploys automatically within ~30 seconds.

```bash
git add index.html
git commit -m "Update round content"
git push
```

---

## Repo structure

A minimal repo only needs two files:

```
next_token/
├── index.html        ← the game (renamed from next-token-tabbed.html)
└── README.md         ← this file
```

---

## Classroom use

### Suggested workflow

1. **Share the URL** with students before a lecture on LLMs or AI in health/statistics
2. Ask them to **play 1–2 tabs** relevant to their background (5–8 minutes)
3. Use their results as discussion prompts:
   - *Which rounds were easiest? Why?* (low entropy = constrained domain vocabulary)
   - *Where did you diverge from the model?* (high entropy = flat distribution = genuinely ambiguous)
   - *What does it mean that `import numpy as` is nearly deterministic?* (code = near-zero entropy language)
   - *Why does the WHO definition have ~88% probability on one word?* (memorisation vs. generation)
4. Use the **temperature slider live** on a projected screen to show how the same distribution changes shape
5. Open the **deep dive panel** for 1–2 rounds to walk through the entropy decomposition

### Key concepts illustrated

- Next-token prediction as the fundamental LLM operation
- Shannon entropy H as a measure of predictability
- Perplexity 2^H as "effective vocabulary size" at a given position
- Temperature as a control on distribution shape (not knowledge)
- Corpus bias: how training data choices propagate into model behaviour
- Domain-specific sub-distributions (epidemiology corpora ≠ Python corpora)
- Memorisation vs. generalisation (canonical phrases vs. open-ended completions)
- Publication bias encoded in token probabilities (health journalism example)

---

## Customisation

The probability distributions and round content live in a single JavaScript object `ALL_ROUNDS` near the top of the HTML file. Each round follows this structure:

```javascript
{
  prompt:  "The sentence stem shown to the player",
  tokens:  [
    { word: "most_likely",   p: 55 },
    { word: "second_likely", p: 22 },
    { word: "third",         p: 12 },
    { word: "fourth",        p:  7 },
    { word: "fifth",         p:  4 },
  ],
  insight: "Short explanation shown after every reveal (HTML allowed)",
  context: "Long domain explanation shown in the deep dive panel (HTML allowed)"
}
```

Probabilities are in percentage points and do not need to sum to exactly 100 (the temperature function re-normalises them). To add a new tab, extend the `TABS` array and add a corresponding key to `ALL_ROUNDS`.

---

## Citation

If you use this tool in a course, workshop, or publication, please cite:

> Karim MR. *next_token[ ] — LLM Next-Token Prediction Game*. 2025. Available at: https://ehsanx.github.io/next_token

---

## Licence

MIT — free to use, adapt, and redistribute with attribution. See `LICENSE` for details.

---

*Built with vanilla HTML/CSS/JS. No frameworks. No tracking. No data collection.*
