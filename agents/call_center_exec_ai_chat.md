# Call Center Executive AI Chat Solution

## User Stories

### Primary Users: Senior Leadership & Executives

1. **As a VP of Customer Experience**, I want to ask "What are the top 3 reasons customers mentioned canceling last month?" and get accurate answers in seconds, so I can prioritize retention initiatives.

2. **As a Regional Director**, I want to see a daily dashboard showing competitor mentions by region, so I can respond quickly to competitive threats.

3. **As a Chief Strategy Officer**, I want to query "How has sentiment changed for customers mentioning Promotion X vs Promotion Y?" to evaluate campaign effectiveness.

### Secondary Users: Analysts & Managers

4. **As a Call Center Manager**, I want to receive weekly automated reports on call sentiment trends, so I can identify training opportunities.

5. **As a Business Analyst**, I want to drill down from summary stats into specific call examples, so I can validate insights and find representative samples.

## Business Logic & Requirements

### Data Processing Requirements

- **Volume**: Scale from 400K to millions of calls monthly
- **Latency**: Near real-time processing (calls processed within 1-4 hours of completion)
- **Accuracy**: >95% entity extraction accuracy with human-in-the-loop validation for edge cases
- **Cost**: Balance LLM processing costs with infrastructure efficiency

### Entity Extraction Priorities

1. **Competitors** (normalized names)
2. **Promotions/Offers** (campaign codes, product names)
3. **Cancellation Reasons** (categorized)
4. **Sentiment** (call-level and topic-level)
5. **Product Issues** (categorized complaints)
6. **Agent Performance Indicators** (resolution, empathy markers)

### Data Retention & Aggregation Strategy

- **Raw transcripts**: 90 days hot storage, 2 years cold storage
- **Enriched call records**: 2 years hot storage
- **Daily aggregates**: 3 years
- **Monthly aggregates**: 7 years

## ROI & Business Value

### The Strategic Importance of the Data Prep Pipeline

**This solution's value is NOT just the chat interface—it's the comprehensive data enrichment pipeline that transforms raw call transcripts into actionable intelligence.** The upfront investment in entity extraction and structured data creation unlocks compounding returns across the entire organization.

### ROI Drivers: Cost Reduction & Revenue Growth

**1. Call Center Automation & Cost Reduction**

*Current State:* Manual handling of 400K-2M calls/month with average cost of $5-15 per call

*With Enriched Data:*
- **Identify automation opportunities:** Analyze top call reasons to build self-service flows
- **Automate routine inquiries:** Reduce call volume by 15-25% through intelligent IVR/chatbot deflection
- **Improve first-call resolution:** Better training data leads to fewer callbacks

**Conservative ROI:**
- 2M calls/month × $8 average cost = $16M/month operating cost
- 20% automation rate = 400K calls automated
- **Savings: $3.2M/month ($38.4M/year)**
- **Solution cost: $26-33K/month**
- **ROI: 97x-123x first year**

**2. Agent Training & Retention Improvements**

*Current State:* High agent turnover (30-40% annually), costly training programs, inconsistent performance

*With Enriched Data:*
- **Identify training gaps:** Pinpoint exactly where agents struggle (product knowledge, empathy, objection handling)
- **Targeted coaching:** Use actual call examples for personalized training
- **Improve retention:** Better-trained agents are more confident and stay longer

**Conservative ROI:**
- Reduce turnover from 35% to 28% (saving 7% of workforce)
- 1,000 agents × $50K replacement cost × 7% = **$3.5M/year savings**
- Improved performance drives cross-sell/upsell: 2% increase on $100M revenue = **$2M/year additional revenue**

**3. Product & Marketing Intelligence**

*Current State:* Limited visibility into customer pain points, competitor threats, and campaign effectiveness

*With Enriched Data:*
- **Product insights:** Identify top complaints and feature requests to prioritize roadmap
- **Competitive intelligence:** Real-time monitoring of competitor mentions and win/loss reasons
- **Campaign optimization:** Measure promotion effectiveness and customer sentiment

**Conservative ROI:**
- Faster product iteration reduces churn by 2%: **$2-4M/year** (varies by customer lifetime value)
- Marketing spend optimization (eliminate underperforming campaigns): **$1-2M/year savings**
- Competitive response speed improves win rate by 3-5%: **$3-5M/year revenue**

