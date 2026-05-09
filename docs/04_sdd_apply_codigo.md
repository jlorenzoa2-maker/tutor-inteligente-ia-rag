# Apartado 4 — Fase sdd-apply: Generación de Código Python del Pipeline RAG

En la fase **sdd-apply**, el objetivo es convertir la especificación técnica del Tutor Inteligente en una implementación base en Python. Esta fase se enfoca en generar el código principal del pipeline RAG, separando la indexación de documentos y la consulta del estudiante.

Para mantener una estructura ordenada, se propone trabajar con dos módulos principales:

- `indexer.py`: encargado de cargar documentos, extraer texto, dividirlo en fragmentos, generar embeddings y almacenarlos en ChromaDB.
- `retriever.py`: encargado de recibir preguntas, recuperar fragmentos relevantes y generar una respuesta usando el contexto recuperado.

---

## 4A — Prompt para sdd-apply

```markdown
## sdd-apply Prompt: RAG Indexer + Retriever

Eres un agente de implementación especializado en Python, sistemas RAG y buenas prácticas de programación.

Genera código Python puro para construir un MVP académico llamado “Tutor Inteligente de Inteligencia Artificial”.

El sistema debe estar dividido en dos módulos principales: `indexer.py` y `retriever.py`.

---

### Módulo 1: indexer.py

Crea un script llamado `indexer.py` que realice las siguientes tareas:

- Leer documentos desde la ruta `data/fuentes/`.
- Soportar archivos en formato PDF, TXT, Markdown y Prolog.
- Extraer el texto de cada documento.
- Dividir los documentos usando la estrategia:
  - `chunk_size = 800`
  - `chunk_overlap = 150`
- Utilizar un modelo de embeddings compatible con LangChain.
- Guardar los embeddings en una base vectorial ChromaDB local.
- Usar persistencia en la carpeta `data/chroma_db/`.
- Mostrar mensajes claros durante el proceso de indexación.
- Incluir manejo básico de errores con `try/except`.
- No mezclar lógica de consulta dentro de este archivo.

---

### Módulo 2: retriever.py

Crea un script llamado `retriever.py` que realice las siguientes tareas:

- Recibir una pregunta del estudiante en lenguaje natural.
- Cargar la base vectorial ChromaDB desde `data/chroma_db/`.
- Recuperar los 3 a 5 fragmentos más relevantes según la pregunta.
- Construir un prompt para el modelo generativo usando únicamente el contexto recuperado.
- Generar una respuesta académica clara para el estudiante.
- Si no existe contexto suficiente, responder que no puede contestar con la información disponible.
- Incluir protección básica contra prompt injection.
- Separar claramente la lógica de recuperación y generación de respuesta.

---

### Restricciones técnicas

- No inventar información fuera del contexto recuperado.
- No responder usando conocimiento externo si los documentos no contienen la respuesta.
- No mezclar la indexación con la consulta.
- No hardcodear rutas innecesarias fuera de las indicadas.
- Usar nombres claros en funciones y variables.
- Agregar comentarios breves en las partes importantes del código.
- Aplicar buenas prácticas de programación en Python.
- Usar `try/except` para manejar errores de lectura de archivos, conexión al vector store o generación de respuesta.

---

### Resultado esperado

Entrega el código completo de ambos archivos:

1. `indexer.py`
2. `retriever.py`

El código debe ser funcional, ordenado y fácil de entender para un proyecto académico de Inteligencia Artificial.
```

---

## 4B — Justificación del modelo para sdd-apply

Para la fase **sdd-apply** se recomienda utilizar un modelo especializado en programación, como **Codestral**, **GPT-4o**, **Claude** o un modelo avanzado de generación de código. Esta fase requiere transformar la especificación técnica en scripts funcionales de Python, por lo que el modelo debe comprender tanto la lógica de RAG como las buenas prácticas de desarrollo.

El modelo seleccionado debe ser capaz de generar código organizado, separar responsabilidades, manejar errores y crear funciones claras. En este caso, es importante que el código no mezcle la indexación con la consulta, porque cada proceso tiene una responsabilidad diferente.

---

## Decisiones técnicas del código

La implementación se plantea en dos archivos principales para mantener ordenado el sistema.

### indexer.py

Este archivo se encargará únicamente de preparar la base de conocimiento del tutor. Sus responsabilidades son cargar documentos, extraer texto, dividirlo en fragmentos, generar embeddings y almacenarlos en ChromaDB.

### retriever.py

Este archivo se encargará únicamente de responder preguntas. Para ello, recibirá la consulta del estudiante, buscará los fragmentos más relevantes en ChromaDB y generará una respuesta basada en el contexto recuperado.

---

## Justificación de la separación indexer / retriever

Separar `indexer.py` y `retriever.py` ayuda a cumplir el principio de responsabilidad única. El proceso de indexación normalmente se ejecuta cuando se agregan o actualizan documentos, mientras que el proceso de consulta se ejecuta cada vez que un estudiante realiza una pregunta.

Esta separación permite que el sistema sea más fácil de mantener, probar y modificar. Por ejemplo, si se desea cambiar la estrategia de chunking, solo se modifica `indexer.py`. Si se desea cambiar la forma en que se generan las respuestas, solo se modifica `retriever.py`.

---

## Conclusión del apartado

La fase **sdd-apply** permite pasar de la especificación a una implementación base. El prompt propuesto guía al modelo para generar código Python claro, modular y alineado con el diseño RAG del Tutor Inteligente. Además, al separar la indexación y la recuperación se mantiene una estructura más limpia y coherente con buenas prácticas de desarrollo.
