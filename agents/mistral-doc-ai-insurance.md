# Mistral Document AI - Insurance Document Processing

## Overview

This project provides a comprehensive solution for extracting structured information from complex insurance documents using **Mistral Document AI** integrated with **Azure Blob Storage**. The system is designed to handle PDFs and images containing complex table structures, multi-page documents, and various formatting styles commonly found in insurance filings and regulatory documents.

### Key Features

- **Multi-Format Document Processing**: Support for PDFs and images (JPEG, PNG)
- **Intelligent PDF Splitting**: Automatically splits large PDFs into chunks for optimal processing
- **Table Extraction**: Extracts complex tables with multi-level headers and side-by-side layouts
- **Document Annotations**: Structured metadata extraction (language, document type, summary, entities)
- **Azure Blob Storage Integration**: Seamless document retrieval from Azure Storage
- **Markdown Output**: Clean, human-readable markdown format for easy validation and downstream processing
- **Batch Processing**: Process multiple documents in a single run with detailed progress tracking

### Use Cases

- **Insurance Document Processing**: Extract tables and structured data from state insurance filings
- **Regulatory Compliance**: Parse complex regulatory documents with accurate data extraction
- **Financial Reports**: Extract financial tables with multi-level breakdowns
- **Contract Analysis**: Extract terms, pricing tables, and conditions from legal documents

---

## Architecture

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────────────┐
│  Azure Blob     │      │   Processing     │      │   Mistral Document  │
│   Storage       │─────▶│     Scripts      │─────▶│        AI API       │
│  (Documents)    │      │  (Python/Azure)  │      │   (OCR & Tables)    │
└─────────────────┘      └──────────────────┘      └─────────────────────┘
                                │                              │
                                │                              │
                                ▼                              ▼
                         ┌──────────────────┐      ┌─────────────────────┐
                         │  Output Files    │      │   Extracted Data    │
                         │  - Raw JSON      │      │   - Table Structures│
                         │  - Tables JSON   │      │   - Cell Content    │
                         │  - Text Files    │      │   - Relationships   │
                         └──────────────────┘      └─────────────────────┘
