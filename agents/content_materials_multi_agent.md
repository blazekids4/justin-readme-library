# Media Material Intelligence Platform - POC

An AI-powered platform for analyzing, querying, and extracting insights from large-scale media material databases using concurrent multi-agent processing and intelligent reasoning.

## 🎯 Summary

This POC demonstrates advanced AI agent orchestration for media operations, transforming how content teams interact with material databases. Using Microsoft's Agent Framework, the platform processes 50K+ media records through concurrent AI agents that work in parallel to answer complex queries, analyze distributions, and uncover insights—all in natural language.

**Think of it as:** Ask your media database questions in plain English and get comprehensive, data-driven answers in minutes instead of days.

## 💡 Value Proposition

### For Media Operations Teams

- **⚡ Faster Analysis**: Concurrent agent processing vs traditional sequential methods
- **🧠 Natural Language Querying**: No SQL, no coding—just ask your questions
- **📊 Instant Insights**: Automatic pattern discovery, distribution analysis, and anomaly detection
- **🎯 Comprehensive Answers**: Get statistics, breakdowns, and recommendations in one response

### For Technical Directors

- **🏗️ Production-Ready Architecture**: Built on Microsoft Agent Framework with proven patterns
- **📈 Scalable Design**: Handles datasets of any size through intelligent chunking
- **🔄 Adaptive Processing**: AI determines optimal data partitioning strategy
- **⚙️ Configurable & Extensible**: Easy to customize for specific use cases

### Business Impact

- **Time Savings**: Hours → Minutes for inventory analysis and reporting
- **Better Decisions**: Data-driven insights about material distribution and availability
- **Operational Efficiency**: Reduce manual database queries and spreadsheet analysis
- **Cost Effective**: Leverage existing Azure AI infrastructure

## 🚀 Key Features

### 1. **Concurrent Multi-Agent Processing**

- Multiple AI agents analyze data chunks in parallel
- Automatic fan-out/fan-in orchestration using `ConcurrentBuilder`
- Graceful error handling with agent isolation
- Significantly faster than sequential processing for large datasets

### 2. **Intelligent Data Chunking**

AI-driven analysis determines optimal chunking strategy:

- **Count-based**: Equal-sized chunks for uniform data
- **Title-based**: Group by content title for title queries
- **MediaType-based**: Group by Audio/Video/Timed Text
- **Library-based**: Group by Servicing/Archive classification
- **Hybrid**: Multi-dimensional strategies for complex datasets

### 3. **AI Reasoning & Synthesis**

Master reasoning agent that:

- Synthesizes findings from all chunk analyzers
- Answers queries with aggregated statistics
- Identifies patterns and anomalies automatically
- Provides recommendations based on data analysis

### 4. **Comprehensive Query Support**

Natural language queries across multiple dimensions:

- Inventory counts and distributions
- Status tracking (Available, Processing, etc.)
- Language and format analysis
- Title and library breakdowns
- Cross-cutting filters (e.g., "English 5.1 audio in Servicing")

### 5. **Rich Output Formats**

- **Markdown reports**: Human-readable analysis with tables and insights
- **JSON data**: Structured results for downstream processing
- **Executive summaries**: High-level findings for stakeholders
- **Processing metadata**: Performance metrics and confidence scores

## 📋 Sample Queries & Use Cases

### Inventory Management

```
Query: "What materials are listed in the database? Provide a breakdown by media type."
Result: Complete inventory with Audio, Video, and Timed Text distribution analysis
Use Case: Quarterly inventory audits and reporting
```

### Technical Format Analysis

```
Query: "How many 5.1 audio layouts do we have? What about Stereo?"
Result: Distribution of audio layouts with 5.1, Stereo, and other format percentages
Use Case: Technical requirements planning for distribution
```

### Content Analysis

```
Query: "Which content items have the most materials? Show top 10."
Result: Ranked list with material counts per content item
Use Case: Content prioritization and archival decisions
```

### Language Distribution

```
Query: "Show me the distribution of materials by language."
Result: Language breakdown with primary languages and percentages
Use Case: Localization planning and resource allocation
```

### Availability Status

```
Query: "How many materials are Available vs Processing vs other statuses?"
Result: Status distribution across Available, Processing, and Archived states
Use Case: Operations monitoring and capacity planning
```

### Cross-cutting Filters

