```python
ollama delete llama3:8b

```

```python
ollama run deepseek-r1:1.5b
```

# Health Assistant
```python
from autogen import ConversableAgent, UserProxyAgent
from datetime import datetime

# Simple in-memory reminder storage
reminders = []

def add_reminder(reminder_type, details):
    now = datetime.now()
    reminders.append((now.strftime("%Y-%m-%d %H:%M:%S"), reminder_type, details))

def list_reminders():
    if not reminders:
        return "No reminders yet."
    response = "Here are your reminders:\n"
    for i, (time, r_type, details) in enumerate(reminders, 1):
        response += f"{i}. [{time}] {r_type} - {details}\n"
    return response

# The system message can guide the agent's behavior
system_message = """
You are a helpful healthcare assistant. You can:
- Add medicine reminders when user says "remind me to take [medicine] at [time]".
- Schedule doctor appointments when user says "schedule appointment with [doctor] on [date] at [time]".
- List current reminders when user says "list reminders".
- Always ask if user wants to add more reminders.
- End chat when user says "exit".
"""

llm_config = {
    "model": "deepseek-r1:1.5b",
    "base_url": "http://localhost:11434/v1",
    "api_key": "ollama",
    "timeout": 120,
}

health_assistant = ConversableAgent(
    name="HealthAssistant",
    llm_config=llm_config,
    system_message=system_message,
)

user = UserProxyAgent(
    name="User",
    human_input_mode="ALWAYS",
    max_consecutive_auto_reply=0,
    code_execution_config={"use_docker": False},
    is_termination_msg=lambda msg: "exit" in msg.get("content", "").lower(),
)

def handle_user_message(msg):
    content = msg.get("content", "").lower()
    
    if "remind me to take" in content:
        # extract medicine name and time with simple parsing (can be improved)
        try:
            # naive parsing example: "remind me to take aspirin at 9 am"
            parts = content.split("remind me to take")[1].strip()
            medicine, at_time = parts.split(" at ")
            add_reminder("Medicine", f"{medicine.strip()} at {at_time.strip()}")
            return f"✅ Got it! Reminder set to take {medicine.strip()} at {at_time.strip()}."
        except:
            return "Sorry, I didn't understand the reminder details. Please say: 'remind me to take [medicine] at [time]'."
    
    elif "schedule appointment with" in content:
        try:
            # naive parse: "schedule appointment with Dr. Smith on 2025-05-25 at 15:00"
            parts = content.split("schedule appointment with")[1].strip()
            doctor_part, date_time_part = parts.split(" on ")
            date_part, time_part = date_time_part.split(" at ")
            add_reminder("Appointment", f"Doctor {doctor_part.strip()} on {date_part.strip()} at {time_part.strip()}")
            return f"✅ Appointment scheduled with {doctor_part.strip()} on {date_part.strip()} at {time_part.strip()}."
        except:
            return "Sorry, I didn't understand the appointment details. Please say: 'schedule appointment with [doctor] on [date] at [time]'."
    
    elif "list reminders" in content:
        return list_reminders()
    
    elif "exit" in content:
        return "Goodbye! Stay healthy! 👋"
    
    else:
        # fallback: ask agent LLM to respond naturally
        return health_assistant.generate_reply(messages=[msg], sender=user)

if __name__ == "__main__":
    print("🩺 HealthCare Assistant started! Type 'exit' to quit.\n")
    while True:
        user_input = input("You: ")
        if user_input.strip().lower() == "exit":
            print("👋 Chat ended.")
            break
        
        # Create a user message dict as Autogen expects
        user_msg = {"role": "user", "content": user_input}
        
        # Handle message and get reply
        response = handle_user_message(user_msg)
        
        print(f"Assistant: {response}")



```

# Code Agent
```python
from autogen import ConversableAgent, UserProxyAgent

llm_config = {
    "model": "deepseek-r1:1.5b",
    "base_url": "http://localhost:11434/v1",
    "api_key": "ollama",
    "timeout": 120,
}

codebuddy = ConversableAgent(
    name="CodeBuddy",
    llm_config=llm_config,
    system_message="You are CodeBuddy, an expert coding assistant. Answer clearly and concisely.",
)

user = UserProxyAgent(
    name="User",
    human_input_mode="ALWAYS",
    max_consecutive_auto_reply=0,
    code_execution_config={"use_docker": False},
    is_termination_msg=lambda msg: "exit" in msg.get("content", "").lower(),
)

if __name__ == "__main__":
    print("💬 Start chatting with CodeBuddy! Type your question or 'exit' to quit.\n")
    
    while True:
        # Get user input
        user_msg = input("👤 You: ")
        if user_msg.strip().lower() == "exit":
            print("👋 Chat ended.")
            break
        
        # Send user message to agent
        user.initiate_chat(codebuddy, message=user_msg)


```

# LM Studio

# LM Studio (Language Model Studio)

**LM Studio** is a desktop application that allows you to easily run and chat with large language models (LLMs) **locally** on your computer — without needing an internet connection or cloud APIs like OpenAI or Hugging Face.

---

## 🔍 Key Features of LM Studio

### ✅ Run Models Locally
- Load and run LLMs such as **LLaMA**, **Mistral**, **Gemma**, **Phi**, **TinyLlama**, etc., directly on your **CPU or GPU**.
- No need for cloud access or an internet connection.

### 💬 Chat Interface
- Comes with a clean and simple UI to chat with LLMs.
- Easily switch between models.

### 🧩 Use Your Own Models
- Supports the **GGUF format** (used by `llama.cpp` and compatible models).
- Just download a GGUF file and load it via LM Studio.

### 📥 Model Downloader Built-In
- Integrated with **Hugging Face** — browse and download models directly within the app.

### 🧑‍💻 Developer-Friendly
- Provides a **local server with OpenAI-compatible API**.
- Easily connect with tools like **AutoGen**, **CrewAI**, **LangChain**, or your own apps — just like you would with OpenAI API.

---

## 🖥️ System Requirements
- Available for **macOS** (Apple Silicon and Intel), **Windows**, and **Linux**.
- The more RAM/GPU power you have, the larger the models you can run.

---

## 🧠 Example Use Cases
- **Chat privately** with models like LLaMA 3, Mistral, or Phi-2 without sending data to the cloud.
- Use **local LLMs in your own apps** using an OpenAI-style API.
- Experiment with open-source models for research or development.

---

## 📥 How to Download
- Visit the official site: [https://lmstudio.ai](https://lmstudio.ai)
- Download for your operating system.
- Install and start running models locally.

---

## Need Help?
Let me know if you'd like:
- ✅ Help setting it up  
- 🧠 List of best models to use  
- ⚙️ Integration with your own apps (e.g., Flask, FastAPI, etc.)

### Here is sample model for Hugging face
https://huggingface.co/hugging-quants/Llama-3.2-1B-Instruct-Q8_0-GGUF