```

### Processing Pipeline

#### 1. Document Ingestion

- Documents are stored in Azure Blob Storage
- Support for PDF, PNG, and JPEG formats
- Entra ID (Azure AD) authentication for secure access
- Automatic file type detection and validation
- Optional prefix-based filtering for organized document sets

#### 2. Document Processing

The solution provides three processing modes optimized for different use cases:

##### Table Extraction Mode (`pdfs_ocr.py`)

Optimized for complex table extraction with minimal overhead:

- **Focus on OCR quality** - No annotations or bounding boxes to maximize table accuracy
- **Preserve table structures** - Maintains cell relationships and hierarchies
- **Multi-format support** - Handles tables with/without borders, sparse tables, multi-level headers
- **Batch processing** - Process multiple documents in a single run
- **Detailed reporting** - Per-document and run-level summaries

##### Image Processing Mode (`images_ocr.py`)

Basic OCR processing for images (JPEG, PNG):

- **Simple image OCR** - Extract text and tables from images
- **Base64 encoding** - Automatic conversion from Azure Blob Storage
- **Batch support** - Process multiple images in sequence
- **Run tracking** - Each run gets a unique timestamp-based ID

##### Advanced Annotation Mode (`annotations_with_splitting.py`)

For documents requiring structured metadata extraction and handling large PDFs:

- **Automatic PDF splitting** - Breaks large PDFs into chunks (default: 8 pages per chunk)
- **Document annotations** - Extract language, document type, key topics, entities, and summary
- **BBox annotations** - Extract document type, descriptions, and comprehensive summaries
- **Configurable schemas** - Enable/disable features via environment variables
- **Chunk recombination** - Automatically combines results from all chunks

#### 3. Response Processing

- **Raw response preservation** - Complete API responses saved as JSON
- **Markdown extraction** - Tables and content extracted in clean markdown format
- **Page-by-page separation** - Individual markdown files for each page
- **Multiple output formats** - JSON for programmatic processing, Markdown for human review
- **Usage tracking** - Token counts and page processing metrics

#### 4. Validation and Reporting

- **Per-document metrics** - Track extraction success, page counts, and table estimates
- **Summary reports** - JSON summaries of batch processing runs
- **Error tracking** - Detailed logging of failures with stack traces
- **Progress indicators** - Real-time feedback during batch processing

---

## Scripts Overview

### Core Processing Scripts

#### 1. `pdfs_ocr.py` - Table Extraction (Production)

**Purpose:** Extract complex tables from PDF documents with optimal OCR quality.

**Features:**

- Simplified payload focused on OCR quality
- No annotations or bounding boxes
- Batch processing with progress tracking
- Markdown output for easy validation
- Per-page and combined markdown files

**When to use:**

- Insurance filings with complex tables
- Financial reports with multi-level headers
- Any document where table structure preservation is critical

**Configuration:**

- Minimal - only requires Azure API key and storage credentials
- Optimized OCR quality for table detection
- Automatic content type detection

#### 2. `images_ocr.py` - Image OCR Processing

**Purpose:** Basic OCR processing for image files (JPEG, PNG).

**Features:**

- Simple image-to-text extraction
- Support for JPEG and PNG formats
- Batch processing with error handling
- Run-based output organization

**When to use:**

- Processing scanned documents as images
- Quick OCR without advanced features
- Single-page document processing

#### 3. `annotations_with_splitting.py` - Advanced Processing

**Purpose:** Handle large PDFs with automatic splitting and extract structured annotations.

**Features:**

- **Automatic PDF splitting:** Handles documents larger than 8 pages
- **Document classification tasks**
- **Entity extraction** (people, organizations, locations, dates)
- **Comprehensive summaries**
- **Chunk management** with automatic recombination

**When to use:**

- Large PDF documents (>8 pages)
- Documents requiring structured metadata
- Classification and summarization tasks

**Configuration:**

- Configurable annotation schemas
- Adjustable chunk size (default: 8 pages)
- Optional image base64 inclusion
- Enable/disable document and bbox annotations via environment variables

#### 4. `storage_utils.py` - Azure Storage Utilities

**Purpose:** Centralized Azure Blob Storage integration.

**Features:**

- Entra ID (Azure AD) authentication using `DefaultAzureCredential`
- Blob listing with prefix filtering
- Base64 encoding for API submission
- Debug utilities for troubleshooting storage access
- Configurable prefix normalization

**Key Functions:**

- `get_container_blobs()` - List all blobs with configured prefix
- `get_blob_base64(blob_name)` - Download and encode blob to base64
- `list_all_blobs_in_container()` - Debug utility to list all blobs
- `set_receipts_prefix(prefix)` - Test different prefixes dynamically

---

## Configuration

All configuration is managed through environment variables in a `.env` file:

```env
# Required: Azure API Configuration
AZURE_API_KEY="your-mistral-api-key"
PROJECT_ENDPOINT="https://foundry-eastus2-niq.services.ai.azure.com/..."

# Required: Azure Storage Configuration
STORAGE_ACCOUNT_NAME="your-storage-account"
STORAGE_CONTAINER_NAME="your-container"
STORAGE_PREFIX=""  # Optional: filter blobs by prefix (e.g., "invoices/")

# Optional: Advanced Processing Options (annotations_with_splitting.py)
INCLUDE_ANNOTATION="False"   # Enable document annotations
INCLUDE_BBOX="False"          # Enable bounding box annotations
INCLUDE_IMAGE_BASE64="False"  # Include base64 images in response (increases size)
MAX_PAGES_PER_CHUNK="8"      # Pages per chunk for PDF splitting

