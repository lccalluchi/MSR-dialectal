# Whisper: Fine-Tuning y Adaptación de Dominio

## Hallazgo principal
Whisper puede ser fine-tuneado efectivamente para dominios específicos,
pero existe un trade-off entre especialización y generalización.

---

## Papers clave

### [Effectiveness of Whisper's Fine-Tuning for Domain-Specific Use Cases in the Industry](https://consensus.app/papers/details/44beac5f00915444b1ce88524b950894/?utm_source=claude_desktop)
- Pawlowicz et al., 2025
- Fine-tuning mejora significativamente en vocabulario técnico
- WER de 1% en dominio industrial específico post fine-tuning
- **Relevancia:** Valida el enfoque de fine-tuning del paper original

### [When Whisper Meets TTS: Domain Adaptation Using only Synthetic Speech Data](https://consensus.app/papers/details/7af98c658361586892dbbad6543c00a2/?utm_source=claude_desktop)
- Vásquez-Correa et al., 2023 | 5 citas
- Fine-tuning con datos sintéticos (TTS) cuando no hay datos reales
- WER reducido entre 2 y 30 puntos según idioma y versión de Whisper
- **Relevancia:** Alternativa para obtener datos de entrenamiento sin grabar

### [Whisper-UT: A Unified Translation Framework for Speech and Text](https://consensus.app/papers/details/2bf4ac1ba7fe5e4a8981057398514c20/?utm_source=claude_desktop)
- Xiao et al., 2025 | ArXiv
- Adapters ligeros para habilitar traducción multimodal (audio + texto fuente)
- Estrategia de decodificación en 2 etapas que mejora ST
- **Relevancia directa:** Framework que combina ASR y ST — muy cercano a nuestro problema

### [Bayesian Low-Rank Factorization for Robust Model Adaptation](https://consensus.app/papers/details/5568c10b6bce5b30936cfd7db179df17/?utm_source=claude_desktop)
- Ugan et al., 2025 | ArXiv
- Adapters bayesianos previenen catastrophic forgetting en fine-tuning
- 54% backward gain sobre LoRA con solo 4% de pérdida en dominio nuevo
- **Relevancia:** Técnica para fine-tuning sin degradar capacidades generales de Whisper

### [Zero-Shot Domain-Sensitive Speech Recognition with Prompt-Conditioning Fine-Tuning](https://consensus.app/papers/details/2d17fe016e3d5b549cec2adb60ff7596/?utm_source=claude_desktop)
- Liao et al., 2023 | ASRU | 17 citas
- Whisper fine-tuneado con prompts de dominio: hasta 33% reducción de WER en dominios no vistos
- **Relevancia:** Técnica de adaptación sin audio — solo texto

---

## Bug identificado en el paper original

El fine-tuning de Whisper para ASR (cascada) con solo 1 época en CoVoST2
puede haber causado **degradación por catastrophic forgetting**.

WER reportado: 9.36% (paper) vs 11.26% (código real)
Whisper-LV2 sin fine-tuning en literatura: ~4-6% WER en español limpio

Esto explicaría parcialmente la brecha cascada: 31.13 vs literatura 40.7 BLEU.
