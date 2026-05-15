# 🧠 RAG System — Retrieval Augmented Generation

A simple but functional RAG (Retrieval Augmented Generation) pipeline built with **LangChain** and **ChromaDB**. This project allows you to chat with your own documents — feed any PDF or text file and ask questions from it using an LLM.

---

## 💡 What is RAG?

Large Language Models (LLMs) like GPT or Claude are powerful — but they only know what they were trained on. They can't answer questions about *your* documents.

**RAG solves this by:**

1. Loading your document
2. Splitting it into chunks
3. Converting chunks into vector embeddings
4. Storing them in a vector database (ChromaDB)
5. When you ask a question — retrieving the most relevant chunks
6. Sending those chunks + your question to the LLM
7. Getting a precise, grounded answer

```
Your Document
      ↓
Split into Chunks
      ↓
Convert to Embeddings
      ↓
Store in ChromaDB
      ↓
User asks a Question
      ↓
Retrieve Relevant Chunks
      ↓
LLM generates Answer
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python | Core language |
| LangChain | RAG pipeline framework |
| ChromaDB | Local vector database |
| OpenAI / Claude API | LLM for answer generation |
| pdfplumber | PDF text extraction |

---

## 📁 Project Structure

```
RAG-System/
│
├── data/                  # Put your PDF or text files here
├── main.py                # Main RAG pipeline
├── requirements.txt       # Dependencies
├── .env                   # API keys (never push this)
├── .gitignore             # Ignores .env and other sensitive files
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
OPENAI_API_KEY=your_api_key_here
```

### 4. Add your document
Place any PDF or text file inside the `data/` folder.

### 5. Run the pipeline
```bash
python main.py
```

---

## 🧪 How it works — step by step

```python
# Step 1 — Load your document
loader = TextLoader("data/your_file.txt")
documents = loader.load()

# Step 2 — Split into chunks
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
docs = text_splitter.split_documents(documents)

# Step 3 — Create embeddings and store in ChromaDB
vectorstore = Chroma.from_documents(docs, embedding=OpenAIEmbeddings())

# Step 4 — Retrieve and answer
retriever = vectorstore.as_retriever()
qa_chain = RetrievalQA.from_chain_type(llm=ChatOpenAI(), retriever=retriever)

# Step 5 — Ask your question
answer = qa_chain.run("What is this document about?")
print(answer)
```

---

## 📌 Key Concepts Learned

- What RAG is and why it matters in AI systems
- How text chunking and overlapping works
- What vector embeddings are and how similarity search works
- How ChromaDB stores and retrieves vectors locally
- How to connect a retriever to an LLM using LangChain

---

## 🔮 What's Next

- [ ] Add support for multiple PDFs
- [ ] Build a simple chat interface using Streamlit
- [ ] Upgrade to cloud vector storage (Typesense / Pinecone)
- [ ] Add memory to maintain conversation history

---

## 👨‍💻 Author

**Krishnakumar M**  
B.Sc Computer Science | SRM Arts and Science College  
[GitHub](https://github.com/Krish00i7) • [Email](mailto:krishna00i777@gmail.com)

---

> Built as part of learning AI engineering fundamentals — from data cleaning to RAG pipelines.