### Total Annual Value Calculation

| Value Driver | Annual Impact | Conservative Estimate |
|--------------|---------------|----------------------|
| Call automation & deflection | $38.4M | $20-30M (accounting for ramp-up) |
| Agent training & retention | $5.5M | $3-5M |
| Product & marketing intelligence | $6-11M | $4-8M |
| **Total Annual Value** | **$49.9M** | **$27-43M** |
| **Solution Cost (Annual)** | **$315-420K** | **$315-420K** |
| **Net Value** | **$49.5M** | **$26.6-42.7M** |
| **ROI** | **~160x** | **~85-135x** |

### Why Organizations Should NOT Balk at Pricing

**The $26-33K/month investment ($315-420K/year) is not an expense—it's a strategic enabler that pays for itself 85-160x over.**

**Key Point:** The entity extraction pipeline is doing the heavy analytical work that would otherwise require:
- 10-15 full-time analysts manually categorizing calls
- Expensive BI consultants building custom reports
- Months of delay to get insights that inform decisions

**With this solution:**
- ✅ Insights are automated and real-time (1-4 hour latency)
- ✅ Executives get instant answers via natural language
- ✅ Data quality is consistent (>95% accuracy with LLM extraction)
- ✅ Historical trends are preserved and queryable
- ✅ The data asset appreciates over time (more history = better insights)

### The Data Goldmine Compounds Over Time

**Month 1-3:** Foundation—building pipelines, establishing baselines
**Month 4-6:** Quick wins—identifying obvious automation opportunities and training gaps
**Month 7-12:** Optimization—iterating on products, refining call center operations
**Year 2+:** Strategic advantage—predictive churn modeling, proactive competitive response, AI-driven product roadmapping

**The longer this system runs, the more valuable it becomes.** Two years of enriched call data enables:
- Seasonal trend analysis
- Year-over-year comparisons
- Predictive modeling for churn risk
- Early warning systems for product issues
- Competitive threat detection

### Bottom Line

**Organizations that hesitate on pricing are missing the forest for the trees.** The investment in the data prep pipeline is what unlocks enterprise-wide transformation:

1. **Automate more** → Reduce call center expenses by millions
2. **Train better** → Improve retention and drive cross-sell revenue
3. **Build smarter** → Mine consumer insights to create better products and campaigns

**The question isn't "Can we afford this?"—it's "Can we afford NOT to do this while competitors are?"**

## Technical Architecture

### Tech Stack Recommendation

**Unified Data Platform: Microsoft Fabric**

- **Data Engineering**: Fabric Spark (notebooks, pipelines)
- **Orchestration**: Fabric Data Factory pipelines
- **Storage**: OneLake (unified data lake)
- **Data Warehouse**: Fabric Data Warehouse
- **Semantic Layer**: Power BI semantic models (built-in)
- **Natural Language Interface**: Fabric Copilot/Data Agent
- **Monitoring**: Fabric monitoring + Azure Monitor

**Data Ingestion Layer**

- **Message Queue**: Azure Event Hubs (for call transcript streaming)

**AI/ML Processing Layer**

- **LLM Service**: Azure OpenAI Service (GPT-4.1 for entity extraction)
- **Prompt Management**: Microsoft Agent Framework / Azure AI Foundry SDK
- **Vector Storage**: Azure AI Search
- **Model Monitoring**: AI Foundry Observability tools

**Analytics & Interface Layer**

- **Natural Language Interface**: Fabric Data Agent (consumed via AI Foundry Agent Service)
- **Visualization**: Power BI (Direct Lake mode)
- **API Layer**: Azure API Management (for programmatic access, optional)
- **Caching**: Azure Redis Cache (for common queries, optional)

**Monitoring & Governance**

- **Observability**: Azure Monitor, Application Insights
- **Data Governance**: Microsoft Purview (integrated with Fabric)
- **Cost Management**: Azure Cost Management

## Process Flow

### 1. Real-Time Ingestion Pipeline

```
Call Completed → Transcription System → Event Hub → Stream Processing
                                                    ↓
                                            OneLake Bronze Layer
```

### 2. Entity Extraction Pipeline (Batch)

