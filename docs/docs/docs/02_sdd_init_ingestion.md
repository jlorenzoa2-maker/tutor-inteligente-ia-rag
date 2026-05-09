# Apartado 2 — Fase sdd-init: Ingestión del Repositorio y Documentación

En la fase **sdd-init**, el sistema prepara el repositorio de documentos académicos que será utilizado por el **Tutor Inteligente de Inteligencia Artificial**. Esta etapa consiste en cargar los documentos del curso, extraer su contenido, dividirlo en fragmentos y convertirlo en vectores para que puedan ser consultados mediante búsqueda semántica.

## Formatos de documentos a ingerir

El sistema podrá trabajar con diferentes tipos de documentos académicos, entre ellos:

- **PDF:** para la Guía Didáctica de Inteligencia Artificial y documentos principales del curso.
- **TXT:** para apuntes, resúmenes o material complementario.
- **Markdown (.md):** para documentación estructurada.
- **Archivos Prolog (.pl):** para reglas, hechos y ejemplos de lógica de predicados.

Para extraer el contenido se propone utilizar **PyMuPDF** en archivos PDF y funciones nativas de Python para archivos `.txt`, `.md` y `.pl`.

## Estrategia de chunking

La estrategia de fragmentación propuesta es:

- **chunk_size = 800 tokens**
- **chunk_overlap = 150 tokens**

Esta configuración permite dividir los documentos en partes manejables, conservando suficiente contexto entre fragmentos. El solapamiento evita que una explicación importante quede cortada entre dos fragmentos distintos.

## Modelo de embeddings

Se propone utilizar el modelo **text-embedding-3-small** o, como alternativa local, **sentence-transformers/all-MiniLM-L6-v2**.

Estos modelos convierten cada fragmento de texto en un vector numérico que representa su significado. Esto permite que el sistema pueda encontrar información relacionada con la pregunta del estudiante, aunque no se utilicen exactamente las mismas palabras del documento original.

## Vector store seleccionado

Para el MVP se propone utilizar **ChromaDB local**.

Se elige ChromaDB porque es una base vectorial sencilla, ligera y adecuada para un proyecto académico. Permite guardar los embeddings generados a partir de los documentos y recuperarlos posteriormente según la similitud semántica con la pregunta del usuario.

## Modelo para la fase sdd-init

Para esta fase se recomienda utilizar un modelo con ventana grande de contexto, como **GPT-4o mini**, **Mistral** o un modelo similar.

Esto es útil porque durante la exploración inicial del repositorio el modelo necesita comprender documentos completos, identificar temas principales, organizar secciones y reconocer conceptos importantes del curso.

## Prompt para explorar el repositorio

```text
Eres un asistente académico especializado en Inteligencia Artificial.

Tu tarea es analizar el repositorio de documentos del curso, que incluye:
- Guía Didáctica de Inteligencia Artificial
- Reglas y ejemplos de Prolog
- Tablas de verdad de Lógica de Predicados
- Material sobre búsqueda informada, aprendizaje automático y redes neuronales

Debes identificar:
1. Los temas principales del repositorio.
2. Los conceptos clave de cada documento.
3. La estructura general del contenido.
4. Posibles preguntas que un estudiante podría realizar.
5. Fragmentos importantes que deberían ser recuperados por un sistema RAG.

No inventes información. Si un tema no aparece en los documentos, indícalo claramente.
Presenta el resultado en forma ordenada y académica.
