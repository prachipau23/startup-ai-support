# 🚀 Startup AI Support - Henry  
> **Your AI-Powered Partner for Startups**  

Henry is an advanced AI assistant designed to help entrepreneurs and startup founders navigate challenges like market validation, funding, legal compliance, and growth strategies, even create their product.  

---

## **🔹 Features**  
✅ AI-Powered Idea Validation  
✅ Market Research & Competitive Analysis  
✅ Business Plan & Growth Strategies  
✅ Fundraising & Investor Outreach  
✅ Legal & Regulatory Compliance  
✅ AI & Machine Learning Integration  
✅ Sales & Marketing Optimization  

---

## **🔹 Getting Started**  
📌 Code Structure
📂 henry-startup-ai
│── 📂 templates
│   ├── index.html  # Frontend UI
│── 📂 static       # CSS & JS Files (if needed)
│── app.py         # Flask API & AI integration
│── requirements.txt  # Python dependencies
│── .env.example   # Example environment variables
│── README.md      # Project documentation

### **📌 1. Installation**
step 1: Open VS Terminal or CMD
python --version

Step 2: Open Terminal & Navigate to Your Project Directory
cd path/to/your/project

Step (OPTIONAL): Create and Activate a Virtual Environment
python -m venv venv
Activate it:
Windows:
venv\Scripts\activate

Step 3: Install Dependencies
pip install flask google-generativeai python-dotenv

Step 4: Set Up Environment Variables
Create a .env file inside your project folder:
and Paste This:
GEMINI_API_KEY="Your API Key"

Step 5:Create a File with .py and paste this code:

from flask import Flask, render_template, request, jsonify
import os
import google.generativeai as genai
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

app = Flask(__name__)

# Initialize Gemini API
def initialize_gemini():
    api_key = os.getenv('GEMINI_API_KEY')
    if not api_key:
        raise ValueError("GEMINI_API_KEY not found in environment variables")
    
    genai.configure(api_key=api_key)
    
    generation_config = {
        "temperature": 1,
        "top_p": 0.95,
        "top_k": 40,
        "max_output_tokens": 8192,
        "response_mime_type": "text/plain",
    }
    
    return genai.GenerativeModel(
        model_name="gemini-2.0-flash",
        generation_config=generation_config,
    )

# Initialize chat history
INITIAL_PROMPT = """You are Henry who knows everything about businesses and startups and you are the best developer in world in terms of coding and building products/digital ai and everything there is. You help users solve their issues, from idea validation to fundraising. You are also an expert in global business regulations and technical development. Be empathetic, friendly, and provide structured responses."""

def get_chat_session():
    model = initialize_gemini()
    return model.start_chat(
        history=[
            {
                "role": "user",
                "parts": [INITIAL_PROMPT]
            }
        ]
    )

# Store chat sessions
chat_sessions = {}

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/init_chat', methods=['POST'])
def init_chat():
    try:
        session_id = request.json.get('session_id', 'default')
        if session_id not in chat_sessions:
            chat_sessions[session_id] = get_chat_session()
        return jsonify({"status": "success", "message": "Chat initialized"})
    except Exception as e:
        return jsonify({"status": "error", "message": str(e)}), 500