```
Query: "English audio files with Dialog purpose in primary library that are Available"
Result: Materials matching all criteria with detailed breakdown
Use Case: Specific material retrieval for delivery projects
```

### Library Operations

```
Query: "Compare materials in primary vs archive libraries"
Result: Library distribution with trend analysis
Use Case: Storage optimization and migration planning
```

## 🏗️ How It Works

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        User Natural Language Query               │
│           "What audio materials are available?"                  │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│  STAGE 1: Data Structure Analysis (JSONAnalyzerExec)            │
│  • Analyzes 50K+ material records                                │
│  • Identifies fields: mediaType, language, status, etc.          │
│  • Determines optimal chunking: count/title/mediaType/library    │
│  • Outputs: Strategy + Chunk size + Rationale                    │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│  STAGE 2: Concurrent Chunk Processing (ChunkAnalyzerExec × N)   │
│                                                                   │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────┐ │
│  │  Chunk 1   │  │  Chunk 2   │  │  Chunk 3   │  │  Chunk N │ │
│  │  Agent     │  │  Agent     │  │  Agent     │  │  Agent   │ │
│  │  Analyzes  │  │  Analyzes  │  │  Analyzes  │  │  Analyzes│ │
│  │  7K records│  │  7K records│  │  7K records│  │  7K recs │ │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘  └────┬─────┘ │
│        │                │                │               │       │
│   Statistics      Statistics       Statistics      Statistics   │
│   Patterns        Patterns         Patterns        Patterns     │
│   Findings        Findings         Findings        Findings     │
└────────┴────────────────┴────────────────┴───────────────┴──────┘
         │                │                │               │
         └────────────────┴────────────────┴───────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│  STAGE 3: Reasoning & Synthesis (ReasoningAgentExec)            │
│  • Aggregates all chunk statistics                               │
│  • Cross-validates findings                                      │
│  • Answers user query with comprehensive data                    │
│  • Generates insights and recommendations                        │
│  • Produces markdown + JSON outputs                              │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Comprehensive Answer                          │
│  • Direct answer to query                                        │
│  • Supporting statistics from 50K+ records                       │
│  • Distribution breakdowns and charts                            │
│  • Key insights and patterns                                     │
│  • Recommendations for action                                    │
└─────────────────────────────────────────────────────────────────┘
```

### Processing Flow

1. **Load Data**: Read media material JSON (56,875 records ≈ 50MB)
2. **Analyze Structure**: AI examines fields, cardinality, distribution
3. **Determine Strategy**: AI recommends chunking approach (e.g., 8 chunks × 7K records)
4. **Create Chunks**: Split data according to optimal strategy
5. **Parallel Processing**: Launch N concurrent chunk analyzer agents
6. **Extract Statistics**: Each agent analyzes its chunk for patterns
7. **Aggregate Results**: Collect findings from all agents
8. **Reason & Synthesize**: Master agent combines insights and answers query
9. **Generate Output**: Create markdown reports, JSON data, summaries

### Technical Implementation

**Framework**: Microsoft Agent Framework

- `Executor` base classes with `@handler` decorators
- `AgentExecutorRequest` / `AgentExecutorResponse` messaging
- `WorkflowContext` for asynchronous output handling
- `ConcurrentBuilder` for parallel orchestration
- `ChatAgent` with custom instructions and tools

**AI Models**: Azure OpenAI (GPT-4.1)

- Structured reasoning for data analysis
- Natural language understanding for queries
- Pattern recognition and anomaly detection

**Performance**:

- **Concurrent Processing**: Suitable for large datasets (50K+ records)
- **Sequential Processing**: Alternative approach for smaller datasets
- **Memory Usage**: Efficient in-memory processing
- **Scalability**: Configurable chunk size for datasets of any size

## 🛠️ Installation & Setup

### Prerequisites

- Python 3.9+
- Azure OpenAI or Azure AI Foundry access
- Azure CLI (for authentication)

### 1. Clone Repository

```bash
git clone https://github.com/your-org/media-material-intelligence-poc.git
cd media-material-intelligence-poc
```

### 2. Create Virtual Environment

```bash
python -m venv venv
.\venv\Scripts\Activate.ps1  # Windows PowerShell
# source venv/bin/activate    # Linux/Mac
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment

Create `.env` file in project root:

```bash
AZURE_AI_PROJECT_ENDPOINT=https://your-project.openai.azure.com
AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4.1
```

### 5. Authenticate with Azure

