Running Ollama

# start ollama
ollama serve

# List installed models
ollama list

# Pull a model
ollama pull codellama

# Remove a model
ollama pull codellama

# Run specific model via interactive chat
ollama run codellama

# Ask for specific tasks
ollama run codellama "Write a Python script to analyze stock prices"

# chat with a model
curl http://localhost:11434/api/chat -d '{"model": "codellama", "messages": [{"role": "user", "content": "5 words about sun"}]}'