```
Scheduled Job (Hourly/4-hourly)
    ↓
Fabric Spark reads new transcripts from Bronze
    ↓
Partition into micro-batches (1000 calls each)
    ↓
Parallel LLM Processing (GPT-4.1)
    ├─ Competitor Extraction
    ├─ Promotion Extraction
    ├─ Cancellation Reason Extraction
    ├─ Sentiment Analysis
    └─ Other Entity Extraction
    ↓
Validate & Normalize Entities
    ↓
Write to Silver Layer (Enriched Call Records)
    ↓
Trigger Downstream Aggregations
```

### 3. Aggregation Pipeline

```
Silver Layer: Enriched Call Records
    ↓
Triggered Aggregation Jobs (Fabric Spark)
    ├─ Daily Rollups (by region, product, competitor)
    ├─ Weekly Rollups
    └─ Monthly Rollups
    ↓
Gold Layer: Curated Analytics Tables
    ↓
Power BI Semantic Models (Direct Lake)
```

### 4. Query & Analytics Flow

```
User Natural Language Query
    ↓
Fabric Data Agent (Copilot)
    ├─ Check Cache (Redis, optional)
    ├─ Semantic Search (Vector via Azure AI Search)
    └─ SQL Generation (NL2SQL)
    ↓
Query Optimizer (routes to appropriate table)
    ├─ Gold Layer Tables (for summary queries)
    └─ Silver Layer (for detailed queries)
    ↓
Results + Citation (specific call IDs)
    ↓
User Interface
```

## Detailed Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                         DATA SOURCES                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│  │ Call Center  │  │ Transcription│  │   CRM/Other  │              │
│  │   Systems    │→ │   Service    │→ │   Metadata   │              │
│  └──────────────┘  └──────────────┘  └──────────────┘              │
└────────────────────────────┬────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    INGESTION LAYER                                   │
│  ┌─────────────────────────────────────────────────────┐            │
│  │           Azure Event Hubs                           │            │
│  │  (Real-time transcript streaming)                    │            │
│  └────────────────────┬────────────────────────────────┘            │
└─────────────────────────┬───────────────────────────────────────────┘
                          ↓
