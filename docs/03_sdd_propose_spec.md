# Apartado 3 — Fases sdd-propose y sdd-spec: Arquitectura y Especificación Técnica

En este apartado se define la propuesta de arquitectura limpia para el **Tutor Inteligente de Inteligencia Artificial basado en RAG**. La finalidad es separar correctamente las responsabilidades del sistema, evitando que la lógica principal dependa directamente de herramientas externas como ChromaDB, LangChain o APIs de modelos generativos.

El sistema debe permitir cargar documentos académicos, indexarlos, recuperar fragmentos relevantes y responder preguntas de estudiantes usando únicamente el contexto recuperado.

---

## 3A — Propuesta de Arquitectura Limpia

Se propone utilizar una arquitectura limpia o hexagonal, ya que permite dividir el sistema en capas independientes. Esto facilita el mantenimiento, las pruebas y la posibilidad de cambiar herramientas en el futuro sin modificar toda la lógica del sistema.

| Capa | Componentes principales | Responsabilidad |
|---|---|---|
| Dominio | `Query`, `AnswerResponse`, `DocumentChunk` | Define las entidades principales del sistema y las reglas básicas, sin depender de frameworks externos. |
| Aplicación | `IndexDocumentsUC`, `AskTutorUC` | Coordina los casos de uso principales: indexar documentos y responder preguntas del estudiante. |
| Infraestructura | `ChromaVectorStore`, `OpenAIEmbeddingService`, `FileLoader` | Implementa las conexiones con tecnologías concretas como ChromaDB, modelos de embeddings y lectura de documentos. |
| Interfaz / Entrega | FastAPI endpoints, CLI scripts o Streamlit | Expone el sistema para que el usuario pueda cargar documentos o realizar preguntas al tutor. |

---

## Explicación de las capas

La **capa de dominio** contiene las estructuras principales del sistema, como las preguntas, respuestas y fragmentos de documentos. Esta capa no debe depender directamente de librerías externas.

La **capa de aplicación** contiene los casos de uso. Por ejemplo, `IndexDocumentsUC` se encarga de coordinar la indexación de documentos, mientras que `AskTutorUC` recibe una pregunta, solicita fragmentos relevantes y prepara la respuesta.

La **capa de infraestructura** implementa herramientas concretas, como ChromaDB para almacenar vectores, un modelo de embeddings para vectorizar texto y un cargador de archivos para leer documentos PDF, TXT, Markdown o Prolog.

La **capa de interfaz o entrega** permite que el usuario interactúe con el sistema. Puede ser mediante una API con FastAPI, una interfaz visual con Streamlit o scripts de consola para pruebas iniciales.

---

## Diagrama textual de la arquitectura

```text
Usuario / Estudiante
        |
        v
Interfaz / Entrega
(FastAPI, Streamlit o CLI)
        |
        v
Capa de Aplicación
(IndexDocumentsUC, AskTutorUC)
        |
        v
Capa de Dominio
(Query, AnswerResponse, DocumentChunk)
        |
        v
Capa de Infraestructura
(FileLoader, EmbeddingService, ChromaVectorStore, LLMService)
        |
        v
Documentos + Vector Store + Modelo Generativo
```

---

## 3B — Prompt para sdd-spec

```markdown
# Spec Request: Tutor Inteligente de IA - RAG MVP

## Contexto

Se requiere diseñar un Tutor Inteligente de Inteligencia Artificial basado en RAG, capaz de responder preguntas de estudiantes usando únicamente documentos académicos locales del curso.

Los documentos pueden incluir:
- Guía Didáctica de Inteligencia Artificial
- Reglas y ejemplos de Prolog
- Tablas de verdad de Lógica de Predicados
- Material sobre búsqueda informada
- Material sobre aprendizaje automático
- Material sobre redes neuronales

## Requisitos Funcionales

- RF-01: El sistema debe permitir cargar documentos en formato PDF, TXT, Markdown y archivos Prolog.
- RF-02: El sistema debe extraer el texto de los documentos cargados.
- RF-03: El sistema debe dividir los documentos en fragmentos usando una estrategia de chunking.
- RF-04: El sistema debe convertir los fragmentos en embeddings.
- RF-05: El sistema debe almacenar los embeddings en un vector store local.
- RF-06: El estudiante debe poder realizar preguntas en lenguaje natural.
- RF-07: El sistema debe recuperar los fragmentos más relevantes según la pregunta.
- RF-08: El sistema debe generar una respuesta basada únicamente en el contexto recuperado.
- RF-09: El sistema debe indicar cuando no exista información suficiente en los documentos.
- RF-10: El sistema debe evitar inventar respuestas fuera del contenido académico disponible.

## Criterios de Aceptación

### Escenario 1:
Dado que existen documentos indexados sobre búsqueda informada,  
cuando el estudiante pregunta “¿Qué es el algoritmo A*?”,  
entonces el sistema debe recuperar fragmentos relacionados con búsqueda heurística y generar una explicación clara basada en esos documentos.

### Escenario 2:
Dado que el estudiante pregunta sobre un tema que no está en los documentos,  
cuando el sistema no encuentre contexto suficiente,  
entonces debe responder que no tiene información disponible en el repositorio y no debe inventar datos.

### Escenario 3:
Dado que existen documentos sobre Prolog,  
cuando el estudiante pregunta por reglas, hechos o consultas,  
entonces el sistema debe recuperar ejemplos relacionados y explicar su relación con la lógica de predicados.

## Restricciones No Funcionales

- La latencia máxima de respuesta debe ser de aproximadamente 5 segundos para consultas simples.
- El sistema debe recuperar entre 3 y 5 fragmentos relevantes por pregunta.
- El sistema debe separar la indexación de documentos y la consulta del estudiante.
- El sistema debe evitar prompt injection mediante instrucciones claras al modelo.
- El sistema debe permitir cambiar ChromaDB por otro vector store sin modificar toda la lógica principal.
- El código debe estar organizado en módulos claros y fáciles de mantener.
```

---

## Justificación del modelo para sdd-spec

Para la fase **sdd-spec** se recomienda utilizar un modelo especializado en razonamiento y especificaciones técnicas, como **GPT-4o**, **Mistral** o un modelo similar. Esta fase requiere convertir una idea general del sistema en requisitos funcionales, criterios de aceptación y restricciones técnicas claras.

El uso de un modelo con buena capacidad de análisis permite generar una especificación más ordenada, completa y verificable. Además, ayuda a evitar ambigüedades en la arquitectura y facilita que posteriormente la fase **sdd-apply** pueda generar código más coherente con los requisitos definidos.

---

## Conclusión del apartado

La arquitectura propuesta permite que el Tutor Inteligente sea modular, mantenible y escalable. Separar dominio, aplicación, infraestructura e interfaz ayuda a evitar dependencias innecesarias y permite que el sistema pueda cambiar de herramientas sin afectar toda la solución. Además, el prompt de especificación técnica establece una base clara para que la implementación posterior sea más ordenada.
