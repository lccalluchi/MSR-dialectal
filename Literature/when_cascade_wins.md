# ¿Cuándo la Cascada Supera al E2E?

## Hallazgo principal
Contrario al consenso popular, la cascada sigue siendo competitiva o superior al E2E
en condiciones específicas: bajo recurso, dominios especializados, y con mejoras modernas.

---

## Papers clave

### [When End-to-End is Overkill: Rethinking Cascaded Speech-to-Text Translation](https://consensus.app/papers/details/b8d57a949d5f565289b439d00dddb0a9/?utm_source=claude_desktop)
- Min et al., 2025 | ArXiv | 5 citas
- Argumenta que la cascada sigue teniendo su lugar
- Propone usar múltiples candidatos ASR + features de habla auto-supervisadas en el MT
- La causa principal del error cascada es la divergencia entre muestras similares en el dominio speech al mapearse a texto
- **Relevancia directa:** Paper más cercano a nuestro ángulo de investigación propuesto

### [Cascade or Direct Speech Translation? A Case Study](https://consensus.app/papers/details/3d117c42578956f098c495b9834655bd/?utm_source=claude_desktop)
- Etchegoyhen et al., 2022 | Applied Sciences | 19 citas
- Par Basque-Español (bajo recurso, morfología compleja)
- **Cascada supera a E2E** en evaluaciones automáticas Y manuales
- Muestra que con recursos limitados E2E no puede competir
- **Relevancia:** Caso concreto donde cascada gana — justifica explorar condiciones

### [Phone Features Improve Speech Translation](https://consensus.app/papers/details/3a1eb2c921b15ce4828cf73a169cb3ea/?utm_source=claude_desktop)
- Salesky et al., 2020 | ArXiv | 27 citas
- Cascada sigue siendo baseline más fuerte en condiciones low y medium-resource
- Hasta 9 BLEU de ventaja sobre E2E en low-resource
- Phone features mejoran ambas arquitecturas cerrando la brecha
- **Relevancia:** Referencia base para comparaciones E2E vs cascada

### [Attention-Passing Models for Robust and Data-Efficient E2E Speech Translation](https://consensus.app/papers/details/08377e4c13195752a455c3dec4024de7/?utm_source=claude_desktop)
- Sperber et al., 2019 | TACL | 108 citas
- E2E requiere más datos para rendir bien que cascada
- E2E es malo explotando datos auxiliares (ASR/MT corpora)
- **Relevancia:** Explica por qué cascada puede ser preferida en práctica

### [Tight Integrated End-to-End Training for Cascaded Speech Translation](https://consensus.app/papers/details/7b373d429d8d57ce9c54c9a34fe34ce2/?utm_source=claude_desktop)
- Bahar et al., 2020 | SLT | 31 citas
- Propone entrenar ASR+MT conjuntamente (cascada diferenciable)
- Supera cascada estándar hasta 1.8 BLEU y es superior a E2E directo
- **Relevancia:** Muestra que la cascada mejorada puede superar E2E

---

## Condiciones donde cascada gana (síntesis)

| Condición | Evidencia |
|-----------|-----------|
| Low/medium resource | Salesky et al. 2020 |
| Pares de idiomas con recursos limitados | Etchegoyhen et al. 2022 |
| Dominio especializado (sin datos E2E) | Múltiples papers |
| Con mejoras modernas al paso MT | Min et al. 2025 |
| Evaluación humana (no solo BLEU) | Etchegoyhen et al. 2022 |
