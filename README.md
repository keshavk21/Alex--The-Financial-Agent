# Alex - Agentic Learning Equities Explainer

## Enterprise-Grade Multi-Agent Financial Planning Platform

A production-ready SaaS application that provides AI-powered financial insights through multi-agent collaboration. Built with serverless architecture on AWS.

## Features

- **Multi-Agent System**: 5 specialized AI agents (Planner, Tagger, Reporter, Charter, Retirement) working in orchestration
- **Real-time Financial Analysis**: Portfolio management, retirement projections, market research, and equity classification
- **Serverless Architecture**: AWS Lambda, Aurora Serverless v2, App Runner, API Gateway, SQS
- **Cost-Optimized**: S3 Vectors for vector storage (90% savings vs traditional solutions)
- **Full-Stack Application**: NextJS React frontend with Clerk authentication
- **Production-Grade**: Observability, monitoring, guardrails, security, and LangFuse integration

## Technology Stack

### Backend
- **Language**: Python (uv for dependency management)
- **AI Framework**: OpenAI Agents SDK
- **LLM Integration**: AWS Bedrock with Nova Pro
- **Infrastructure**: AWS Lambda, SQS, App Runner, RDS Aurora
- **Vector Search**: S3 Vectors + SageMaker embeddings
- **Observability**: LangFuse, CloudWatch

### Frontend
- **Framework**: Next.js (TypeScript)
- **Authentication**: Clerk
- **Styling**: PostCSS/Tailwind
- **Deployment**: CloudFront + S3

### Infrastructure as Code
- **Tool**: Terraform
- **State Management**: Local state per module

## Project Structure

```
alex/
├── backend/                 # Agent implementations
│   ├── planner/            # Orchestration agent
│   ├── tagger/             # Instrument classification
│   ├── reporter/           # Portfolio analysis
│   ├── charter/            # Data visualization
│   ├── retirement/         # Retirement projections
│   ├── researcher/         # Market research (App Runner)
│   ├── api/                # FastAPI backend
│   ├── database/           # Shared DB library
│   └── ingest/             # Document ingestion
├── frontend/               # NextJS application
├── terraform/              # AWS infrastructure
│   ├── 2_sagemaker/       # Embedding endpoint
│   ├── 3_ingestion/       # Vector storage
│   ├── 4_researcher/      # Research service
│   ├── 5_database/        # Aurora Serverless
│   ├── 6_agents/          # Lambda agents
│   ├── 7_frontend/        # CDN & hosting
│   └── 8_enterprise/      # Monitoring & observability
└── scripts/               # Deployment utilities
```

## Getting Started

### Prerequisites
- AWS account with appropriate IAM permissions
- Docker (for Lambda packaging)
- uv (Python package manager)
- Node.js 18+ (for frontend)
- Terraform

### Quick Start

1. **Configure AWS credentials**
   ```bash
   aws configure
   ```

2. **Review architecture**
   - Read [architecture.md](architecture.md) for system design
   - Read [agent_architecture.md](guides/agent_architecture.md) for agent patterns

3. **Deploy infrastructure**
   - Each terraform subdirectory is independent
   - Configure `terraform.tfvars` before applying
   - Deploy in order: SageMaker → Ingestion → Researcher → Database → Agents → Frontend

4. **Deploy backend agents**
   - Use `package_docker.py` in each agent directory (requires Docker running)
   - Configure environment variables

5. **Deploy frontend**
   - Install dependencies: `npm install`
   - Configure Clerk credentials in `.env.local`
   - Run `scripts/deploy.py`

## Key Implementation Details

### Agent Orchestration
Agents use OpenAI Agents SDK with AWS Bedrock integration via LiteLLM. Each agent is independent but can communicate through the orchestration queue.

### Database Design
Aurora Serverless v2 with Data API enabled (no VPC configuration needed). Supports multi-tenant architecture with user isolation.

### Cost Optimization
- S3 Vectors for vector embeddings (vs OpenSearch)
- Serverless computing (pay per invocation)
- Data API reduces need for connection pooling
- CloudFront caching for frontend

## Development

### Testing
- `test_simple.py` - Local testing with mocks
- `test_full.py` - Integration testing with AWS
- CloudWatch logs for production debugging

### Local Development
```bash
cd backend
uv run python run_local.py
```

## Security Considerations

- API Gateway with API key authentication
- IAM role-based access per service
- Encrypted credentials in Secrets Manager
- VPC endpoints for AWS services (available in enterprise config)
- GuardDuty monitoring

## Monitoring & Observability

- CloudWatch dashboards for metrics and logs
- LangFuse for AI observability
- CloudWatch alarms for critical services
- Structured logging across agents

## Cost Management

Monitor costs regularly in AWS Cost Explorer:
- Aurora Serverless (largest cost) - destroy when not in use
- Lambda invocations
- SageMaker endpoints
- Data transfer charges

Destroy infrastructure with: `python scripts/destroy.py`