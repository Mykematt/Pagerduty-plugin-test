# PagerDuty Buildkite Plugin Test

This pipeline tests the incident-pagerduty-buildkite-plugin functionality.

## Setup Instructions

### 1. Get PagerDuty Integration Key

1. Log into your PagerDuty account
2. Go to **Services** → Select or create a service
3. Click **Integrations** tab
4. Add a new integration with type **Events API V2**
5. Copy the **Integration Key**

### 2. Add Secret to Buildkite

```bash
# Using Buildkite CLI
buildkite-agent secret set PAGERDUTY_INTEGRATION_KEY "your-integration-key-here"
```

Or via Buildkite UI:
1. Go to your organization settings
2. Navigate to **Secrets**
3. Add new secret: `PAGERDUTY_INTEGRATION_KEY`
4. Paste your integration key

### 3. Create Pipeline in Buildkite

```bash
# Upload the pipeline
buildkite-agent pipeline upload pipeline.yml
```

Or create a new pipeline in Buildkite UI and point it to this repository.

## Test Scenarios

The pipeline includes three test cases:

### ✅ Test 1: Deployment Failure (High Urgency)
- **Expected**: Creates a PagerDuty incident with high urgency
- **Trigger**: Step succeeds (no incident created in this example)
- **Annotation**: Error style with incident details

### ✅ Test 2: Test Failure (Low Urgency)
- **Expected**: Creates a PagerDuty incident with low urgency
- **Trigger**: Step exits with code 1 (simulated failure)
- **Annotation**: Warning style

### ✅ Test 3: Successful Step
- **Expected**: No incident created
- **Trigger**: Step completes successfully
- **Annotation**: None (only created on failure)

## Verification

After running the pipeline:

1. **Check PagerDuty**: Verify incidents were created for failed steps
2. **Check Buildkite**: Verify annotations appear on failed builds
3. **Check Logs**: Review plugin output in Buildkite job logs

## Troubleshooting

If incidents aren't created:

1. Verify `PAGERDUTY_INTEGRATION_KEY` is set correctly
2. Check PagerDuty service is active
3. Review Buildkite job logs for plugin errors
4. Ensure plugin reference is correct: `incident-pagerduty#Ola-building`
