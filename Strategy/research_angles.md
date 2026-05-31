# Ángulos de Investigación — Análisis Estratégico

## Contexto
Paper original rechazado. Causa raíz: resultado predecible + bug de evaluación BLEU.
Necesitamos reposicionar con una pregunta de investigación genuinamente abierta.

---

## Bug de evaluación confirmado

El paper original calcula BLEU **por oración y promedia** en lugar de BLEU a nivel corpus.
Esto produce deflación artificial: 56% de oraciones obtienen BLEU=0 por sentence-level penalty.

```python
# INCORRECTO (paper original):
bleus = [bleu_metric.compute(pred=[p], ref=[[r]])["bleu"] for p, r in zip(preds, refs)]
avg = sum(bleus) / len(bleus)

# CORRECTO:
bleu = bleu_metric.compute(predictions=all_preds, references=[[r] for r in all_refs])
```

**Impacto estimado:** Cascada real probablemente ~38-42 BLEU (no 31.13). 
Esto cambia completamente el análisis.

---

## Ángulo A: Condiciones donde cascada supera E2E
**Pregunta:** ¿Bajo qué condiciones (ruido, dominio, recursos) la cascada moderna supera a Whisper E2E?

- Fundamento: Min et al. 2025, Etchegoyhen et al. 2022, Salesky et al. 2020
- Experimentos: variar SNR, dominio, volumen de datos de fine-tuning
- Métrica: BLEU corpus-level + chrF + TER + análisis cualitativo

**Pro:** Contribución clara, contraría el consenso  
**Con:** Muchos experimentos, necesita múltiples condiciones controladas

---

## Ángulo B: Cascada moderna con LLMs (RECOMENDADO)
**Pregunta:** ¿Puede una cascada con LLM moderno en el paso MT cerrar el gap con E2E?

| Sistema | Modelo MT | Estado |
|---------|-----------|--------|
| Whisper LV3 + mBART-L50 | 2020 | baseline (paper original) |
| Whisper LV3 E2E | — | baseline (paper original) |
| Whisper LV3 + NLLB-200 | 2022 | **nuevo** |
| Whisper LV3 + Tower | 2024 | **nuevo** |
| Whisper LV3 Turbo E2E | — | **nuevo** |

- Fundamento: mBART es de 2020; la comparación es injusta temporalmente
- Dataset: CoVoST2 (sigue en HuggingFace: facebook/covost2) + MuST-C
- Métrica: BLEU corpus-level, chrF, TER

**Pro:** Muy actual, pregunta abierta, código base ya existe, modelos en HF  
**Con:** Necesita GPU para NLLB-200/Tower

---

## Ángulo C: Cuantificación de error propagation Es→En (COMPLEMENTARIO)
**Pregunta:** ¿Qué tipos de errores ASR degradan más la traducción MT en Español-Inglés?

- Clasificar errores: substitución / inserción / deleción
- Medir impacto de cada tipo en BLEU final
- Comparar con Hindi-English de Gupta et al. 2025
- Usar pipeline existente + texto correcto como upper bound

**Pro:** Vacío real en literatura, usa pipeline existente, análisis  
**Con:** Más trabajo de análisis, menos novedad técnica

---

## Estrategia recomendada: B + C

**Paper title tentativo:**
"Modern Cascade vs End-to-End Speech Translation: Closing the Gap with LLM-Based MT"

**Estructura:**
1. Replicar experimentos originales con BLEU correcto
2. Cuantificar error propagation (Ángulo C) — explica el gap
3. Proponer cascada moderna con NLLB/Tower (Ángulo B) — mitiga el gap
4. Comparar con Whisper E2E en condiciones equivalentes
5. Analizar cuándo cada enfoque es preferible

---

## Validación del gap con Elicit (mayo 2025)

Búsquedas cruzadas en Elicit confirmaron:

| Pregunta | Hallazgo |
|----------|----------|
| ¿Hay comparación Whisper E2E vs Whisper+NLLB para Es→En? | **No existe.** IWSLT 2024/2025 cubre En→De/Zh/Ja e idiomas indígenas. |
| ¿Hay análisis cuantitativo de error propagation para Es→En con modelos modernos? | **Vacío confirmado.** Gupta 2025 lo hace para Hindi-Inglés, no Español-Inglés. |
| ¿Alguien usó BLEU corpus-level correcto para este par con Whisper? | No hay paper que corrija este error metodológico para Es→En. |
| Paper de mayor riesgo: "Hearing to Translate" (Papi et al. 2025) | Evalúa 13 pares — monitorear si incluye Es→En con cascade moderno. |

**Conclusión: el gap es real, específico y verificado con Consensus + Elicit.**

---

## Título definitivo recomendado

> "Quantifying the E2E vs. Cascade Trade-off in Spanish-English Speech Translation: A Controlled Evaluation with Corpus-Level Metrics and Error Propagation Analysis"

**Contribuciones concretas:**
1. Primera evaluación Whisper-LV3 E2E vs Whisper-LV3 + NLLB-200 para Es→En con BLEU corpus-level
2. Cuantificación de error propagation para Español-Inglés (Gupta 2025 solo cubre Hindi)
3. Corrección metodológica del bug de BLEU por oración vs corpus-level
4. Análisis multicriterio: BLEU, chrF, TER, WER

---

## Venue sugerido
- **INTERSPEECH 2026** — deadline ~marzo 2026
- **ICASSP 2026** — deadline ~septiembre 2025 (ya pasó para 2026)
- **EACL 2026** — orientado a NLP europeo, acepta estudios empíricos
- **Revista:** Computer Speech and Language (Elsevier) — acepta estudios comparativos

---

## Estado de recursos disponibles

| Recurso | Estado |
|---------|--------|
| CoVoST2 Es→En | Disponible en HuggingFace (facebook/covost2) |
| Whisper LV3 fine-tuneado (ASR) | HuggingFace: Berly00/whisper-large-v3-spanish |
| Whisper LV3 fine-tuneado (E2E) | HuggingFace: Berly00/whisper-large-v3-spanish-to-english |
| mBART fine-tuneado (MT) | HuggingFace: Berly00/mbart-large-50-spanish-to-english |
| Code base notebooks | CodeBase/ (3 notebooks disponibles) |
| GPU necesaria | A100 40GB (usado originalmente en Colab) |
