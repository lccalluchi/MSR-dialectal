# Evidencia del Gap — Papers de Elicit y Consensus que lo Validan

## ⚠️ Alerta crítica: paper que restringe el gap

### [Cascade versus Direct Speech Translation: Do the Differences Still Make a Difference?](https://aclanthology.org/2021.acl-long.224.pdf)
- Bentivogli et al., **ACL 2021** (equivalente Q1, **93 citas**)
- Compara cascade vs E2E para En→De, En→It, **En→Es**
- Hallazgo: **"the gap between the two paradigms is now closed"** (2021)
- **Impacto en nuestro paper**: invalida el ángulo de Es→En estándar como contribución Q1. Un revisor de IEEE TASLP o Computer Speech & Language citaría este paper para rechazarnos.
- **Limitación clave**: evalúa inglés como fuente (En→Es), NO español como fuente (Es→En). No cubre dialectos latinoamericanos. Modelos pre-Whisper.

---

## Papers Q1/Q2 relevantes para nuestro marco de referencia

| Paper | Venue (nivel) | Relevancia para nosotros |
|-------|--------------|--------------------------|
| Bentivogli et al. 2021 — Gap cerrado | ACL (Q1 equiv., 93 citas) | **Obstáculo principal** — restringe el ángulo estándar |
| Cattoni et al. 2021 — MuST-C corpus | **Computer Speech & Language** (Q1, 183 citas) | Dataset que usaríamos; nos publicaría si el paper es sólido |
| Sethiya et al. 2024 — Survey E2E ST | **Computer Speech & Language** (Q1, 10 citas) | Survey reciente que publicó CSL; define el estado del arte |
| Barrault et al. 2025 — SeamlessM4T | **Nature** (Q1, 41 citas) | Sistema E2E moderno a incluir como baseline |
| Costa-jussà et al. 2024 — NLLB-200 | **Nature** (Q1, 222 citas) | Modelo MT de la cascada moderna que proponemos |
| Giménez et al. 2021 — Streaming cascade | **Neural Networks** (Q1, 17 citas) | Metodología de evaluación sistemática de cascada |
| Papi et al. 2022 — Direct ST subtitling | **Transactions of ACL** (Q1, 19 citas) | Análisis cualitativo E2E vs cascade |
| Etchegoyhen et al. 2022 — Basque-Es | Applied Sciences (Q2, 19 citas) | Cascade > E2E en condiciones específicas |

---

## Nuestro gap — versión revisada post Q1 analysis

El gap de **Es→En estándar** no es publicable en Q1 porque Bentivogli 2021 ya declaró el debate cerrado.

**El gap publicable en Q1 es:**
> ¿Se reabre el gap entre cascade y E2E cuando el español fuente tiene variación dialectal latinoamericana (peruano, andino, caribeno)?

- Bentivogli 2021 no cubre dialectos latinoamericanos
- Ningún paper Q1 estudia español como fuente dialectal para traducción
- La pregunta tiene respuesta no obvia: cascade podría ser más robusto (adaptas solo el ASR), E2E necesita reentrenamiento completo
- Ventaja geográfica de UNSA Arequipa: acceso natural a hablantes peruanos

---

## Papers que validan el gap (por aspecto)

### A. Validan que Whisper+NLLB se usa — pero NO para Es→En estándar

#### [NICT's Cascaded and End-To-End Speech Translation Systems using Whisper and IndicTrans2](https://aclanthology.org/2024.iwslt-1.3.pdf)
- Dabre & Song, **IWSLT 2024** (proceedings — revisado por pares)
- Usa Whisper (ASR) + IndicTrans2 (MT) para inglés→Hindi, Bengali, Tamil
- Cascada supera E2E: 51.0 vs 19.1 BLEU
- **Cómo valida el gap**: demuestra que Whisper+MT moderno gana a Whisper E2E, pero solo para pares Indic. Es→En no estudiado.

#### [JHU IWSLT 2024 Dialectal and Low-resource System Description](https://aclanthology.org/2024.iwslt-1.19.pdf)
- Robinson et al., **IWSLT 2024** (proceedings — revisado por pares)
- Usa Whisper+NLLB para pares de bajo recurso (no Es→En estándar)
- Hallazgo clave: "cascaded systems with Whisper and NLLB tend to outperform Whisper alone"
- **Cómo valida el gap**: confirma la superioridad de Whisper+NLLB sobre Whisper solo — pero solo demostrado para bajo recurso, no para el par español-inglés estándar.

