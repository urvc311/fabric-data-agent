# Fabric Data Agent - AI-Powered CSV Processing

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Microsoft Fabric](https://img.shields.io/badge/Microsoft-Fabric-0078D4.svg)](https://www.microsoft.com/en-us/analytics/fabric)

An enterprise-grade AI-powered data agent for Microsoft Fabric that intelligently processes CSV files, validates data quality, applies transformations, and generates business insights using a medallion architecture (bronze → silver → gold).

## ✨ Features

### 🤖 AI-Powered Intelligence
- **Natural Language Processing**: Describe data operations in plain English
- **Automatic Schema Detection**: Infer data types and structure automatically
- **Intelligent Error Recovery**: Self-healing with exponential backoff
- **Anomaly Detection**: Identify statistical outliers and unusual patterns
- **Trend Analysis**: Generate business insights automatically

### 🏗️ Medallion Architecture
- **Bronze Layer**: Raw data preservation with metadata
- **Silver Layer**: Cleaned, deduplicated, standardized data
- **Gold Layer**: Aggregated, business-ready metrics and KPIs

### 🔍 Quality Validation
- 6 comprehensive data quality rules
- Schema consistency validation
- Null value detection
- Duplicate identification & removal
- Data type validation with auto-coercion
- Outlier detection using IQR method
- Referential integrity checks

### 📊 Processing Capabilities
- CSV, TSV, JSON, Parquet file support
- Auto encoding detection (UTF-8, Latin-1, ISO-8859-1)
- Automatic delimiter detection
- Multi-file batch processing
- Scalable to 10M+ rows per file

### 🛡️ Enterprise Features
- Role-based security & audit logging
- GDPR compliance
- Encryption at rest & in transit
- Error tracking with 5+ recovery strategies
- Real-time monitoring & alerts
- Email & Teams notifications
- Automated scheduling

## 🚀 Quick Start

### 1. Prerequisites
```bash
pip install pandas pyspark azure-identity azure-storage-filedatalake
```

### 2. Deploy in Fabric Notebook

```python
from ai_agent_setup import AgentDeploymentOrchestrator
import asyncio

# Define CSV files to process
csv_files = [
    "/Files/csv_data/sales.csv",
    "/Files/csv_data/customers.csv",
    "/Files/csv_data/products.csv"
]

# Initialize and deploy
orchestrator = AgentDeploymentOrchestrator("agent-config.json")
results = await orchestrator.deploy_with_csv_files(csv_files)

# Display results
import json
print(json.dumps(results, indent=2))
```

### 3. Monitor Results

The agent automatically:
1. ✅ **Initializes** - Validates workspace & config
2. ✅ **Ingests** - Loads CSVs → Bronze layer
3. ✅ **Validates** - Runs 6 quality checks
4. ✅ **Transforms** - Creates Silver & Gold layers
5. ✅ **Generates Insights** - Identifies trends & anomalies
6. ✅ **Completes** - Generates comprehensive reports

## 📁 Project Structure

```
fabric-data-agent/
├── agent-config.json              # AI agent configuration
├── ai_agent_setup.py             # Agent implementation
├── agent_deployment_guide.md     # Detailed deployment guide
├── README.md                      # This file
└── examples/
    ├── basic_deployment.ipynb
    ├── custom_validation_rules.ipynb
    └── power_bi_integration.ipynb
```

## 🔧 Configuration

### Agent Identity
The agent operates as an enterprise-grade data orchestrator with:
- Automatic CSV ingestion
- Quality validation
- Schema transformation
- Medallion architecture management
- Error handling & recovery
- Performance optimization

### Data Quality Rules

| Rule | Severity | Auto-Fix | Purpose |
|------|----------|----------|----------|
| Schema Validation | Critical | ❌ | Ensure consistent schema |
| Null Check | Warning | ❌ | Detect missing values |
| Duplicate Detection | High | ✅ | Identify & remove duplicates |
| Data Type Validation | Critical | ✅ | Validate & coerce types |
| Outlier Detection | Medium | ❌ | Find statistical anomalies |
| Referential Integrity | High | ❌ | Check foreign keys |

### AI Tools Available

```python
tools = [
    "load_csv_files",                # Load with format detection
    "validate_data_quality",         # Run quality checks
    "detect_schema",                 # Auto schema inference
    "transform_data",                # Apply transformations
    "generate_insights",             # Create analytics
    "manage_medallion_architecture",  # Bronze→Silver→Gold
    "handle_errors",                 # Error detection & recovery
    "generate_report"                # Generate comprehensive reports
]
```

## 📊 Output Destinations

- **Fabric Warehouse**: Delta format with overwrite mode
- **Fabric Lakehouse**: Parquet format with append mode
- **Power BI Semantic Model**: Auto-refresh every 60 minutes

## 🔐 Security & Compliance

- ✅ Azure AD authentication
- ✅ Row-level security (RLS)
- ✅ Encryption at rest
- ✅ Encryption in transit
- ✅ Audit logging
- ✅ GDPR compliance
- ✅ 90-day data retention policy

## ⚡ Performance

### File Size Limits
- Max file size: 5 GB
- Max rows per file: 10 million
- Max columns: 500
- Processing timeout: 30 minutes

### Parallel Processing
- Max parallel workers: 4
- Sequential execution by default
- Optional parallel mode for independent files

## 🐛 Troubleshooting

### Issue: Quality Score Below Threshold
```python
# Check detailed quality report
quality = results['stages']['validation']
for check in quality['checks']:
    if check['status'] != 'passed':
        print(f"Failed: {check['check']}")
```

### Issue: Encoding Errors
```python
# Specify encoding manually
csv_config = CSVFileConfig(
    file_path="/path/to/file.csv",
    encoding="latin-1"
)
```

### Issue: Memory/Timeout
```python
# Split large files or increase timeout
agent_config['file_limits']['timeout_seconds'] = 3600
```

See [agent_deployment_guide.md](./agent_deployment_guide.md) for more troubleshooting.

## 📖 Documentation

- **[Deployment Guide](./agent_deployment_guide.md)** - Complete setup instructions
- **[Configuration Reference](./agent-config.json)** - All config options
- **[Implementation Guide](./ai_agent_setup.py)** - Source code documentation

## 🤝 Integration Examples

### With Power BI
```python
# Gold layer automatically syncs to Power BI
# Configure auto-refresh in agent-config.json
"power_bi_semantic_model": {
    "auto_refresh": true,
    "refresh_interval_minutes": 60
}
```

### Custom Quality Rules
```python
# Add custom validation rules
custom_rule = DataValidationRule(
    rule_id="custom_business_logic",
    description="Validate business-specific rules",
    severity=DataQualityLevel.HIGH,
    auto_remediate=False
)
```

### Scheduled Execution
```json
{
  "scheduling_config": {
    "enabled": true,
    "frequency": "daily",
    "time_utc": "02:00",
    "timeout_minutes": 120
  }
}
```

## 📞 Support

- 📖 [Microsoft Fabric Docs](https://learn.microsoft.com/en-us/fabric/)
- 🤖 [GitHub Copilot SDK](https://docs.github.com/en/copilot/overview)
- 🔗 [Microsoft Agent Framework](https://github.com/Microsoft/teams-ai)
- 🐍 [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)

## 📝 License

MIT License - See LICENSE file for details

## 💡 Use Cases

✅ **Data Warehouse Loading** - Automate CSV ingestion into Fabric
✅ **ETL Pipelines** - Build automated data transformation workflows
✅ **Data Quality Monitoring** - Track and improve data quality metrics
✅ **Business Analytics** - Generate insights from raw CSV data
✅ **Data Governance** - Maintain audit trails and compliance
✅ **Multi-Source Integration** - Process multiple data sources

## 🚀 Next Steps

1. Clone this repository
2. Configure your CSV files
3. Deploy in Fabric notebook
4. Monitor execution
5. Set up automated scheduling
6. Connect to Power BI
7. Configure alerts

For detailed instructions, see [agent_deployment_guide.md](./agent_deployment_guide.md)

---

**Built with ❤️ for enterprise data teams using Microsoft Fabric**
