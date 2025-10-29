# AI Agent Scripts for Financial Data Extraction

## Overview

This directory contains four distinct AI agent implementations designed to extract financial data and business information from various sources. Each script represents a different architectural approach to solving the same fundamental problem: **finding and extracting comprehensive financial data about companies from web sources and PDF documents**.

These scripts leverage Azure AI services, including Azure AI Foundry agents, Bing Search, Azure Document Intelligence, and Mistral AI for advanced document analysis.

---

## 📁 Script Summary

| Script | Type | Agents | Key Features | Best For |
|--------|------|--------|--------------|----------|
| `single_browser_auto.py` | Single Agent | 1 | Browser automation, interactive navigation | Dynamic web scraping with JavaScript |
| `sequential_pdf_extraction.py` | Sequential Multi-Agent | 4 | Step-by-step processing, Mistral OCR | Complex workflows requiring ordered steps |
| `concurrent_framework_revenue_retrieval.py` | Concurrent Multi-Agent | 5 | Parallel processing, multiple validation sources | Fast, comprehensive data gathering |
| `concurrent_simple_financial_data_extractor.py` | Concurrent Multi-Agent | 3 | Simplified workflow, comprehensive extraction | Quick financial overviews |

---

## 🤖 Detailed Script Descriptions

### 1. Single Browser Automation Agent (`single_browser_auto.py`)

**Location**: `scripts/agents/singe-agents/single_browser_auto.py`

#### Purpose
A single-agent implementation that uses browser automation (Playwright) to navigate websites, handle dynamic content, and extract information through simulated human interaction.

#### How It Works
1. Creates or reuses a persistent agent with browser automation capabilities
2. Uses Playwright to navigate to specified URLs
3. Handles cookie notifications, bot verification, and page interactions
4. Waits for page loads and extracts content interactively
5. Can navigate through multiple pages sequentially
6. Saves responses as timestamped markdown files

#### Key Features
- **Browser Automation**: Uses Playwright connection for JavaScript-heavy sites
- **Persistent Agent**: Saves agent configuration to avoid recreating on each run
- **Interactive Navigation**: Can click through multiple announcements or pages
- **Bot Detection Handling**: Includes instructions for handling access restrictions
- **Executive Summaries**: Generates summaries of visited content with statistics

#### When to Use
- Websites requiring JavaScript execution
- Content behind interactive elements (buttons, dropdowns)
- Sites with cookie consent or bot verification
- When you need to navigate through paginated content
- Extracting content from investor relations pages with announcements

#### Limitations
- Slower than API-based approaches (waits for page loads)
- May encounter bot detection on some sites
- Single-threaded execution
- Requires Playwright connection setup
- 5-minute timeout for complex navigations

#### Example Use Case
```python
# Navigate to investor relations, find latest financial announcements,
# click through top 5, and provide executive summaries with financials
message = "Visit https://www.kantar.com/campaigns/investor-relations..."
```

---

### 2. Sequential PDF Extraction Agent (`sequential_pdf_extraction.py`)

**Location**: `scripts/agents/sequential-agents/sequential_pdf_extraction.py`

#### Purpose
A sophisticated four-agent sequential workflow that processes financial data extraction in ordered steps, with advanced PDF analysis using Mistral Document AI OCR.

#### Architecture
```
Step 1: Entity Disambiguator
    ↓
Step 2: Revenue Retriever (finds and downloads PDFs)
    ↓
Step 3: Mistral PDF Analyzer (OCR + AI reasoning)
    ↓
Step 4: Validator (synthesizes and validates)
```

#### How It Works
1. **Entity Disambiguator**: Identifies the exact legal entity, jurisdiction, and business identifiers
2. **Revenue Retriever**: Searches official investor relations sites for PDF annual reports
3. **Mistral PDF Analyzer**: 
   - Converts PDFs to base64
   - Uses Mistral Document AI endpoint for OCR extraction
   - Applies AI reasoning to extract revenue with context
   - Handles up to 30 pages per document
4. **Validator**: Cross-references findings, calculates confidence scores, detects red flags