# Optional: Telemetry
APPLICATION_INSIGHTS_CONNECTION_STRING="..."
```

### Environment Variable Details

- **AZURE_API_KEY**: Your Mistral AI API key from Azure AI Foundry
- **PROJECT_ENDPOINT**: Azure AI Foundry project endpoint URL
- **STORAGE_ACCOUNT_NAME**: Azure Storage account name (without `.blob.core.windows.net`)
- **STORAGE_CONTAINER_NAME**: Container name where documents are stored
- **STORAGE_PREFIX**: Optional folder prefix (e.g., `documents/insurance/`)
- **MAX_PAGES_PER_CHUNK**: Maximum pages per chunk when splitting large PDFs (default: 8)

---

## Project Structure

```text
mistral-doc-ai-lnrisk-insurance/
├── scripts/
│   ├── mistral/
│   │   ├── pdfs_ocr.py                      # Table extraction (production)
│   │   ├── images_ocr.py                    # Image OCR processing
│   │   ├── annotations_with_splitting.py    # Advanced with PDF splitting
│   │   ├── storage_utils.py                 # Azure Storage utilities
│   │   └── ANNOTATIONS_GUIDE.md            # Annotation schema guide
│   └── content-understanding/
│       └── content_understanding.py         # Alternative processing
├── data/
│   ├── inputs/                              # Source documents (optional)
│   ├── ground-truth/                        # Validation data (optional)
│   └── responses/
│       ├── mistral/                         # Mistral processing outputs
│       │   ├── tables_run_<timestamp>/      # Table extraction runs
│       │   └── split_run_<timestamp>/       # Annotation runs with splitting
│       └── content-understanding/           # Alternative processor outputs
├── .env                                     # Configuration (not in git)
├── .gitignore
├── requirements.txt                         # Python dependencies
└── README.md                                # This file
```

### Output Directory Structure

#### Table Extraction Output (`tables_run_<timestamp>/`)

```text
tables_run_20251015_160424/
├── raw_document_name_pdf.json              # Complete API response
├── extracted_document_name_pdf.json        # Extraction metadata
├── extraction_summary.json                 # Batch summary
└── markdown/
    ├── markdown_document_name_pdf.md       # Combined markdown
    ├── page_0_document_name_pdf.md         # Page 0 markdown
    ├── page_1_document_name_pdf.md         # Page 1 markdown
    └── ...
```

#### Annotation with Splitting Output (`split_run_<timestamp>/`)

```text
split_run_20251016_105540/
├── processing_summary.json                 # Overall run summary
└── document_name/
    ├── document_name_combined.md           # Combined markdown from all chunks
    ├── document_name_summary.json          # Document-level summary
    ├── chunks/
    │   ├── chunk_0_pages_1-8_raw.json     # Raw API response for chunk 0
    │   ├── chunk_0_pages_1-8_extracted.json
    │   ├── chunk_1_pages_9-16_raw.json
    │   └── ...
    ├── markdown/
    │   ├── chunk_0_pages_1-8.md           # Markdown per chunk
    │   ├── chunk_1_pages_9-16.md
    │   └── ...
    └── annotations/
        ├── chunk_0_pages_1-8.json         # Annotations per chunk
        ├── chunk_0_pages_1-8.txt          # Human-readable annotations
        └── ...
```

---

## Getting Started

### Prerequisites

- **Python 3.8 or higher**
- **Azure subscription** with:
  - Azure AI Foundry project with Mistral Document AI model deployed
  - Azure Blob Storage account
  - Appropriate Entra ID (Azure AD) permissions
- **Azure CLI** installed (for local authentication)

### Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd mistral-doc-ai-lnrisk-insurance
   ```

