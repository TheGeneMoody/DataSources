# 🔐 VERTIC Dataset Schema  
### Vulnerability Exploitation Real-Time Intel & Context

> **VERTIC** is a structured dataset designed to track the real-world lifecycle of exploited vulnerabilities (CVEs). It provides technical and executive stakeholders with enriched timelines, contextual intelligence, financial impact, and attacker behavior data to support enterprise risk analysis and threat modeling.

---

## ⚙️ Top-Level Fields Per Record

| Field | Type | Description |
|-------|------|-------------|
| `cve_id` | `string` | The CVE identifier (e.g., `CVE-2021-26855`) |
| `vuln_disclosure_date` | `string` (ISO date) | Public disclosure date of the vulnerability |
| `first_known_exploit_date` | `string` (ISO date or `null`) | Date of first confirmed exploitation in the wild |
| `peak_exploit_period` | `string` (date or date range) | Period with highest observed exploitation activity |
| `last_known_exploit_date` | `string` (ISO date or `null`) | Most recent observed use in an exploit |
| `known_zero_day` | `boolean` | Whether the CVE was exploited before public disclosure |
| `patch_available_date` | `string` (ISO date or `null`) | Date a patch was first made available |
| `exploit_campaigns` | `array[string]` | Names of malware campaigns or APT groups (e.g., `["WannaCry", "Hafnium"]`) |
| `industry_targets` | `array[string]` | Industry verticals affected (e.g., `["Healthcare", "Finance", "Retail"]`) |
| `geographic_targets` | `array[string]` | Regions or countries where exploitation was reported (e.g., `["Global", "US", "APAC"]`) |
| `estimated_financial_loss_usd` | `number` or `null` | Estimated total financial loss in USD |
| `loss_attribution_method` | `string` | Description of how financial loss was calculated (e.g., `"Incident report"`, `"Aggregation from cases"`) |
| `exploit_timeline_monthly` | `array[object]` | Monthly exploitation records (see below) |
| `cvss_base_score` | `number` | CVSS base score at time of disclosure |
| `mitigations` | `array[string]` | Suggested or known mitigations (e.g., `"Disable SMBv1"`, `"Update to version 2.4.6"`) |
| `exploitability` | `string` | Type of exploitation (e.g., `"Remote, No Auth"`, `"Local, Requires Admin"`) |
| `attck_tactics` | `array[string]` | Mapped MITRE ATT&CK tactics (e.g., `["Initial Access", "Privilege Escalation"]`) |
| `attck_techniques` | `array[string]` | Mapped ATT&CK techniques (e.g., `["T1190", "T1203"]`) |
| `exploit_confidence_level` | `string` | `"Confirmed"`, `"Likely"`, or `"Suspected"` |
| `primary_sources` | `array[string]` | URLs to advisories, KEV, threat intel, or IR reports |
| `notes` | `string` or `null` | Additional notes or caveats |

---

## 📊 `exploit_timeline_monthly` Subfield

If present, this is a list of activity levels per month:

```json
[
  { "month": "2021-03", "activity": "High" },
  { "month": "2021-04", "activity": "Medium" },
  { "month": "2021-05", "activity": "Low" }
]
```

| Subfield | Type | Description |
|----------|------|-------------|
| `month` | `string` (YYYY-MM) | Calendar month |
| `activity` | `string` | `"High"`, `"Medium"`, `"Low"`, `"None"` |

---

## ✅ Example VERTIC Record (Abbreviated)

```json
{
  "cve_id": "CVE-2021-26855",
  "vuln_disclosure_date": "2021-03-02",
  "first_known_exploit_date": "2021-02-28",
  "peak_exploit_period": "2021-03",
  "last_known_exploit_date": "2021-07-15",
  "known_zero_day": true,
  "patch_available_date": "2021-03-02",
  "exploit_campaigns": ["Hafnium"],
  "industry_targets": ["Government", "Healthcare"],
  "geographic_targets": ["US", "Europe"],
  "estimated_financial_loss_usd": 28000000,
  "loss_attribution_method": "Incident Reports (aggregated)",
  "exploit_timeline_monthly": [
    { "month": "2021-03", "activity": "High" },
    { "month": "2021-04", "activity": "Medium" },
    { "month": "2021-05", "activity": "Low" }
  ],
  "cvss_base_score": 9.1,
  "mitigations": ["Apply KB5000871", "Restrict access to OWA"],
  "exploitability": "Remote, No Auth",
  "attck_tactics": ["Initial Access"],
  "attck_techniques": ["T1190"],
  "exploit_confidence_level": "Confirmed",
  "primary_sources": [
    "https://www.cisa.gov/known-exploited-vulnerabilities",
    "https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-26855"
  ],
  "notes": null
}
```