#### Key Features
- **True Sequential Processing**: Each step waits for previous completion (no race conditions)
- **Mistral Document AI Integration**: Advanced OCR with reasoning capabilities
- **PDF Download and Storage**: Saves PDFs to local directory for analysis
- **Red Flag Detection**: Identifies discrepancies and data quality issues
- **Multi-source Validation**: Cross-checks findings across different sources
- **Confidence Scoring**: Provides numerical confidence metrics
- **Structured Output**: Executive summary + full JSON + README per run

#### When to Use
- When accuracy is more important than speed
- Complex entity disambiguation required (private companies, subsidiaries)
- Documents with poor OCR quality (handwriting, complex layouts)
- When you need detailed audit trails of the extraction process
- Financial due diligence requiring high confidence levels

#### Output Structure
```
data/results/COMPANY_FY2023_TIMESTAMP_ENHANCED/
├── executive_summary.md      # Concise findings
├── full_results.json          # Complete agent outputs
└── README.md                  # Run documentation
```

#### Limitations
- Slower than concurrent approaches (sequential processing)
- Requires Azure API key for Mistral endpoint
- Limited to 30 pages per PDF (configurable)
- Higher cost due to Mistral AI usage
- May timeout on very large documents

---

### 3. Concurrent Framework Revenue Retrieval (`concurrent_framework_revenue_retrieval.py`)

**Location**: `scripts/agents/concurrent-agents/concurrent_framework_revenue_retrieval.py`

#### Purpose
A comprehensive five-agent concurrent system that processes multiple data sources simultaneously for maximum speed and thoroughness.

#### Architecture
```
           ┌─────────────────────────┐
           │   User Query: Company   │
           └───────────┬─────────────┘
                       ↓
        ┌──────────────┴──────────────┐
        │   Concurrent Execution      │
        └──────────────┬──────────────┘
                       ↓
    ┌──────────┬───────┴────────┬──────────┐
    ↓          ↓                ↓          ↓
┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
│ Entity  │ │Struct.  │ │  PDF    │ │ Mistral │
│Disambig.│ │ Data    │ │ Search  │ │   PDF   │
└────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘
     └───────────┼───────────┼───────────┘
                 ↓
            ┌─────────┐
            │Validator│
            └────┬────┘
                 ↓
         Consolidated Results
```

#### How It Works
1. **Entity Disambiguator**: Identifies legal entity with jurisdictional information
2. **Structured Data Retriever**: Searches SEC EDGAR, Companies House, regulatory databases
3. **PDF Search & Extraction**: Finds and analyzes PDFs using Azure Document Intelligence
4. **Mistral PDF Analyzer**: Runs in parallel with Azure Doc Intel for comparison
5. **Validator**: Synthesizes results from all sources with reliability scoring

All agents (except validator) run **concurrently** and their results are aggregated.

#### Key Features
- **Concurrent Processing**: Multiple agents work simultaneously
- **Dual PDF Analysis**: Both Azure Document Intelligence AND Mistral AI
- **Source Reliability Classification**: Primary/Secondary/Tertiary source ranking
- **Caching System**: Disk-based cache for API calls (7-day expiration)
- **Comprehensive Coverage**: Web search + structured databases + PDF analysis
- **Advanced Metrics**: Confidence scores, source analysis, validation results
- **Structured JSON Output**: Machine-readable results with full metadata

#### When to Use
- When speed is critical (parallel processing)
- Need maximum data coverage from multiple sources
- Comparing results across different extraction methods
- Building automated pipelines requiring reliability scoring
- Public companies with SEC filings and official reports
- When you want both Azure and Mistral PDF analysis for comparison

#### Data Structures
```python
@dataclass
class RevenueResult:
    company: str
    fiscal_year: int
    revenue_amount: Optional[float]
    currency: str
    source_type: SourceType
    source_url: str
    confidence_score: float
    extraction_method: str
```

#### Performance
- **Typical Runtime**: 2-4 minutes for comprehensive analysis
- **Agents Running**: 4-5 concurrent agents
- **Sources Checked**: 10-20+ sources across web, PDFs, and databases
- **Caching**: Reduces repeat queries by 70-90%

#### Limitations
- More complex setup (requires Document Intelligence + Mistral endpoints)
- Higher Azure resource consumption (concurrent API calls)
- Can be overwhelming for simple queries
- Requires careful rate limit management