2. **Create and activate virtual environment**

   ```bash
   # Windows PowerShell
   python -m venv venv
   .\venv\Scripts\activate

   # Linux/Mac
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install required packages**

   ```bash
   pip install -r requirements.txt
   ```

   This installs:
   - `azure-ai-projects` - Azure AI project integration
   - `azure-ai-inference` - AI model inference
   - `azure-identity` - Authentication (DefaultAzureCredential)
   - `azure-storage-blob` - Blob Storage client
   - `python-dotenv` - Environment variable management
   - `requests` - HTTP requests to Mistral API
   - `PyPDF2` - PDF manipulation for splitting
   - `opentelemetry-*` - Telemetry and monitoring (optional)

4. **Configure environment variables**

   Create a `.env` file in the project root:

   ```env
   # Required: Azure API Configuration
   AZURE_API_KEY="your-mistral-api-key-from-azure-ai-foundry"
   PROJECT_ENDPOINT="https://foundry-eastus2-niq.services.ai.azure.com/..."

   # Required: Azure Storage Configuration
   STORAGE_ACCOUNT_NAME="yourstorageaccount"
   STORAGE_CONTAINER_NAME="documents"
   STORAGE_PREFIX=""  # Optional: e.g., "insurance/filings/"

   # Optional: Advanced Processing (annotations_with_splitting.py)
   INCLUDE_ANNOTATION="False"
   INCLUDE_BBOX="False"
   INCLUDE_IMAGE_BASE64="False"
   MAX_PAGES_PER_CHUNK="8"

   # Optional: Telemetry
   APPLICATION_INSIGHTS_CONNECTION_STRING="InstrumentationKey=..."
   ```

5. **Set up Azure permissions**

   Ensure your Azure identity has:
   - **Storage Blob Data Reader** role on the storage account or container
   - **Azure AI Developer** or **Contributor** role on the Azure AI Foundry project

6. **Authenticate with Azure**

   ```bash
   az login
   ```

   This configures `DefaultAzureCredential` for local development.

---

## Usage

### Quick Start - Table Extraction

For most use cases (complex table extraction from PDFs), use the table-focused processor:

```bash
cd scripts/mistral
python pdfs_ocr.py
```

This will:

1. Connect to Azure Blob Storage using your configured credentials
2. List all documents matching the `STORAGE_PREFIX` filter (or all if empty)
3. Process each PDF document through Mistral Document AI
4. Extract tables and text content in markdown format
5. Save results to `data/responses/mistral/tables_run_<timestamp>/`

### Processing Modes

#### 1. Table Extraction Mode (Recommended for Complex Tables)

**Script:** `pdfs_ocr.py`

**Best for:**

- Insurance filings with complex tables
- Financial reports with multi-level headers
- Documents with side-by-side tables
- Sparse and waterfall table formats
- Multi-page tables

**Features:**

- Optimized OCR quality for table detection
- No annotation overhead (faster processing)
- Clean table structure extraction
- Markdown output format for easy validation
- Per-page and combined outputs

**Usage:**

```bash
python scripts/mistral/pdfs_ocr.py
```

**Output Files:**

- `raw_<filename>.json` - Complete Mistral API response
- `extracted_<filename>.json` - Extraction metadata and summary
- `markdown/<filename>.md` - Combined markdown from all pages
- `markdown/page_N_<filename>.md` - Individual page markdown
- `extraction_summary.json` - Batch processing summary

#### 2. Image OCR Mode

**Script:** `images_ocr.py`

**Best for:**

- Scanned documents as images (JPEG, PNG)
- Single-page documents
- Quick OCR without PDF complexity

**Usage:**

```bash
python scripts/mistral/images_ocr.py
```

**Output:**

- `response_<filename>.json` - API response per image
- Run organized by `run_<timestamp>/`

#### 3. Advanced Processing with PDF Splitting

**Script:** `annotations_with_splitting.py`

**Best for:**

- Large PDFs (>8 pages)
- Document classification tasks
- Metadata extraction (language, document type, entities)
- Structured annotations and summaries

**Features:**

- **Automatic PDF splitting** - Chunks large PDFs for annotation support (max 8 pages per chunk)
- **Document annotations** - Language, document type, key topics, entities, summary
- **BBox annotations** - Document type, short description, comprehensive summary
- **Chunk recombination** - Automatically merges results from all chunks
- **Configurable schemas** - Enable/disable features via environment variables

**Configuration (.env):**

```env
INCLUDE_ANNOTATION="True"   # Enable document-level annotations
INCLUDE_BBOX="True"          # Enable bounding box annotations
MAX_PAGES_PER_CHUNK="8"      # Pages per chunk (default: 8)
```

**Usage:**

```bash
python scripts/mistral/annotations_with_splitting.py
```

**Output:**

- Per-chunk raw and extracted JSON
- Per-chunk markdown and annotations
- Combined document markdown
- Document summary with all chunks merged

---

## Testing and Troubleshooting

### Test Storage Access

Before processing documents, verify your Azure Storage configuration:

```bash
cd scripts/mistral
python storage_utils.py
```

This will:

- List all blobs in your container with the configured prefix
- Display available prefixes in the container
- Show the first blob encoded as base64 (validation)
- Suggest alternative prefixes if no blobs found

### Verify API Configuration

Test your Mistral API endpoint and credentials:

```bash
# Simple connectivity test
python -c "from storage_utils import get_container_blobs; print(f'Found {len(get_container_blobs())} documents')"
```

### Common Issues

#### No Blobs Found

If you see "No blobs found with prefix", check:

1. **STORAGE_PREFIX** in `.env` - Try empty string `""` to see all blobs
2. **Container name** - Verify `STORAGE_CONTAINER_NAME` is correct
3. **Permissions** - Ensure you have Storage Blob Data Reader role
4. Run `python storage_utils.py` to see available prefixes

#### Authentication Errors

If you see authentication errors:

1. Run `az login` to authenticate
2. Verify your Azure account has access to the storage account
3. Check that `STORAGE_ACCOUNT_NAME` matches your storage account

#### API Errors

If Mistral API returns errors:

1. Verify `AZURE_API_KEY` is correct
2. Check `PROJECT_ENDPOINT` URL is complete and correct
3. Ensure Mistral Document AI model is deployed in your Azure AI Foundry project

### Authentication

The project uses Azure's `DefaultAzureCredential`, which tries multiple authentication methods in order:

1. **Environment variables** - `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`, `AZURE_TENANT_ID`
2. **Managed Identity** - When running on Azure services (VMs, Functions, etc.)
3. **Azure CLI** - `az login` **(recommended for local development)**
4. **Visual Studio Code** - VS Code Azure account extension
5. **Azure PowerShell** - `Connect-AzAccount`
6. **Interactive browser** - Fallback browser-based authentication

**For local development:**

```bash
az login
```

**For Azure deployment:**

Use Managed Identity or configure environment variables for service principal authentication.

---

## Supported File Types

The solution automatically detects and processes:

- **PDF** (`application/pdf`) - Recommended for complex documents with tables
- **PNG** (`image/png`) - High-quality scanned images
- **JPEG** (`image/jpeg`) - Standard photo and scan formats

---

## Understanding Mistral's Output Format

### Markdown Table Format

Mistral Document AI returns extracted content in **markdown format**, including tables. This approach differs from other OCR solutions that return structured JSON, prioritizing human readability and ease of validation.

**Example Markdown Table:**

```markdown
| Zone | $500 | $750 | $1,000 | $1,500 |
|:----:|:----:|:----:|:------:|:------:|
| 1    | 0.08 | 0.07 | 0.07   | 0.07   |
| 2    | 0.25 | 0.24 | 0.23   | 0.23   |
| 3    | 0.51 | 0.50 | 0.48   | 0.47   |
```

### Benefits of Markdown Format

1. **Human-Readable** - Easy to review and validate without special tools
2. **Version Control Friendly** - Git-friendly text format for change tracking
3. **Universal** - Can be rendered in any markdown viewer or editor
4. **Preserves Structure** - Maintains table relationships, hierarchies, and formatting
5. **Easy to Parse** - Standard format with many existing parsing libraries
6. **Lightweight** - No heavy dependencies for basic viewing and editing

### Working with Markdown Tables

**For Manual Review:**

- Open `markdown_*.md` files in any markdown viewer
- VS Code: built-in markdown preview (`Ctrl+Shift+V` or `Cmd+Shift+V`)
- Compare side-by-side with source PDF for validation
- Search and navigate easily with standard text editors

**For Programmatic Processing:**

- Use Python libraries: `pandas`, `markdown-it-py`, `mistletoe`, or `python-markdown`
- Convert markdown tables to CSV, JSON, or pandas DataFrames
- Apply custom parsing logic for domain-specific table formats
- Extract specific cells, rows, or columns programmatically

**Example: Parse Markdown Table to DataFrame**

```python
import pandas as pd
import re

