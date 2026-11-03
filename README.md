# Customer360-AI

## Executive summary

Customers, prospects, opportunities, meetings and job leads in one place, answered in plain language. Deals live in [Twenty](https://twenty.com), a CRM the business owns and hosts wherever it chooses. [Fireflies](https://fireflies.ai) records meetings, and they are indexed by meaning. One assistant, Customer360, answers from the CRM, the meetings and the web ([Brave Search](https://search.brave.com)), cites each source, and changes the CRM only after a person approves.

- **Dashboard assistant (Customer360).** Ask what a kickoff decided, what stage a deal is in, or a company's latest news, and get one cited answer from <img src="assets/icons/twenty.png" width="18" alt=""> [Twenty CRM](https://twenty.com), <img src="assets/icons/fireflies.png" width="18" alt=""> [Fireflies](https://fireflies.ai) and <img src="assets/icons/brave.svg" width="18" alt=""> [Brave Search](https://search.brave.com). <img src="assets/icons/react.png" width="18" alt=""> [React](https://react.dev/) serves the pages, <img src="assets/icons/litellm.png" width="18" alt=""> [LiteLLM](https://litellm.ai/) routes model and tool calls, <img src="assets/icons/agent-gateway.png" width="18" alt=""> [Agent Gateway](https://agentgateway.dev/) fronts the MCP servers, <img src="assets/icons/okta.png" width="18" alt=""> [Okta](https://www.okta.com/) issues the JWT for the MCP gateway, and <img src="assets/icons/agent-core.png" width="18" alt=""> [AgentCore](https://aws.amazon.com/bedrock/agentcore/) hosts the three MCP servers. Changes need a person's approval. One question replaces three tools and a spreadsheet.
- **Jobs and Application Tracking.** Jobs arrive as files from <img src="assets/icons/github-actions.png" width="18" alt=""> [GitHub Actions](https://github.com/features/actions) or as <img src="assets/icons/csv.svg" width="18" alt=""> CSV uploads to <img src="assets/icons/s3.png" width="18" alt=""> [S3](https://aws.amazon.com/s3/). <img src="assets/icons/dynamodb.svg" width="18" alt=""> [DynamoDB](https://aws.amazon.com/dynamodb/) stages each run from raw to selected, S3 keeps the raw archive and the selected jobs, <img src="assets/icons/glue.svg" width="18" alt=""> [Glue](https://aws.amazon.com/glue/) catalogs the tables, and <img src="assets/icons/bedrock.png" width="18" alt=""> [Bedrock](https://aws.amazon.com/bedrock/) embeds each job for search. <img src="assets/icons/sqs.svg" width="18" alt=""> [SQS](https://aws.amazon.com/sqs/) hands each file to <img src="assets/icons/redpanda.png" width="18" alt=""> [Redpanda Connect](https://www.redpanda.com/connect), which feeds <img src="assets/icons/kafka.png" width="18" alt=""> [Kafka](https://kafka.apache.org/) (one topic per lane) into <img src="assets/icons/opensearch.png" width="18" alt=""> [OpenSearch](https://opensearch.org/). Each posting is graded for fit and flagged when off target (wrong employer type, below the pay floor, not AI); nothing is deleted and all stay searchable. Approved jobs move to <img src="assets/icons/twenty.png" width="18" alt=""> [Twenty CRM](https://twenty.com) and through the board from first contact to outcome, with no re-typing.
- **Slack and Jira (optional, off in this deployment).** Ask in a Slack channel and get a short answer in the thread. A new or changed <img src="assets/icons/jira.svg" width="18" alt=""> [Jira](https://www.atlassian.com/software/jira) ticket is queued for a separate job that prepares a pull request for review. <img src="assets/icons/end-user-messaging.svg" width="18" alt=""> [AWS End User Messaging](https://aws.amazon.com/end-user-messaging/) sends outbound customer notifications. All three verify the sender and alert on failure.
- **Platform.** Code lives in <img src="assets/icons/github.png" width="18" alt=""> [GitHub](https://github.com/) and <img src="assets/icons/git-sync.png" width="18" alt=""> [Git sync](https://github.com/kubernetes/git-sync) pulls it into a <img src="assets/icons/talos.png" width="18" alt=""> [Talos](https://www.talos.dev/) cluster, which <img src="assets/icons/argo-cd.png" width="18" alt=""> [Argo CD](https://argo-cd.readthedocs.io/en/stable/) keeps converged to Git. Credentials come from <img src="assets/icons/secrets-manager.png" width="18" alt=""> [Secrets Manager](https://aws.amazon.com/secretsmanager/) through <img src="assets/icons/external-secrets.png" width="18" alt=""> [External Secrets](https://external-secrets.io/). One commit deploys the product, and nothing secret lives in git.

## In action

**Three questions about a made-up company, then the Job Insights page.**

![Asking three questions about a made-up company, answered from the CRM, the meetings and the web](assets/customer360-chat-demo-2026-10-30.gif)

**Search the jobs.**

![Searching the Jobs page](assets/jobs-page-2026-10-03_03-06-33.gif)

Both clips are recorded on demo data. The company, employers and every figure (Acme Demo Co (DEMO)) are made up; no real customer or system is in it.

## Architecture

Scheduled collection and a CSV upload feed the same lake.

![Customer360-AI public architecture](assets/diagrams/public/diagram-public-v2.png)

- **Web UI.** The <img src="assets/icons/react.png" width="18" alt=""> [React](https://react.dev) pages and their API ship together to Kubernetes through <img src="assets/icons/argo-cd.png" width="18" alt=""> [Argo CD](https://argo-cd.readthedocs.io).
- **Credentials.** Every key comes from a password manager at run time; none is in the code or the images.

## License

PolyForm Noncommercial 1.0.0: free for noncommercial use; resale or any commercial use needs written permission first. See [LICENSE](LICENSE).
