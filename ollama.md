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

curl http://localhost:11434/api/chat -d '{
  "model": "codellama",
  "messages": [{"role": "user", "content": "5 words about sun"}]
}'

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "codellama",
  "prompt": "Write me a function that outputs the fibonacci sequence",
  "stream": false  
}'
```

#oterm - terminal based client for ollama
https://github.com/ggozad/oterm


Reference 

https://ollama.com/search
