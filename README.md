# 🤖 Data Analyst Agent

<div align="center">

![Python](https://img.shields.io/badge/Python-3.12%2B-blue?style=for-the-badge&logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)
![LangChain](https://img.shields.io/badge/LangChain-121212?style=for-the-badge&logo=chainlink)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?style=for-the-badge&logo=google)

**An intelligent autonomous data analysis system that interprets natural language queries and generates executable Python code for data processing, visualization, and statistical analysis**

[Features](#-features) • [Architecture](#-architecture) • [Installation](#-installation) • [Usage](#-usage) • [API Documentation](#-api-documentation)

</div>

---

## 📊 Overview

This project implements a production-ready **autonomous data analysis agent** that bridges the gap between natural language questions and complex data analysis tasks. The system leverages large language models to automatically generate and execute Python code, providing users with insights, visualizations, and statistical computations without requiring programming knowledge.

### 🎯 Key Capabilities

- **Natural Language Processing**: Converts plain English questions into executable Python code
- **Intelligent Code Generation**: Automatically generates data processing pipelines using Pandas and NumPy
- **Advanced Visualization**: Creates charts and graphs with Matplotlib and Seaborn
- **Web Scraping Engine**: Fetches and structures data from URLs (CSV, JSON, HTML tables, Excel)
- **Robust LLM Integration**: Multi-model fallback system with Google Gemini API
- **Sandboxed Execution**: Safe code execution in isolated Python environments
- **Production-Ready API**: FastAPI-based REST API with comprehensive error handling

---

## ✨ Features

### 🧠 Intelligent Code Generation

- **Natural Language Understanding**
  - Interprets user queries written in conversational language
  - Generates Python code for data processing, analysis, and visualization
  - Supports multi-step analytical workflows

- **Dynamic Code Execution**
  - Sandboxed subprocess execution for security
  - Automatic resource cleanup and timeout management
  - Comprehensive error handling with detailed diagnostics

### 🔄 Robust LLM Integration

- **Multi-Model Fallback System**
  - Hierarchical model selection (gemini-2.5-pro → gemini-2.0-flash-lite)
  - Automatic fallback on API failures or quota exhaustion
  - Support for up to 10 API keys with intelligent rotation

- **Quota Management**
  - Real-time quota monitoring and tracking
  - Exponential backoff on rate limits
  - Automatic exclusion of failing keys

### 📁 Advanced Data Processing

- **Multi-Format Support**
  - CSV, Excel (XLS/XLSX), JSON, Parquet
  - HTML tables and web content
  - Image files (PNG, JPG, JPEG) with PIL

- **Intelligent Data Handling**
  - Automatic schema detection and type inference
  - Dynamic DataFrame operations with Pandas
  - Statistical computations with NumPy

### 🌐 Web Scraping Capabilities

- **Custom Scraping Tool**
  - Converts URLs into structured DataFrames
  - Content-type detection with intelligent fallback
  - Support for HTML tables, CSV endpoints, JSON APIs

- **Robust Content Extraction**
  - BeautifulSoup4 for HTML parsing
  - Multiple table extraction from single page
  - Plain text fallback for unstructured content

### 📈 Visualization and Reporting

- **Automatic Chart Generation**
  - Matplotlib and Seaborn integration
  - Base64 image encoding for web delivery
  - Intelligent size optimization (target: <100KB)

- **Image Optimization**
  - Dynamic DPI adjustment
  - Format conversion (PNG → WebP)
  - Quality optimization for bandwidth efficiency

### 🏥 System Health Monitoring

- **Comprehensive Diagnostics**
  - API key validation and quota checking
  - Network connectivity tests
  - System resource monitoring (CPU, memory, disk)

- **Performance Metrics**
  - Environment verification
  - Package integrity checks
  - Database and storage health

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Frontend Layer                         │
│                     (index.html)                            │
├─────────────────────────────────────────────────────────────┤
│  • Responsive Web Interface                                 │
│  • Drag-and-Drop File Upload                                │
│  • Real-time Progress Indicators                            │
│  • Interactive Visualization Display                        │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    FastAPI Backend (app.py)                 │
├─────────────────────────────────────────────────────────────┤
│  Request Handler                                            │
│    ├─ File Upload Processing (multipart/form-data)          │
│    ├─ Question Parsing and Validation                       │
│    ├─ Dataset Preprocessing (Pandas)                        │
│    └─ Error Handling and Response Formatting                │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                   LLM Agent Layer (LangChain)               │
├─────────────────────────────────────────────────────────────┤
│  LLMWithFallback                                            │
│    ├─ Model Hierarchy (Pro → Flash → Lite)                  │
│    ├─ Multi-Key Rotation (up to 10 keys)                    │
│    ├─ Quota Tracking and Backoff                            │
│    └─ Error Logging and Diagnostics                         │
│                                                             │
│  Agent Executor                                             │
│    ├─ Prompt Engineering                                    │
│    ├─ Tool-Calling Integration                              │
│    ├─ JSON Response Parsing                                 │
│    └─ Code Generation Output                                │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                 Code Execution Pipeline                     │
├─────────────────────────────────────────────────────────────┤
│  Preprocessing                                              │
│    ├─ DataFrame Pickling (if dataset uploaded)              │
│    ├─ URL Scraping (if external data needed)                │
│    └─ Context Injection                                     │
│                                                             │
│  Sandboxed Execution                                        │
│    ├─ Temporary Python File Generation                      │
│    ├─ Subprocess Isolation (timeout: 240s)                  │
│    ├─ Plot Helper Injection (plot_to_base64)                │
│    └─ JSON Result Extraction                                │
│                                                             │
│  Cleanup                                                    │
│    ├─ Temporary File Deletion                               │
│    ├─ Pickle Cleanup                                        │
│    └─ Resource Release                                      │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                      Response Layer                         │
├─────────────────────────────────────────────────────────────┤
│  • Type Casting (number, string, integer)                   │
│  • Key Mapping (question → output key)                      │
│  • Base64 Image Encoding                                    │
│  • JSON Response Formatting                                 │
└─────────────────────────────────────────────────────────────┘
```

### 🔧 Model Hierarchy

```python
MODEL_HIERARCHY = [
    "gemini-2.5-pro",        # Highest capability, best reasoning
    "gemini-2.5-flash",      # Balanced performance and speed
    "gemini-2.5-flash-lite", # Lightweight, faster responses
    "gemini-2.0-flash",      # Fast inference
    "gemini-2.0-flash-lite"  # Fastest, cost-effective
]
```

---

## 🚀 Installation

### Prerequisites

- **Python 3.12+**
- **pip** (Python package manager)
- **Git**
- **Google Gemini API Key** ([Get one here](https://ai.google.dev))

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/TDS-PROJECT-2.git
cd TDS-PROJECT-2
```

### Step 2: Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Environment Configuration

Create a `.env` file in the project root:

```env
# Google Gemini API Keys (minimum 1, recommended 2+ for fallback)
gemini_api_1=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
gemini_api_2=AIzaSyYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
gemini_api_3=AIzaSyZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
# ... up to gemini_api_10

# Optional Configuration
LLM_TIMEOUT_SECONDS=240
PORT=8000
```

> **Note**: If you only have one API key, you can copy it to all variables. However, using at least 2 different keys is recommended for the fallback mechanism to work effectively.

---

## 💻 Usage

### 1️⃣ Start the Server

```bash
uvicorn app:app --host 0.0.0.0 --port 8000
```

Or run directly:

```bash
python app.py
```

**Server will be available at**: `http://localhost:8000`

### 2️⃣ Access the Web Interface

Open your browser and navigate to:

```
http://localhost:8000
```

### 3️⃣ Prepare Your Questions File

Create a `.txt` file with your questions and expected output format:

```text
1. What is the average sales revenue for Q4?
2. Which product has the highest sales volume?
3. Create a bar chart showing monthly sales trends.

Expected Output Format:
- `avg_revenue`: number
- `top_product`: string
- `sales_chart`: string
```

### 4️⃣ Upload and Analyze

1. Click on "Questions File" and upload your `.txt` file
2. (Optional) Upload your dataset (CSV, Excel, JSON, Parquet)
3. Click "🚀 Analyze Data"
4. Wait for processing (typically 10-60 seconds)
5. View results with visualizations

---

## 📡 API Documentation

### **POST /api**

Submit questions and optional dataset for AI-powered analysis.

**Request**:
```bash
curl -X POST http://localhost:8000/api \
  -F "questions_file=@questions.txt" \
  -F "data_file=@sales_data.csv"
```

**Request Body** (multipart/form-data):
- `questions_file` (required): `.txt` file with questions
- `data_file` (optional): Dataset (CSV, Excel, JSON, Parquet, images)

**Response**:
```json
{
  "avg_revenue": 1250000.50,
  "top_product": "Widget Pro",
  "sales_chart": "iVBORw0KGgoAAAANSUhEUgAAAA..."
}
```

**Status Codes**:
- `200`: Success
- `400`: Invalid input (missing questions file, unsupported format)
- `408`: Processing timeout
- `500`: Server error

---

### **GET /api**

Health check endpoint to verify server status.

**Request**:
```bash
curl http://localhost:8000/api
```

**Response**:
```json
{
  "ok": true,
  "message": "Server is running. Use POST /api with 'questions_file' and optional 'data_file'."
}
```

---

### **GET /summary**

Comprehensive system diagnostics and health monitoring.

**Request**:
```bash
curl http://localhost:8000/summary
```

**With Extended Checks**:
```bash
curl "http://localhost:8000/summary?full=true"
```

**Response**:
```json
{
  "status": "ok",
  "server_time": "2025-11-27T10:30:45Z",
  "summary": {
    "failed_checks": []
  },
  "checks": {
    "env": {
      "status": "ok",
      "result": {
        "GOOGLE_API_KEY": {"present": true, "masked": "AIza...XXXX"},
        "LLM_TIMEOUT_SECONDS": "240"
      }
    },
    "system": {
      "status": "ok",
      "result": {
        "platform": "Darwin",
        "python_version": "3.12.0",
        "cpu_logical_cores": 8,
        "memory_total_gb": 16.0,
        "cwd_free_gb": 245.3
      }
    },
    "network": {
      "status": "ok",
      "result": {
        "Google AI": {"ok": true, "status_code": 200, "latency_ms": 145},
        "GitHub": {"ok": true, "status_code": 200, "latency_ms": 89}
      }
    },
    "llm_keys_models": {
      "status": "ok",
      "result": {
        "models_tested": [
          {
            "model": "gemini-2.5-pro",
            "attempts": [
              {
                "key_mask": "AIza...XXXX",
                "ok": true,
                "summary": "pong"
              }
            ]
          }
        ]
      }
    }
  },
  "elapsed_seconds": 3.42
}
```

---

### **GET /**

Serves the main web interface (HTML frontend).

**Request**:
```bash
curl http://localhost:8000/
```

**Response**: HTML page with interactive file upload interface

---

## 🐍 Programmatic Usage

### Python Example

```python
import requests
import json

# Prepare files
files = {
    'questions_file': open('questions.txt', 'rb'),
    'data_file': open('sales_data.csv', 'rb')
}

# Send request
response = requests.post('http://localhost:8000/api', files=files)

# Process results
if response.status_code == 200:
    results = response.json()
    
    for key, value in results.items():
        print(f"{key}: {value}")
        
        # Handle images
        if isinstance(value, str) and value.startswith('iVBOR'):
            import base64
            with open(f'{key}.png', 'wb') as f:
                f.write(base64.b64decode(value))
            print(f"  → Saved as {key}.png")
else:
    print(f"Error: {response.status_code}")
    print(response.json())
```

### JavaScript Example

```javascript
const formData = new FormData();
formData.append('questions_file', questionsFile);
formData.append('data_file', dataFile);

fetch('http://localhost:8000/api', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(data => {
  console.log('Results:', data);
  
  // Display images
  for (const [key, value] of Object.entries(data)) {
    if (typeof value === 'string' && value.length > 200) {
      const img = document.createElement('img');
      img.src = `data:image/png;base64,${value}`;
      document.body.appendChild(img);
    }
  }
})
.catch(error => console.error('Error:', error));
```

---

## ⚙️ Configuration

### Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `gemini_api_1` to `gemini_api_10` | Google Gemini API keys for load balancing | None | ✅ (at least 1) |
| `LLM_TIMEOUT_SECONDS` | Maximum execution time for LLM operations (seconds) | 240 | ❌ |
| `PORT` | Application port number | 8000 | ❌ |

### Retrieval Parameters (app.py)

```python
LLM_TIMEOUT_SECONDS = 240      # Maximum execution time
MAX_RETRIES_PER_KEY = 2        # Retry attempts per API key
TIMEOUT = 30                   # Individual API call timeout
```

### Execution Parameters

```python
# Code Execution
EXECUTION_TIMEOUT = 60         # Subprocess timeout (seconds)

# Image Optimization
MAX_IMAGE_SIZE = 100_000       # Target image size (bytes)
DEFAULT_DPI = 100              # Initial plot DPI
MIN_DPI = 20                   # Minimum DPI for compression
```

---

## 📚 Example Queries

### Example 1: Statistical Analysis

**questions.txt**:
```text
1. Calculate the mean, median, and standard deviation of the 'price' column
2. Identify outliers in the dataset using IQR method
3. Create a histogram of price distribution

Output Format:
- `statistics`: string
- `outliers_count`: integer
- `histogram`: string
```

**Expected Response**:
```json
{
  "statistics": "Mean: 45.67, Median: 42.30, Std Dev: 12.45",
  "outliers_count": 7,
  "histogram": "iVBORw0KGgoAAAANSUhEUgAAAA..."
}
```

---

### Example 2: Web Scraping

**questions.txt**:
```text
1. Scrape the population data from https://example.com/demographics
2. What is the total population across all cities?
3. Which city has the highest population density?

Output Format:
- `total_population`: number
- `highest_density_city`: string
- `density_value`: number
```

**No dataset upload needed** - system will automatically scrape the URL.

---

### Example 3: Correlation Analysis

**questions.txt**:
```text
1. Calculate the correlation between 'temperature' and 'sales'
2. Create a scatter plot showing the relationship
3. Is the correlation statistically significant at p<0.05?

Output Format:
- `correlation_coefficient`: number
- `scatter_plot`: string
- `is_significant`: string
```

---

## 🎨 Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Web Framework** | FastAPI | REST API server with async support |
| **LLM Integration** | LangChain | Agent framework and tool orchestration |
| **Language Model** | Google Gemini | Code generation and natural language understanding |
| **Data Processing** | Pandas, NumPy | DataFrame operations and numerical computing |
| **Visualization** | Matplotlib, Seaborn | Chart generation and statistical plotting |
| **Web Scraping** | BeautifulSoup4, Requests | HTML parsing and content extraction |
| **Image Processing** | Pillow (PIL) | Image format conversion and optimization |
| **Graph Analysis** | NetworkX | Network and graph data structures |
| **HTTP Server** | Uvicorn | ASGI server for FastAPI |
| **System Monitoring** | psutil | Resource monitoring (CPU, memory, disk) |
| **HTTP Client** | httpx | Async HTTP requests for diagnostics |
| **Environment** | python-dotenv | Configuration management |
| **File Formats** | openpyxl, pyarrow | Excel and Parquet support |

---

## 📊 Performance Considerations

### Query Performance

- **Typical Analysis Time**: 10-60 seconds end-to-end
  - Question parsing: ~500ms
  - Code generation: 5-15s (LLM dependent)
  - Code execution: 3-30s (complexity dependent)
  - Image optimization: 1-5s

- **Timeout Limits**:
  - LLM operations: 240s (configurable)
  - Code execution: 60s per subprocess
  - Individual API calls: 30s

### Resource Usage

- **Memory**: ~200-500MB baseline, +100MB per concurrent request
- **CPU**: Burst during code execution and image processing
- **Disk**: Temporary files (~1-10MB per request, auto-cleaned)

### Optimization Tips

- Use multiple API keys to distribute load
- Reduce dataset size for faster processing
- Set appropriate timeouts based on query complexity
- Monitor `/summary` endpoint for system health

---

## 🐳 Deployment

### Docker Deployment

**Build Image**:
```bash
docker build -t tds-data-analyst .
```

**Run Container**:
```bash
docker run -d \
  -p 8000:8000 \
  --env-file .env \
  --name tds-agent \
  tds-data-analyst
```

**View Logs**:
```bash
docker logs -f tds-agent
```

---

### Railway Deployment

#### Option A: Dashboard

1. Visit [railway.app](https://railway.app) and sign in
2. Click **New Project** → **Deploy from GitHub**
3. Select your repository
4. Add environment variables in **Variables** tab
5. Railway will auto-deploy from Dockerfile

#### Option B: CLI

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login and deploy
railway login
railway init
railway link
railway up
```

#### Configure Variables in Railway

Navigate to your project → **Variables** tab and add:

```
gemini_api_1=your_key_here
gemini_api_2=your_key_here
LLM_TIMEOUT_SECONDS=240
PORT=8000
```

See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) for detailed deployment instructions.

---

## 🗂️ Project Structure

```
TDS-PROJECT-2/
├── app.py                      # FastAPI application and core logic
│   ├── LLMWithFallback         # Multi-model/multi-key LLM wrapper
│   ├── Agent Setup             # LangChain agent configuration
│   ├── API Routes              # /api, /summary, /health endpoints
│   └── Diagnostics             # System health monitoring
│
├── index.html                  # Frontend web interface
│   ├── File Upload UI          # Drag-and-drop interface
│   ├── Progress Indicators     # Loading animations
│   └── Results Display         # Table and image rendering
│
├── requirements.txt            # Python dependencies
├── Dockerfile                  # Container configuration
├── entrypoint.sh              # Container startup script
├── Procfile                   # Railway/Heroku deployment
├── runtime.txt                # Python version specification
├── .env                       # Environment variables (create this)
├── DEPLOYMENT_GUIDE.md        # Detailed deployment instructions
├── LICENSE                    # MIT License
└── README.md                  # This file
```

---

## 🛠️ Troubleshooting

### Common Issues

**API Key Errors**
```
Error: "All models/keys failed"
```
**Solution**: 
- Verify keys in `.env` file
- Check key validity at [Google AI Studio](https://aistudio.google.com)
- Ensure keys have sufficient quota

---

**Timeout Errors**
```
Error: "Processing timeout"
```
**Solution**:
- Increase `LLM_TIMEOUT_SECONDS` in `.env`
- Simplify questions or reduce dataset size
- Check `/summary` endpoint for system health

---

**Import Errors**
```
ModuleNotFoundError: No module named 'langchain'
```
**Solution**:
```bash
pip install -r requirements.txt
```

---

**Port Already in Use**
```
Error: [Errno 48] Address already in use
```
**Solution**:
```bash
# Find and kill process
lsof -ti:8000 | xargs kill -9

# Or use different port
uvicorn app:app --port 8080
```

---

### Debug Mode

Enable verbose logging by setting log level:

```python
# In app.py
import logging
logging.basicConfig(level=logging.DEBUG)
```

Or set environment variable:
```bash
export LOG_LEVEL=DEBUG
python app.py
```

---

## 🎓 Use Cases

- **Educational Data Analysis**: Teaching data science without coding barriers
- **Business Intelligence**: Ad-hoc analysis for non-technical stakeholders
- **Research Assistance**: Quick exploratory data analysis
- **Automated Reporting**: Scheduled analysis with natural language queries
- **Prototyping**: Rapid data exploration and hypothesis testing

---

## 🚧 Future Enhancements

- [ ] **Multi-LLM Support**: Add OpenAI, Anthropic, Cohere providers
- [ ] **Vector Database**: Integrate Pinecone/Weaviate for large datasets
- [ ] **Streaming Responses**: WebSocket support for real-time progress
- [ ] **Query History**: Store and retrieve past analyses
- [ ] **Advanced Caching**: Redis layer for frequently asked questions
- [ ] **Collaborative Features**: Multi-user sessions and shared workspaces
- [ ] **Custom Functions**: User-defined Python functions in sandbox
- [ ] **Data Validation**: Automatic data quality checks and suggestions
- [ ] **Export Formats**: PDF, PowerPoint, and Word report generation
- [ ] **Scheduled Analysis**: Cron-based automated reports
- [ ] **Authentication**: User accounts and API key management
- [ ] **Rate Limiting**: Per-user quotas and request throttling




</div>