# Read markdown file
with open('data/responses/mistral/tables_run_20251015_160424/markdown_document.md', 'r', encoding='utf-8') as f:
    content = f.read()

# Extract first table (basic approach)
# Split by table markers and process
tables = re.findall(r'\|.*\|(?:\n\|.*\|)+', content)
if tables:
    # Parse first table to DataFrame
    from io import StringIO
    df = pd.read_csv(StringIO(tables[0]), sep='|', skipinitialspace=True)
    df = df.dropna(axis=1, how='all')  # Remove empty columns
    print(df)
```

### When to Use Further Processing

**Markdown output alone is sufficient for:**

- Manual validation and review
- Quality assessment and comparison
- Documentation and archival purposes
- Human-in-the-loop workflows

**Consider additional parsing when you need:**

- Automated data extraction pipelines
- Database insertion and ETL workflows
- Quantitative analysis and reporting
- Integration with existing enterprise systems
- Real-time processing and alerting

---

## Output and Results

### Results Structure

Each processing run creates a timestamped directory with organized outputs:

**Table Extraction Run Example:**

```text
data/responses/mistral/tables_run_20251015_160424/
├── raw_combined_samples_pdf.json           # Complete Mistral API response
├── extracted_combined_samples_pdf.json     # Extraction metadata
├── extraction_summary.json                 # Batch processing summary
└── markdown/
    ├── markdown_combined_samples_pdf.md    # Combined markdown (all pages)
    ├── page_0_combined_samples_pdf.md      # Individual page 0
    ├── page_1_combined_samples_pdf.md      # Individual page 1
    └── ...                                 # Additional pages
