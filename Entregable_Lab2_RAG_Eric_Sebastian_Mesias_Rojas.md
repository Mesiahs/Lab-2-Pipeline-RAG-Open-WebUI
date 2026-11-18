# Informe de Laboratorio 2: Pipeline RAG con Open WebUI y Ollama

- **Estudiante / Equipo:** Eric Sebastian Mesias Rojas
- **Stack Utilizado:** qwen2.5:1.5b-instruct + nomic-embed-text + Open WebUI + Docker Compose
- **Fecha:** 17/11/2026

---

## 1. Comparativa: Antes del RAG (Línea Base)
Sin adjuntar ningún documento, el modelo qwen2.5:1.5b-instruct no pudo dar la cifra ni el código reales. Reconoció que "no tiene datos específicos" sobre Innovatech, pero a continuación alucinó: interpretó "conectividad" como infraestructura de red y generó una lista de 10 puntos genéricos sobre 5G, fibra óptica, IoT y realidad virtual, sin relación con un auxilio económico. Incluyó frases sin sentido como "conectividad aérea (avión, avión)". No mencionó los $245.000 COP ni el código #INV-CON-2027. Esto confirma que el conocimiento privado de la empresa no está en los pesos del modelo y que, sin contexto, un SLM tiende a rellenar con información plausible pero irrelevante.

## 2. Comparativa: Después del RAG (Con Knowledge Base)
La indexación en Open WebUI fue correcta: el archivo se vectorizó con nomic-embed-text (motor Ollama) y se guardó en 2 fragmentos (chunks de 1024 con superposición de 100).

Con la configuración por defecto (recuperación focalizada) y la colección adjunta, el modelo no recibió el contenido del documento y volvió a alucinar: en un intento inventó un "Instituto de Desarrollo Social" y afirmó que la política no especificaba un monto; en otro respondió "100 %" y un código inexistente "IDU: 123456789". En ninguno apareció una fuente citada. Esto muestra que, en un modelo de 1.5B, el RAG depende de que la recuperación entregue el contexto correcto.

Al activar "Usar documento completo" para la colección, el modelo respondió correctamente: $245.000 COP y el código #INV-CON-2027, con "1 Fuente" citada (politica_innovatech_2027.txt). También acertó la consulta condicional (7 días hábiles y directora Laura Martínez) y el protocolo técnico (menos de 2 horas, canal #sec-incidentes-urgente y cifrado AES-256 con rotación trimestral). Omitió detalles menores: la aprobación del líder de proyecto y la extensión 809. El documento es corto, por lo que cabe entero en el contexto; en una base de conocimiento grande habría que depender de la recuperación por fragmentos.

## 3. Evaluación de Control de Alucinaciones (Fuera de Dominio)
Ante la pregunta "¿Cuál es el horario de atención de la cafetería los sábados en Innovatech?", el modelo respondió que la información no está presente en el contexto proporcionado, que no se menciona ningún horario de cafetería y recomendó contactar a la empresa. No inventó ningún horario, que era el comportamiento deseado: el documento solo cubre conectividad, vacaciones anticipadas y ciberseguridad.

Cabe señalar que Open WebUI mostró "1 Fuente" también en esta respuesta, porque en modo documento completo siempre inyecta el archivo, así que la etiqueta de fuente no indica por sí sola que el contenido fuera relevante. Con una sola corrida no se puede asegurar que el comportamiento sea consistente en un modelo de este tamaño.

---

## 4. Evidencias Adjuntas (Capturas de Pantalla)
- [ ] Captura 1: Respuesta del modelo sin RAG (alucinación / desconocimiento).
- [ ] Captura 2: Documento `politica_innovatech_2027.txt` indexado en *Workspace -> Knowledge*.
- [ ] Captura 3: Respuesta fundamentada usando `#Politicas-Innovatech-2027` con citas desplegadas.
