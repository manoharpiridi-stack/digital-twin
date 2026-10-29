# Digital Twin

A chatbot that represents me on a website: visitors ask about my background and it answers as me, from my own notes,
and remembers the conversation. Built as a course project while learning to deploy AI applications to production
(Ed Donner's course).

**Status:** not deployed at the moment. The code, infrastructure and pipeline are complete.

## How it works

| Part | Technology | Where |
|---|---|---|
| Frontend | Next.js (static export), hosted on S3 and served through CloudFront | `frontend/` |
| API | FastAPI on AWS Lambda (via Mangum), behind API Gateway (HTTP API): `POST /chat`, `GET /health` | `backend/server.py`, `backend/lambda_handler.py` |
| Model | Amazon Bedrock (Nova Micro) through the Converse API | `backend/server.py` |
| What the twin knows | The system prompt is built from my facts, summary, writing style and LinkedIn profile | `backend/context.py`, `backend/data/` |
| Memory | Each conversation is saved as JSON per session: in S3 on AWS, in a local `memory/` folder when run locally | `backend/server.py` |
| Infrastructure | Terraform with remote state in S3; optional custom domain with Route 53 | `terraform/` |
| CI/CD | GitHub Actions: deploy on push to `main`, manual destroy; AWS access through an OIDC role, no stored AWS keys | `.github/workflows/` |

## Deploy

```bash
./scripts/deploy.sh      # or scripts/deploy.ps1 on Windows
./scripts/destroy.sh     # removes everything
```

The GitHub Actions workflows do the same with the repository secrets `AWS_ROLE_ARN`, `AWS_ACCOUNT_ID` and
`DEFAULT_AWS_REGION`.
