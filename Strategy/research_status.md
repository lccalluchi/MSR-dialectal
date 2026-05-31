# Estado de Investigación — MSR Project
**Última actualización:** Mayo 2026  
**Autores:** Berly Diaz-Castro, Roxana Flores-Quispe (UNSA, Arequipa, Perú)

---

## 1. Contexto: Paper original rechazado

**Título original:** Speech-To-Text Translation Using End-To-End Transformer Models  
**Journal:** Journal of Computer Science and Technology (Manuscript ID 4001, enero 2026)  
**Resultado:** Rechazado

### Razones del rechazo (5 causas)
1. Sin contribución metodológica novedosa — resultado esperado por diseño de Whisper
2. Resultado cascada (31.13 BLEU) sospechosamente bajo vs literatura comparable (~40 BLEU)
3. Introducción sesgada — solo presenta ventajas del enfoque E2E
4. Sin análisis de discrepancias con otros papers
5. Falta de detalle experimental (hardware, splits, horas de audio)

### Bug crítico confirmado
```python
# INCORRECTO — paper original (sentence-level BLEU):
bleus = [bleu_metric.compute(predictions=[pred], references=[[ref]])["bleu"]
         for pred, ref in zip(preds, refs)]
avg = sum(bleus) / len(bleus)  # 56% de oraciones → BLEU=0

# CORRECTO — corpus-level BLEU:
bleu = bleu_metric.compute(predictions=all_preds, references=[[r] for r in all_refs])
```
**Impacto estimado:** cascada real ~38-42 BLEU (no 31.13). Brecha real con E2E: ~1-3 BLEU, no 10.

---

## 2. Recursos disponibles en HuggingFace

| Recurso | Modelo HuggingFace | Estado |
|---------|-------------------|--------|
| Whisper LV3 fine-tuned ASR (español) | `Berly00/whisper-large-v3-spanish` | ✅ Disponible |
| Whisper LV3 fine-tuned E2E (Es→En) | `Berly00/whisper-large-v3-spanish-to-english` | ✅ Disponible |
| mBART fine-tuned MT (Es→En) | `Berly00/mbart-large-50-spanish-to-english` | ✅ Disponible |
| CoVoST2 Es→En | `facebook/covost2` / `fixie-ai/covost2` | ✅ Activo (actualizado 2025) |

---

## 3. Análisis del gap — evolución del pensamiento

### Bentivogli 2021 — análisis definitivo (verificado mayo 2026)

**Paper:** "Cascade versus Direct Speech Translation: Do the Differences Still Make a Difference?" (ACL 2021, 93 citas)  
**Pares evaluados:** En→De, En→It, **En→Es** ← español como **DESTINO**  
**Conclusión:** "the gap is now closed" — para inglés como fuente

> ⚠️ **NO nos bloquea.** Nuestro paper usa español como **FUENTE** (Es→En), dirección opuesta. Bentivogli nunca evaluó ASR de español, nunca tocó dialectos latinoamericanos. Son investigaciones distintas.

### Papi et al. 2025 — análisis definitivo (verificado mayo 2026)

**Paper:** "Hearing to Translate" (arXiv:2512.16378, 3 citas, bajo revisión)  
**Pares evaluados:** {de, fr, it, **es**, pt, zh}→en + en→{de, nl, fr, it, es, pt, zh}  
**Cascade systems:** Whisper/Seamless/Canary + LLMs (Aya, Gemma3, **Tower+**) — NO usa NLLB-200  
**Dialectos LA:** solo menciona Rioplatense (Argentina) una vez en condición "ACCENTS" vía CommonAccent — sin evaluación sistemática de variedades LA

> ⚠️ **Cubre Es→En estándar** — debilita Gap 1. Pero usa cascadas LLM-based, no NLLB. Y el análisis dialectal sistemático es inexistente.

### Gap 1 — Es→En estándar (⚠️ debilitado por Papi 2025)
**Rol en el paper:** baseline/contexto, no contribución principal  
**Diferenciación de Papi:** ellos usan Whisper+LLM; nosotros usamos Whisper+NLLB-200 (modelo MT especializado vs LLM de propósito general)

### Gap 2 — Variación dialectal latinoamericana (✅ totalmente abierto)
**Pregunta central del paper:**
> ¿Qué arquitectura —cascade o E2E— es más robusta ante la variación dialectal del español latinoamericano?

**Por qué está completamente abierto:**
- Bentivogli 2021: no cubre dialectos LA ni español como fuente
- Papi 2025: solo menciona Rioplatense una vez, sin evaluación sistemática
- Ningún paper Q1 evalúa esto
- La respuesta no es obvia: cascade podría ser más robusto (adaptas el ASR por dialecto sin tocar el MT)

