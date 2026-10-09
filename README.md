# Bats Test Results

This repository serves as the immutable, durable archive for the [GitOps Control Plane](https://github.com/brunobml/gitops-control-plane) Bats smoke-test reports.

## Layout

Reports are organized hierarchically by UTC date and unique run identifier:

```text
runs/
└── YYYY/
    └── MM/
        └── DD/
            └── <run-id>/
                ├── report.xml     # Sanitized JUnit XML report from Bats
                └── metadata.json   # Run metadata and verification checksum
```

### Run ID Format
Run IDs follow the format:
`YYYYMMDDTHHMMSSZ-<6-hex-chars>` (e.g., `20261009T013000Z-a1b2c3`).

### Metadata Schema (`metadata.json`)
```json
{
  "$schema": "https://raw.githubusercontent.com/brunobml/bats-test-reporter/main/schemas/metadata-v1.json",
  "schema_version": "1.0.0",
  "run_id": "20261009T013000Z-a1b2c3",
  "start_time_utc": "2026-10-09T01:30:00Z",
  "end_time_utc": "2026-10-09T01:31:01Z",
  "bats_version": "1.14.0",
  "suite": "smoke",
  "filter": null,
  "exit_code": 0,
  "summary": {
    "total": 31,
    "passed": 31,
    "failed": 0,
    "skipped": 0,
    "duration_seconds": 61.2
  },
  "source": {
    "gitops_control_plane_commit": "ab634ee",
    "catalog_revisions": {
      "spoke_nonprod": "main",
      "spoke_prod": "v1.2.0"
    }
  },
  "report_sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
}
```

## Security & Sanitization
* All reports published to this repository are sanitized host-side **before** commit and push.
* Sensitive values (passwords, tokens, IAM keys, private paths, and internal hostnames) are scrubbed.
* If sanitization validation is uncertain or fails, publication is blocked.

## Retention Policy & Storage Growth
* **Retention:** Indefinite retention for historical trends and post-reboot continuity.
* **Storage Footprint:** Each test run produces approximately 5–8 KiB of data (`report.xml` + `metadata.json`).
* **Expected Growth:** At ~10–20 test runs per day during development, expected storage growth is < 150 KiB/day (~50 MiB/year).