```bash
az login
```

## 🚀 Usage

### Quick Start - Concurrent Material Analyzer

```bash
cd scripts/concurrent-agents
python concurrent_material_analyzer.py
```

**Interactive Prompt:**

```
🔍 Enter your query (or press Enter for example query 1):
> What materials are listed in the database? Provide a breakdown by media type.
```

**Output Location:**

```
output/material_analysis_20251029_143022/
  ├── final_answer.md           # Comprehensive answer
  ├── chunk_analyses.json       # Detailed chunk data
  └── executive_summary.md      # Processing summary
```

### Command Line Options

```bash
# Use default example query
python concurrent_material_analyzer.py

# Interactive mode (default)
python concurrent_material_analyzer.py --interactive

# Programmatic query
python concurrent_material_analyzer.py --query "How many audio files?"
```

## 📊 Project Structure

```
media-material-intelligence-poc/
├── data/
│   ├── material-data.json                    # Material records dataset
│   ├── rendition-data.json                   # Rendition metadata
│   └── cache/                                # Processing cache
├── scripts/
│   ├── concurrent-agents/
│   │   ├── concurrent_material_analyzer.py   # Main concurrent analyzer
│   │   ├── concurrent_framework_revenue_retrieval.py  # Reference implementation
│   │   ├── README_MATERIAL_ANALYZER.md       # Full documentation
│   │   └── QUICK_START.md                    # Quick reference guide
│   ├── experiments/
│   │   ├── extract_schema_llm.py             # Schema extraction
│   │   ├── json_to_excel_csv.py              # Data export utilities
│   │   └── process_json_for_embedding.py     # Embedding prep
│   ├── foundry-data-agent/
│   │   └── single_foundry_agent.py           # Single-agent implementation
│   └── analyze_material_llm.py               # Material analysis utilities
├── output/
│   └── material_analysis_*/                   # Analysis results by timestamp
├── prompts/
│   └── user_1.J2                             # Jinja2 prompt templates
├── requirements.txt                          # Python dependencies
├── .env                                      # Environment configuration
└── README.md                                 # This file
```

## 🎓 Key Components

### Concurrent Material Analyzer

**Location**: `scripts/concurrent-agents/concurrent_material_analyzer.py`

The flagship implementation demonstrating:

- AI-driven chunking strategy selection
- Concurrent multi-agent processing
- Master reasoning agent for synthesis
- Natural language query interface

**Documentation**:

- `README_MATERIAL_ANALYZER.md` - Complete technical docs
- `QUICK_START.md` - Quick reference for users

### Schema Extraction

**Location**: `scripts/experiments/extract_schema_llm.py`

Analyzes JSON structure to extract:

- Field types and distributions
- Nullable fields and occurrence rates
- Array structures and examples
- Data quality metrics

### Data Export Utilities

**Location**: `scripts/experiments/json_to_excel_csv.py`

Convert JSON analysis results to:

- Excel spreadsheets with formatting
- CSV files for downstream processing
- Custom report templates

## 🔧 Configuration

### Chunking Strategy

Adjust in `concurrent_material_analyzer.py`:

```python
# Target number of chunks (default: 8)
target_chunks = 8  # Line 415

# Minimum chunk size (default: 100)
chunk_size = max(100, total_records // target_chunks)
```

### Agent Instructions

Customize agent behavior:

- **JSONAnalyzerExec**: Modify chunking analysis instructions
- **ChunkAnalyzerExec**: Customize statistics extraction focus
- **ReasoningAgentExec**: Adjust synthesis and query answering approach

### Model Configuration

Update `.env`:

```bash
# Use different model
AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4.1

# Adjust timeout
AGENT_TIMEOUT_SECONDS=120
```

## 📈 Performance Characteristics

### Sample Dataset Performance

*Note: Performance metrics are estimates based on typical dataset characteristics and should be validated in your specific environment.*

| Processing Mode        | Relative Speed | Throughput       | Memory Usage |
| ---------------------- | -------------- | ---------------- | ------------ |
| Concurrent (8 agents)  | Fast           | High             | Moderate     |
| Concurrent (12 agents) | Faster         | Higher           | Moderate     |
| Sequential             | Baseline       | Lower            | Lower        |

### Query Response Characteristics

