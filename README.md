<h1 align="center">
  medical-openjev
  <img src="web/logo/清华大学-logo-1024px.png" alt="Tsinghua University" height="42" align="absmiddle">
  <img src="web/logo/山东大学-logo-1024px.png" alt="Shandong University" height="42" align="absmiddle">
  <img src="web/logo/香港城市大学（东莞）-logo-1024px.png" alt="City University of Hong Kong (Dongguan)" height="42" align="absmiddle">
</h1>

<p align="center">
  <strong>An open, local-first, calibrated medical decision gate for trustworthy AI agents.</strong>
</p>

<p align="center">
  <a href="https://arxiv.org"><strong>arXiv Paper ↗</strong></a>&nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="https://huggingface.co/Qwen/Qwen3.5-4B"><strong>Hugging Face Model ↗</strong></a>
</p>

<p align="center">
  <code>Local deployment · Single GPU</code>&nbsp;&nbsp;
  <code>Research only</code>
</p>

<p align="center">
  <img src="web/assets/medical-openjev-method.svg" alt="medical-openjev evaluates evidence sufficiency and specialty routing to decide whether a medical LLM output should act or escalate" width="900">
</p>

> **medical-openjev** is the safety traffic light for medical AI agents. It does not generate a diagnosis. Instead, it decides whether a model's current answer is sufficiently trustworthy to **act**, or whether the case should be **escalated** for deeper questioning or human review.

## Why medical-openjev?

Medical language models can sound confident even when their answers are unreliable. In our evaluation, ordinary LLM self-assessment reported an average confidence of **0.74** for answers whose actual accuracy was only **0.28**.

medical-openjev is an open-source, locally deployable, rigorously calibrated decision-gating model designed as an auditable alternative to closed safety guardrails such as TypeSafe Jev. It attaches a reliability score to any medical LLM output and converts that score into an operational decision:

```text
Medical LLM response + clinical context
                 │
                 ▼
          medical-openjev
       ┌─────────┴──────────┐
       ▼                    ▼
     ACT              ESCALATE
Use with care    Ask for missing evidence,
                 route to a specialty, or
                 hand off to a clinician
```

## Key capabilities

| Capability | What it provides |
| --- | --- |
| **Confidence gating** | A calibrated trust score and an actionable `act` / `escalate` decision for the output of any medical LLM. |
| **Intelligent triage** | Specialty-aware routing for cases that require a more appropriate pathway. |
| **Evidence-gap prompting** | Real-time identification of the missing clinical evidence needed before a response can be trusted. |
| **High-risk interception** | Detection of confidently wrong outputs before they reach a downstream workflow or user. |
| **Open and auditable evaluation** | Open weights and evaluation assets intended for transparent review and reproducible safety research. |

## Design

The core model combines a **frozen ModernBERT backbone** with two lightweight, pluggable heads:

- **Evidence sufficiency head** — estimates whether the available evidence supports a safe decision.
- **Specialty-routing head** — identifies the appropriate specialty or escalation path.

Training uses reinforcement learning with a strict proper scoring rule, optimizing calibration directly rather than merely optimizing answer quality. The resulting gate is lightweight enough for **millisecond-scale, single-GPU inference** while retaining a transparent, modular architecture.

## Experimental snapshot

The current results are based on **Qwen3.5-4B**. Compared with the base LLM and a random baseline, medical-openjev improves calibration and concentrates accuracy in its most-confident decisions.

| Metric | medical-openjev | Base LLM | Random | Outcome |
| --- | ---: | ---: | ---: | --- |
| ECE ↓ | **0.226** | 0.533 | 0.331 | **58% lower** than the Base LLM |
| Brier score ↓ | **0.241** | 0.523 | 0.345 | Best overall |
| AURC ↓ | **0.597** | 0.666 | 0.746 | Best across all methods |
| Coverage accuracy @ 95% ↑ | **0.281** | 0.246 | 0.263 | Best overall |
| Coverage @ 95% precision ↑ | **0.067** | 0.000 | 0.000 | Achieved only by medical-openjev |
| Accuracy of top-confidence 10% ↑ | **0.667** | 0.333 | 0.167 | **2×** the Base LLM |

<p align="center">
  <sub>↓ lower is better &nbsp;·&nbsp; ↑ higher is better</sub>
</p>

These results indicate substantially improved calibration on dermatology-oriented evaluation: the calibration error falls by roughly 58%, and accuracy in the top 10% confidence band doubles relative to the base model. Importantly, confidence becomes more monotonic with correctness—high confidence is more meaningfully associated with correct answers.

## Intended use

medical-openjev is designed to sit alongside medical AI systems as a decision-safety layer. Typical uses include:

- Adding a trustworthy confidence signal to a medical LLM or agent.
- Routing uncertain cases to deeper questioning, another specialty, or a human reviewer.
- Surfacing missing evidence before an agent acts on a recommendation.
- Building safer research prototypes for medical-agent workflows.

## Safety notice

> [!WARNING]
> **Research use only.** medical-openjev is not a medical device and must not be used as a substitute for professional clinical judgment, diagnosis, treatment, or emergency care. Its outputs are decision-support signals, not clinical determinations. Any deployment involving patients requires appropriate validation, governance, and qualified human oversight.

## Roadmap

- [ ] Release model weights and inference package
- [ ] Release evaluation datasets, protocols, and reproducibility scripts
- [ ] Publish the technical report on arXiv
- [ ] Expand validation across specialties, languages, and clinical settings

## Team

| Person | Role | Affiliation |
| --- | --- | --- |
| Tian Gan | Supervision & Project Lead | Shandong University |
| Ruifan Zuo | Student Project Leadership | Shandong University |
| Guocheng Hu | Student Project Leadership | Shandong University |
| Ziyang Meng | Team Member | City University of Hong Kong (Dongguan) |
| Dai Zichao | Contributor | Shandong University |
| Zhao Qichao | Contributor | Tsinghua University |
| Rui Wang | Medical Support | Qingdao Endocrine and Diabetes Hospital |
| San Zhang | Medical Support | Affiliation to be announced |

## Links

- [arXiv paper](https://arxiv.org) *(placeholder)*
- [Hugging Face — Qwen3.5-4B](https://huggingface.co/Qwen/Qwen3.5-4B) *(temporary placeholder)*

## Citation

If you use medical-openjev in your research, please cite the forthcoming paper. Citation details will be added once the preprint is available.

```bibtex
@article{medical_openjev_2026,
  title   = {medical-openjev: Calibrated Decision Gating for Medical AI Agents},
  author  = {{medical-openjev Team}},
  year    = {2026},
  note    = {Manuscript in preparation}
}
```

---

<p align="center">
  Built for transparent, calibrated, and human-supervised medical AI research.
</p>
