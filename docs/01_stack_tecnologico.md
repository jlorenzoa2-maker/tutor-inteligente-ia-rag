# Apartado 1 — Definición del Stack Tecnológico Requerido

Para el desarrollo del MVP del **Tutor Inteligente de Inteligencia Artificial**, se propone un stack tecnológico basado en Python y herramientas especializadas en sistemas RAG. Estas herramientas permiten procesar documentos académicos, convertirlos en vectores, almacenarlos y recuperarlos para generar respuestas fundamentadas en el contenido del curso.

| Capa / Componente | Herramienta sugerida | ¿Qué hace? | ¿Por qué se eligió? | ¿Cómo se integra? |
|---|---|---|---|---|
| Lenguaje base | Python 3.11+ | Permite desarrollar los scripts principales del sistema. | Es ampliamente usado en IA, aprendizaje automático y procesamiento de lenguaje natural. | Será usado para crear los módulos `indexer.py` y `retriever.py`. |
| Orquestación RAG | LangChain | Organiza el flujo de carga, división, vectorización, búsqueda y respuesta. | Facilita la conexión entre documentos, embeddings, vector store y LLM. | Conecta el cargador de documentos, el modelo de embeddings, ChromaDB y el modelo generativo. |
| Modelo de Embeddings | `text-embedding-3-small` o Sentence Transformers | Convierte fragmentos de texto en vectores numéricos. | Permite comparar semánticamente preguntas con contenido del curso. | Cada fragmento de documento se convierte en embedding y se guarda en el vector store. |
| Vector Store | ChromaDB local | Almacena los vectores generados a partir de los documentos. | Es ligero, fácil de usar localmente y adecuado para un MVP académico. | Guarda los embeddings y permite recuperar los fragmentos más relevantes. |
| LLM generativo | GPT-4o mini / Mistral / Llama 3 | Genera respuestas naturales usando el contexto recuperado. | Permite explicar temas de IA en lenguaje claro para estudiantes. | Recibe la pregunta del estudiante y los fragmentos recuperados del repositorio. |
| Gestión de documentos | PyMuPDF / Unstructured | Extrae texto desde archivos PDF, TXT y Markdown. | Permite procesar diferentes formatos de material académico. | Carga la guía didáctica, reglas de Prolog y tablas de verdad. |
| Interfaz | FastAPI + Streamlit | Expone el tutor como API o interfaz web sencilla. | Permite probar el sistema de forma rápida y visual. | FastAPI puede recibir preguntas y Streamlit puede mostrar respuestas al usuario. |

## Justificación general

El stack tecnológico seleccionado permite construir un sistema RAG de forma ordenada y funcional. Python se utiliza como lenguaje principal por su compatibilidad con herramientas de inteligencia artificial. LangChain facilita la construcción del flujo RAG, mientras que ChromaDB permite almacenar y recuperar vectores de forma local.  

El uso de embeddings permite que el sistema compare preguntas de los estudiantes con los documentos del curso de manera semántica. Finalmente, el modelo generativo se encarga de redactar respuestas claras utilizando únicamente el contexto recuperado.
