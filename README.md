# AI Lead Research & Website Audit Automation

> **Architecture:** See [ARCHITECTURE.md](ARCHITECTURE.md) for the infographic, detailed flow, and trust boundaries.

An n8n workflow that turns a user-defined business search into a reviewed lead-research packet. It discovers public business listings, fetches public website text through a reader service, creates structured AI-assisted website and lead assessments, drafts outreach for human review, and prepares a proposal PDF for configured storage.

> **Portfolio-safe workflow export:** `workflow.json` is a sanitized template. Credentials and workspace-specific Google/Slack identifiers have been removed. Configure your own credentials and resources before importing or activating it.

## Project snapshot

- **Category:** Workflow automation, AI agents, lead research, n8n
- **Orchestration:** n8n workflow with validation, branching, retries, and human approval
- **External services represented:** SerpAPI, Jina Reader, an OpenRouter chat model, Google Sheets, Gmail, Slack, Google Drive, and PDFBolt
- **Outputs:** Structured website audit, lead score, outreach draft, reviewer decision, CRM status, and proposal PDF
- **Contact policy:** The workflow drafts outreach for review. It does not send an email to a prospect or schedule a follow-up; no verified prospect email is enriched in this design.

## What it does

1. Accepts a POST request containing a search query, location, optional industry/business type/country, and reviewer email.
2. Normalizes and validates the request, then returns an HTTP `202` for accepted work or `400` for invalid input.
3. Searches public business listings and normalizes the returned name, URL, snippet, and domain.
4. Rejects URLs that fail the current HTTPS hostname pattern, fetches website text through Jina Reader, and limits the content passed to the audit agent.
5. Produces a structured website audit and validates its score/output shape.
6. Checks a Google Sheets CRM for an existing domain to avoid processing duplicates.
7. Produces a structured lead score and a personalized outreach draft; validates both before continuing.
8. Saves the lead for review and sends the draft to the configured reviewer using a Gmail approval gate.
9. Records approval or rejection, posts a status notification to Slack, and on approval creates a proposal, renders it as a PDF, and uploads it to the configured Drive folder.

## Workflow inputs

| Field | Required | Notes |
|---|---:|---|
| `search_query` | Yes | 3–150 characters |
| `location` | Yes | 2–100 characters |
| `reviewer_email` | Yes | Must be a valid email address; this is the internal reviewer, not a prospect |
| `industry` | No | Optional search context |
| `business_type` | No | Optional search context |
| `country` | No | Two-letter country code; defaults to `us` in the template |

Example request (replace host and value with your own deployment data):

```bash
curl -X POST "$N8N_WEBHOOK_URL" \
  -H "Content-Type: application/json" \
  -H "X-Workflow-Auth: $WORKFLOW_WEBHOOK_SECRET" \
  -d '{"search_query":"independent dental clinic","location":"Austin, TX","industry":"healthcare","reviewer_email":"reviewer@example.com","country":"us"}'
```

Do not place real secrets in the command, source control, screenshots, or public portfolio materials. Prefer a dedicated header-auth credential configured in n8n.

## Import and configure

1. Use a current n8n installation compatible with the node types/versions in the export. Install any community nodes required by your own deployment (notably PDFBolt and the n8n LangChain nodes).
2. Import `workflow.json` into a **test** n8n workspace. The JSON is a template, not a ready-to-run deployment.
3. Create credentials in n8n for SerpAPI, OpenRouter, Google Sheets, Gmail, Slack, Google Drive, and PDFBolt. The export intentionally contains no credential bindings or credential values.
4. Configure your own spreadsheet ID and tab, Drive folder, Slack channel, and authorized reviewer. Replace the marked `REPLACE_WITH_YOUR_...` values in the imported workflow using your own non-production resources first.
5. Add webhook Header Auth (the note on the Webhook node is intentional), network-level rate limiting, request/body-size limits, and an execution/data-retention policy. Do not activate the webhook publicly until these are verified.
6. Harden the outbound URL validation as described under **Security considerations** before using the workflow with untrusted search results.
7. Run test cases for invalid input, no search results, an unsafe URL, a fetch failure, duplicate domain, invalid AI output, reviewer rejection, and reviewer approval. Confirm that no prospect email is sent.
8. Activate only after the integrations, privacy notices, provider terms, and access controls are configured for your own environment.

## Security and privacy notes

This public export has been scrubbed of credential bindings, webhook IDs, the source n8n instance identifier, original Google Sheet/Drive identifiers, the Slack channel ID, and the named owner signature. That cleanup **does not prove that the original workflow or any previous copy is secure**. If a credential or endpoint secret was ever exposed in a public repository, rotate/revoke it at the provider. Review repository history and backups too.

### Important hardening before production

- **Outbound URL / SSRF:** The source workflow's regex checks HTTPS and excludes a few hostname patterns, but does not reliably block private, loopback, link-local, IPv6, unusual IP encodings, DNS rebinding, or all redirect cases. The fetch is routed through an external reader service, which is another trust boundary. Replace the pattern-only check with a dedicated URL validation/proxy design that resolves DNS, blocks all non-public address ranges, re-validates redirects and DNS, enforces allowed schemes/ports, and applies egress controls. Test adversarial URLs before activation.
- **Webhook protection:** Add authentication, rate limits, abuse monitoring, body-size caps, and non-production testing. Keep the webhook path private until protected.
- **AI prompt injection:** Web content/search snippets are untrusted. The prompts instruct models to ignore embedded commands, but this is defense-in-depth, not a guarantee. Never let fetched content control tools, credentials, destinations, or automatic outreach.
- **Data minimization:** Business names, domains, snippets, website text, lead scores, drafts, and reviewer email may be stored or processed by third parties. Disclose this, minimize retention, restrict access, and check provider terms/data-processing requirements for your use case.
- **Human approval:** Keep the review gate. Approval should mean review of the draft only; it must not bypass legal/compliance checks or authorize mass unsolicited contact.
- **Output safety:** The proposal HTML escapes model-derived text before rendering. Preserve escaping if changing the template, and do not embed arbitrary HTML from scraped pages.
- **Credentials and least privilege:** Use n8n's credential vault, separate least-privilege service accounts, rotate credentials, and avoid exporting credentials or execution data.
- **No automatic ROI claims:** AI-generated scores, budget estimates, and proposal benefits are estimates and may be unsupported. Validate them with evidence before sharing with a client.

## Known limitations

- Search and website text coverage depend on external providers and public-site availability.
- The workflow depends on external APIs, compatible n8n nodes, credentials, and quotas.
- Model outputs require human fact-checking; they are not a verified business or financial assessment.
- A PDF conversion/storage path is available only after the PDFBolt and Drive integrations are configured.
- The workflow is provided as a portfolio template; no production uptime, compliance certification, or security guarantee is implied.

## Suggested portfolio description

> Built an n8n-based AI lead-research workflow that validates requests, discovers public businesses, audits website content with structured AI outputs, scores and de-duplicates leads, and routes outreach drafts through a human approval gate. Includes CRM status updates, Slack notifications, and proposal PDF generation. Designed with validation, prompt-injection defenses, and privacy-conscious credential removal; production deployment requires environment-specific SSRF and access-control hardening.

## License

Add a license only if you have the rights to publish and reuse this workflow and its embedded prompts/templates. Review third-party node and service terms before redistribution.


## Synthetic request and response examples

- [Sample request](sample/request.json)
- [Sample successful response](sample/response.json)

These examples use synthetic demonstration data and show the successful route only. HTTP status codes are returned separately by the workflow. Configure authentication and credentials in your own n8n instance before activation.