```

### Output File Details

#### Raw Response (`raw_*.json`)

Complete API response from Mistral Document AI containing:

- **Pages array** - Individual page data with markdown content
- **Markdown formatted tables** - Tables preserved in markdown syntax
- **Document metadata** - Page dimensions, DPI information
- **Usage statistics** - Pages processed, document size, token counts
- **Model information** - Model version used for processing

Example structure:

```json
{
  "pages": [
    {
      "index": 0,
      "markdown": "# HEADING\n\n| Column 1 | Column 2 |\n|----------|----------|\n| Value 1  | Value 2  |",
      "dimensions": {"width": 612, "height": 792}
    }
  ],
  "model": "mistral-document-ai-2505",
  "usage_info": {
    "pages_processed": 20,
    "doc_size_bytes": 1229914
  }
}
```

#### Extraction Metadata (`extracted_*.json`)

Summary of extracted content with:

```json
{
  "page_count": 20,
  "combined_markdown": "...",
  "table_count_estimate": 12,
  "total_markdown_length": 45000,
  "response_type": "mistral_ocr",
  "usage_info": {
    "pages_processed": 20,
    "doc_size_bytes": 1229914
  }
}
```

#### Markdown Files (`markdown_*.md` and `page_*.md`)

Human-readable markdown content for:

- **Manual review** - Easy to read tables and text
- **Quality validation** - Compare against source documents
- **Text search** - Standard markdown format for indexing
- **Table verification** - Visual inspection of extracted tables
- **Version control** - Track changes in extracted content

**Combined Markdown** (`markdown_*.md`): All pages combined with page break separators

**Individual Pages** (`page_N_*.md`): Separate files for each page for easier navigation

#### Summary Report (`extraction_summary.json`)

Batch processing metrics:

```json
{
  "run_id": "tables_run_20251015_160859",
  "total_documents": 2,
  "successful": 2,
  "failed": 0,
  "table_extraction_summary": [
    {
      "document": "combined_samples_new_chunk_001.pdf",
      "pages": 20,
      "tables_estimated": 12,
      "markdown_chars": 45000,
      "status": "success",
      "response_type": "mistral_ocr"
    }
  ]
}
```

**Key Metrics:**

- **pages** - Number of pages processed
- **tables_estimated** - Rough count of markdown tables detected
- **markdown_chars** - Total characters in extracted markdown
- **status** - Processing status (success/error)
- **response_type** - Mistral response format identifier

## Performance Testing and Optimization

### Load Testing Tool

Comprehensive load testing capabilities for performance measurement:

**Features:**

- Multiple iterations against Mistral OCR endpoint
- Latency metrics (min, max, mean, median, p90, p95, p99)
- Success rate tracking
- Error analysis
- Rate limit handling with exponential backoff
- Real document testing
- Async execution with concurrency control
- Application Insights telemetry integration

### Load Test Commands

```bash
# Basic test - 50 iterations with sample image
python scripts/mistral/mistral_load_test.py