---

### 4. Concurrent Simple Financial Data Extractor (`concurrent_simple_financial_data_extractor.py`)

**Location**: `scripts/agents/concurrent-agents/concurrent_simple_financial_data_extractor.py`

#### Purpose
A streamlined three-agent concurrent system focused on extracting **ALL** financial data comprehensively without over-engineering.

#### Architecture
```
┌─────────────────────────────────────┐
│      Financial Data Finder          │
│  (Comprehensive web search)         │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│   PDF Discovery & Analysis          │
│  (Find and analyze all PDFs)        │
└──────────────┬──────────────────────┘
               ↓
┌─────────────────────────────────────┐
│      Data Synthesizer               │
│  (Consolidate into clean JSON)      │
└─────────────────────────────────────┘
```

#### How It Works
1. **Financial Data Finder**: 
   - Comprehensive Bing searches for any financial data
   - Extracts revenue, profit, assets, employees, margins
   - Gathers business descriptions and company profiles
   
2. **PDF Discovery & Analysis**:
   - Searches for ALL PDF types (annual reports, presentations, filings)
   - Downloads and analyzes using Azure Document Intelligence
   - Extracts both financial data AND ancillary information
   
3. **Data Synthesizer**:
   - Consolidates findings from both agents
   - Creates clean, structured JSON output
   - Provides executive summary

#### Key Features
- **Comprehensive Extraction**: Captures ALL financial metrics, not just revenue
- **Simplified Workflow**: Only 3 agents vs 5 in full framework
- **Clean JSON Output**: Well-structured company profile + financial data
- **Ancillary Information**: Business activities, services, management details
- **Multiple PDF Types**: Annual reports, investor decks, fact sheets, sustainability reports
- **Source Diversity**: Web pages, news, industry reports, regulatory filings

#### Output Format
```json
{
  "company_profile": {
    "legal_name": "Company Inc.",
    "jurisdiction": "Delaware, USA",
    "industry": "Technology Services",
    "business_description": "..."
  },
  "financial_data": {
    "revenue": [{"value": "15.6 billion USD", "year": 2023, "source": "..."}],
    "profit": [...],
    "assets": [...],
    "other_metrics": [
      {"metric": "Employees", "value": "35,000", "year": 2023},
      {"metric": "EBITDA", "value": "...", "year": 2023}
    ]
  },
  "ancillary_information": {
    "business_activities": [...],
    "services_offered": [...],
    "key_people": [...]
  },
  "sources": {
    "total_sources": 15,
    "pdfs_analyzed": [...]
  }
}
```

#### When to Use
- Quick financial overviews needed
- Want comprehensive data without complex validation
- Need business context beyond just financials
- Simpler setup requirements (only need Document Intelligence)
- Building dashboards or reports requiring structured JSON
- Researching companies with limited public information

#### Advantages Over Full Framework
- **Faster Setup**: Fewer dependencies (no Mistral required)
- **Cleaner Output**: Focused on usability over academic rigor
- **Lower Cost**: Only uses Azure Document Intelligence, not dual PDF analysis
- **Better for Discovery**: Finds more diverse information types
- **Simpler Maintenance**: Less code complexity

#### Limitations
- No advanced OCR (only Azure Document Intelligence)
- Less rigorous validation
- No source reliability classification
- Limited to 5 PDFs per run (configurable)
- No entity disambiguation step

---

## 🔧 Technical Requirements

### Common Dependencies
```bash
# Core packages
pip install azure-identity azure-ai-projects azure-ai-agents
pip install agent-framework agent-framework-azure-ai
pip install python-dotenv aiohttp beautifulsoup4
pip install diskcache

# For PDF processing
pip install PyPDF2

# For Document Intelligence (concurrent scripts)
pip install azure-ai-documentintelligence
```

