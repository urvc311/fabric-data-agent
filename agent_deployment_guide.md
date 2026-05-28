# Fabric Data Agent AI Configuration Guide

## Overview

This guide provides step-by-step instructions for deploying an AI-powered Fabric Data Agent configured to process CSV files with intelligent orchestration, quality validation, and automated transformation.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│           Fabric Data Agent (AI-Powered)                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Stage 1: Initialization                             │  │
│  │ • Validate workspace access                         │  │
│  │ • Load AI agent configuration                       │  │
│  │ • Initialize logging & monitoring                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Stage 2: Data Ingestion (Bronze Layer)              │  │
│  │ • Load CSV files with auto encoding detection       │  │
│  │ • Create bronze layer (raw data)                    │  │
│  │ • Add ingestion metadata & timestamps               │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Stage 3: Quality Validation                         │  │
│  │ • Run 6+ data quality checks                        │  │
│  │ • Detect schema issues, nulls, duplicates           │  │
│  │ • Generate quality report (95%+ threshold)          │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Stage 4: Transformation (Silver & Gold Layers)      │  │
│  │ • Silver: Clean, dedupe, standardize formats        │  │
│  │ • Gold: Aggregate, apply business rules             │  │
│  │ • Create medallion architecture                     │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Stage 5: Insight Generation                         │  │
│  │ • Generate business insights & trends               │  │
│  │ • Detect anomalies & outliers                       │  │
│  │ • Create summary analytics                          │  │
│  └──────────────────────────────────────────────────────┘  │
│                           ↓                                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Stage 6: Completion & Reporting                     │  │
│  │ • Generate final comprehensive report               │  │
│  │ • Publish metrics & KPIs                            │  │
│  │ • Send notifications                                │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘

Output Destinations:
├── Fabric Warehouse (Delta format)
├── Fabric Lakehouse (Parquet format)
└── Power BI Semantic Model (Auto-refresh)
```

## Prerequisites

### 1. **Microsoft Fabric Environment**
- Active Microsoft Fabric workspace
- Data Engineering or Data Science experience access
- Workspace admin permissions for first deployment

### 2. **Python Environment**
```bash
# Required packages
pip install pandas==2.0.0+
pip install pyspark==3.4.0+
pip install azure-identity==1.13.0+
pip install azure-storage-filedatalake==12.13.0+
```

### 3. **Azure Configuration**
- Azure AD credentials
- Storage connection strings
- Workspace identifiers

## Deployment Steps

### Step 1: Prepare Configuration Files

**Location:** Project root directory

```bash
# Files to create:
- agent-config.json          # AI agent configuration
- ai_agent_setup.py          # Agent implementation
```

### Step 2: Configure CSV Files

Update your CSV file list in the configuration:

```json
{
  "csv_files": [
    "/workspace/data/sales.csv",
    "/workspace/data/customers.csv",
    "/workspace/data/products.csv"
  ],
  "csv_parameters": {
    "delimiter_detection": true,
    "encoding_detection": ["utf-8", "latin-1", "iso-8859-1"],
    "header_row": 1,
    "null_values": ["null", "NULL", "None", "N/A", ""]
  }
}
```

### Step 3: Deploy in Fabric Notebook

Create a new Fabric notebook and execute:

```python
# In Fabric Notebook Cell 1: Install dependencies
%pip install pandas pyspark azure-identity azure-storage-filedatalake

# In Fabric Notebook Cell 2: Import and configure
from ai_agent_setup import AgentDeploymentOrchestrator
import asyncio
import json

# Load agent configuration
with open('/Workspace/agent-config.json', 'r') as f:
    config = json.load(f)

# Define CSV files to process
csv_files = [
    "/Files/csv_data/sales.csv",
    "/Files/csv_data/customers.csv",
    "/Files/csv_data/products.csv"
]

# In Fabric Notebook Cell 3: Deploy agent
orchestrator = AgentDeploymentOrchestrator('/Workspace/agent-config.json')
results = await orchestrator.deploy_with_csv_files(csv_files)

