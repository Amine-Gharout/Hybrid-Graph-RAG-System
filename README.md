# Constitutional GraphRAG System

A Python project implementing a **Retrieval-Augmented Generation (RAG)** system that combines vector databases (ChromaDB) and graph databases (Neo4j) to analyze and query Algerian constitutional and legal documents (1996-2020).

> 💡 **Educational Purpose**: This project serves as a comprehensive demonstration of how to build RAG (Retrieval-Augmented Generation) systems using different data storage approaches. It illustrates three distinct methodologies: **vector database-only** (ChromaDB for semantic search), **graph database-only** (Neo4j for relationship queries), and a **hybrid approach** that combines both vector and graph databases to leverage the strengths of each paradigm. This makes it an excellent reference for understanding when and how to use each approach in real-world applications.

## 📋 Overview

This project demonstrates an advanced **GraphRAG architecture** that leverages two complementary approaches for intelligent question-answering:

- **Vector-based RAG**: Semantic search through document content using ChromaDB embeddings
- **Graph-based RAG**: Relationship queries between legal documents using Neo4j and Cypher
- **Hybrid RAG**: Combined approach for complex queries requiring both content and structural information

### 🎯 Key Capabilities

- Semantic search across multiple constitution versions and legal texts
- Track modifications, citations (visas), and relationships between documents
- Answer complex questions about legal document content and structure
- Temporal queries (find documents by publication date)
- Hierarchical analysis of constitutional authority and document relationships

## 🚀 Features

- **Three RAG Modes**:
  - **Vector-Only**: Content-based questions ("What does the 2020 constitution say?")
  - **Graph-Only**: Relationship questions ("Which documents modified the constitution?")
  - **Combined**: Complex questions requiring both approaches

- **Advanced Cypher Query Generation**: Automatic translation of natural language to Neo4j queries
- **Markdown Document Processing**: Structured splitting preserving document hierarchy
- **Maximum Marginal Relevance (MMR)**: Balanced retrieval for diverse and relevant results

## 📁 Project Structure

```
push/
├── .env.example              # Environment variables template
├── .gitignore               # Git ignore rules
├── requirements.txt         # Python dependencies
├── LICENSE                  # MIT License
├── README.md               # This file
├── consitutions.csv        # Constitution metadata (1996-2020)
├── rag_graph_demo.ipynb    # Main demo notebook (10 examples)
├── data/                   # Markdown documents directory
│   ├── constitution_1996.md
│   ├── constitution_2002.md
│   ├── constitution_2008.md
│   ├── constitution_2016.md
│   ├── constitution_2020.md
│   └── [other legal documents].md
└── chroma_db/              # ChromaDB persistence (auto-generated)
```

