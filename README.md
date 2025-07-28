LinkedIn Post Generator using Streamlit and OpenAI
This project generates LinkedIn posts based on a selected topic, language, and length using Streamlit for the UI and OpenAI for text generation.

📌 Features
✔ Select a topic from predefined categories
✔ Choose post length (Short, Medium, Long)
✔ Choose language (English or Hinglish)
✔ Generates engaging LinkedIn-style posts instantly

🛠 Tech Stack
Python 3.x

Streamlit – For building the web app

OpenAI API – For generating posts

Custom Python scripts (few_shot.py, post_generator.py)

📂 Project Structure
graphql
Copy
Edit
LinkedIn-Post-Generator/
│
├── main.py             # Main Streamlit app
├── few_shot.py         # Contains FewShotPosts class for tags/topics
├── post_generator.py   # Contains function to generate posts using OpenAI
├── requirements.txt    # All dependencies
└── README.md           # Project documentation


Add OpenAI API Key - in my case i have use groq(easy to use and free)
Create a file named .env in the project root and add:

ini
Copy
Edit
OPENAI_API_KEY=your_api_key_here
Then install python-dotenv to load environment variables:

bash
Copy
Edit
pip install python-dotenv
▶️ Run the App
Use:

bash
Copy
Edit
python -m streamlit run main.py
Streamlit will show:

nginx
Copy
Edit
Local URL: http://localhost:8501
Open it in your browser.

📜 How It Works
1. main.py
Creates the UI using Streamlit

Dropdowns for:

Topic → fetched from FewShotPosts.get_tags()

Length → Short | Medium | Long

Language → English | Hinglish

On Generate button click → calls generate_post()

2. few_shot.py
Contains the FewShotPosts class

Holds example topics like:

python
Copy
Edit
class FewShotPosts:
    def __init__(self):
        self.tags = ["AI", "Tech Careers", "Startups", "Productivity"]

    def get_tags(self):
        return self.tags
3. post_generator.py
Uses OpenAI to generate posts:

python
Copy
Edit
import openai
import os
from dotenv import load_dotenv

load_dotenv()
openai.api_key = os.getenv("OPENAI_API_KEY")

def generate_post(length, language, topic):
    prompt = f"Write a {length} LinkedIn post in {language} about {topic}."
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=prompt,
        max_tokens=200
    )
    return response.choices[0].text.strip()
🚀 Deployment
You can deploy this on:

Streamlit Cloud → https://share.streamlit.io

Render or Heroku for free hosting

✅ Example Usage
Select:

Topic: AI

Length: Medium

Language: English

Click Generate

Output:
"AI is transforming industries at lightning speed. Here’s why you should stay ahead..."

📌 Notes
Make sure you have OpenAI API Key

Internet connection is required for OpenAI API

For large requests, you may need OpenAI Billing enabled