# Extended test with real documents
python scripts/mistral/mistral_load_test.py --iterations 100 --use-real-blobs

# High-concurrency async test
python scripts/mistral/mistral_load_test.py --iterations 200 --async --concurrency 10

# Custom output location
python scripts/mistral/mistral_load_test.py --csv-path custom_results.csv
```

### Load Test Options

| Option | Description | Default |
|--------|-------------|---------|
| `--iterations N` | Number of API calls | 50 |
| `--use-real-blobs` | Use actual storage documents | False |
| `--async` | Enable async execution | False |
| `--concurrency N` | Max concurrent requests | 5 |
| `--csv-path PATH` | Custom CSV output path | Auto-generated |

### Load Test Output

#### Per-Request Metrics (CSV)

`data/responses/mistral/load_test_<timestamp>.csv`

```csv
iteration,attempt,timestamp,status_code,latency_ms,success,error,blob_name
1,1,2025-10-15T10:30:45,200,1245.3,True,,document.pdf
2,1,2025-10-15T10:30:47,200,1189.7,True,,document.pdf
```

#### Statistical Summary (JSON)

`data/responses/mistral/load_test_summary_<timestamp>.json`

```json
{
  "count": 100,
  "min_ms": 895.2,
  "max_ms": 3421.8,
  "mean_ms": 1245.6,
  "median_ms": 1203.4,
  "p50_ms": 1203.4,
  "p90_ms": 1876.3,
  "p95_ms": 2104.7,
  "p99_ms": 2891.2,
  "stdev_ms": 342.8,
  "success_rate": 98.0,
  "total_errors": 2
}
```

### Performance Benchmarks

Typical performance for complex PDF documents (20 pages, multiple tables):

| Metric | Value |
|--------|-------|
| Average Latency | 1,200-1,500 ms |
| P95 Latency | ~2,100 ms |
| Success Rate | >98% |
| Tables Extracted | 10-15 per document |

---

## Troubleshooting

### API and Authentication Issues

**Problem:** `401 Unauthorized` or `403 Forbidden`

- Verify `AZURE_API_KEY` in `.env` file is correct
- Ensure API key has not expired
- Check Azure AI Foundry project permissions
- Confirm authentication: `az login`
- Verify user has API access to the Foundry project

**Problem:** `404 Not Found` from API

- Verify `PROJECT_ENDPOINT` URL is complete and correct
- Check that Mistral Document AI model is deployed in your project
- Confirm model name is `mistral-document-ai-2505`
- Ensure endpoint URL includes the correct path

### Azure Storage Issues

**Problem:** `Storage authentication failed`

- Run `az login` to authenticate with Azure
- Verify `STORAGE_ACCOUNT_NAME` and `STORAGE_CONTAINER_NAME` are correct
- Check Azure role assignments: need **Storage Blob Data Reader** role
- Test with: `python scripts/mistral/storage_utils.py`

**Problem:** `No documents found` or `Empty blob`

- Verify documents exist in the container (check Azure Portal)
- Try `STORAGE_PREFIX=""` (empty string) to see all files
- Run `python scripts/mistral/storage_utils.py` to list available blobs
- Confirm file formats are supported (PDF, PNG, JPEG)
- Check that files aren't in unexpected nested folders

### Processing Issues

**Problem:** `Response keys: None` or `NoneType error`

- API response may be malformed or empty
- Check raw response files (`raw_*.json`) in output directory
- Verify document is not corrupted (open manually)
- Ensure file size is within API limits
- Check for API rate limiting

**Problem:** `No tables detected` or low table count

- Document may not contain machine-readable tables
- Open `markdown_*.md` files to verify content was extracted
- Review individual `page_*.md` files for tables
- Check if tables are actually images (scanned documents require good resolution)
- Table count is an estimate based on markdown table syntax

**Problem:** Slow processing or timeouts

- Large PDFs take longer (e.g., 20 pages ~1-2 seconds)
- Check network connectivity and latency
- Large files may time out - use PDF splitting for >50 pages
- Monitor Azure AI Foundry metrics for throttling

**Problem:** Rate limiting (429 errors)

- Scripts include automatic exponential backoff and retry
- Reduce batch size or processing concurrency
- Contact Azure support for rate limit increases if needed

### Environment Issues

**Problem:** `Module not found` errors

```bash
pip install -r requirements.txt
```

**Problem:** Virtual environment issues

```bash
# Recreate virtual environment
deactivate
rm -rf venv  # Windows: Remove-Item -Recurse -Force venv
python -m venv venv
.\venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
```

### Getting Help

1. **Check output files** - Review `markdown_*.md`, `raw_*.json`, and `extraction_summary.json`
2. **Verify extraction** - Open markdown files to confirm content is present
3. **Test connectivity** - Run `python scripts/mistral/storage_utils.py`
4. **Review logs** - Check terminal output for specific error messages
5. **Compare outputs** - Side-by-side review of markdown vs. original PDF

---

## Best Practices

### Document Preparation

- **Use PDF format** for complex multi-page documents with tables
- **Ensure good quality** - Scanned documents should be at least 300 DPI
- **Test first** - Process sample documents before large batches
- **Verify readability** - Ensure source documents are not corrupted

### Configuration

- **Keep `INCLUDE_IMAGE_BASE64=False`** to reduce response size and costs
- **Disable annotations** (`pdfs_ocr.py`) for pure table extraction
- **Use `STORAGE_PREFIX`** to filter specific document sets
- **Set appropriate chunk size** (`MAX_PAGES_PER_CHUNK=8`) for large PDFs

### Processing

- **Batch processing** - Process documents in manageable batches
- **Monitor progress** - Review extraction summaries after each run
- **Start small** - Test with a few documents before full batch
- **Use splitting** for large PDFs (>8 pages) when annotations needed

### Validation

- **Review markdown files** - Open `markdown_*.md` files to verify extraction quality
- **Compare with source** - Side-by-side validation with original PDFs
- **Check individual pages** - Use `page_*.md` files for detailed inspection
- **Validate table structure** - Ensure markdown tables match source document

### Cost Optimization

**Typical Token Usage:**

- 20-page PDF: ~230,000 input tokens, ~3,000 output tokens
- Image files: Varies by size and resolution

**Optimization Tips:**

1. **Filter documents** - Use `STORAGE_PREFIX` to process only needed files
2. **Disable features** - Skip annotations if not required (`INCLUDE_ANNOTATION=False`)
3. **Monitor usage** - Check token counts in summary reports and usage_info
4. **Use PDFs** - More efficient than processing individual page images

---

## License

This project is provided as-is for insurance document processing use cases.

## Contributing

Contributions are welcome! Areas for improvement:

- Additional output formats (CSV, Excel)
- Enhanced table parsing libraries
- Comparison tools for validation
- Performance optimizations
- Error handling improvements

---

## Support

For issues or questions:

1. Review this README and the troubleshooting section
2. Check output files (`markdown_*.md`, `raw_*.json`, `extraction_summary.json`)
3. Test connectivity with `python scripts/mistral/storage_utils.py`
4. Open an issue with error details (redact sensitive information)

---

## Acknowledgments

Built for complex insurance document processing with:

- **Mistral Document AI** via Azure AI Foundry
- **Azure Blob Storage** for document management
- **Entra ID** authentication
- Designed for enterprise-scale document processing workflows