╔═════════════════════════════════════════════════════════════════════╗
║                     MICROSOFT FABRIC WORKSPACE                       ║
║                                                                      ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │                    ONELAKE (Unified Storage)                   │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Lakehouse: CallCenter_Bronze                           │  │ ║
║  │  │  - Raw transcripts (JSON/Parquet)                       │  │ ║
║  │  │  - Partitioned by date                                  │  │ ║
║  │  │  - 90-day hot, 2-year archive                           │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │              DATA FACTORY PIPELINES                            │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Pipeline 1: Ingest_Transcripts                         │  │ ║
║  │  │  - Triggered by Event Hub                               │  │ ║
║  │  │  - Writes to Bronze layer                               │  │ ║
║  │  │  - Frequency: Real-time/Micro-batch (15 min)            │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  │                                                                 │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Pipeline 2: Entity_Extraction                          │  │ ║
║  │  │  - Scheduled: Every 4 hours                             │  │ ║
║  │  │  - Triggers Spark notebook                              │  │ ║
║  │  │  - Includes error handling & retry logic                │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  │                                                                 │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Pipeline 3: Daily_Aggregations                         │  │ ║
║  │  │  - Scheduled: Daily at 2 AM                             │  │ ║
║  │  │  - Triggers aggregation notebook                        │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  │                                                                 │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Pipeline 4: Weekly_Monthly_Analytics                   │  │ ║
║  │  │  - Scheduled: Weekly (Monday) / Monthly (1st)           │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │            FABRIC SPARK (Data Engineering)                     │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Notebook: Entity_Extraction_Engine                     │  │ ║
║  │  │  - Reads new transcripts from Bronze layer              │  │ ║
║  │  │  - Partitions into micro-batches (1000 calls each)      │  │ ║
║  │  │  - Calls Azure OpenAI for parallel processing           │  │ ║
║  │  │                                                           │  │ ║
║  │  │  Entity Extraction Tasks:                                │  │ ║
║  │  │    • Competitor extraction & normalization               │  │ ║
║  │  │    • Promotion/offer identification                      │  │ ║
║  │  │    • Cancellation reason categorization                  │  │ ║
║  │  │    • Sentiment analysis (call & topic level)             │  │ ║
║  │  │    • Product issue categorization                        │  │ ║
║  │  │    • Agent performance indicators                        │  │ ║
║  │  │                                                           │  │ ║
║  │  │  Validation & Normalization:                             │  │ ║
║  │  │    • Entity deduplication                                │  │ ║
║  │  │    • Confidence scoring                                  │  │ ║
║  │  │    • Reference data lookups                              │  │ ║
║  │  │                                                           │  │ ║
║  │  │  - Writes enriched data to Silver layer                  │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  │                                                                 │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Notebook: Aggregation_Engine                           │  │ ║
║  │  │  - Reads from Silver layer                               │  │ ║
║  │  │  - Generates daily/weekly/monthly rollups                │  │ ║
║  │  │  - Anomaly detection                                     │  │ ║
║  │  │  - Predictive churn scoring                              │  │ ║
║  │  │  - Writes to Gold layer (optimized tables)               │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │              ONELAKE LAKEHOUSE - SILVER LAYER                  │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  📊 ENRICHED CALL RECORDS (Primary Table)               │  │ ║
║  │  │                                                           │  │ ║
║  │  │  Columns:                                                │  │ ║
║  │  │    - CallID, Date, Duration, CustomerID                  │  │ ║
║  │  │    - Transcript, TranscriptEmbedding (vector)            │  │ ║
║  │  │    - Competitor1, Competitor2, Competitor3               │  │ ║
║  │  │    - Promotions, CancellationReasons                     │  │ ║
║  │  │    - SentimentScore, SentimentLabel                      │  │ ║
║  │  │    - ProductIssues, AgentID, Region                      │  │ ║
║  │  │                                                           │  │ ║
║  │  │  Partitioned by: Date (day)                              │  │ ║
║  │  │  Format: Delta Lake (ACID transactions)                  │  │ ║
║  │  │  Z-Ordered on: Date, CustomerID, Competitors             │  │ ║
║  │  │  Retention: 2 years hot storage                          │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │              ONELAKE LAKEHOUSE - GOLD LAYER                    │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  📈 AGGREGATED ANALYTICS TABLES                         │  │ ║
║  │  │  ┌───────────────────────────────────────────────────┐  │  │ ║
║  │  │  │  • daily_competitor_stats                         │  │  │ ║
║  │  │  │    (Date, Competitor, Region, MentionCount,       │  │  │ ║
║  │  │  │     AvgSentiment, TopIssues)                      │  │  │ ║
║  │  │  │                                                    │  │  │ ║
║  │  │  │  • daily_promotion_performance                    │  │  │ ║
║  │  │  │    (Date, Promotion, MentionCount,                │  │  │ ║
║  │  │  │     ConversionRate, AvgSentiment)                 │  │  │ ║
║  │  │  │                                                    │  │  │ ║
║  │  │  │  • daily_cancellation_summary                     │  │  │ ║
║  │  │  │    (Date, Reason, Count, Region, Product)         │  │  │ ║
║  │  │  │                                                    │  │  │ ║
║  │  │  │  • daily_sentiment_trends                         │  │  │ ║
║  │  │  │  • weekly_analytics                               │  │  │ ║
║  │  │  │  • monthly_executive_summary                      │  │  │ ║
║  │  │  └───────────────────────────────────────────────────┘  │  │ ║
║  │  │                                                           │  │ ║
║  │  │  Format: Delta Lake (optimized, compacted)               │  │ ║
║  │  │  Retention: Daily (3 years), Monthly (7 years)           │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │              FABRIC DATA WAREHOUSE (Optional)                  │ ║
║  │  - SQL views over Gold lakehouse tables                        │ ║
║  │  - Optimized for BI tool queries                               │ ║
║  │  - Row-level security for sensitive data                       │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │                   FABRIC COPILOT / DATA AGENT                  │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Natural Language Query Interface                       │  │ ║
║  │  │  ┌───────────────────────────────────────────────────┐  │  │ ║
║  │  │  │  Query Understanding Layer                        │  │  │ ║
║  │  │  │  - Intent classification                          │  │  │ ║
║  │  │  │  - Entity recognition                             │  │  │ ║
║  │  │  │  - Context awareness                              │  │  │ ║
║  │  │  └───────────────────────────────────────────────────┘  │  │ ║
║  │  │              ↓                                            │  │ ║
║  │  │  ┌───────────────────────────────────────────────────┐  │  │ ║
║  │  │  │  Query Router                                     │  │  │ ║
║  │  │  │  IF summary/aggregate query:                      │  │  │ ║
║  │  │  │    → Gold layer (fast response)                   │  │  │ ║
║  │  │  │  ELIF specific call examples:                     │  │  │ ║
║  │  │  │    → Silver layer + filters                       │  │  │ ║
║  │  │  │  ELIF semantic similarity:                        │  │  │ ║
║  │  │  │    → Vector search (Azure AI Search)              │  │  │ ║
║  │  │  └───────────────────────────────────────────────────┘  │  │ ║
║  │  │              ↓                                            │  │ ║
║  │  │  ┌───────────────────────────────────────────────────┐  │  │ ║
║  │  │  │  Query Execution                                  │  │  │ ║
║  │  │  │  - NL2SQL generation                              │  │  │ ║
║  │  │  │  - Query optimization                             │  │  │ ║
║  │  │  │  - Result formatting                              │  │  │ ║
║  │  │  │  - Source citation (CallIDs)                      │  │  │ ║
║  │  │  └───────────────────────────────────────────────────┘  │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                             ↓                                       ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │                      POWER BI                                  │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Semantic Models (Direct Lake mode)                     │  │ ║
║  │  │  - Direct query to Delta tables in lakehouse            │  │ ║
║  │  │  - No data import needed (real-time)                    │  │ ║
║  │  │  - Automatic refresh                                    │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  │              ↓                                                  │ ║
║  │  ┌─────────────────────────────────────────────────────────┐  │ ║
║  │  │  Executive Dashboards                                   │  │ ║
║  │  │  • Executive KPI Dashboard (daily refresh)              │  │ ║
║  │  │  • Competitor Intelligence Dashboard                    │  │ ║
║  │  │  • Promotion Performance Dashboard                      │  │ ║
║  │  │  • Sentiment & Churn Risk Dashboard                     │  │ ║
║  │  │  • Daily Operations Dashboard                           │  │ ║
║  │  │  • Drill-through to call details                        │  │ ║
║  │  └─────────────────────────────────────────────────────────┘  │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
║                                                                      ║
║  ┌───────────────────────────────────────────────────────────────┐ ║
║  │                 FABRIC MONITORING                              │ ║
║  │  - Pipeline execution status                                   │ ║
║  │  - Spark job performance                                       │ ║
║  │  - Capacity utilization (CU consumption)                       │ ║
║  │  - Data quality metrics                                        │ ║
║  └───────────────────────────────────────────────────────────────┘ ║
╚═════════════════════════════════════════════════════════════════════╝
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    EXTERNAL INTEGRATIONS                             │
│  ┌──────────────────────────────────────────────────────────┐       │
│  │         Azure OpenAI Service                             │       │
│  │  - Entity extraction via API calls from Spark            │       │
│  │  - Batch API for cost optimization                       │       │
│  │  - Token usage monitoring                                │       │
│  └──────────────────────────────────────────────────────────┘       │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────┐       │
│  │         Azure AI Search (Vector Database)                │       │
│  │  - Stores transcript embeddings                          │       │
│  │  - Enables semantic search                               │       │
│  │  - Synced from enriched_calls table                      │       │
│  └──────────────────────────────────────────────────────────┘       │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────┐       │
│  │         Azure Redis Cache (Optional)                     │       │
│  │  - Caches frequent query results                         │       │
│  │  - Reduces load on Fabric capacity                       │       │
│  └──────────────────────────────────────────────────────────┘       │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────┐       │
│  │         Microsoft Purview                                │       │
│  │  - Data lineage (automatic from Fabric)                  │       │
│  │  - PII detection & classification                        │       │
│  │  - Compliance reporting                                  │       │
│  └──────────────────────────────────────────────────────────┘       │
└─────────────────────────────────────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                         END USERS                                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│  │  Executives  │  │   Managers   │  │   Analysts   │              │
│  │  (Copilot +  │  │ (Dashboards) │  │  (Reports +  │              │
│  │  Dashboards) │  │              │  │   Ad-hoc)    │              │
│  └──────────────┘  └──────────────┘  └──────────────┘              │
└─────────────────────────────────────────────────────────────────────┘
```

## Implementation Recommendations

### Phase 1: Foundation (Months 1-2)

1. **Infrastructure Setup**

   - Provision Fabric workspace (F64 or F128 capacity)
   - Set up lakehouses (Bronze, Silver, Gold)
   - Configure Event Hub integration
   - Establish data governance policies

2. **Pipeline Development**
   - Build ingestion pipeline (Event Hub → Bronze)
   - Create Spark notebook for entity extraction
   - Set up Azure OpenAI connection
   - Develop validation logic

### Phase 2: Scale & Optimize (Months 3-4)

1. **Performance Optimization**

   - Implement batch processing at scale
   - Optimize Spark partitioning
   - Optimize LLM prompts for cost efficiency
   - Add error handling & retry logic
   - Monitor capacity utilization
   - Tune partitioning strategies

2. **Expand Entity Coverage**
   - Add remaining entity types
   - Build entity reference tables
   - Implement confidence scoring

### Phase 3: Analytics & Intelligence (Months 5-6)

1. **Build Aggregation Pipelines**

   - Build daily aggregation notebooks
   - Create Gold layer tables
   - Set up scheduled pipelines
   - Test incremental processing
   - Daily/weekly/monthly rollups
   - Trend analysis
   - Anomaly detection

2. **Create Curated Dashboards**
   - Create semantic models (Direct Lake)
   - Build executive dashboards
   - Set up row-level security
   - Configure automatic refresh
   - Executive dashboards
   - Operational dashboards
   - Self-service analytics

### Phase 4: Refinement & MLOps (Ongoing)

1. **Model Monitoring**

   - Track extraction accuracy
   - A/B test prompt variations
   - Implement human-in-the-loop feedback

2. **Cost Optimization**
   - Monitor LLM token usage
   - Optimize batch sizes
   - Implement tiered storage
   - Optimize Delta table layout (VACUUM, OPTIMIZE)
   - Add Z-ordering on key columns

## Key Design Decisions & Rationale

**Why Batch vs. Real-Time for Entity Extraction?**

- Batch processing (hourly/4-hourly) balances cost and latency
- Allows for micro-batch optimization and retry logic
- Real-time extraction would be 10x more expensive

**Why Pre-Aggregate Data?**

- 80% of executive queries hit the same patterns
- Pre-aggregation reduces query latency from 10-30s to <1s
- Dramatically reduces costs (fewer LLM calls for analysis)
- Enables complex trend analysis without scanning millions of rows

**Why Hybrid Search (NL2SQL + Vector)?**

- SQL for structured queries (counts, filters, aggregations)
- Vector search for semantic similarity ("Find calls similar to this complaint")
- Best of both worlds for enterprise analytics

**Why Fabric Instead of Databricks?**

- **Unified Platform**: Single workspace for all components (Spark, storage, BI, Copilot)
- **Direct Lake Mode**: Power BI queries Delta tables directly with no import/refresh delays
- **OneLake Architecture**: One copy of data shared across all Fabric workloads
- **Native Copilot Integration**: Built into the platform with seamless data model understanding
- **Cost Predictability**: Capacity Units (CU) pricing model easier to forecast than DBU consumption
- **Simpler Governance**: Integrated with Purview for automatic lineage tracking
- **Volume Appropriate**: Fabric Spark handles 2M calls/month efficiently without needing Databricks Photon

**Query Routing Strategy**

```
User Query → Fabric Data Agent
    ↓
