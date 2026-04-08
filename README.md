#  Outreachy Fedora Project — RamaLama Exploration

## Introduction

At first glance, this task looked simple: install RamaLama, run a model, and move on.

But very quickly, it turned into something deeper — understanding how AI tools behave under the hood, especially when they don’t cooperate

This documentation captures not just what worked, but also what didn’t, and how I navigated through it.

---

##  Environment

* **OS:** Windows (WSL - Ubuntu)
* **Python:** 3.12.3
* **RamaLama:** 0.18.0

---

##  Step 1: Install RamaLama

### Command:

```bash
sudo apt update
sudo apt install pipx -y
pipx ensurepath
source ~/.bashrc
pipx install ramalama
```

### Output:

```
installed package ramalama 0.18.0, installed using Python 3.12.3
These apps are now globally available
  - ramalama
```

---

##  Step 2: Display Version

### Command:

```bash
ramalama version
```

### Output:

```
ramalama version 0.18.0
```

---

##  Step 3: Pull First Model (Attempt 1 - Hugging Face)

### Command:

```bash
ramalama pull huggingface/google/flan-t5-base
```

### Output:

```
Error: Manifest for flan-t5-base:latest was not found in the Ollama registry
```

---

##  What Happened?

This was unexpected.

I explicitly specified a Hugging Face model, but RamaLama kept defaulting to the Ollama registry.

Even when I tried:

```bash
ramalama --nocontainer pull huggingface/google/flan-t5-base
```

I got the same error.

---

# Decision

Instead of forcing it, I adapted.

Since RamaLama was clearly leaning toward Ollama, I decided to work with Ollama directly.

---

##  Step 4: Install Ollama

### Command:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

### Issue:

```
ERROR: This version requires zstd for extraction
```

### Fix:

```bash
sudo apt install zstd -y
```

---

##  Service Behavior

### Command:

```bash
ollama serve
```

### Output:

```
Error: listen tcp 127.0.0.1:11434: bind: address already in use
```

### Insight:

Ollama was already running in the background after installation.

---

##  Step 5: Pull First Working Model (Ollama)

### Command:

```bash
ollama pull llama3
```

### Output (shortened):

```
pulling manifest
pulling layers...
success
```

---

##  Step 6: Run Model (Direct Ollama)

### Command:

```bash
ollama run llama3
```

### Prompt:

```
What are the Four Foundations of the Fedora Project?
```

### Output:

```
The Four Foundations of the Fedora Project are:

1. Freedom – Fedora is committed to free and open-source software.
2. Friends – Fedora is built by a strong, inclusive community.
3. Features – Fedora focuses on innovation and integrating new technologies.
4. First – Fedora aims to be at the forefront of new developments.
```

---

##  Step 7: Use RamaLama with Ollama

### Pull via RamaLama:

```bash
ramalama pull ollama/llama3
```

### Output:

```
Using Ollama backend
Model already available
```

### Run via RamaLama:

```bash
ramalama run ollama/llama3 "What are the Four Foundations of the Fedora Project?"
```

### Output:

```
Freedom, Friends, Features, and First are the core foundations of the Fedora Project. They emphasize open-source principles, community collaboration, innovation, and leadership in technology.
```

---

##  Step 8: Second Model (Different Attempt)

### Command:

```bash
ollama pull mistral
```

### Run:

```bash
ramalama run ollama/mistral "What are the Four Foundations of the Fedora Project?"
```

### Output:

```
The Fedora Project is guided by four principles: Freedom, Friends, Features, and First. These reflect its commitment to open-source values, collaboration, innovation, and leadership.
```

---

##  Comparison & Analysis

| Model        | Transport    | Behavior | Output Quality |
| ------------ | ------------ | -------- | -------------- |
| flan-t5-base | Hugging Face | Failed   | N/A            |
| llama3       | Ollama       | Smooth   | Detailed       |
| mistral      | Ollama       | Smooth   | Concise        |

---

##  Observations

* RamaLama defaults to Ollama in my environment
* Hugging Face transport did not work as expected
* Ollama models worked consistently and required less effort after setup
* Different models produced similar answers but varied in depth

---

##  Challenges Faced

* Hugging Face model not resolving correctly
* Missing dependency (`zstd`)
* Confusion around Ollama service state
* Understanding transport behavior

---

##  Does RamaLama Make AI “Boring”?

Honestly… yes.

But not in a bad way.

Once everything worked, running models became:

* predictable
* consistent
* almost routine

And that’s powerful.

The chaos was in the setup.
The “boring” came after things finally clicked.

---

##  Final Thoughts

This task taught me more than just commands.

It showed me:

* how tools behave under pressure
* how to debug systematically
* when to adapt instead of forcing a solution

RamaLama does simplify AI workflows, but understanding what happens underneath makes you appreciate that simplicity more.

---

##  Conclusion

I successfully:

* Installed RamaLama
* Verified installation
* Attempted multiple transports
* Debugged failures
* Ran models successfully using Ollama
* Compared outputs and behavior

And most importantly, I now understand the system beyond just surface-level usage.

---

 If anything, this wasn’t just setup… it was initiation.
