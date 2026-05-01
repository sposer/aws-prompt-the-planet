# Startup Production Debug Prompt

> One prompt to diagnose and fix your AWS production issues — from ECS container crashes to RDS connection storms.

## What This Prompt Does

Turns your AI assistant into a senior SRE that systematically diagnoses AWS production incidents. Give it your symptoms, and it outputs a structured diagnostic report with root cause analysis and copy-paste fix commands.

**Works with:** Claude, ChatGPT, Kiro, Amazon Q, or any AI assistant.

## When To Use This

- Your ECS service is showing 502 errors after a deploy
- Lambda functions are timing out but you don't know why
- RDS connections are maxing out and your app is slow
- You see CloudWatch alarms but can't figure out the root cause
- You need to quickly assess "is it the app, the infra, or AWS?"

## Prerequisites

- AWS account with read-only access
- AWS CLI v2 installed and configured
- CloudWatch Container Insights or basic CloudWatch metrics enabled
- IAM permissions: `ecs:Describe*`, `logs:FilterLogEvents`, `rds:Describe*`, `cloudwatch:GetMetricStatistics`, `elasticloadbalancing:Describe*`, `lambda:GetFunction`, `ec2:DescribeSecurityGroups`

## Quick Start

1. Copy the full prompt from `prompt.md`
2. Paste into your AI assistant
3. Fill in the `[YOUR ANSWER]` placeholders with your actual values
4. Run it — get your diagnostic report

## Example Output

```json
{
  "incident": {
    "severity": "P1",
    "summary": "ECS task crash loop caused by OOM (exit code 137)",
    "duration_minutes": 45,
    "affected_scope": "All API requests returning 502"
  },
  "diagnosis": {
    "root_cause_category": "B",
    "root_cause_detail": "Container memory limit (256MB) insufficient after dependency update increased baseline to ~310MB",
    "evidence": [
      {"phase": "Log Analysis", "finding": "OOM killed exit code 137 in stopped tasks", "confidence": "high"},
      {"phase": "Resource Diagnosis", "finding": "MemoryUtilized peaked at 248MB (limit 256MB)", "confidence": "high"}
    ]
  },
  "immediate_fixes": [
    {"priority": 1, "action": "Redeploy with higher memory limit", "command": "aws ecs update-service ...", "rollback": "Revert task definition to previous version"}
  ],
  "prevention": ["Set memory reservation to 80% of limit", "Add CloudWatch alarm on MemoryUtilized > 80%"]
}
```

## AWS Services Covered

| Service | Diagnosis Scope |
|---|---|
| ECS (Fargate/EC2) | Task crashes, deployment failures, networking, resource limits |
| Lambda | Timeouts, cold starts, permission errors, concurrency limits |
| RDS | Connection exhaustion, slow queries, storage pressure, failover events |
| ALB | Health check failures, 5xx errors, target group issues |
| CloudWatch | Log analysis, metric anomalies, alarm configuration |
| IAM | Permission errors, role assumption failures |

## Why This Prompt Wins

1. **Clear & Actionable** — Every step is a specific CLI command or console path. No vague advice.
2. **Production-Ready** — Outputs real commands you can execute, not tutorial-level examples.
3. **Well-Documented** — Covers prerequisites, expected output, troubleshooting, and edge cases.
4. **Best Practice Aligned** — Follows AWS Well-Architected Operational Excellence pillar. Structured incident response, least-privilege IAM, automated monitoring recommendations.

## File Structure

```
prompt.md          — The complete, copy-paste-ready prompt
README.md          — This file
```
