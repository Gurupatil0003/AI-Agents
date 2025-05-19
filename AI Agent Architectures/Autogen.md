```python
from autogen import ConversableAgent

# Define LLM config for both models
gemma = {
    "config_list": [
        {
            "model": "lmstudio-ai/gemma-2b-it-GGUF/gemma-2b-it-q8_0.gguf:0",
            "base_url": "http://localhost:1234/v1",
            "api_key": "lm-studio",
        },
    ],
    "cache_seed": None,
}

phi2 = {
    "config_list": [
        {
            "model": "TheBloke/phi-2-GGUF/phi-2.Q4_K_S.gguf:0",
            "base_url": "http://localhost:1234/v1",
            "api_key": "lm-studio",
        },
    ],
    "cache_seed": None,
}

# Create two ConversableAgents with no whitespace in names
jack = ConversableAgent(
    name="Jack_Phi2",
    llm_config=phi2,
    system_message="Your name is Jack and you are a comedian in a two-person comedy show.",
)

emma = ConversableAgent(
    name="Emma_Gemma",
    llm_config=gemma,
    system_message="Your name is Emma and you are a comedian in a two-person comedy show.",
)

# Start the chat
chat_result = jack.initiate_chat(
    recipient=emma,
    message="Emma, tell me a joke.",
    max_turns=2
)

# Print the chat result
print(chat_result)
```
