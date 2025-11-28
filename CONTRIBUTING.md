# Contributing to LLM-Trace
**Version:** 1.0 (Strict Enforcement)

Welcome to the development team for **LLM-Trace**.

This project is not a standard web application; it is a **systems-oriented interpretability tool** designed to inspect the internal computation paths of transformer-based language models.

Because this tool is intended for academic analysis and engineering diagnostics, **correctness, reproducibility, and clarity are non-negotiable.**

This document serves as the definitive manual for contributing. **Read it fully.** Ignorance of these guidelines will result in PR closure.

---

## 1. Core Principles

Before writing code, understand the ethos:

*   **Observability > Aesthetics:** The frontend is an inspection console (like Chrome DevTools), not a marketing site. Precision matters more than flair.
*   **Causality > Correlation:** Backend metrics must be rigorous. If a circuit is detected, it must be validated via ablation, not just visualization.
*   **Atomic Development:** Massive, unstructured code dumps make debugging impossible. We work in small, verifiable units.

---

## 2. Environment & Setup

### A. Prerequisites
*   **Python 3.10+**: Required for all model hooking and backend analysis.
*   **Node.js 18+**: Required for the frontend dashboard.
*   **PyTorch**: (Specific version defined in `requirements.txt`).
*   **Git**: Must be configured with your real name and email.

### B. Repository Structure
```text
YET TO BE FRAMED
```

---

## 3. The Workflow: From Idea to Merge

We follow a strict **Feature Branch Workflow**. Direct pushes to `main` are protected and will fail.

### Step 1: Issue Selection
1.  Find an issue on the Board.
2.  Assign yourself to the issue so others know you are working on it.
3.  **If no issue exists:** Create one describing the bug or feature proposal.

### Step 2: Branching
Create a branch from `main`. Use the strict naming convention:

| Prefix | Use Case | Example |
| :--- | :--- | :--- |
| `feat` | New capabilities | `feat/residual-stream-hook` |
| `fix` | Bug fixes | `fix/attention-mask-off-by-one` |
| `refactor` | Code cleanup (no logic change) | `refactor/decompose-heatmap-component` |
| `docs` | Documentation only | `docs/update-contributing-guide` |
| `perf` | Performance optimization | `perf/optimize-tensor-transfer` |

**Command:**
```bash
git checkout main
git pull origin main
git checkout -b feat/your-feature-name
```

### Step 3: Development & Atomic Commits
**The "1K Lines" Rule:**
*   **Do not** work for a week and push one massive commit.
*   **Do** commit often (e.g., every time you finish a function or component).
*   **Rule:** If a commit exceeds **500 lines of code** (excluding auto-generated lockfiles), it triggers a mandatory manual review by the Project Lead.

### Step 4: Pull Request (PR)
1.  Push your branch: `git push origin feat/your-feature-name`
2.  Open a PR against `main`.
3.  **Fill out the PR Template.** If you delete the template sections, your PR will be closed.

---

## 4. Coding Standards & Integrity

### A. The AI & LLM Policy (Strict)
We use AI tools (ChatGPT, Copilot, Claude), but strictly governed:

1.  **The "Verification" Mandate:** You are personally responsible for every line of code. If an AI writes a bug, **you wrote the bug**.
2.  **No Hallucinated APIs:** AI often invents PyTorch hooks or React props that don't exist. Verify documentation before pasting.
3.  **Comment Intent, Not Syntax:**
    *   **Bad:** `// Loop through the array` (AI generated fluff).
    *   **Good:** `// We iterate in reverse to preserve causal ordering during ablation.`
4.  **Prohibited:** Copy-pasting entire files from an LLM. This leads to "spaghetti code" and bloat.

### B. Backend Standards (Python/PyTorch)
*   **Type Hinting:** All function signatures must have type hints.
    ```python
    # Required
    def extract_activations(model: HookedTransformer, layer: int) -> torch.Tensor:
    ```
*   **Tensor Shapes:** Comments must indicate expected tensor shapes for complex operations.
    ```python
    # [batch, seq_len, d_model]
    ```
*   **Device Management:** Never hardcode `.cuda()`. Use device variables to support MPS/CPU for development.

### C. Frontend Standards (React/Viz)
*   **Professional UI:** No "Lorem Ipsum". Use realistic mock data (tokens, attention scores). No broken layouts on window resize.
*   **Performance:**
    *   Attention matrices can be large ($N \times N$). Use HTML5 Canvas or WebGL for rendering if DOM elements lag.
    *   Memoize heavy calculations (`useMemo`) to prevent freezing the dashboard during interaction.

---

## 5. Commit Message Protocol

We use **Conventional Commits** to automate our changelogs.

**Format:**
```text
<type>(<scope>): <short summary>

[Optional: Detailed description of the problem and solution]

[Optional: Footer with breaking changes or issue links]
```

**Scope Dictionary:**
*   `(hook)`: Internal model hooking logic.
*   `(viz)`: Visualization components.
*   `(api)`: Communication between frontend and backend.
*   `(core)`: Core system architecture.

**Examples:**
*   `feat(hook): add support for key/query extraction in layer 4`
*   `fix(viz): correct color interpolation in attention heatmap`
*   `perf(core): batch tensor transfers to reduce latency`
*   `refactor(api): standardize JSON response format for circuit data`

---

## 6. Testing & Quality Assurance

"Improperly Tested Code" will be rejected.

### A. Backend Testing
*   **Unit Tests:** Every hook must verify output shape and data type.
*   **Sanity Checks:** If you implement a new metric, run it on a known prompt (e.g., "The cat sat on the mat") and verify it aligns with manual calculation.
*   **Ablation Tests:** If you add an ablation feature, ensure it actually modifies the model output.

### B. Frontend Testing
*   **Edge Cases:** Test your UI with:
    *   Empty prompts.
    *   Prompts > 2048 tokens.
    *   Models with 0 attention (flat distribution).
*   **Cross-Browser:** Verify layout on Chrome and Firefox.

---

## 7. Reviewer Guide

**Who Reviews What?**
*   **Project Lead (Ashwin):** Reviews architectural changes, circuit definitions, and safety logic.
*   **Model Engineer:** Reviews PyTorch code, hooks, and efficient activation extraction.
*   **Signal Engineer:** Reviews metric calculations, threshold logic, and statistical validity.
*   **Frontend Engineer:** Reviews UI components, UX flows, and visualization performance.

**Review Checklist:**
- [ ] Does the code match the ticket requirements?
- [ ] Are there any "mass commits" hiding bad code?
- [ ] Are complex algorithms explained in comments?
- [ ] Did the CI/CD pipeline pass?
- [ ] **Security:** Are we exposing raw model internals safely?

---

## 8. Final Checklist for Contributors

Before clicking "Create Pull Request":

- [ ] Did I run the linter? (Formatters must be green).
- [ ] Did I remove all `print()` debugging statements?
- [ ] Is my commit history clean? (Squash "wip" commits if necessary).
- [ ] Did I add tests for the new feature?

**Code with rigor. We are building a tool to understand intelligence, not just a web app.**