## 🛠️ Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.8+** - [Download Python](https://www.python.org/downloads/)
- **Neo4j Desktop or Neo4j AuraDB** - [Download Neo4j Desktop](https://neo4j.com/download/)
- **Git** - [Download Git](https://git-scm.com/downloads)
- **Jupyter Notebook or VS Code** - For running the demo notebook

## 📦 Installation


### 1. Set Up Python Virtual Environment

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On Linux:
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

**Required packages include**:
- `neo4j>=5.14.0` - Neo4j Python driver
- `python-dotenv>=1.0.0` - Environment variable management
- `pandas>=2.0.0` - Data processing
- `langchain` and related packages - RAG framework
- `chromadb` - Vector database
- `sentence-transformers` - Embeddings model
- `groq` - LLM provider

## 🗄️ Neo4j Setup

### Using Neo4j AuraDB (Cloud Solution) 

1. **Create Free AuraDB Instance**
   - Go to [console.neo4j.io](https://console.neo4j.io/)
   - Sign up/Login
   - Click "New Instance" → Select "AuraDB Free"
   - Name your instance (e.g., "constitution-graph")
   - Click "Create"

2. **Save Credentials** ⚠️ **Important**
   - A popup will appear with your credentials
   - **Download the credentials file immediately** (you can only do this once!)
   - Save the file securely
   - Note down:
     - Connection URI (e.g., `neo4j+s://xxxxx.databases.neo4j.io`)
     - Username (usually `neo4j`)
     - Password (auto-generated)

3. **Wait for Instance to be Ready**
   - Status should change to "Running" (takes 1-2 minutes)
   - Once ready, click on your instance name

4. **Import Graph Data Using the Data Importer** 🎯

   This is the **easiest and recommended method** to create your graph:

   a. **Access the Import Section**
      - In your AuraDB instance dashboard, look for the **"Import"** tab on the left sidebar
      - Click on "Import"

   b. **Open the Data Importer**
      - Click the **three dots (⋮)** in the top-right corner of the Import section
      - Select **"Open with Importer"** from the dropdown menu
      - This will open the Neo4j Data Importer interface

   c. **Upload the Model File**
      - In the Data Importer interface, look for the **"Load Model"** or **"Open Model"** button
      - Click on it and select the `neo4j_importer.json` file from this repository
      - The file contains the complete graph schema (nodes, relationships, properties)

   d. **Map Your Data** (if using CSV files)
      - Upload your data files (`consitutions.csv` and any other document metadata files)
      - The importer will automatically map the data to the model structure

   e. **Run the Import**
      - Review the import preview
      - Click **"Run Import"** 
      - Wait for the import to complete (usually takes a few seconds)
      - You should see a success message with the number of nodes and relationships created

   f. **Verify the Import**
      - Click on **"Query"** or **"Explore"** to open Neo4j Browser
      - Run this query to verify: 
        ```cypher
        MATCH (n) RETURN count(n) as total_nodes;
        ```
      - You should see nodes for all constitutions and legal documents




## ⚙️ Configuration

### 1. API Keys Required

You'll need API keys from:
- **GROQ** (for LLM): [Get API key](https://console.groq.com/)
- **HuggingFace** (for embeddings): [Get API key](https://huggingface.co/settings/tokens)

### 2. Copy Environment Template

```bash
cp .env.example .env
```

### 3. Edit `.env` File

Open `.env` in a text editor and fill in your credentials:

```env
# Neo4j Configuration
# For Neo4j Desktop (Local):
NEO4J_URI=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your_neo4j_password

# For Neo4j AuraDB (Cloud) - Use these values from your downloaded credentials:
# NEO4J_URI=neo4j+s://xxxxx.databases.neo4j.io
# NEO4J_USERNAME=neo4j
# NEO4J_PASSWORD=your_generated_auradb_password

# LLM API Keys
GROQ_API_KEY=your_groq_api_key_here
HUGGINGFACEHUB_API_KEY=your_huggingface_token_here

# Application Settings
DEBUG=True
LOG_LEVEL=INFO
```

> 📝 **Note**: If you're using **Neo4j AuraDB**, make sure to:
> - Use `neo4j+s://` protocol (secure connection)
> - Use the exact URI from your AuraDB credentials file
> - Uncomment the AuraDB lines and comment out the local Neo4j lines

### 4. Prepare Document Data

Create a `data/` directory and add your Markdown documents:

```bash
mkdir data
# Copy your .md files (constitutions, laws, decrees) into data/
```

**Document Format**: Markdown files with clear header structure (H1, H2, H3) for proper splitting.

## 🎯 Usage

### Running the Demo Notebook


Or open with **VS Code** (recommended).

2. **Open `rag_graph_demo.ipynb`**

3. **Run cells sequentially** (Shift+Enter):
   - Cell 1: Load environment variables
   - Cell 2: Configure models and parameters
   - Cell 3: Load and vectorize documents
   - Cell 4: Connect to Neo4j
   - Cell 5-7: Define RAG chains
   - Cell 8-17: Run 10 demonstration examples

### 📚 10 Demonstration Examples in the Notebook

The notebook includes comprehensive examples covering:

| Example | Type | Question Focus |
|---------|------|---------------|
| 1 | Vector RAG | General content ("What is the 2020 constitution?") |
| 2 | Graph RAG | Modification relationships |
| 3 | Combined | Comparative analysis of modifications |
| 4 | Graph RAG | Specific document modifications |
| 5 | Graph RAG | Constitutional citations (visas) |
| 6 | Graph RAG | Temporal queries (by date) |
| 7 | Combined | Modification content analysis |
| 8 | Combined | Document structure (articles) |
| 9 | Combined | Constitutional authority mapping |
| 10 | Combined | Bidirectional relationship analysis |

### Basic Usage Examples

#### Vector-Only Query
```python
question = "Que dit la constitution de 2020 sur la liberté d'expression ?"
docs = retriever.invoke(question)
answer = rag_chain.invoke({"context": docs, "question": question})
print(answer)
```

#### Graph-Only Query
```python
question = "Quels documents ont modifié la constitution ?"
graph_response = query_graph_db(question)
answer = summarize_graph_results(question, graph_response)
print(answer)
```

#### Combined Query
```python
question = "Quelles modifications ont été apportées aux articles sur la liberté ?"
answer = query_combined_dbs(question)
print(answer)
```

## 🔍 Understanding the RAG Architecture

### Vector RAG Chain (ChromaDB)
- **When to use**: Content-based questions about document text
- **How it works**: Converts text to embeddings → Searches similar vectors → Retrieves relevant passages
- **Models**: 
  - Embeddings: `sentence-transformers/all-MiniLM-L12-v2`
  - LLM: `llama-3.1-8b-instant`

### Graph RAG Chain (Neo4j + Cypher)
- **When to use**: Relationship and structural questions
- **How it works**: Natural language → Cypher query → Graph traversal → Structured results
- **Models**: 
  - Cypher generation: `openai/gpt-oss-120b`
  - Answer synthesis: `openai/gpt-oss-120b`

### Combined RAG Chain
- **When to use**: Complex questions requiring both content and structure
- **How it works**: 
  1. Query graph for relationships
  2. Use graph context to search vector DB
  3. Synthesize comprehensive answer
- **Advantage**: Enriched context from both sources

## 📊 Data Schema

### Neo4j Graph Schema

```
Nodes:
  Document
    - id: Integer
    - title: String
    - type: String (Constitution, LOI, DECRET EXECUTIF, etc.)
    - date: Date (optional)
    - year: Integer (optional)

Relationships:
  MODIFIE: (Document)-[MODIFIE]->(Document)
    - Tracks modifications between documents  
  CITE: (Document)-[CITE {type: 'visa'}]->(Document)
    - Tracks citations and legal references
```

### ChromaDB Collection

```
Collection: "data_base"
  - Documents split by markdown headers (H1, H2, H3)
  - Chunk size: 512 tokens
  - Chunk overlap: 64 tokens
  - Embedding model: all-MiniLM-L12-v2
```



## 🆘 Troubleshooting

### Common Issues

**1. "GROQ_API_KEY is not set"**
- Ensure `.env` file exists and contains valid GROQ_API_KEY
- Restart notebook kernel after updating `.env`

**2. "Cannot connect to Neo4j"**
- Verify Neo4j database is running (green status in Neo4j Desktop)
- Check URI, username, and password in `.env`
- Ensure no firewall blocking port 7687

**3. "No module named 'langchain'"**
- Activate virtual environment: `venv\Scripts\activate`
- Reinstall dependencies: `pip install -r requirements.txt`

**4. "ChromaDB collection empty"**
- Ensure `data/` directory contains .md files
- Check file permissions
- Re-run Cell 3 in notebook to reload documents

**5. "Cypher query generation fails"**
- Verify graph schema matches expected structure
- Check that documents are loaded in Neo4j
- Review Cypher prompt rules in Cell 6

**6. "HuggingFace API rate limit"**
- Use local embeddings model (already default)
- Or upgrade HuggingFace account

## 📚 Resources

### Documentation
- [LangChain Documentation](https://python.langchain.com/)
- [Neo4j Cypher Manual](https://neo4j.com/docs/cypher-manual/current/)
- [ChromaDB Documentation](https://docs.trychroma.com/)
- [Groq API Documentation](https://console.groq.com/docs)

### Learning Resources
- [Neo4j Graph Academy](https://graphacademy.neo4j.com/)
- [RAG Fundamentals](https://python.langchain.com/docs/use_cases/question_answering/)
- [GraphRAG Pattern](https://microsoft.github.io/graphrag/)



## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.



## 🚀 Quick Start Checklist

- [ ] Install Python 3.8+
- [ ] **Neo4j Setup** (choose one):
  - [ ] **Option A**: Install Neo4j Desktop, create database, start it
  - [ ] **Option B**: Create Neo4j AuraDB instance, download credentials
- [ ] Clone repository
- [ ] Create virtual environment
- [ ] Install dependencies (`pip install -r requirements.txt`)
- [ ] Get GROQ API key
- [ ] Get HuggingFace token
- [ ] Create `.env` file with credentials (use AuraDB URI if using cloud)
- [ ] **Import graph data**:
  - [ ] For AuraDB: Use Import → Three dots → Open Model → Upload `neo4j_importer.json`
  - [ ] For Desktop: Use Neo4j Browser to run Cypher queries or use importer
- [ ] Verify graph import (run `MATCH (n) RETURN count(n)` in Neo4j Browser)
- [ ] Add Markdown documents to `data/` directory
- [ ] Open `rag_graph_demo.ipynb`
- [ ] Run all cells sequentially
- [ ] Explore the 10 demonstration examples!