**Ventaja de UNSA:** acceso natural a hablantes peruanos/andinos — los autores son del país del dialecto que estudian

### Diseño definitivo del paper
```
Evaluación 1: CoVoST2 estándar (Es→En)   → baseline con modelos post-2022
Evaluación 2: OpenSLR + Google LATAM      → contribución principal: 6 dialectos LA
              (Perú, Venezuela, Puerto Rico, Colombia, Chile, Argentina)
Análisis:     ¿Varía el gap cascade vs E2E por dialecto?
              ¿Qué tipo de errores ASR propagan más al MT?
```

---

## 4. Literatura clave

### Papers Q1/Q2 que definen el campo

| Paper | Venue | Citas | Rol en nuestro paper |
|-------|-------|-------|----------------------|
| Bentivogli et al. 2021 — Gap cerrado | ACL (Q1 equiv.) | 93 | **Obstáculo principal** — diferenciar de él |
| Cattoni et al. 2021 — MuST-C corpus | Computer Speech & Language (Q1) | 183 | Dataset estándar + revista objetivo |
| Sethiya et al. 2024 — Survey E2E ST | Computer Speech & Language (Q1) | 10 | Estado del arte reciente |
| Barrault et al. 2025 — SeamlessM4T | Nature (Q1) | 41 | Sistema E2E moderno a evaluar |
| Costa-jussà et al. 2024 — NLLB-200 | Nature (Q1) | 222 | Modelo MT de la cascada moderna |
| Etchegoyhen et al. 2022 — Basque-Es | Applied Sciences (Q2) | 19 | Cascade > E2E en condiciones específicas |
| Gupta et al. 2025 — Error propagation Hindi | InCACCT (conf.) | 1 | Análogo metodológico más cercano |

### Papers de soporte (workshops/preprints)

| Paper | Venue | Relevancia |
|-------|-------|------------|
| Dabre & Song 2024 — Whisper+IndicTrans2 | IWSLT 2024 | Whisper+MT moderno supera E2E (Indic) |
| Robinson et al. 2024 — JHU IWSLT | IWSLT 2024 | Whisper+NLLB supera Whisper solo |
| Min et al. 2025 — When E2E is Overkill | arXiv:2502.00377 ⚠️ | Cascade sigue siendo relevante |
| Papi et al. 2025 — Hearing to Translate | arXiv:2512.16378 ⚠️ | Cubre Es→En estándar con cascadas LLM-based — debilita Gap 1 pero NO cubre dialectos LA |

---

## 5. Datasets disponibles para el nuevo paper

### Dataset de evaluación estándar (ya disponible)
| Dataset | Fuente | Estado |
|---------|--------|--------|
| CoVoST2 Es→En | `fixie-ai/covost2` HuggingFace | ✅ Activo, incluye audio |

### Datasets dialectales latinoamericanos (nueva contribución)

**Fuente Google / OpenSLR** — audio + texto en español, libre descarga:

