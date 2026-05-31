# Dialectal Robustness in Spanish-to-English Speech Translation
### Cascade vs. End-to-End Architectures Across Latin American Varieties

**Authors:** Berly Diaz-Castro, Roxana Flores-Quispe  
**Institution:** Universidad Nacional de San Agustín (UNSA), Arequipa, Perú  
**Target venue:** Computer Speech & Language (Q1, Elsevier)

---

## Research Question

> Which architecture — cascade (ASR + MT) or end-to-end — is more robust to dialectal variation in Latin American Spanish speech translation?

No Q1 paper has systematically evaluated cascade vs. E2E systems across multiple Latin American Spanish varieties. This is the gap we fill.

---

## Key Contributions

1. **Dialectal evaluation corpus** — 3,000 sentence pairs across 6 Latin American dialects (Perú, Venezuela, Puerto Rico, Colombia, Chile, Argentina), built on OpenSLR 73/74/75 and Google LATAM datasets with English references
2. **Systematic comparison** — cascade (Whisper + MT) vs. E2E (Whisper direct, SeamlessM4T) under dialectal variation
3. **Corpus-level BLEU** — corrects the sentence-level BLEU bug from the prior paper that artificially deflated cascade results (~10 BLEU points)
4. **Error propagation analysis** — which dialect varieties degrade cascade vs. E2E most

---

## Differentiation from Related Work

| Paper | Their coverage | What we add |
|-------|---------------|-------------|
| Bentivogli et al. 2021 | En→Es (English source), no dialects | Es→En (Spanish source), 6 LA dialects |
| Papi et al. 2025 | Es→En standard with LLM cascades | NLLB-200 cascade + 6 LA dialects systematically |
| Gupta et al. 2025 | Error propagation Hindi→English | Error propagation Spanish LA→English |

---

## Repository Structure

```
MSR/
├── CodeBase/
│   ├── 01_build_dialectal_corpus.ipynb             # Step 1 — Build 3,000-sentence corpus
│   ├── 02_evaluate_systems.ipynb                   # Step 2 — Evaluate systems across dialects
│   ├── Copia_de_ASR+MT.ipynb                       # Original paper cascade (reference only)
│   ├── Copia_de_Original_Whisper_For_Translation.ipynb  # Original E2E (reference only)
│   └── Copia_de_mBART.ipynb                        # Original mBART fine-tuning (reference only)
├── Strategy/
│   ├── research_status.md                          # Full gap analysis and research plan
│   └── research_angles.md                          # Strategic research angles
├── Literature/
│   ├── error_propagation.md
│   ├── gap_validation.md
│   ├── when_cascade_wins.md
│   └── whisper_domain_adaptation.md
├── Paper original/
│   ├── ST Translation using E2E.pdf                # Rejected paper
│   └── observaciones journal.pdf                   # Reviewer feedback
└── README.md
```

---

## Datasets

### Dialectal Corpus (our contribution)

| Dialect | Source | Audio | Samples |
|---------|--------|-------|---------|
| Peruvian | OpenSLR 73 | WAV (download in notebook) | 500 |
| Puerto Rican | OpenSLR 74 | WAV (download in notebook) | 500 |
| Venezuelan | OpenSLR 75 | WAV (download in notebook) | 500 |
| Colombian | `ylacombe/google-colombian-spanish` (HF) | HuggingFace | 500 |
| Chilean | `ylacombe/google-chilean-spanish` (HF) | HuggingFace | 500 |
| Argentinian | `ylacombe/google-argentinian-spanish` (HF) | HuggingFace | 500 |
| **Total** | | | **3,000** |

English references generated with `Helsinki-NLP/opus-mt-es-en`.

### Standard Baseline
- **CoVoST2** Es→En: `fixie-ai/covost2` (HuggingFace)

---

## Systems Evaluated

### Prototype — runs on T4 free Colab (~3.3 GB VRAM total)

| System | Type | Models |
|--------|------|--------|
| Whisper E2E | E2E | `openai/whisper-small` (task=translate) |
| Cascade | ASR + MT | `openai/whisper-small` + `Helsinki-NLP/opus-mt-es-en` |
| SeamlessM4T | E2E modern | `facebook/seamless-m4t-medium` |

### Full Evaluation — requires A100 40GB+

| System | Type | Models |
|--------|------|--------|
| Whisper E2E | E2E | `openai/whisper-large-v3` (task=translate) |
| Cascade + mBART | ASR + MT | `openai/whisper-large-v3` + `facebook/mbart-large-50-many-to-many-mmt` |
| Cascade + NLLB-200 | ASR + MT | `openai/whisper-large-v3` + `facebook/nllb-200-distilled-600M` |
| SeamlessM4T v2 | E2E modern | `facebook/seamless-m4t-v2-large` |

> When own fine-tuned models are ready, replace base model IDs with `your_hf_username/model-name`.

---

## Metrics

| Metric | Tool | Purpose |
|--------|------|---------|
| BLEU (corpus-level) | sacrebleu | Main translation quality metric |
| chrF | sacrebleu | Less sensitive to length variation |
| WER | jiwer | ASR component quality (cascade only) |

---

## How to Run

### Requirements

```bash
pip install transformers datasets sacrebleu soundfile librosa tqdm sentencepiece pandas numpy
```

### Step 1 — Build dialectal corpus

Run `CodeBase/01_build_dialectal_corpus.ipynb` in Google Colab (T4 or CPU):

- Downloads text from OpenSLR 73/74/75 and HuggingFace LATAM datasets
- Samples 500 sentences per dialect
- Generates English references with Helsinki-NLP/opus-mt-es-en
- Output: `MSR_Dialectal/csv/corpus_dialectal_completo.csv` — **3,000 rows, 0 empty**

### Step 2 — Evaluate systems

Run `CodeBase/02_evaluate_systems.ipynb` in Google Colab (T4 minimum):

- Downloads OpenSLR audio (Perú, Puerto Rico, Venezuela) — ~2 GB
- Loads HuggingFace audio (Colombia, Chile, Argentina)
- Loads 3 systems simultaneously (~3.3 GB VRAM with small models)
- Saves checkpoint every 100 samples — safe to resume after disconnection
- Output: `MSR_Dialectal/results/metrics_small_models.csv`

---

## GPU Requirements

| Task | GPU | VRAM needed | Estimated time |
|------|-----|-------------|----------------|
| Build corpus (notebook 01) | T4 free Colab | ~2 GB | ~3 min |
| Prototype eval (notebook 02, small models) | T4 free Colab | ~3.3 GB | ~30 min |
| Full evaluation (large models) | A100 40GB | ~17 GB | ~3–4 hours |
| Fine-tune Whisper-large-v3 | A100 40GB+ | ~30 GB | Several days |

---

## Roadmap

- [x] Build dialectal text corpus (3,000 sentences, 6 dialects)
- [x] Generate English references
- [x] Build evaluation pipeline with small models (prototype)
- [ ] Run prototype on T4 — validate full pipeline end to end
- [ ] Run full evaluation on GPU server with large models
- [ ] Fine-tune own Whisper + mBART models on dialectal corpus
- [ ] Replace base models with own fine-tuned versions
- [ ] Error propagation analysis by dialect
- [ ] Write paper (methodology, results, discussion)
- [ ] Submit to Computer Speech & Language

---

## License

- OpenSLR datasets: CC BY-SA 4.0
- HuggingFace LATAM datasets: CC BY 4.0
- Code in this repository: MIT License
