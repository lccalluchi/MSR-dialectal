# Error Propagation in Cascade Speech Translation

## Hallazgo principal
El error propagation en sistemas cascada es cuantificable y sistemático.
La caída de BLEU por error propagation oscila entre 11-12 puntos dependiendo del par de idiomas.

---

## Papers clave

### [Quantitative Analysis of Error Propagation in Hindi-English Cascaded S2ST Models](https://consensus.app/papers/details/9123df00e41757b2a78f37cb08af2863/?utm_source=claude_desktop)
- Gupta et al., 2025 | InCACCT | 1 cita
- Caída de BLEU: 11.55 (En→Hi) y 12.16 (Hi→En) por error propagation
- Usa BLEU, TER y BLASER como métricas
- Dataset: FLEURS
- **Relevancia:** Vacío para Español-Inglés con modelos modernos — nadie lo ha hecho

### [Disentangling ASR and MT Errors in Speech Translation](https://consensus.app/papers/details/531c0e9ce1bb596da92cbfaf209ad0d4/?utm_source=claude_desktop)
- Le et al., 2017 | 9 citas
- Propone clasificar errores en 3 clases: good / badASR / badMT
- Francés-Inglés usando features conjuntas ASR+MT
- **Relevancia:** Marco de análisis de errores aplicable a nuestro pipeline

### [Lexical Modeling of ASR Errors for Robust Speech Translation](https://consensus.app/papers/details/ca2f5027bb7d5ce0918bd1636b27841a/?utm_source=claude_desktop)
- Martucci et al., 2021 | 6 citas
- Técnica para corromper transcripciones limpias y emular errores ASR
- El MT adaptado supera al entrenado en texto limpio
- **Relevancia:** Técnica para hacer el MT más robusto a errores ASR

### [Knowledge Distillation for Translating Erroneous Speech Transcriptions](https://consensus.app/papers/details/27ceb138e2435f33989d09b3dca4b425/?utm_source=claude_desktop)
- Fukuda et al., 2022 | Journal of NLP
- Knowledge distillation para entrenar MT robusto a errores ASR
- Evalúa en MuST-C (En-It) y Fisher (Es-En)
- **Relevancia:** Dataset Fisher Es-En es exactamente nuestro dominio objetivo

### [An Experiment in Error Analysis of Real-time Speech Machine Translation](https://consensus.app/papers/details/43f42f3089685c7282ec28d46b9320ce/?utm_source=claude_desktop)
- Di Nuovo, 2023
- Análisis cualitativo y cuantitativo de errores ASR+MT en Parlamento Europeo
- La segmentación de oraciones es el mayor problema en ASR (no medido por WER)
- **Relevancia:** Muestra limitaciones de WER como única métrica de ASR

---

## Brecha identificada
Nadie ha hecho un análisis cuantitativo del error propagation para **Español-Inglés con modelos modernos** (Whisper + LLMs). 
El paper de Gupta (2025) lo hace para Hindi pero con modelos más simples.