| Dialecto | URL | Estructura |
|----------|-----|-----------|
| **Español Peruano** | [OpenSLR 73](https://openslr.org/73/) | WAV + `line_index.tsv` |
| Español Puertorriqueño | [OpenSLR 74](https://openslr.org/74/) | WAV + TSV |
| Español Venezolano | [OpenSLR 75](https://openslr.org/75/) | WAV + TSV |
| Español Colombiano | `ylacombe/google-colombian-spanish` HF | 4,903 muestras, 2GB |
| Español Chileno | `ylacombe/google-chilean-spanish` HF | ~7h, 31 hablantes |
| Español Argentino | `ylacombe/google-argentinian-spanish` HF | audio + texto |

**Todos tienen:** audio WAV + texto en español (oraciones leídas para TTS).  
**Falta:** traducción al inglés → se genera con GPT-4 (metodología aceptada en Q1, ver SeamlessM4T Nature 2025).

---

## 6. Pipeline experimental propuesto

### Fase 1 — Dataset de evaluación dialectal
```
Google OpenSLR + HuggingFace (audio + texto español dialectal)
    ↓ GPT-4 traduce texto → inglés  [NO usar NLLB para evitar sesgo circular]
    ↓ Validación manual subconjunto (~100 oraciones por bilingüe)
    = Corpus de evaluación: 6 dialectos × ~500 muestras
```

### Fase 2 — Sistemas a evaluar
| Sistema | Tipo | Modelos |
|---------|------|---------|
| Whisper-LV3 E2E | E2E | `openai/whisper-large-v3` |
| Whisper-LV3 + mBART | Cascada original | baseline del paper rechazado |
| Whisper-LV3 + NLLB-200 | Cascada moderna | `facebook/nllb-200-distilled-600M` |
| SeamlessM4T v2 | E2E moderno | `facebook/seamless-m4t-v2-large` |

### Fase 3 — Métricas
- **BLEU** corpus-level (sacrebleu) — corregido vs paper original
- **chrF** — menos sensible a longitud
- **TER** — error de edición
- **COMET** — no depende del estilo de referencia (clave para referencias MT)
- **WER** — solo componente ASR en cascada

### Pregunta experimental central
> ¿Varía el gap cascade vs E2E según el dialecto latinoamericano del español?

**Hipótesis:** cascade (Whisper+NLLB) es más robusto a dialectos porque el ASR se puede adaptar por dialecto sin reentrenar el MT. E2E degrada más ante dialectos no vistos en entrenamiento.

---

## 7. Título y contribuciones del nuevo paper

**Título definitivo:**
> "Dialectal Robustness in Spanish-to-English Speech Translation: Cascade vs. End-to-End Architectures Across Latin American Varieties"

**Contribuciones (ordenadas por importancia):**
1. **[Principal]** Primer corpus de evaluación de traducción de voz para 6 dialectos latinoamericanos (Perú, Venezuela, Puerto Rico, Colombia, Chile, Argentina) — construido sobre OpenSLR 73/74/75 + Google LATAM Spanish con referencias en inglés generadas por GPT-4
2. **[Principal]** Primera comparación sistemática de cascade (Whisper+NLLB-200) vs E2E (Whisper-LV3, SeamlessM4T) bajo variación dialectal del español latinoamericano
3. **[Secundaria]** Evaluación baseline en CoVoST2 estándar Es→En con modelos post-2022 y BLEU corpus-level correcto
4. **[Metodológica]** Análisis de error propagation por tipo de dialecto: qué variedades del español degradan más la cascada vs el E2E

**Diferenciación explícita de papers relacionados:**
| Paper | Su cobertura | Lo que nosotros agregamos |
|-------|-------------|--------------------------|
| Bentivogli 2021 | En→Es (inglés fuente), sin dialectos | Es→En (español fuente), 6 dialectos LA |
| Papi 2025 | Es→En estándar con cascadas LLM | NLLB-200 cascade + 6 dialectos LA sistemáticos |
| Gupta 2025 | Error propagation Hindi-Inglés | Error propagation Español LA-Inglés |

---

## 8. Venue objetivo

| Venue | Tipo | Deadline aprox. | Nivel |
|-------|------|----------------|-------|
| **Computer Speech & Language** | Revista | Rolling (sin deadline fijo) | Q1 — **principal** |
| **Speech Communication** | Revista | Rolling | Q1/Q2 |
| INTERSPEECH 2027 | Conferencia | ~marzo 2027 | Top-tier |
| ICASSP 2027 | Conferencia | ~septiembre 2026 | Top-tier |

---

## 9. Pendientes inmediatos

### ✅ Completado
- [x] Verificar CoVoST2 en HuggingFace → activo (`fixie-ai/covost2`, 2025)
- [x] Verificar OpenSLR 73/74/75 → WAV + TSV con texto español, CC BY-SA 4.0
- [x] Verificar Google LATAM Colombian/Chilean/Argentinian → HuggingFace, audio + texto
- [x] Analizar Bentivogli 2021 → evalúa En→Es, no Es→En, no cubre dialectos LA ✅ gap blindado
- [x] Analizar Papi et al. 2025 → cubre Es→En estándar con LLMs, NO cubre dialectos LA sistemáticamente ✅ Gap 2 abierto

### 🔲 Siguiente paso (sin GPU — esta semana)
- [ ] Descargar TSV de OpenSLR 73 y contar muestras exactas por dialecto
- [ ] Diseñar script GPT-4 API para traducir texto español → inglés (referencias del corpus dialectal)
- [ ] Definir cuántas muestras por dialecto usar para evaluación (~500 por variedad)

### 🔲 Requiere GPU
- [ ] Confirmar acceso a A100 40GB en Google Colab
- [ ] Correr 4 sistemas sobre CoVoST2 test set (baseline estándar)
- [ ] Correr 4 sistemas sobre corpus dialectal (contribución principal)
- [ ] Calcular BLEU corpus-level, chrF, TER, COMET por sistema × dialecto
- [ ] Análisis de error propagation por tipo de dialecto
