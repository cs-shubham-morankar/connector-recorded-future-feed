#### Initial release of the Recorded Future Threat Intel Feed connector — version 1.0.0:

- Connects to the Recorded Future Connect API (`https://api.recordedfuture.com/v2/`) using an `X-RFToken` API token.
- Configurable malicious / suspicious risk-score thresholds (defaults `65` / `25`); the connector enforces that malicious > suspicious at configuration time.
- Configurable base URL, request timeout, and TLS verification.
- Operations:
    - **Fetch Indicators** — streams the gzipped CSV risklist from `/{type}/risklist` (or `?list=<rule>` for a named risk rule), decompresses and parses incrementally, computes a 0–3 score per indicator using the configured thresholds, attaches RF evidence details, and returns a flat list. Supports `ip`, `domain`, `url`, `hash`, and `vulnerability` indicator types. Used by scheduled data ingestion and available on-demand.
    - **Get Risk Rules** — returns the available RF risk rules for a given indicator type, for use when configuring ingestion.
    - **Lookup IP / Domain / URL / Hash / Vulnerability (CVE)** — on-demand reputation lookups against `/{type}/{value}` with a configurable `fields` query parameter.
- **Data ingestion**: scheduled mode wired to the `Fetch Indicators` action, with an `ingest_mapping_template` mapping RF risklist rows to the FortiSOAR Indicators module (value, type, reputation, TLP, confidence, validity window, and source data).
- Health check: `GET /ip/riskrules` with the configured token.

#### Dependencies

- Threat Intel Management Solution Pack (required for ingestion).