Query Classifier
    ├─ "How many calls mentioned Verizon last week?"
    │   → SQL on daily_competitor_stats (Gold layer - fast)
    │
    ├─ "Show me examples of angry customers mentioning prices"
    │   → Vector search + sentiment filter on enriched_calls (Silver layer)
    │
    └─ "Compare our promotion performance to Q3"
        → SQL on aggregated tables (Gold layer) + previous quarter comparison
```

## Cost Estimation Framework

### LLM Processing Costs (Azure OpenAI GPT-4.1 Series)

> **Note:** Fabric Data Agent queries (NL2SQL + vector search) are NOT included in these costs - they use a separate quota/budget. These costs are ONLY for the entity extraction pipeline processing.

**Model Selection Strategy:**

- **GPT-4.1:** Complex entity extraction (competitors, promotions, cancellation reasons, product issues)
- **GPT-4.1-mini:** Simpler tasks (sentiment analysis, basic categorization, agent performance indicators)
- **Fabric Data Agent:** Handles all user queries separately (not counted here)

**Pricing Tiers (Per 1M Tokens) - Global Deployment:**

| Model            | Standard Input | Cached Input | Standard Output | Batch Input | Batch Output |
| ---------------- | -------------- | ------------ | --------------- | ----------- | ------------ |
| **GPT-4.1**      | $2.00          | $0.50        | $8.00           | $1.00       | $4.00        |
| **GPT-4.1-mini** | $0.40          | $0.10        | $1.60           | $0.20       | $0.80        |

**Provisioned Throughput Units (PTU):**

- Monthly Reservation: $286/PTU
- Yearly Reservation: $286 × 12 = $3,432/PTU (no discount mentioned, but commitment)

**Per-Call Processing Breakdown:**

Each call requires multiple LLM operations:

**High-Complexity Extraction (GPT-4.1):**

- Transcript: 1,500 tokens
- System prompt + entity reference lists: 800 tokens (cached after first use)
- Output (structured entities): 600 tokens
- Tasks: Competitor extraction, promotion identification, cancellation reasons, product issues

**Low-Complexity Extraction (GPT-4.1-mini):**

- Transcript: 1,500 tokens (reused)
- System prompt: 300 tokens (cached)
- Output: 200 tokens
- Tasks: Sentiment analysis, basic categorization, agent performance scoring

**Option 1: Standard API (No Caching)**

_GPT-4.1 (Complex Extraction):_

- Input: (2,300 tokens × $2.00) / 1M = $0.0046
- Output: (600 tokens × $8.00) / 1M = $0.0048
- Subtotal: $0.0094

_GPT-4.1-mini (Simple Tasks):_

- Input: (1,800 tokens × $0.40) / 1M = $0.00072
- Output: (200 tokens × $1.60) / 1M = $0.00032
- Subtotal: $0.00104

**Per call: $0.01044**
**Monthly (2M calls): $20,880**

**Option 2: Standard API (With Prompt Caching)**

_GPT-4.1 (with caching):_

- First call: $0.0094
- Subsequent: (1,500 × $2.00 + 800 × $0.50 + 600 × $8.00) / 1M = $0.0082
- Average: ~$0.0083

_GPT-4.1-mini (with caching):_

- First call: $0.00104
- Subsequent: (1,500 × $0.40 + 300 × $0.10 + 200 × $1.60) / 1M = $0.00095
- Average: ~$0.00096

**Per call: ~$0.00926**
**Monthly (2M calls): ~$18,520**
**Savings: 11% vs no caching**

**Option 3: Batch API (Recommended for This Use Case)**

_GPT-4.1 (Batch):_

- Input: (2,300 tokens × $1.00) / 1M = $0.0023
- Output: (600 tokens × $4.00) / 1M = $0.0024
- Subtotal: $0.0047

_GPT-4.1-mini (Batch):_

- Input: (1,800 tokens × $0.20) / 1M = $0.00036
- Output: (200 tokens × $0.80) / 1M = $0.00016
- Subtotal: $0.00052

**Per call: $0.00522**
**Monthly (2M calls): $10,440**
**Savings: 50% vs Standard API**

**Why Batch API is Ideal for This Scenario:**

- ✅ Processing can be delayed 1-4 hours (meets "near real-time" requirement)
- ✅ Processes calls in micro-batches (1000 calls each)
- ✅ 50% cost reduction is significant at scale
- ✅ No impact on user experience (queries hit pre-processed data)
- ✅ Leverages both GPT-4.1 and GPT-4.1-mini efficiently

**Option 4: Provisioned Throughput Units (PTU)**

PTU considerations for 2M calls/month with hybrid model usage:

- Total processing needed: ~1,540 calls/hour sustained (accounting for both model calls)
- With burst processing (4-hour windows): ~4,000 calls/hour
- Estimated PTU requirement: 25-35 PTUs (accounting for both GPT-4.1 and mini workloads)

**PTU Cost Analysis:**

- Monthly Reservation: 30 PTUs × $286 = $8,580/month
- Yearly Commitment: 30 PTUs × $286 × 12 = $102,960/year ($8,580/month)

**PTU Breakeven Analysis:**

- Batch API cost @ 2M calls: $10,440/month
- PTU cost: $8,580/month
- **PTU becomes cheaper at ~1.65M calls/month**

**When PTU Makes Sense:**

- ✅ **For workloads exceeding 1.65M calls/month** (immediate ROI)
- ✅ **For predictable, high-volume workloads** (your 2M calls/month qualifies)
- ✅ **When you need guaranteed throughput** (no rate limiting during peak processing windows)
- ✅ **For cost predictability** (fixed monthly cost vs variable token costs)

**PTU Recommendation for This Use Case:**

- **Year 1 (400K calls/month):** Use Batch API (~$2,088/month)
- **Scale-up (1M calls/month):** Continue with Batch API (~$5,220/month)
- **Steady State (2M+ calls/month):** Switch to PTU Monthly Reservation (~$8,580/month for unlimited processing)

### Recommended LLM Cost Strategy

**Hybrid Model + Batch API Approach (Recommended):**

1. **Primary Processing (GPT-4.1 Batch):** Complex entity extraction ($9,400/month @ 2M calls)
2. **Secondary Processing (GPT-4.1-mini Batch):** Sentiment & categorization ($1,040/month @ 2M calls)
3. **Fabric Data Agent Queries:** Separate budget - NOT counted in these costs

**Total Entity Extraction LLM Costs:**

- **400K calls/month (Initial):** ~$2,088/month (Batch API)
- **2M calls/month (Standard State - Batch API):** ~$10,440/month
- **2M calls/month (Optimized - PTU):** ~$8,580/month (16% savings + guaranteed throughput)

### Infrastructure Costs (Azure)

- **Microsoft Fabric:** ~$5,000-10,000/month
  - Spark processing (entity extraction orchestration)
  - Data warehouse (aggregated tables)
  - OneLake storage (hot + cold tiers)
- **Azure OpenAI (Entity Extraction Only):**
  - Batch API: $10,440/month @ 2M calls
  - PTU: $8,580/month @ 2M calls (cheaper at scale)
- **Azure OpenAI (Fabric Data Agent Queries):** ~$500-1,500/month (separate budget)
  - Natural language queries via Fabric Data Agent
  - NL2SQL + Vector search operations
  - Estimated 10K-30K queries/month
- **Azure AI Search:** ~$500-1,000/month (vector indexing)
- **Azure Redis Cache:** ~$200-500/month (query caching)
- **Azure Monitor + App Insights:** ~$500/month
- **Storage (Blob + Archive):** ~$1,000/month

**Total Monthly Cost:**

- **Standard Deployment (Batch API):** $28,140-34,940/month for 2M calls
- **Optimized Deployment (PTU for extraction):** $26,280-32,580/month for 2M calls
- **Per-call cost (fully loaded):** $0.013-0.017

### Cost Optimization Strategies

1. **Leverage Batch API (Primary Strategy)**

   - 50% immediate cost reduction on LLM processing
   - Fits perfectly with 1-4 hour processing latency requirement
   - Enables hybrid GPT-4.1 + GPT-4.1-mini approach

2. **Implement Prompt Caching**

   - Cache system prompts and entity reference lists
   - 50-75% reduction on cached tokens
   - Critical for minimizing repetitive prompt costs across 2M calls

3. **Strategic Model Selection (GPT-4.1 + mini hybrid)**

   - **GPT-4.1:** Complex extractions requiring reasoning (competitors, promotions, cancellation root cause)
   - **GPT-4.1-mini:** Pattern matching tasks (sentiment, categorization, scoring)
   - Hybrid approach saves ~10% vs GPT-4.1-only ($1,040/month @ 2M calls)

4. **Pre-filter Calls**

   - Skip entity extraction for calls <30 seconds (disconnects, wrong numbers)
   - Reduce processing volume by 10-15%
   - Potential savings: $1,000-1,500/month

5. **Optimize Batch Sizes**

   - Process in optimal micro-batches (1000 calls)
   - Maximize throughput while minimizing retries
   - Reduce token waste from failed/partial batches

6. **PTU for Predictable Costs at Scale**

   - Switch to PTU at 1.65M+ calls/month for immediate ROI
   - Lock in pricing with monthly reservation (no long-term commitment needed)
   - Eliminate variable cost risk and rate limiting
   - Guaranteed throughput for 4-hour batch processing windows

7. **Separate Fabric Data Agent Budget**
   - Don't count user query costs against extraction pipeline
   - Query costs are variable based on user activity, not call volume
   - Cache common executive queries aggressively (Redis)

This architecture provides enterprise-scale processing, maintains low query latency, and keeps costs predictable while delivering the natural language interface your executives need.
