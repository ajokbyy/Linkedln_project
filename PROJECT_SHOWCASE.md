# My Journey: Building the LinkedIn Post Generator

Welcome! This isn't just a code dump; it's a look into a tool I built to solve a genuine problem I had: **Writing LinkedIn posts is exhausting, and AI usually makes them sound terrible.**

I wanted to share how I tackled this, the messy reasoning process behind it, and how I turned a generic LLM into a tool that actually sounds like *me*.

## 1. The Core Problem (Why I built this)
I tried using ChatGPT to write my posts, but they always came out sounding robotic and cringey. You know the vibe—"Thrilled to announce," "Delve into," and overuse of rocket emojis.

I realized that to fix this, I didn't need a better model; I needed **context**. An LLM doesn't know my style unless I show it. So, I decided to build a system that learns from *my past successful posts*.

## 2. Agentic Reasoning & Automation
This isn't just a script that calls an API. I had to design a workflow that "thinks" before it writes.

*   **The "Brain" (Preprocessing):** I couldn't just feed raw text to the model. I created a pre-processor (`preprocess.py`) that acts like a librarian. It reads my messy data dump and organizes it.
    *   *Real-world challenge:* My tags were a mess. I had "Job Hunting," "Job Search," and "Looking for Work"—all meaning the same thing. I programmed the AI to "reason" through this and unify them into single, clean categories.
*   **The "Memory" (Few-Shot Retrieval):** When you ask for a post about "Motivation," the system doesn't guess. It digs into the processed data (`few_shot.py`), finds the top 2-3 posts I've written on that topic before, and says, *"Okay Llama, write it like THIS."*

## 3. The Art of Prompt Engineering
Getting the LLM to behave was the hardest part. Here are two specific techniques I tailored:

### "The Strict Librarian" (JSON Extraction)
I needed the data to be machine-readable, but LLMs love to chat.
*   **My Solution:** I designed a prompt in `preprocess.py` that forces the model to ignore pleasantries and output *only* valid JSON.
    *   *Why:* This turned a creative writing tool into a rigid data parser, which was essential for the app's stability.

### "The Cleaner" (Data Preparation)
Garbage in, garbage out. To create the high-quality dataset (`data/raw_posts.json`), I didn't manually format hundreds of posts. I developed a specific prompt to normalize raw influencer text into our schema.
```json
// Prompt I used to create the dataset
I will give you text. Convert that to Json format as per the example below. 
Example Text - Hey I am going There Do you want to Join ? 
Output text { "text":"Hey I am going there \n Do you want to Join ?", "engagement":0, } 
One more instruction if there are double quotes inside the text, convert them to a single quote
```
*   *Why:* This ensured that every post in the system had a consistent structure (`text`, `engagement`), even if the original source was messy.

### "The Style Mimic" (Dynamic Context)
In `post_generator.py`, I didn't just say "Write a post." I dynamically injected my past work:
```python
# I tell the model: 'Here is how I usually write about this topic.'
prompt += "4) Use the writing style as per the following examples."
```
*   *Result:* The difference was night and day. The output shifted from "As an AI language model..." to casual, punchy sentences that felt authentic.

## 4. What I Learned (Problem Solving)
This project taught me that **data quality > model size**.
*   **Struggle:** At first, the retrieval was random because my data was noisy.
*   **Fix:** I spent more time building `preprocess.py` to clean the data than I did on the actual generation. Once the inputs were clean (unified tags, correct metadata), the "smart" features just started working.

## 5. Check It Out
I've kept the code clean and readable because I want others to learn from it.
*   **`main.py`**: The simple frontend I built so I could actually use the tool daily.
*   **`llm_helper.py`**: A centralized place for my API logic, so I'm not pasting keys everywhere.

Feel free to poke around the code. I've left comments explaining *why* I did things, not just *what* the code does.