#### [End-to-end ASR and Speech Translation: Integration of Speech Foundational Models and LLMs](https://arxiv.org/pdf/2510.10329) ⚠️ PREPRINT
- Luu & Bojar, **arXiv:2510.10329**, octubre 2025
- Compara Whisper+NLLB cascade vs modelos LLM para **inglés→alemán** (En→De)
- Resultado: "Whisper+NLLB cascade matches SpeechLLMs" en COMET
- **Cómo valida el gap**: misma arquitectura que proponemos pero para En→De. Nadie replicó para Es→En.

---

### B. Validan que el error propagation para Es→En no ha sido cuantificado con modelos modernos

#### [Quantitative Analysis of Error Propagation in Hindi-English Cascaded S2ST Models](https://ieeexplore.ieee.org/document/11011459)
- Gupta et al., **InCACCT 2025** (proceedings — revisado por pares)
- Cuantifica error propagation para Hindi-Inglés: caída de 11.55 BLEU (En→Hi) y 12.16 BLEU (Hi→En)
- Dataset: FLEURS. Métricas: BLEU, TER, BLASER
- **Cómo valida el gap**: el propio paper declara que es el primero en hacer análisis cuantitativo para este par — y el gap para Español-Inglés con modelos modernos sigue abierto.

---

### C. Validan que cascade sigue siendo competitivo (justifican el ángulo del paper)

#### [When End-to-End is Overkill: Rethinking Cascaded Speech-to-Text Translation](https://arxiv.org/pdf/2502.00377) ⚠️ PREPRINT
- Min et al., **arXiv:2502.00377**, febrero 2025
- Argumenta que cascade sigue teniendo lugar; propone múltiples candidatos ASR + features de habla en el MT
- **Relevancia**: fundamento teórico de que la comparación E2E vs cascade sigue siendo una pregunta abierta

#### [Hearing to Translate: The Effectiveness of Speech Modality Integration into LLMs](https://arxiv.org/pdf/2512.16378) ⚠️ PREPRINT
- Papi et al., **arXiv:2512.16378**, diciembre 2024
- Benchmarkea 5 SpeechLLMs vs 16 sistemas cascade/E2E en 13 pares, 16 benchmarks
- Hallazgo: "cascaded systems remain the most reliable overall"
- **Riesgo**: si incluye Es→En con Whisper+NLLB, reduce nuestro gap. No confirmado aún.
- **Acción**: verificar el paper completo para ver los 13 pares cubiertos.

#### [Cascade or Direct Speech Translation? A Case Study](https://www.mdpi.com/2076-3417/12/3/1097/pdf)
- Etchegoyhen et al., **Applied Sciences 2022** (revisado por pares, Q2)
- Cascade supera a E2E para Basque→Español en evaluaciones automáticas y manuales
- **Relevancia**: evidencia de que cascade gana en condiciones específicas — análogo al escenario Es→En

---

## Resumen: Preprints en la literatura de soporte

| Paper | arXiv ID | Año | Estado |
|-------|----------|-----|--------|
| Min et al. — "When E2E is Overkill" | arXiv:2502.00377 | 2025 | Solo preprint, 5 citas |
| Papi et al. — "Hearing to Translate" | arXiv:2512.16378 | 2024 | Solo preprint, 3 citas |
| Luu & Bojar — "Whisper+NLLB vs LLMs" | arXiv:2510.10329 | 2025 | Solo preprint, 1 cita |

> **Nota sobre citar preprints**: Los 3 tienen pocas citas y no están revisados por pares. Usarlos como evidencia de que el campo está activo es aceptable, pero no deben ser la única fuente de soporte para un claim central. Priorizar Dabre 2024, Robinson 2024, Gupta 2025 y Etchegoyhen 2022 (todos revisados por pares).

---

## Acciones pendientes

- [ ] Leer PDF completo de Bentivogli 2021 — confirmar que NO cubre dialectos latinoamericanos (es el paper que más necesitamos diferenciar)
- [ ] Leer PDF de "Hearing to Translate" (Papi 2025) — confirmar los 13 pares y si incluye Es→En dialectal
- [ ] Verificar si Min et al. 2025 fue aceptado en conferencia o sigue en arXiv
- [ ] Buscar datasets de español latinoamericano disponibles: HABLA (Perú, Colombia, Venezuela, Chile, Argentina), CoVoST2 dialectal, CommonVoice por acento
