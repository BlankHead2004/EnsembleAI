# 🤖 EnsembleAI

<p align="center">
  <img src="https://via.placeholder.com/200x200.png?text=EnsembleAI" alt="EnsembleAI Logo" width="200" height="200">
</p>

<p align="center">
  <b>A powerful orchestration framework for building multi-agent AI systems</b>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#features">Features</a> •
  <a href="#installation">Installation</a> •
  <a href="#usage">Usage</a> •
  <a href="#examples">Examples</a> •
  <a href="#roadmap">Roadmap</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#license">License</a>
</p>

## 📋 Overview

EnsembleAI is a versatile framework for creating and orchestrating multi-agent AI systems. It allows you to define specialized agents with different roles, provide them with tools, and organize them in a workflow. Agents can process information sequentially or in parallel, allowing for complex AI pipelines.

## ✨ Features

- **Agent Orchestration**: Create, manage, and connect multiple AI agents in a single environment
- **Tool Integration**: Equip agents with specialized tools for diverse tasks
- **Pipeline Configuration**: Define agent execution order and data flow between agents
- **Multi-Round Execution**: Run agents through multiple rounds of processing
- **Powerful LLM Support**: Built-in support for Groq LLMs
- **Extensible Architecture**: Easily add custom tools and agents

## 🔧 Tools Library

- **YouTubeTranscriptTool**: Extract and analyze YouTube video transcripts
- **ImageAnalysisTool**: Analyze and extract insights from images
- **WebScrapingTool**: Scrape and analyze web content
- **RAGTool**: Implement Retrieval-Augmented Generation with PDF and CSV documents
- **WikipediaTool**: Retrieve and summarize information from Wikipedia

## 🚀 Installation

### Prerequisites

- Python 3.8+
- pip

### Installation Steps

1. Clone the repository:
```bash
git clone https://github.com/BlankHead2004/EnsembleAI.git
cd EnsembleAI
```

OR

```bash
pip install ensembleai
```

---

## 🔐 Environment Variables

Set up your environment variables:

```bash
export GROQ_API_KEY=your_groq_api_key
export YOUTUBE_API_KEY=your_youtube_api_key  # Only needed for YouTube tool
```

---

## 🎮 Usage

### 🚀 Quick Start

```python
from ensembleai import Environment, LLMModel
from ensembleai.agents import Agent
from ensembleai.tools import RAGTool, WebScrapingTool, WikipediaTool, YouTubeTranscriptTool, ImageAnalysisTool

# Initialize LLM model
model = LLMModel(
    name="llama3-70b-8192",  # or any Groq model
    api_key="your_groq_api_key"
)

# Create agents
researcher = Agent(
    name="Researcher",
    model_instance=model,
    role="research specialist",
    work="gather and analyze information on quantum computing"
)

writer = Agent(
    name="Writer",
    model_instance=model,
    role="content writer",
    work="create engaging content based on research findings"
)

# Add tools to agents
researcher.add_tool("wikipedia", WikipediaTool(topic="quantum computing"))

# Connect agents
researcher.give_to(writer)

# Create environment and run
env = Environment(agents=[researcher, writer], num_rounds=1)
env.start()
```

---

### 🔁 Creating a Pipeline

```python
# Create four different agents
data_collector = Agent(name="DataCollector", model_instance=model, role="data collector", work="collect data from various sources")
analyst = Agent(name="Analyst", model_instance=model, role="data analyst", work="analyze data for insights")
content_creator = Agent(name="ContentCreator", model_instance=model, role="content creator", work="create content from insights")
reviewer = Agent(name="Reviewer", model_instance=model, role="reviewer", work="review and refine content")

# Add tools
data_collector.add_tool("web_scraping", WebScrapingTool(url="https://example.com/data"))
analyst.add_tool("RAG", RAGTool(file_paths=["research.pdf", "data.csv"]))

# Define connections
data_collector.give_to(analyst)
analyst.give_to(content_creator)
content_creator.give_to(reviewer)

# Create environment and run for 2 rounds
env = Environment(agents=[data_collector, analyst, content_creator, reviewer], num_rounds=2)
env.start()
```

---

## 🧩 Examples

### 🎥 YouTube Video Analysis

```python
from ensembleai import Environment, LLMModel
from ensembleai.agents import Agent
from ensembleai.tools import YouTubeTranscriptTool

# Initialize model
model = LLMModel(name="llama3-70b-8192", api_key="your_groq_api_key")

# Create agent
video_analyst = Agent(
    name="VideoAnalyst",
    model_instance=model,
    role="video content analyst",
    work="analyze and summarize the key points from the video"
)

# Add YouTube tool
video_analyst.add_tool(
    "youtube_transcript", 
    YouTubeTranscriptTool(
        api_key="your_youtube_api_key",
        keyword="quantum computing explained",
        channel_name="PBS Space Time"
    )
)

# Run the environment
env = Environment(agents=[video_analyst])
env.start()
```

---

### 🖼️ Image Analysis

```python
from ensembleai import Environment, LLMModel
from ensembleai.agents import Agent
from ensembleai.tools import ImageAnalysisTool

# Initialize model
model = LLMModel(name="claude-3-sonnet-20240229", api_key="your_groq_api_key")

# Create agent
image_analyst = Agent(
    name="ImageAnalyst",
    model_instance=model,
    role="image interpreter",
    work="describe and analyze the contents of the image in detail"
)

# Add image analysis tool
image_analyst.add_tool(
    "image_analysis", 
    ImageAnalysisTool(
        text="What can you see in this image?",
        url="https://example.com/image.jpg"
    )
)

# Run the environment
env = Environment(agents=[image_analyst])
env.start()
```

---

### 🔍 Multi-Agent Research

```python
from ensembleai import Environment, LLMModel
from ensembleai.agents import Agent
from ensembleai.tools import WebScrapingTool, RAGTool, WikipediaTool

# Initialize model
model = LLMModel(name="llama3-70b-8192", api_key="your_groq_api_key")

# Create agents
researcher = Agent(
    name="Researcher",
    model_instance=model,
    role="research specialist",
    work="gather information about climate change"
)

fact_checker = Agent(
    name="FactChecker",
    model_instance=model,
    role="fact checking specialist",
    work="verify information and identify potential inaccuracies"
)

content_creator = Agent(
    name="ContentCreator",
    model_instance=model,
    role="content creator",
    work="create informative content based on verified research"
)

# Add tools
researcher.add_tool("web_scraping", WebScrapingTool(url="https://climate.nasa.gov/"))
researcher.add_tool("wikipedia", WikipediaTool(topic="climate change"))
fact_checker.add_tool("RAG", RAGTool(file_paths=["climate_research.pdf"]))

# Define connections
researcher.give_to(fact_checker)
fact_checker.give_to(content_creator)

# Run the environment
env = Environment(agents=[researcher, fact_checker, content_creator])
env.start()
```

---

## 🛣️ Roadmap

- ✅ Support for more LLM providers  
- 🧰 Additional specialized tools  
- 🌐 Web interface for agent orchestration  
- 💬 Real-time agent interaction  
- 📊 Enhanced visualization of agent workflows  
- 🧠 Memory and state management improvements  

---

## 👥 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create your feature branch:  
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. Commit your changes:  
   ```bash
   git commit -m 'Add some amazing feature'
   ```
4. Push to the branch:  
   ```bash
   git push origin feature/amazing-feature
   ```
5. Open a Pull Request

