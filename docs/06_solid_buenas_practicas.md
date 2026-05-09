# Apartado 6 — Buenas Prácticas: SOLID y Separación de Responsabilidades

En este apartado se explica cómo se aplican los principios **SOLID** al diseño del **Tutor Inteligente de Inteligencia Artificial basado en RAG**. Estos principios permiten que el sistema sea más ordenado, fácil de mantener y preparado para futuros cambios.

En este MVP es importante separar correctamente la indexación de documentos y la consulta del estudiante. Por esa razón, se propone dividir la lógica principal en dos archivos:

- `indexer.py`: encargado de procesar e indexar documentos.
- `retriever.py`: encargado de recuperar información y generar respuestas.

---

## Aplicación de principios SOLID

| Principio | Aplicación en el Tutor RAG | Ejemplo concreto |
|---|---|---|
| S — Single Responsibility | Cada módulo debe tener una única responsabilidad. | `indexer.py` solo carga, divide y guarda documentos. `retriever.py` solo recibe preguntas, recupera contexto y genera respuestas. |
| O — Open / Closed | El sistema debe permitir agregar nuevas herramientas sin modificar toda la lógica existente. | Se puede agregar Pinecone como nuevo vector store sin cambiar el caso de uso principal de consulta. |
| L — Liskov Substitution | Las implementaciones deben poder sustituirse sin romper el sistema. | ChromaDB y Pinecone pueden intercambiarse si ambos cumplen la misma interfaz de `VectorStore`. |
| I — Interface Segregation | Las interfaces deben ser específicas y no obligar a implementar métodos innecesarios. | Una interfaz `DocumentLoader` solo carga documentos, mientras que una interfaz `Retriever` solo recupera fragmentos. |
| D — Dependency Inversion | Las capas superiores no deben depender directamente de herramientas concretas. | `AskTutorUC` debe depender de una abstracción `Retriever`, no directamente de ChromaDB. |

---

## Separación entre indexer.py y retriever.py

La separación entre `indexer.py` y `retriever.py` ayuda a mantener el sistema más claro y fácil de modificar.

### indexer.py

Este archivo se encarga de preparar la base de conocimiento del tutor. Sus responsabilidades principales son:

- Leer documentos desde la carpeta `data/fuentes/`.
- Extraer texto de archivos PDF, TXT, Markdown y Prolog.
- Dividir el contenido en fragmentos.
- Convertir los fragmentos en embeddings.
- Guardar los vectores en ChromaDB.

### retriever.py

Este archivo se encarga de responder las preguntas de los estudiantes. Sus responsabilidades principales son:

- Recibir una pregunta en lenguaje natural.
- Buscar fragmentos relevantes en la base vectorial.
- Construir un prompt con el contexto recuperado.
- Enviar el contexto al modelo generativo.
- Devolver una respuesta clara y fundamentada en los documentos.

---

## Ejemplo concreto de separación de responsabilidades

```text
indexer.py
- Carga documentos.
- Extrae texto.
- Divide contenido en fragmentos.
- Genera embeddings.
- Guarda los vectores en ChromaDB.

retriever.py
- Recibe la pregunta del estudiante.
- Recupera fragmentos relevantes.
- Construye el prompt de respuesta.
- Consulta el modelo generativo.
- Devuelve una respuesta basada en el contexto recuperado.
```

---

## Justificación de buenas prácticas

Aplicar SOLID en este proyecto permite que cada parte del Tutor Inteligente tenga una función clara. Esto evita que el código se vuelva desordenado o difícil de mantener.

Por ejemplo, si en el futuro se desea cambiar ChromaDB por Pinecone, no debería ser necesario modificar todo el sistema. Únicamente se tendría que cambiar la implementación del vector store. De la misma manera, si se desea cambiar el modelo de embeddings, se podría modificar solo la capa correspondiente sin afectar la lógica de consulta.

Además, separar la indexación de la recuperación ayuda a probar cada parte de forma independiente. El proceso de indexación se ejecuta cuando se cargan o actualizan documentos, mientras que el proceso de recuperación se ejecuta cada vez que un estudiante realiza una pregunta.

---

## Conclusión del apartado

Los principios SOLID ayudan a que el Tutor Inteligente sea más mantenible, flexible y ordenado. La separación entre `indexer.py` y `retriever.py` permite cumplir con el principio de responsabilidad única y facilita futuras mejoras del sistema.

Gracias a esta organización, el MVP puede iniciar como una solución académica sencilla, pero con una estructura preparada para crecer en el futuro.