### Environment Variables
```env
# Required for all scripts
AZURE_AI_PROJECT_ENDPOINT=https://your-project.services.ai.azure.com/api/projects/your-project
AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o
BING_CONNECTION_ID=your-bing-connection-id

# For single_browser_auto.py
PLAYWRIGHT_CONNECTION_NAME_FOUNDRY=your-playwright-connection
MODEL_NAME=gpt-4o

# For sequential_pdf_extraction.py
AZURE_API_KEY=your-mistral-api-key
MISTRAL_OCR_ENDPOINT=https://your-mistral-endpoint

# For concurrent scripts
DOCUMENT_INTELLIGENCE_ENDPOINT=https://your-doc-intel.cognitiveservices.azure.com/
DOCUMENT_INTELLIGENCE_KEY=your-doc-intel-key
```

---

## 📊 Comparison Matrix

| Feature | Browser Auto | Sequential | Concurrent Full | Concurrent Simple |
|---------|-------------|------------|-----------------|-------------------|
| **Number of Agents** | 1 | 4 | 5 | 3 |
| **Processing Model** | Single | Sequential | Concurrent | Concurrent |
| **Typical Runtime** | 3-5 min | 5-8 min | 2-4 min | 2-3 min |
| **PDF Analysis** | ❌ | Mistral AI | Dual (Azure + Mistral) | Azure Only |
| **Entity Validation** | ❌ | ✅ Advanced | ✅ Advanced | ❌ Basic |
| **Source Diversity** | Web pages | Official reports | Maximum | High |
| **Browser Automation** | ✅ | ❌ | ❌ | ❌ |
| **Confidence Scoring** | ❌ | ✅ | ✅ | ✅ |
| **Setup Complexity** | Medium | High | High | Low |
| **Cost** | Low | High | Highest | Medium |
| **Best Accuracy** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Best Speed** | ⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Output Format** | Markdown | MD + JSON | MD + JSON | JSON |

---

## 🎯 Use Case Recommendations

### Choose **Browser Automation** when:
- Target site requires JavaScript/dynamic loading
- Content behind interactive elements
- Need to simulate human navigation patterns
- Scraping investor relations announcements
- Bot detection is a concern

### Choose **Sequential Processing** when:
- Maximum accuracy is critical
- Entity disambiguation is complex (private companies)
- Need detailed audit trail
- Processing sensitive financial due diligence
- OCR quality is poor (scanned documents)

### Choose **Concurrent Full Framework** when:
- Speed is critical
- Need maximum data coverage
- Want validation across multiple sources
- Building production pipelines
- Comparing extraction methods (Azure vs Mistral)

### Choose **Concurrent Simple** when:
- Need quick financial overview
- Want comprehensive data types (not just revenue)
- Building dashboards or reports
- Budget constraints (lower API costs)
- Simpler deployment requirements

---

## 🚀 Getting Started

### Quick Start - Browser Automation
```bash
cd scripts/agents/singe-agents
python single_browser_auto.py
```

### Quick Start - Sequential Processing
```bash
cd scripts/agents/sequential-agents
python sequential_pdf_extraction.py
```

### Quick Start - Concurrent Processing
```bash
cd scripts/agents/concurrent-agents
# Full framework
python concurrent_framework_revenue_retrieval.py

# Simplified version
python concurrent_simple_financial_data_extractor.py
```

---

## 📂 Output Structure

All scripts save results to: `data/results/COMPANY_FY{YEAR}_{TIMESTAMP}_{TYPE}/`

### Browser Automation Output
```
agent_response_{timestamp}.md           # Single markdown file with findings
```

### Sequential Processing Output
```
COMPANY_FY2023_TIMESTAMP_ENHANCED/
├── executive_summary.md                # Concise findings
├── full_results.json                   # Complete agent conversation
└── README.md                           # Run documentation
```

### Concurrent Processing Output
```
COMPANY_FY2023_TIMESTAMP_CONCURRENT/
├── consolidated_results.md             # All agent outputs
├── structured_results.json             # Machine-readable data
├── executive_summary.md                # Key findings
└── README.md                           # Analysis documentation
```

---

## 🔍 Key Innovations

### 1. Multi-Agent Architecture
All scripts (except browser automation) use the `agent_framework` pattern with specialized agents:
- **Separation of Concerns**: Each agent has one specific job
- **Composability**: Agents can be reused across workflows
- **Testability**: Individual agent behavior can be validated

### 2. Dual PDF Analysis
The full concurrent framework uses both Azure Document Intelligence AND Mistral AI:
- **Cross-validation**: Compare results from two different OCR engines
- **Accuracy**: Catch extraction errors by comparing outputs
- **Flexibility**: Fall back to alternative if one fails

