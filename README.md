# Tutor Inteligente de Inteligencia Artificial - MVP RAG

Este repositorio contiene la documentación técnica para el diseño de un **Tutor Inteligente de Inteligencia Artificial** basado en RAG, desarrollado como propuesta de MVP académico.

El objetivo del sistema es permitir que un estudiante realice preguntas sobre temas del curso de Inteligencia Artificial y que el tutor responda utilizando documentos académicos previamente cargados, como guías didácticas, reglas de Prolog, tablas de verdad y material relacionado con búsqueda, lógica, aprendizaje automático y redes neuronales.

---

## Descripción general del proyecto

El sistema propuesto utiliza un enfoque RAG, es decir, recuperación aumentada por generación. Esto significa que primero se recuperan fragmentos relevantes desde una base de documentos y luego un modelo generativo utiliza ese contexto para construir una respuesta clara y fundamentada.

El flujo general del sistema es:

```text
Documentos del curso
        ↓
Extracción de texto
        ↓
División en fragmentos
        ↓
Generación de embeddings
        ↓
Almacenamiento en ChromaDB
        ↓
Consulta del estudiante
        ↓
Recuperación de fragmentos relevantes
        ↓
Respuesta generada por el tutor