# Display results
print(json.dumps(results, indent=2))
```

### Step 4: Monitor Deployment

**Real-time Monitoring:**
- Check Fabric notebook execution logs
- Monitor workspace items for bronze/silver/gold layers
- View quality validation results

**Key Metrics to Track:**
- Files processed: `results['stages']['ingestion']['files_processed']`
- Data quality score: `results['stages']['validation']['quality_score']`
- Total rows processed: `results['stages']['ingestion']['total_rows']`
- Insights generated: `len(results['stages']['insights']['insights'])`

## AI Agent Configuration Details

### System Prompt

The agent operates under this system prompt:

```
You are an enterprise-grade Fabric Data Agent responsible for ingesting, 
validating, transforming, and optimizing CSV files in Microsoft Fabric.

Core Responsibilities:
1. Load CSV files with automatic encoding detection
2. Validate data quality and detect anomalies
3. Apply business transformations
4. Create medallion architecture (bronze/silver/gold)
5. Handle errors gracefully with automatic recovery
6. Provide actionable insights and metadata
```

### Available AI Tools

| Tool ID | Name | Purpose | Timeout |
|---------|------|---------|----------|
| `load_csv_files` | Load CSV Files | Ingest CSV with format detection | 300s |
| `validate_data_quality` | Validate Data Quality | Run quality checks | 600s |
| `detect_schema` | Detect Schema | Infer schema automatically | 120s |
| `transform_data` | Transform Data | Apply transformations | 1800s |
| `generate_insights` | Generate Insights | Create analytics | 300s |
| `manage_medallion_architecture` | Manage Medallion | Bronze→Silver→Gold | 3600s |
| `handle_errors` | Handle Errors | Error detection & recovery | 300s |
| `generate_report` | Generate Report | Create reports | 300s |

### Data Quality Rules

The agent validates against 6 quality rules:

```json
{
  "rules": [
    {
      "rule_id": "schema_validation",
      "severity": "critical",
      "auto_remediate": false
    },
    {
      "rule_id": "null_check",
      "threshold_percent": 20,
      "severity": "warning",
      "auto_remediate": false
    },
    {
      "rule_id": "duplicate_detection",
      "severity": "high",
      "auto_remediate": true
    },
    {
      "rule_id": "data_type_validation",
      "severity": "critical",
      "auto_remediate": true
    },
    {
      "rule_id": "outlier_detection",
      "method": "iqr",
      "severity": "medium",
      "auto_remediate": false
    },
    {
      "rule_id": "referential_integrity",
      "severity": "high",
      "auto_remediate": false
    }
  ]
}
```

## Medallion Architecture Output

### Bronze Layer (Raw Data)
- **Purpose:** Store raw CSV data as-is
- **Format:** Delta
- **Partitioning:** By date
- **Path:** `/Lakehouse/bronze/data`
- **Operations:** Raw ingestion, metadata addition

### Silver Layer (Cleaned Data)
- **Purpose:** Cleansed and validated data
- **Format:** Delta
- **Partitioning:** By date and category
- **Path:** `/Lakehouse/silver/data`
- **Operations:** Deduplication, null handling, standardization

### Gold Layer (Business-Ready)
- **Purpose:** Aggregated, optimized data for BI
- **Format:** Delta
- **Partitioning:** By business unit and date
- **Path:** `/Lakehouse/gold/data`
- **Operations:** Aggregation, metrics calculation, business rules

## Error Handling & Recovery

The agent implements intelligent error recovery:

| Error Type | Action | Retry | Backoff |
|-----------|--------|-------|----------|
| File not found | Skip & log | 0 | N/A |
| Encoding error | Auto-detect & retry | 3 | Default |
| Schema mismatch | Flag & review | 0 | N/A |
| Timeout | Retry with backoff | 3 | 2x exponential |
| Storage error | Fallback & notify | 5 | 2x exponential |

## Monitoring & Alerts

### Key Metrics

```python
metrics = {
    "files_processed": 3,
    "total_rows_processed": 150000,
    "processing_time_seconds": 1243,
    "data_quality_score": 96.5,
    "error_count": 0,
    "transformation_success_rate": 100.0
}
```

### Alert Thresholds

```json
{
  "alerts": [
    {
      "metric": "data_quality_score",
      "threshold": 90,
      "condition": "below",
      "action": "alert_and_pause"
    },
    {
      "metric": "error_count",
      "threshold": 5,
      "condition": "above",
      "action": "alert_and_notify"
    },
    {
      "metric": "processing_time_seconds",
      "threshold": 3600,
      "condition": "above",
      "action": "alert_and_timeout"
    }
  ]
}
```

## Scheduling Automated Runs

### Configure Scheduled Execution

```json
{
  "scheduling_config": {
    "enabled": true,
    "schedule_type": "recurring",
    "frequency": "daily",
    "time_utc": "02:00",
    "timeout_minutes": 120,
    "max_concurrent_runs": 1,
    "retry_policy": {
      "max_retries": 3,
      "backoff_strategy": "exponential"
    }
  }
}
```

### Setup Fabric Scheduler

1. Create Fabric Notebook
2. Add agent deployment code
3. Schedule execution via Fabric UI
4. Set frequency (daily, hourly, weekly)
5. Configure notifications

## Notifications

### Email Notifications

```json
{
  "type": "email",
  "recipients": ["data-team@company.com"],
  "on_events": ["completion", "failure", "quality_issues"]
}
```

### Teams Notifications

```json
{
  "type": "teams",
  "webhook_url": "YOUR_TEAMS_WEBHOOK_URL",
  "on_events": ["critical_errors", "quality_threshold_breach"]
}
```

## Performance Optimization

### File Size Limits
- Max file size: 5 GB
- Max rows per file: 10 million
- Max columns: 500
- Timeout: 30 minutes

### Parallel Processing
- Max parallel workers: 4
- Execution mode: Sequential by default
- Can enable parallel for independent files

### Partitioning Strategy
- Bronze: By date
- Silver: By date + category
- Gold: By business unit + date

## Troubleshooting

### Common Issues

**Issue: Files Not Found**
```python
# Solution: Verify file paths
import os
csv_files = [f for f in os.listdir("/Files/csv_data") if f.endswith('.csv')]
print(csv_files)
```

**Issue: Encoding Errors**
```python
# Solution: Specify encoding
csv_config = CSVFileConfig(
    file_path="/path/to/file.csv",
    encoding="latin-1"  # Try different encoding
)
```

**Issue: Quality Score Below Threshold**
```python
# Solution: Check quality report details
quality = results['stages']['validation']
for check in quality['checks']:
    if check['status'] != 'passed':
        print(f"Failed check: {check['check']}")