| Query Type               | Complexity | Relative Response Time |
| ------------------------ | ---------- | ---------------------- |
| Simple count             | Low        | Fast                   |
| Distribution analysis    | Medium     | Moderate               |
| Multi-dimensional filter | High       | Slower                 |
| Top-N ranking            | High       | Slower                 |

_Response times include full data processing, reasoning, and output generation_

**Performance Factors**: Actual performance depends on dataset size, complexity, network conditions, Azure region, current API load, and specific query patterns.

## 🎯 Use Cases by Role

### Content Operations Manager

- **Daily**: Material availability monitoring
- **Weekly**: Status distribution reports for team meetings
- **Monthly**: Inventory audits and trend analysis
- **Quarterly**: Cross-library migration planning

### Technical Director

- **Project Planning**: Format and layout distribution for delivery specs
- **Resource Allocation**: Language and localization capacity planning
- **Quality Assurance**: Anomaly detection in material metadata
- **Optimization**: Storage and archival strategy based on usage patterns

### Media Librarian

- **Cataloging**: Title-based material organization
- **Discovery**: Finding specific materials by multiple criteria
- **Reporting**: Library statistics for stakeholder updates
- **Compliance**: Verifying material completeness for audits

### Data Analyst

- **Exploration**: Understanding dataset characteristics
- **Validation**: Cross-checking material counts and distributions
- **Integration**: Extracting structured data for BI tools
- **Automation**: Building repeatable analysis workflows

## 🔬 Advanced Features

### Custom Chunking Strategies

Implement domain-specific chunking:

```python
def custom_chunking_strategy(data, metadata):
    """Chunk by release date and content type"""
    # Your custom logic here
    return chunks
```

### Query Templates

Pre-built templates for common queries:

```python
QUERY_TEMPLATES = {
    "inventory_audit": "What materials are listed? Breakdown by type, status, and library.",
    "format_analysis": "Analyze audio layouts and video formats. Show distribution.",
    "language_report": "Language distribution across all materials. Include percentages.",
}
```

### Output Customization

Generate custom report formats:

```python
async def generate_custom_report(results, template="executive"):
    # Use Jinja2 templates for custom formatting
    # Export to PDF, Excel, or custom formats
    pass
```

## 🐛 Troubleshooting

### Common Issues

**"AZURE_AI_PROJECT_ENDPOINT not set"**

```bash
# Ensure .env file exists with correct values
# Verify environment variables are loaded
python -c "import os; from dotenv import load_dotenv; load_dotenv(); print(os.getenv('AZURE_AI_PROJECT_ENDPOINT'))"
```

**"Authentication failed"**

```bash
# Re-authenticate with Azure CLI
az login
az account show
```

**"Memory error with large datasets"**

```python
# Reduce chunk count to process smaller portions
target_chunks = 12  # Instead of 8, creates smaller chunks
```

**"Agent timeout"**

```bash
# Increase timeout in .env
AGENT_TIMEOUT_SECONDS=300
```

## 🤝 Contributing

This is a proof-of-concept project. For enhancements:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/enhancement`)
3. Make your changes
4. Test thoroughly with sample queries
5. Commit changes (`git commit -am 'Add enhancement'`)
6. Push to branch (`git push origin feature/enhancement`)
7. Create Pull Request

## 📚 Documentation

- **Quick Start**: `scripts/concurrent-agents/QUICK_START.md`
- **Full Documentation**: `scripts/concurrent-agents/README_MATERIAL_ANALYZER.md`
- **Script Reference**: `scripts/README.md`
- **Agent Framework**: [Microsoft Agent Framework Docs](https://docs.microsoft.com/azure/ai)

## 🔐 Security & Compliance

- Azure AD authentication via Azure CLI
- No API keys stored in code
- Environment variables for configuration
- Secure credential management with Azure Identity
- Data processed in memory, not persisted by default

## 📝 License

This is a proof-of-concept project for demonstration purposes.

## 🙏 Acknowledgments

- **Microsoft Agent Framework**: Concurrent agent orchestration
- **Azure OpenAI**: AI reasoning and natural language processing
- **Azure AI Foundry**: Integrated AI development platform

## 📞 Support

For questions or issues:

- Review documentation in `scripts/concurrent-agents/`
- Check troubleshooting section above
- Open an issue in the repository

---

**Built with** ❤️ **using Microsoft Agent Framework and Azure AI**

**Last Updated**: October 29, 2025
**Version**: 1.0.0
**Status**: Proof of Concept - Production Ready
