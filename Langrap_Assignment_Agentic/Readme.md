# Market/Enterprise Q&A Agentic Workflow (LangGraph + LangChain + OpenAI + FAISS)

This project demonstrates a robust agentic workflow for answering market, enterprise, or general knowledge questions using [LangGraph](https://github.com/langchain-ai/langgraph), [LangChain](https://github.com/langchain-ai/langchain), OpenAI LLMs, and FAISS vector search. The workflow supports direct LLM answers, retrieval-augmented generation (RAG) from internal documents, simulated web search, answer validation, and retry logic.

---

## Features

- **Multi-node agentic workflow** using LangGraph
- **LLM, RAG, and Web Crawler nodes** for flexible information sourcing
- **Validation node** to check answer quality and trigger retries
- **Retry logic** with exponential backoff and error reporting
- **FAISS vectorstore** for internal document retrieval
- **OpenAI GPT-3.5/4 integration** for all LLM tasks
- **Clear logging** for each node and workflow step

---

## Workflow Overview

1. **Supervisor Node**: Initializes or resets state, manages retries, and adds system messages.
2. **Router Node**: Decides which specialist node to use based on input keywords:
    - **RAG Node**: For internal/company document queries.
    - **Web Crawler Node**: For current/latest/external information.
    - **LLM Node**: For general questions.
3. **Specialist Nodes**: Generate answers using the chosen method.
4. **Validation Node**: Checks if the answer is valid or needs a retry.
5. **Final Output Node**: Formats and returns the final answer, including sources if available.

---

## File Structure

```
Real_implementation.ipynb      # Main notebook with all code and workflow
```

---

## How to Run

1. **Install dependencies**  
   Make sure you have Python 3.9+ and install the required packages:
   ```sh
   pip install langgraph langchain langchain-openai langchain-community faiss-cpu openai pydantic beautifulsoup4
   ```

2. **Set your OpenAI API key**  
   The notebook expects your OpenAI API key in the environment variable `OPENAI_API_KEY`.

3. **Run the notebook**  
   Open `Real_implementation.ipynb` in VS Code or Jupyter and run all cells.

---

## Example Usage

The notebook includes test queries such as:

```python
test_queries = [
    "What's the latest news about AI?",      # Uses web crawler node
    "What's our Q3 revenue growth?",         # Uses RAG node
    "Explain quantum computing",             # Uses LLM node
    "cause error in web crawler",            # Simulates error and triggers retry
    "Ask me a question"                      # Unknown, defaults to LLM
]
```

For each query, the workflow will:
- Route to the appropriate node
- Attempt to answer
- Validate the answer
- Retry up to 3 times if validation fails
- Print the final output and sources

---

## Key Code Snippets

**Supervisor Node Example:**
```python
def supervisor_node(state: AgentState):
    print(f"[supervisor_node] Retry count: {state.retry_count}")
    # ...reset state, add system message, handle backoff...
    state.retry_count += 1
    return state
```

**Validation Node Example:**
```python
def validation_node(state: AgentState):
    print("[validation_node] Validating result")
    # ...check for errors or low-quality answers...
    state.validation_status = "fail" if validation_failed else "pass"
    print(f"[validation_node] Status: {state.validation_status}")
    return state
```

---

## Customization

- **Add more internal documents** to the `sample_docs` list for richer RAG responses.
- **Improve web crawling** by integrating a real web search API.
- **Tune validation logic** in `validation_node` for your use case.
- **Switch to GPT-4** by changing the model name in `ChatOpenAI`.

---

## License

This project is for educational and demonstration purposes.  
See individual package licenses for dependencies.

---

## Acknowledgements

- [LangGraph](https://github.com/langchain-ai/langgraph)
- [LangChain](https://github.com/langchain-ai/langchain)
- [OpenAI](https://platform.openai.com/)
- [FAISS](https://github.com/facebookresearch/faiss)