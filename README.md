- - -

# AI 電影百寶袋：基於 RAG 技術之智慧電影推薦平台 (Movie Rag Recommendation System)

本專案建構了一個基於 RAG 技術的電影推薦系統。透過結合外部電影知識庫與大型語言模型（LLM），為使用者提供具備事實依據、精準且個人化的電影推薦與劇情分析。

## 專案核心與實作技術

* **精準的知識檢索**：利用 RAG 架構，系統在回答使用者問題前，會先從結構化/非結構化的電影資料庫中檢索出相關的劇情摘要與背景資訊，確保回答的真實性與時效性。
* **語意向量搜尋**：透過 Embedding 將電影文字資料轉化為高維度向量，即使使用者用模糊的語意描述（例如：「我想看那種主角會超能力的科幻片」），系統也能透過計算向量相似度精準捕捉其意圖。
* **LangChain AI 框架整合**：專案採用 **LangChain** 框架進行開發，並串接主流大模型 API 與高效向量資料庫，實作完整的 AI 應用程式 POC 驗證。
* 
- - -

## 系統架構與運作流程

開發流程遵循標準的 RAG Pipeline，主要分為兩個階段：

### 1. 資料準備與向量化階段 (Ingestion Pipeline)

1. **資料載入 (Document Loading)**：匯入非結構化的電影文本資料（如劇情簡介、導演、演員、影評等）。
2. **文本切分 (Text Splitting)**：為了符合模型 Token 限制並提高檢索精準度，使用 LangChain 的文本切分器將冗長的電影資料切分為適當大小的文字塊（Chunks）。
3. **向量嵌入 (Embedding & Vector Store)**：將文字塊送入 Embedding 模型轉換為語意向量，並儲存至高效能的向量資料庫中，建立可供即時檢索的「電影知識庫」。

### 2. 即時問答與生成階段 (Generation Pipeline)

1. **問題檢索 (Retrieve)**：當使用者輸入查詢後，系統會計算其語意向量，並在向量資料庫中進行相似度搜尋（Similarity Search），撈出最相關的幾部電影資料。
2. **提示詞增強 (Augment)**：將撈出來的電影真實數據結合設計好的 System Prompt，包裝成增強後的提示詞。
3. **精準生成 (Generate)**：將增強提示詞送給 LLM，模型會嚴格限制**只能根據提供的電影背景資訊**進行回答與推薦，徹底消除幻覺。

---

## 環境需求

### 1. 環境準備

本專案基於 Python 3.11 環境開發，請先確保已安裝以下核心套件：

```bash
# 安裝 LangChain、向量庫與生成式 AI 相關核心套件
pip install langchain langchain-community openai chromadb pandas matplotlib gradio

```

### 2. 快速執行

您可以直接在 Google Colab 或本地 Jupyter Notebook 環境中執行 `RAG_電影百寶袋.ipynb`：

```python
# RAG 核心邏輯啟動範例
from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI, OpenAIEmbeddings

# 1. 初始化 Embedding 與 LLM
embeddings = OpenAIEmbeddings()
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.2)

# 2. 建立 RAG 問答鏈
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=vector_store.as_retriever(search_kwargs={"k": 3})
)

# 3. 進行智慧電影推薦
response = qa_chain.run("推薦我三部刺激、適合週末晚上看的科幻動作電影")
print(response)

```

## 開發者與專案資訊

* **開發者**：歐靜嬡 (Skylar Ou)
* **專案類型**：生成式 AI 應用 / 檢索增強生成 (RAG) / LangChain 實作
* **GitHub 倉庫**：[Al-Movie-Recommendation](https://www.google.com/search?q=https://github.com/SkylarOu9005/Al-Movie-Recommendation)

---
