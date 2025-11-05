# Core Pipeline - Synthetic Data + Production Analysis

This folder contains the end-to-end pipeline that combines synthetic data generation with production analysis using Azure Cosmos DB.

## 🚀 Quick Start

### Prerequisites

1. **Environment Variables** - Create a `.env` file in the project root with:

```env
YOUR_PROJECT_ENDPOINT=https://your-endpoint.services.ai.azure.com/api/projects/your-project
COSMOS_ENDPOINT=https://your-cosmos-account.documents.azure.com:443/
STORAGE_ACCOUNT_URL=https://your-storage-account.blob.core.windows.net/ (optional)
```

2. **Azure Authentication** - Run `az login` to authenticate with Azure AD

3. **Azure Permissions** - Ensure you have:
   - Azure AI Project access
   - Cosmos DB Built-in Data Contributor role
   - Storage Blob Data Contributor role (if using blob storage)

### Run the Full Pipeline

The easiest way to run the complete workflow is with `test_full_pipeline.py`:

```powershell
# Navigate to the core folder
cd scripts\code-interpreter\core

# Run a predefined scenario
python test_full_pipeline.py tv_show_season_1

# Run all predefined scenarios
python test_full_pipeline.py all

# Run a custom scenario
python test_full_pipeline.py custom "Create materials for TV Show Season 1 with 5 episodes and Spanish/English languages"
```

## 📋 Available Scenarios

Run `python test_full_pipeline.py` (no arguments) to see all available scenarios:

- **tv_show_season_1** - TV Show Season 1 analysis
- **animated_series_multi_season** - Animated series across 3 seasons
- **multilanguage_coverage** - Content with extensive language coverage
- **delivery_readiness** - Content delivery readiness check

## 🔧 How It Works

### The Pipeline Flow

```
User Query → Synthetic Data Generation → Validation → Analysis → Multi-Turn Q&A
```

1. **Synthetic Data Generation** (`synthetic_data_generator.py`)

   - Takes your natural language query
   - Generates realistic JSON data matching your request
   - Saves to `data/synthetic/`

2. **Production Analysis** (`production_pipeline_cosmosdb.py`)

   - Loads the synthetic data
   - Uses Azure AI Agent with code interpreter
   - Stores conversation history in Cosmos DB
   - Supports multi-turn conversations

3. **Results**
   - Answers are provided in real-time
   - Full conversation history persisted in Cosmos DB
   - Data snapshots stored in Blob Storage (if configured)

## 📁 Key Files

| File                              | Purpose                                                |
| --------------------------------- | ------------------------------------------------------ |
| `test_full_pipeline.py`           | **Main entry point** - Run this for end-to-end testing |
| `synthetic_data_generator.py`     | Generates realistic test data                          |
| `production_pipeline_cosmosdb.py` | Production pipeline with Cosmos DB backend             |
| `test_production_scenarios.py`    | Interactive testing with predefined datasets           |
| `query_pipeline.py`               | Simplified query-based pipeline                        |

## 🎯 Example Usage

### Run a Specific Scenario

```powershell
python test_full_pipeline.py tv_show_season_1
```

**What happens:**

1. Generates synthetic data for TV Show Season 1
2. Creates 5 episodes with Audio, Video, and Timed Text materials
3. Includes English, French, and Spanish languages
4. Analyzes with multiple queries:
   - "What materials are available for Season 1?"
   - "Which episodes have French translation?"
   - "Show me all the audio files"
   - "What's the completion status breakdown?"

### Run Custom Query

```powershell
python test_full_pipeline.py custom "Create materials for TV Series Season 2 with Japanese and German audio"
```

## 🗄️ Data Storage

### Cosmos DB Collections

- **agent-entity-store** - Agent metadata
- **thread-message-store** - Conversation messages (partition key: `thread_id`)
- **system-thread-message-store** - System messages

### Blob Storage (Optional)

- **data-snapshots** - JSON data snapshots for each query

## 🐛 Troubleshooting

### "COSMOS_ENDPOINT environment variable required"

- Add `COSMOS_ENDPOINT` to your `.env` file or set as environment variable

### "Permission denied" errors

- Ensure you've run `az login`
- Verify you have Cosmos DB Built-in Data Contributor role
- Check that your Azure subscription is active

### Agent not found errors

- Delete `.agent_config.json` to force creation of new agents
- Check that `YOUR_PROJECT_ENDPOINT` is correct

## 📊 Output

Results are saved to:

- **Synthetic data**: `data/synthetic/`
- **Query results**: `output/query_results/`
- **Evaluation outputs**: `output/evaluations/`

## 🔄 Agent Reuse

The pipeline automatically reuses agents across runs for efficiency:

- Synthetic generator agent ID stored in `.agent_config.json`
- Code interpreter agent ID stored in the same config
- Delete `.agent_config.json` to create fresh agents

## 💡 Tips

- Start with a predefined scenario to verify setup
- Use `custom` mode to test your own data requirements
- Check `data/synthetic/` to see what data was generated
- Review Cosmos DB containers in Azure Portal to see persisted conversations