```

**Issue: Memory/Timeout Errors**
```python
# Solution: Process in batches
# Split large CSV files into smaller chunks
# Increase timeout settings in config
"timeout_seconds": 3600  # Increase as needed
```

## Best Practices

1. **Validate CSV files before upload**
   - Check encoding (UTF-8 recommended)
   - Verify schema consistency
   - Clean null/missing values

2. **Monitor quality scores**
   - Aim for 95%+ quality score
   - Review recommendations
   - Address data issues proactively

3. **Secure configuration**
   - Store credentials in Key Vault
   - Enable encryption at rest/in transit
   - Audit all data access

4. **Document transformations**
   - Add comments to business rules
   - Maintain transformation logs
   - Track data lineage

5. **Schedule regular runs**
   - Daily automated processing
   - Incremental updates
   - Historical data retention

## Support & Resources

- [Microsoft Fabric Documentation](https://learn.microsoft.com/en-us/fabric/)
- [GitHub Copilot SDK Documentation](https://docs.github.com/en/copilot/overview)
- [Microsoft Agent Framework](https://github.com/Microsoft/teams-ai)
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)

## Next Steps

1. Deploy the agent with your CSV files
2. Monitor initial execution
3. Review quality reports
4. Validate medallion layers
5. Set up automated scheduling
6. Integrate with Power BI
7. Configure alerts and notifications
