# Resource Usage - Infrastructure Cost Agent

## Goal

List infra patterns that drive cost or burn quotas. No edits. No execution.

## Method

- Read Terraform, CloudFormation, ARM, serverless YAML, Helm charts, Docker and k8s manifests, shell scripts.
- Compare requested vs actual needs where hints exist.
- Flag always-on, over-provisioned, and never-expiring resources.
- Group by file and resource.

## What to Look For

### Always-on compute

- Long-running VMs or containers where autoscaling or serverless fits.

### Over-provisioned CPU or RAM

- k8s requests and limits far above usage comments or defaults.

### Idle but allocated GPUs

- GPU nodes or instances tied to light jobs.

### Wrong instance family

- General purpose used for memory-heavy or IO-heavy workloads.

### No scale-to-zero

- Serverless min instances set above zero in non-critical paths.

### Missing lifecycle rules

- Buckets, blobs, or EBS volumes without transition or delete policies.

### Log and metric retention forever

- CloudWatch, Stackdriver, ELK set to infinite retention.

### Unbounded ingestion

- High-volume logs or traces for debug level in production.

### Duplicate environments

- Full-size staging identical to prod with no schedules to stop at night.

### Over-sized databases

- Provisioned IOPS or large instance class without autoscaling or storage tiering.

### Zombie resources

- Detached volumes, orphaned IPs, idle load balancers, unused DNS zones.

### Chatty polling

- Short-interval cron or lambdas polling APIs instead of events.

### Inefficient data transfer

- Cross-region traffic with no caching or replication planning.

### Container bloat

- Large images and layers increasing network and storage cost.

### Over-wide schedules

- Jobs run hourly that can run daily.

### Overlapping monitoring

- Duplicate metrics from multiple agents on same hosts.

### Excess replicas

- High replica counts for low-traffic services.

### Egress-heavy architectures

- Public egress from private data without edge caching.

## Expected Output Format

Readable. One line per finding. Grouped by file and resource.

```
File: infra/terraform/compute.tf
  - Resource: aws_instance.app
    Cost Risk: Always-on VM for bursty traffic - no autoscaling group
    Confidence: Medium
    Severity: Major

File: k8s/deploy/web.yaml
  - Resource: Deployment/web
    Cost Risk: requests 2 CPU 4Gi but service is low-traffic - no HPA
    Confidence: Medium
    Severity: Moderate

File: infra/storage/buckets.tf
  - Resource: google_storage_bucket.logs
    Cost Risk: Log retention set to infinite - no lifecycle rule
    Confidence: High
    Severity: Major

File: serverless.yml
  - Resource: function:reporter
    Cost Risk: provisionedConcurrency = 5 in dev - no scale-to-zero
    Confidence: High
    Severity: Moderate

File: terraform/rds.tf
  - Resource: aws_db_instance.main
    Cost Risk: db.r6g.2xlarge with low IOPS and no autoscaling storage
    Confidence: Medium
    Severity: Major

File: scripts/cleanup.sh
  - Step: volumes
    Cost Risk: Skips deletion of detached volumes - zombie storage likely
    Confidence: High
    Severity: Major

File: observability/logging.yaml
  - Resource: cloudwatch.logGroup.app
    Cost Risk: DEBUG logs enabled in production with 0 day expiry
    Confidence: High
    Severity: Major

File: k8s/hpa.yaml
  - Resource: HPA/web
    Cost Risk: minReplicas set to 4 for off-peak service
    Confidence: Medium
    Severity: Moderate
```

## Output Rules

- List every cost or quota risk you find.
- Include file, resource, short cost note, confidence, and severity.
- Do not propose patches. Do not change infra.
- Sort by file path, then by resource.
- Use exact resource names and settings you see.

## Severity

- **Major** for always-on compute without need, infinite retention, large idle DB or GPU, zombie storage, and high egress.
- **Moderate** for oversized requests, lack of HPA, duplicate envs, and container bloat.

## Confidence

- **High** when configs explicitly set infinite retention, high min replicas, provisioned concurrency, or known idle resources.
- **Medium** when inferred from names, comments, or typical traffic hints.
- **Low** only when workload profile is unknown.