@app.route('/chat', methods=['POST'])
def chat():
    try:
        data = request.json
        user_message = data.get('message', '')
        session_id = data.get('session_id', 'default')

        if not user_message:
            return jsonify({"error": "No message provided"}), 400

        if session_id not in chat_sessions:
            chat_sessions[session_id] = get_chat_session()

        response = chat_sessions[session_id].send_message(user_message)
        return jsonify({"response": response.text})

    except Exception as e:
        print(f"Error: {str(e)}")
        return jsonify({"error": "An error occurred", "details": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True, port=5000)

Step 5:Create a requirements.txt file and paste this to ensure fully functional Setup:
Flask
python-dotenv
google-generativeai

Step 6: Create a folder templates inside that create a file named .html and paste this:
<!DOCTYPE html>
<html lang="en" class="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Henry - Your AI Startup Advisor</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root {
            --bg-primary: #ffffff;
            --bg-secondary: #f8fafc;
            --text-primary: #1a202c;
            --text-secondary: #4a5568;
            --accent-color: #2563eb;
            --border-color: #e2e8f0;
            --message-user: #2563eb;
            --message-bot: #ffffff;
            --shadow-color: rgba(0, 0, 0, 0.1);
        }

        .dark {
            --bg-primary: #1a202c;
            --bg-secondary: #2d3748;
            --text-primary: #f7fafc;
            --text-secondary: #cbd5e0;
            --accent-color: #4299e1;
            --border-color: #4a5568;
            --message-user: #4299e1;
            --message-bot: #2d3748;
            --shadow-color: rgba(0, 0, 0, 0.3);
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--bg-secondary);
            color: var(--text-primary);
            transition: all 0.3s ease;
        }

        .chat-container {
            max-width: 1000px;
            margin: 0 auto;
            background-color: var(--bg-primary);
            border-radius: 16px;
            box-shadow: 0 4px 20px var(--shadow-color);
            padding: 24px;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
            padding-bottom: 16px;
            border-bottom: 2px solid var(--border-color);
        }

        .header-content {
            text-align: center;
            flex-grow: 1;
        }

        .header h1 {
            color: var(--text-primary);
            margin: 0;
            padding: 0;
        }

        .header p {
            color: var(--text-secondary);
            margin: 8px 0 0 0;
        }

        .theme-toggle {
            background: none;
            border: none;
            color: var(--text-primary);
            cursor: pointer;
            padding: 8px;
            font-size: 1.2rem;
            transition: transform 0.3s ease;
        }

        .theme-toggle:hover {
            transform: rotate(30deg);
        }

        .chat-box {
            height: 600px;
            overflow-y: auto;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 12px;
            background-color: var(--bg-secondary);
            border: 1px solid var(--border-color);
        }

        .message {
            margin-bottom: 16px;
            padding: 12px 16px;
            border-radius: 12px;
            max-width: 80%;
            line-height: 1.5;
            position: relative;
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .user-message {
            background-color: var(--message-user);
            color: white;
            margin-left: auto;
            box-shadow: 0 2px 8px rgba(37, 99, 235, 0.2);
        }

        .bot-message {
            background-color: var(--message-bot);
            border: 1px solid var(--border-color);
            color: var(--text-primary);
            box-shadow: 0 2px 8px var(--shadow-color);
        }

        .input-area {
            display: flex;
            gap: 12px;
            padding: 16px;
            background-color: var(--bg-primary);
            border-radius: 12px;
            box-shadow: 0 2px 8px var(--shadow-color);
        }

        #userInput {
            flex: 1;
            padding: 12px 16px;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            font-size: 16px;
            background-color: var(--bg-primary);
            color: var(--text-primary);
            transition: all 0.3s ease;
        }

        #userInput:focus {
            outline: none;
            border-color: var(--accent-color);
            box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
        }

        button {
            padding: 12px 24px;
            background-color: var(--accent-color);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        button:hover {
            filter: brightness(110%);
            transform: translateY(-1px);
        }

        button:active {
            transform: translateY(1px);
        }

        .typing-indicator {
            display: none;
            padding: 12px 16px;
            background-color: var(--message-bot);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            margin-bottom: 16px;
            width: fit-content;
        }

        .typing-indicator span {
            display: inline-block;
            width: 8px;
            height: 8px;
            margin-right: 5px;
            background-color: var(--accent-color);
            border-radius: 50%;
            animation: typing 1s infinite;
        }

        .typing-indicator span:nth-child(2) { animation-delay: 0.2s; }
        .typing-indicator span:nth-child(3) { animation-delay: 0.4s; }

        @keyframes typing {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }

        .disclaimer {
            color: var(--text-secondary);
            font-size: 0.9rem;
            text-align: center;
            margin-top: 16px;
            padding-top: 16px;
            border-top: 1px solid var(--border-color);
        }

        pre {
            white-space: pre-wrap;
            word-wrap: break-word;
            background-color: var(--bg-secondary);
            padding: 12px;
            border-radius: 8px;
            border: 1px solid var(--border-color);
        }

        code {
            font-family: 'Courier New', Courier, monospace;
            color: var(--text-primary);
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="header">
            <button class="theme-toggle" onclick="toggleTheme()">
                <i class="fas fa-moon"></i>
            </button>
            <div class="header-content">
                <h1>Henry - Your AI Startup Advisor</h1>
                <p>Expert guidance for your entrepreneurial journey</p>
            </div>
        </div>
        <div class="chat-box" id="chatBox">
            <div class="message bot-message">
                Hello! I'm Henry, your AI startup advisor and technical expert. I'm here to help you with everything from idea validation to coding solutions. What would you like to discuss today?
            </div>
        </div>
        <div class="typing-indicator" id="typingIndicator">
            <span></span>
            <span></span>
            <span></span>
        </div>
        <div class="input-area">
            <input type="text" id="userInput" placeholder="Ask me anything about startups, business, or development...">
            <button onclick="sendMessage()">Send</button>
        </div>
        <div class="disclaimer">
            Note: While I strive for accuracy, there's always a possibility of error. Please verify critical information with appropriate professionals.
        </div>
    </div>

    <script>
        let sessionId = 'user_' + Date.now();
        let isDark = false;

        // Initialize chat
        fetch('/init_chat', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ session_id: sessionId })
        });

        function toggleTheme() {
            isDark = !isDark;
            document.documentElement.classList.toggle('dark');
            const icon = document.querySelector('.theme-toggle i');
            icon.classList.toggle('fa-moon');
            icon.classList.toggle('fa-sun');
        }

        function appendMessage(message, isUser) {
            const chatBox = document.getElementById('chatBox');
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${isUser ? 'user-message' : 'bot-message'}`;
            
            if (!isUser && message.includes('```')) {
                const parts = message.split('```');
                let formattedMessage = '';
                parts.forEach((part, index) => {
                    if (index % 2 === 0) {
                        formattedMessage += `<p>${part}</p>`;
                    } else {
                        formattedMessage += `<pre><code>${part}</code></pre>`;
                    }
                });
                messageDiv.innerHTML = formattedMessage;
            } else {
                messageDiv.textContent = message;
            }
            
            chatBox.appendChild(messageDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function showTypingIndicator() {
            document.getElementById('typingIndicator').style.display = 'block';
        }

        function hideTypingIndicator() {
            document.getElementById('typingIndicator').style.display = 'none';
        }

        async function sendMessage() {
            const userInput = document.getElementById('userInput');
            const message = userInput.value.trim();
            
            if (message) {
                appendMessage(message, true);
                userInput.value = '';
                showTypingIndicator();
                
                try {
                    const response = await fetch('/chat', {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({
                            message: message,
                            session_id: sessionId
                        })
                    });
                    
                    const data = await response.json();
                    hideTypingIndicator();
                    
                    if (data.error) {
                        appendMessage('Sorry, I encountered an error. Please try again.', false);
                    } else {
                        appendMessage(data.response, false);
                    }
                } catch (error) {
                    hideTypingIndicator();
                    appendMessage('Sorry, I encountered an error. Please try again.', false);
                }
            }
        }

        document.getElementById('userInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>

Step 7: Access the Website
Once the server is running, open your browser and go to:

🚀 http://127.0.0.1:5000

#### **Step 1: Clone the Repository**  
Run the following command to clone the project:  
```sh
git clone https://github.com/your-username/startup-ai-support.git
cd startup-ai-support
