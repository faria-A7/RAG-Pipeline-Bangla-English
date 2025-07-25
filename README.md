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
- **query**: "বিয়ের সময় কল্যাণীর প্রকত বয়স কত ছিল?"
  - **Output**: "(খ) ১৫ বছর"

### English Queries
- **Query**: "What is the age of Kalyani at the time of marriage?"
  - **Output**: "15 years old"
- **Query**: "Who is the real guardian of Anupom?"
  - **Output**: "His Uncle"
 
---

## API Documentation (if implemented)
- NONE (Done upto successfully retrieving the NGROK_AUTH_TOKEN)

---

## Evaluation Matrix (It has been sucessfully implemented)

- **Groundedness Score**: Measures cosine similarity between the answer and retrieved context (range: 0 to 1).
- **Relevance Score**: Ratio of the query terms found in the top retrieved chunk (range: 0 to 1).
- **Overall Score**: Average of Groundedness and Relevance (range: 0 to 1).
- **Example**: For "অনুপমের ভাষায় সুপুরুষ কাকে বলা হয়েছে?", Groundedness: 0.91, Relevance: 0.78, Overall: 0.85.

---

## Questions and Answers

### 1. What method or library did you use to extract the text, and why? Did you face any formatting challenges with the PDF content?
- **Method/Library**: Used `pdfplumber` to extract text from the PDF.
- **Why**: `pdfplumber` is efficient for handling scanned or complex PDFs and provides raw text extraction with minimal setup.
- **Challenges**: Some formatting issues (e.g., extra whitespace, inconsistent line breaks) were present, addressed using `re.sub(r'\\s+', ' ', text)` in `preprocess_text`.
- Also, I did the "OCR PDF" for the given raw pdf through the platform (https://www.ilovepdf.com/) for better letter recognization. 

### 2. What chunking strategy did you choose (e.g., paragraph-based, sentence-based, character limit)? Why do you think it works well for semantic retrieval?
- **Strategy**: Character limit-based chunking (max 500 characters) with sentence boundaries (`re.split(r'[।\\n]'`).
- **Why**: This balances context size for semantic understanding while ensuring chunks fit within the embedding model's context window. Sentence-based splitting preserves meaning units, aiding retrieval accuracy.

### 3. What embedding model did you use? Why did you choose it? How does it capture the meaning of the text?
- **Model**: `banglabert` from `sentence-transformers`.
- **Why**: Chosen for its support of Bengali text and ability to generate contextual embeddings, suitable for a bilingual corpus.
- **How**: It uses transformer-based architecture to capture semantic relationships by encoding word contexts, enabling meaningful similarity comparisons.

### 4. How are you comparing the query with your stored chunks? Why did you choose this similarity method and storage setup?
- **Method**: Cosine similarity via FAISS IndexFlatL2.
- **Why**: FAISS provides fast approximate nearest neighbor search on large vector datasets, and cosine similarity is effective for text semantics. The flat L2 index is simple and scalable for the small corpus.

### 5. How do you ensure that the question and the document chunks are compared meaningfully? What would happen if the query is vague or missing context?
- **Ensuring Meaningful Comparison**: Uses `banglabert` embeddings for both query and chunks, ensuring semantic alignment. The QA model (`xlm-roberta-large-squad2`) further refines answers with context.
- **Vague/Missing Context**: If vague, the relevance score drops, and the system may return "উত্তর পাওয়া যায়নি" if the overall score is below 0.5, indicating a need for better context or query refinement.

### 6. Do the results seem relevant? If not, what might improve them (e.g., better chunking, better embedding model, larger document)?
- **Relevance**: Results are generally relevant (e.g., Groundedness ~0.85), especially for mapped queries.
- **Improvements**: Better chunking (e.g., dynamic sizing based on semantic coherence), a multilingual embedding model (e.g., `mBERT`), or a larger, diverse document corpus could enhance performance.

---

## Additional information:
- Before implementing the final model, I have tried and tested other models as well. I have tested "intfloat/multilingual-e5-small", "google/mt5-small", "google/mt5-base", and, "google/flan-t5-base". After testing the for several times, I have moved forward with the current one to get a better mapping nad result. Those models were failing at some points or the other way around. 
