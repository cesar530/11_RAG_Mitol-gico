# 🏛️ RAG Mitológico — Motor de Preguntas y Respuestas

Sistema **RAG (Retrieval-Augmented Generation)** especializado en mitología griega. Usa **LangChain**, **FAISS** y **Ollama** para responder preguntas como:

- *"¿Quién mató a Medusa?"*
- *"Relación entre Atenea y Odiseo"*

Cita contexto extraído de PDFs y responde en español.

---

## 🎯 Características

| Feature | Descripción |
| ------- | ----------- |
| **Pipeline RAG completo** | Carga PDFs → chunking → embeddings → retrieval → respuesta |
| **100% local** | Ollama (llama3 + nomic-embed-text), sin APIs externas |
| **Vector Store FAISS** | Rápido, ligero, sin dependencias problemáticas |
| **API REST** | FastAPI con endpoints `/ask` e `/ingest` |
| **Notebook interactivo** | Demostración paso a paso en Jupyter |

---

## 📋 Requisitos

- **Python 3.10+**
- **Ollama** instalado y corriendo

```powershell
# Descargar modelos necesarios
ollama pull llama3
ollama pull nomic-embed-text

# Iniciar servidor (dejar corriendo)
ollama serve
```

---

## 🔧 Instalación

```powershell
# 1. Crear entorno virtual
python -m venv venv
venv\Scripts\activate

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Agregar PDFs de mitología
# Copiar archivos .pdf a la carpeta data/
```

---

## 🚀 Uso

### Opción A: Notebook Jupyter (recomendado)

```powershell
jupyter notebook nb_principal.ipynb
```

Ejecutar celdas en orden:

1. **Configuración** — imports y rutas
2. **Cargar PDFs** — lee documentos de `data/`
3. **Build vectorstore** — genera embeddings con Ollama
4. **Preguntas** — consulta el RAG

### Opción B: API FastAPI

```powershell
uvicorn app.proyecto_principal:app --reload
```

Endpoints:

- `GET /` — status
- `GET /ask?question=...` — hacer pregunta
- `POST /ingest` — subir PDF

Ejemplo:

```powershell
curl "http://localhost:8000/ask?question=Quien%20mato%20a%20Medusa"
```

---

## 🏗️ Arquitectura

```
┌─────────────────┐
│   PDFs (data/)  │
└────────┬────────┘
         │ PyPDFLoader
         ▼
┌─────────────────┐
│    Chunking     │  chunk_size=1000, overlap=150
└────────┬────────┘
         │ RecursiveCharacterTextSplitter
         ▼
┌─────────────────┐
│   Embeddings    │  nomic-embed-text (Ollama)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│     FAISS       │  Vector Store local
└────────┬────────┘
         │ Top-K similarity (k=4)
         ▼
┌─────────────────┐
│     llama3      │  LLM (Ollama)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Respuesta     │  + fuentes citadas
└─────────────────┘
```

---

## 📁 Estructura del Proyecto

```
11_RAG_Mitológico/
├── app/
│   ├── proyecto_principal.py   # FastAPI server
│   └── utils.py                # RAGConfig, load_pdfs, build_vectorstore, get_qa_chain
├── data/                       # PDFs de mitología griega
├── nb_principal.ipynb          # Notebook principal
├── requirements.txt
├── README.md
└── .gitignore
```

---

## 🧪 Ejemplo de Código

```python
from app.utils import RAGConfig, load_pdfs, split_docs, build_vectorstore, get_qa_chain

# Configurar
cfg = RAGConfig(
    model="llama3",
    embedding_model="nomic-embed-text",
    chunk_size=1000,
    chunk_overlap=150,
    persist_directory="faiss_index"
)

# Pipeline
docs = load_pdfs("data/")
chunks = split_docs(docs, cfg)
vectorstore = build_vectorstore(chunks, cfg)
ask = get_qa_chain(cfg)

# Consultar
result = ask("¿Quién mató a Medusa?", vectorstore)
print(result["answer"])
print(result["sources"])
```

---

## 🎓 Tecnologías

| Componente    | Tecnología               |
| ------------- | ------------------------ |
| Framework RAG | LangChain                |
| Vector Store  | FAISS                    |
| Embeddings    | Ollama (nomic-embed-text)|
| LLM           | Ollama (llama3)          |
| API           | FastAPI                  |
| Lectura PDFs  | PyPDF                    |

---

## 📊 Parámetros de Chunking

| Parámetro        | Valor | Descripción                                |
|------------------|-------|--------------------------------------------|
| `chunk_size`     | 1000  | Máx. caracteres por chunk (~200 palabras)  |
| `chunk_overlap`  | 150   | Traslape entre chunks (15%)                |

**Trade-off:** chunks más pequeños = retrieval más preciso pero menos contexto.

---

## ✅ Impacto Profesional

- Demuestra dominio de **arquitectura RAG**
- Experiencia con **LLMs locales** (Ollama)
- Conocimiento de **vector databases** (FAISS)
- Habilidades en **NLP y embeddings**
- APIs **production-ready** con FastAPI

---

## 👤 Autor

- 👤 Autor : **César Adrián Delgado Díaz**
- 💼 LinkedIn: [linkedin.com/in/cesar-delgado-diaz](linkedin.com/in/cesar-delgado-diaz)
- 🐙 GitHub: [github.com/cesar530](https://github.com/cesar530)

---
