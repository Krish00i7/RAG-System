# 🧠 RAG System — Retrieval Augmented Generation Pipeline

A fully functional **Retrieval Augmented Generation (RAG)** pipeline built from scratch using **LangChain**, **ChromaDB**, and **SentenceTransformers**. This system allows you to load your own documents (PDFs or text files), embed them into a vector store, and retrieve precise answers to natural language questions using an LLM.

---

## 💡 What is RAG?

Large Language Models are powerful — but their knowledge is static. They can't answer questions about *your* private documents.

**RAG solves this:**

```
Your Documents (PDF / TXT)
         ↓
Load & Parse with LangChain Loaders
         ↓
Split into Chunks (RecursiveCharacterTextSplitter)
         ↓
Generate Embeddings (SentenceTransformer: all-MiniLM-L6-v2)
         ↓
Store in ChromaDB (Persistent Vector Store)
         ↓
User asks a Question
         ↓
Retrieve Relevant Chunks (Cosine Similarity Search)
         ↓
LLM generates a grounded Answer
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python | Core language |
| LangChain | Document loading, text splitting, pipeline |
| ChromaDB | Persistent local vector database |
| SentenceTransformers | `all-MiniLM-L6-v2` embedding model (free, local) |
| PyPDFLoader / PyMuPDFLoader | PDF parsing |
| TextLoader / DirectoryLoader | Text file loading |
| langchain-groq | LLM integration |
| python-dotenv | API key management |

---

## 📁 Project Structure

```
RAG-System/
│
├── data/
│   ├── pdf/                  # Place your PDF files here
│   ├── text_files/           # Place your text files here
│   └── vector_store/         # ChromaDB persists embeddings here
│
├── notebook/
│   ├── document.ipynb        # Document loading & structure exploration
│   └── pdf_loader.ipynb      # Full RAG pipeline implementation
│
├── main.py                   # Entry point
├── requirements.txt          # All dependencies
├── pyproject.toml            # Project configuration
├── .gitignore                # Keeps API keys safe
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/Krish00i7/RAG-System.git
cd RAG-System
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Set up your API key
Create a `.env` file in the root folder:
```
GROQ_API_KEY=your_groq_api_key_here
```

### 4. Add your documents
- Place PDF files inside `data/pdf/`
- Place text files inside `data/text_files/`

### 5. Run the notebook
Open `notebook/pdf_loader.ipynb` in Jupyter and run all cells.

---

## ⚙️ How It Works — Key Components

### 📄 Document Loading
```python
# Load all PDFs from a directory
loader = DirectoryLoader("../data", glob="**/*.pdf", loader_cls=PyPDFLoader)
documents = loader.load()
```

### ✂️ Text Splitting
```python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=["\n\n", "\n", " ", ""]
)
chunks = text_splitter.split_documents(documents)
```

### 🔢 Embedding Generation
```python
# Free local embeddings — no API key needed
embedding_manager = EmbeddingManager(model_name="all-MiniLM-L6-v2")
embeddings = embedding_manager.generate_embeddings(texts)
```

### 🗄️ Vector Store (ChromaDB)
```python
# Persistent storage — embeddings survive restarts
vectorstore = VectorStore(
    collection_name="pdf_documents",
    persist_directory="../data/vector_store"
)
vectorstore.add_documents(chunks, embeddings)
```

### 🔍 Retrieval
```python
# Cosine similarity search to find relevant chunks
retriever = RAGRetriever(vector_store=vectorstore, embedding_manager=embedding_manager)
results = retriever.retrieve(query="What is machine learning?", top_k=5)
```

---

## 📌 Key Concepts Learned

- How to load and parse PDFs and text files using LangChain loaders
- Why chunking matters and how `chunk_overlap` preserves context
- What vector embeddings are and how cosine similarity enables semantic search
- How ChromaDB persists embeddings locally without any cloud service
- How to build a custom `EmbeddingManager` and `VectorStore` class from scratch
- How to retrieve the most relevant document chunks for any query

---

## 🔮 What's Next

- [ ] Connect retriever to an LLM for full end-to-end Q&A
- [ ] Build a Streamlit chat interface
- [ ] Add conversation memory for multi-turn Q&A
- [ ] Upgrade to cloud vector storage (Typesense / Pinecone)
- [ ] Evaluate retrieval quality with similarity scores

---

## 👨‍💻 Author

**Krishnakumar M**  
B.Sc Computer Science | SRM Arts and Science College, Chennai  
[GitHub](https://github.com/Krish00i7) • [Email](mailto:krishna00i777@gmail.com)

---

> Built from scratch to understand the internals of RAG — from raw documents to semantic retrieval. No shortcuts.
