# RAG-Pipeline-Bangla-English
This is a basic RAG pipeline capable of understanding and responding to both  English and Bengali queries. The system is capable of fetching relevant information from a pdf  document corpus and it also generates a meaningful answer grounded in retrieved content. 
## Used Tools, Libraries, and Packages

- **pdfplumber**: For extracting text from PDF files.
- **sentence-transformers**: For generating text embeddings (using `banglabert`).
- **faiss-cpu**: For efficient vector similarity search and storage.
- **transformers**: For the `xlm-roberta-large-squad2` question-answering model.
- **torch**: Backend for machine learning models.
- **numpy**: For numerical operations on embeddings.
- **fastapi**: For creating a REST API (if implemented in the notebook).
- **uvicorn**: ASGI server for running the FastAPI app.
- **nest-asyncio**: For running async code in Jupyter.
- **pyngrok**: For exposing the local API via a public URL.

---

## Sample Queries and Outputs

### Bengali Queries
- **Query**: "অপ্রমত্ত কারণ কী করে জীবিকা নির্বাহ করবে?"
  - **Output**: "(খ) অকার্যতই"
- **Query**: "গজাননের মায়ের নাম কী?"
  - **Output**: "(খ) অন্নপূর্ণা"

### English Queries
- **Query**: "What is the age of Kalyani at the time of marriage?"
  - **Output**: "15 years old"
- **Query**: "Who is the real guardian of Anupom?"
  - **Output**: "His Uncle"
