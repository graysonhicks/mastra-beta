# Mastra Workflow Examples

## Setup

1. Install dependencies:
   ```bash
   bun install
   ```

2. Start the dev server:
   ```bash
   bun dev
   ```

---

## Double Nested Workflow

A workflow demonstrating double nesting (workflow → nested workflow → double nested workflow).

### Run the Double Nested Workflow

1. Create a workflow run:
   ```bash
   curl -X POST http://localhost:4111/api/workflows/double-nested-workflow/create-run
   ```
   
   Copy the `runId` from the response.

2. Start the workflow:
   ```bash
   curl -X POST "http://localhost:4111/api/workflows/double-nested-workflow/start?runId=<runId>" \
     -H "Content-Type: application/json" \
     -d '{"inputData": {"initialValue": 5, "label": "test"}}'
   ```

### Data Flow

Starting with `initialValue: 5`:
1. `parentPreStep`: passes value `5`
2. `middlePreStep`: `5 + 10 = 15`
3. `innerStep`: `15 * 2 = 30`
4. `middlePostStep`: `30 + 5 = 35`
5. `parentPostStep`: formats summary with `finalResult: 35`

---

## Bun S3 Workflow (Bug Reproduction)

This workflow reproduces a bug where Bun's native S3 client fails inside Mastra workflow steps.

### Run the Bun S3 Workflow

1. Create a workflow run:
   ```bash
   curl -X POST http://localhost:4111/api/workflows/bun-s3-workflow/create-run
   ```
   
   Copy the `runId` from the response.

2. Start the workflow:
   ```bash
   curl -X POST "http://localhost:4111/api/workflows/bun-s3-workflow/start?runId=<runId>" \
     -H "Content-Type: application/json" \
     -d '{"inputData": {"content": "Hello from Bun!", "key": "test-file.txt"}}'
   ```

3. Check the terminal - you'll see the error:
   ```
   Error: ReferenceError: Bun is not defined
   ```

---

## Environment

- Bun v1.3.2
- @mastra/core beta
- macOS