### 3. Intelligent Caching
Sequential and concurrent scripts use disk-based caching:
- **Performance**: Avoid repeat API calls
- **Cost Savings**: Reduce Azure consumption
- **Development**: Faster iteration during testing

### 4. Source Reliability Classification
The full framework classifies sources by reliability:
- **Primary Structured**: SEC XBRL, regulatory APIs (highest confidence)
- **Primary PDF**: Official annual reports
- **Secondary**: News articles, industry reports
- **Tertiary**: Aggregators, third-party databases

---

## 🛠️ Advanced Configuration

### Adjusting PDF Processing Limits
```python
# In concurrent_simple_financial_data_extractor.py
for i, url in enumerate(pdf_urls[:5], 1):  # Change 5 to desired limit
```

### Modifying Cache Duration
```python
# In concurrent scripts
cache = diskcache.Cache(str(CACHE_DIR))
# Modify expiration in download_pdf method
# @cache.memoize(expire=86400 * 7)  # 7 days -> change as needed
```

### Customizing Agent Instructions
Each agent's behavior can be modified by editing the `instructions` parameter in the agent constructor.

---

## 📈 Performance Optimization Tips

1. **Use Caching**: The cache can reduce runtime by 50-70% on repeat queries
2. **Limit PDF Count**: Process only the most relevant PDFs (top 3-5)
3. **Concurrent Over Sequential**: Use concurrent processing when order doesn't matter
4. **Simple Over Full**: Use simple extractor for discovery, full framework for validation
5. **Browser Only When Needed**: Browser automation is slowest, use only for dynamic content

---

## 🐛 Troubleshooting

### "Agent not found" Error (Browser Auto)
- Delete `agent_config.json` to force recreation
- Verify Playwright connection exists in Azure AI Foundry

### PDF Download Failures
- Check firewall/proxy settings
- Some sites block automated downloads
- Try shorter timeout values

### Document Intelligence Errors
- Verify endpoint and key are correct
- Check quota limits in Azure portal
- Ensure PDF is not corrupted

### Mistral OCR Timeouts
- Reduce max_pages parameter (default 30)
- Check Azure API key permissions
- Verify endpoint URL format

### "No financial data found"
- Company may not publish data publicly (private company)
- Try different fiscal year
- Check if company name spelling is correct
- Review agent search queries in output

---

## 🔐 Security Considerations

- **API Keys**: Never commit `.env` files to version control
- **Credentials**: Use Azure CLI authentication when possible
- **Data Storage**: Be aware of GDPR/privacy when storing company data
- **Rate Limits**: Implement throttling for production use
- **Access Control**: Restrict who can run these scripts in production

---

## 📝 Future Enhancements

Potential improvements for these scripts:

1. **Hybrid Approach**: Combine browser automation with concurrent processing
2. **ML Validation**: Train model to validate extraction accuracy
3. **Multi-language Support**: Handle financial reports in different languages
4. **Time Series Analysis**: Extract historical data across multiple years
5. **Graph Database Integration**: Store entity relationships
6. **Real-time Monitoring**: Alert on new financial disclosures
7. **Batch Processing**: Process multiple companies in one run
8. **API Endpoint**: Expose as REST API for integration

---

## 📚 Additional Resources

- [Azure AI Foundry Documentation](https://learn.microsoft.com/azure/ai-services/)
- [Agent Framework GitHub](https://github.com/Azure/agent-framework)
- [Azure Document Intelligence](https://learn.microsoft.com/azure/ai-services/document-intelligence/)
- [Mistral AI Documentation](https://docs.mistral.ai/)

---

## 👥 Contributing

When contributing to these scripts:
1. Maintain the existing agent framework patterns
2. Add comprehensive error handling
3. Include example outputs in comments
4. Update this README with new features
5. Test with multiple company types (public/private, US/international)

---

## 📞 Support

For questions or issues:
1. Check the troubleshooting section above
2. Review Azure AI Foundry logs in the portal
3. Examine the output JSON files for error details
4. Consult the individual script docstrings

---

**Last Updated**: October 29, 2025
**Version**: 1.0.0
