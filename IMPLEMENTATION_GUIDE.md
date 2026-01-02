# Step-by-Step Implementation Guide

If you were to build this project from scratch, you should follow this order. The logic flows from **Configuration** -> **Data** -> **Backend Logic** -> **Frontend**.

## Step 1: Configuration & Setup (The Foundation)
You need the environment ready before writing code.
1.  **`requirements.txt`**: Define your dependencies (Streamlit, LangChain, Groq).
2.  **`.env`**: Store your `GROQ_API_KEY`.
3.  **`llm_helper.py`**:
    *   *Why first?* Every other file needs the LLM. You create the `llm` object here once so you can import it everywhere else.

## Step 2: Data Engineering (The "Brain")
You can't do "Few-Shot" learning without data.
1.  **`data/raw_posts.json`**: Create your raw dataset.
2.  **`preprocess.py`**:
    *   *Why now?* You need to clean your raw data before you can use it.
    *   *Action:* Write the script to extract tags/metadata and run it to generate `data/processed_posts.json`.

## Step 3: Backend Logic (The "Engine")
Now you assume you have clean data and a working LLM.
1.  **`few_shot.py`**:
    *   *Why now?* The generator needs to valid examples. This class handles loading the JSON you just created and filtering it.
2.  **`post_generator.py`**:
    *   *Why now?* This is the core function. It imports `llm` (Step 1) and `FewShotPosts` (Step 3.1) to construct the final prompt and get the result.

## Step 4: Frontend (The "Face")
Finally, you build the UI.
1.  **`main.py`**:
    *   *Why last?* It simply calls the functions you built in Step 3. It shouldn't contain complex logic, just dropdowns and buttons.

---

### Summary Checklist
- [ ] 1. Config (`llm_helper.py`)
- [ ] 2. Data Pipeline (`preprocess.py`)
- [ ] 3. Retrieval Logic (`few_shot.py`)
- [ ] 4. Generation Logic (`post_generator.py`)
- [ ] 5. UI (`main.py`)
