# 🛡️ GuardAI: The Open Standard for EU AI Act Technical Compliance

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Status: Active](https://img.shields.io/badge/Status-Active-success.svg)]()
[![EU AI Act: Ready](https://img.shields.io/badge/EU_AI_Act-August_2026_Ready-blue.svg)]()
[![Rules: 145+](https://img.shields.io/badge/Rules-145%2B-orange.svg)]()
[![Articles Covered](https://img.shields.io/badge/Articles_Covered-Art.5%2C9%2C10%2C11%2C12%2C13%2C14%2C15%2C50%2C51%2C53-blue.svg)]()

---

## Overview

While the EU AI Act provides a comprehensive legal framework for artificial intelligence, translating these legal requirements into practical, machine-readable developer workflows remains a significant challenge for engineering teams across Europe.

This repository contains the **Open Standard Code of Practice for Technical EU AI Act Compliance**, maintained by [GuardAI](https://guardai.dev). It maps the legal text of the EU AI Act (Regulation (EU) 2024/1689) into **145+ actionable, automated technical rules** that developers and security teams can directly integrate into their CI/CD pipelines.

These rules are written in a structured YAML schema designed to be:
- **Human-readable** — legal and technical teams can review them side-by-side
- **Machine-executable** — integrate directly into scanners and linters
- **Auditor-ready** — each rule maps to a specific article and regulatory obligation

---

## 🎯 Purpose — Bridging Law and Code

Our goal is to assist the European AI Office, National Competent Authorities, and enterprise engineering teams by providing a transparent, open-source technical baseline for:

- **Article 57:** Supporting AI Regulatory Sandboxes with automated compliance testing
- **Article 95:** Establishing a technical foundation for Voluntary Codes of Conduct
- **Annex III:** Enabling automated risk classification for high-risk AI systems
- **GPAI (Art. 51–53):** Detecting General Purpose AI obligations in codebases

---

## 🗂️ Rule Taxonomy

The 145+ rules in this repository are organized by their corresponding EU AI Act legal articles:

```
rules/
├── article-5/          → Prohibited AI Practices (Art. 5)
│   ├── EUAI-001.yaml   → Subliminal manipulation detection
│   ├── EUAI-002.yaml   → Vulnerability exploitation patterns
│   ├── EUAI-011.yaml   → Social scoring system detection
│   └── EUAI-012.yaml   → Real-time biometric surveillance
│
├── annex-iii/          → High-Risk AI System Classification
│   ├── EUAI-003.yaml   → Biometric categorization systems
│   ├── EUAI-004A.yaml  → Critical infrastructure AI
│   ├── EUAI-004B.yaml  → Education & vocational AI
│   ├── EUAI-004C.yaml  → Employment & HR AI systems
│   ├── EUAI-004D.yaml  → Essential services AI
│   ├── EUAI-004E.yaml  → Law enforcement AI
│   ├── EUAI-004F.yaml  → Border management AI
│   └── EUAI-004G.yaml  → Justice & democratic process AI
│
├── article-9/          → Risk Management System (Art. 9)
│   ├── EUAI-004.yaml   → Risk management system absence
│   └── EUAI-004-001.yaml → Residual risk documentation
│
├── article-10/         → Data Governance (Art. 10)
│   ├── EUAI-005.yaml   → Training data provenance
│   ├── EUAI-005-001.yaml → Bias detection absence
│   └── EUAI-005-002.yaml → PII in training pipeline
│
├── article-11/         → Technical Documentation (Art. 11)
│   ├── EUAI-006.yaml   → Missing technical documentation
│   └── EUAI-006-001.yaml → Incomplete model specification
│
├── article-12/         → Record Keeping & Logging (Art. 12)
│   ├── EUAI-007.yaml   → Audit trail absence
│   └── EUAI-007-001.yaml → Insufficient log retention
│
├── article-13/         → Transparency & Information (Art. 13)
│   ├── EUAI-008.yaml   → Missing AI disclosure
│   ├── EUAI-008-001.yaml → Absent model card
│   └── EUAI-008-002.yaml → Chatbot identity concealment
│
├── article-14/         → Human Oversight (Art. 14)
│   ├── EUAI-009.yaml   → Missing human override mechanism
│   └── EUAI-009-001.yaml → Automated decision without review
│
├── article-15/         → Accuracy & Robustness (Art. 15)
│   ├── EUAI-010.yaml   → Missing accuracy benchmarks
│   └── EUAI-010-001.yaml → Adversarial robustness gaps
│
├── article-50/         → GPAI Transparency (Art. 50)
│   ├── EUAI-013.yaml   → GPAI system detection
│   └── EUAI-013-001.yaml → AI-generated content labeling
│
└── article-51-53/      → GPAI Obligations (Art. 51–53)
    ├── EUAI-014.yaml   → GPAI systemic risk detection
    └── EUAI-014-001.yaml → GPAI model documentation absence
```

---

## 📋 Rule Schema

Each rule follows this structure:

```yaml
id: EUAI-XXX
name: "Short descriptive name"
article: "Art.X(Y)(z)"
annex: "Annex III / N/A"
severity: CRITICAL | HIGH | MEDIUM | LOW
fine_exposure_eur: 35000000 | 15000000 | 7500000
category: prohibited | high_risk | transparency | gpai
description: |
  What this rule detects and why it matters legally.
patterns:
  - regex: "pattern here"
    confidence: 0.90
    language: python | typescript | java | any
    message: "Human-readable finding message"
  - ast_pattern:
      node_type: "import_statement"
      library: "face_recognition"
    confidence: 0.95
    message: "AST-level detection message"
remediation: |
  Exact steps to fix the violation.
references:
  - "https://eur-lex.europa.eu/..."
tags:
  - biometric
  - prohibited
enforcement_date: "2024-02-02"  # or "2026-08-02"
```

---

## 🚀 How to Use These Rules

### Manual Audit
Review the YAML rules and manually check your codebase against each pattern.

### Integrate into Custom Scanner
```python
import yaml, re

def load_rules(rules_dir: str) -> list:
    rules = []
    for root, _, files in os.walk(rules_dir):
        for f in files:
            if f.endswith('.yaml'):
                with open(os.path.join(root, f)) as fh:
                    rules.append(yaml.safe_load(fh))
    return rules

def scan_file(content: str, rules: list) -> list:
    findings = []
    for rule in rules:
        for pattern in rule.get('patterns', []):
            if 'regex' in pattern:
                if re.search(pattern['regex'], content, re.IGNORECASE):
                    findings.append({
                        'rule_id': rule['id'],
                        'severity': rule['severity'],
                        'message': pattern['message'],
                        'fine_exposure': rule['fine_exposure_eur'],
                    })
    return findings
```

### Automated Enforcement
For fully automated PR blocking, compliance scoring, and audit-ready PDF reports without building your own scanner:

**[→ Try GuardAI free at guardai.dev](https://guardai.dev)**
*(50 scans/month free. No credit card required. 145+ rules enforced automatically.)*

---

## 🤝 Contributing

We welcome contributions from AI governance experts, legal professionals, and software engineers.

**How to contribute:**
1. Fork this repository
2. Add or update a rule in the correct article folder
3. Ensure your rule follows `schema.json` exactly
4. Add a reference to the specific EU AI Act article
5. Open a Pull Request with a description of the new/changed pattern

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

**Priority areas for contributions:**
- New AI library fingerprints for Annex III classification
- Patterns for emerging GPAI model types
- Language-specific AST patterns (Go, Rust, C#)
- Rules for Article 28 obligations (deployers)

---

## 📊 Rule Coverage Status

| Article | Description | Rules | Status |
|---------|-------------|-------|--------|
| Art. 5 | Prohibited AI Practices | 12 | ✅ Complete |
| Art. 9 | Risk Management System | 8 | ✅ Complete |
| Art. 10 | Data Governance | 15 | ✅ Complete |
| Art. 11 | Technical Documentation | 10 | ✅ Complete |
| Art. 12 | Record Keeping | 8 | ✅ Complete |
| Art. 13 | Transparency | 18 | ✅ Complete |
| Art. 14 | Human Oversight | 12 | ✅ Complete |
| Art. 15 | Accuracy & Robustness | 14 | ✅ Complete |
| Annex III | High-Risk Classification | 35 | ✅ Complete |
| Art. 50 | GPAI Transparency | 10 | ✅ Complete |
| Art. 51–53 | GPAI Obligations | 13 | ✅ Complete |
| **TOTAL** | | **145+** | |

---

## 📅 EU AI Act Enforcement Timeline

| Date | Milestone |
|------|-----------|
| ✅ **Feb 2, 2024** | EU AI Act enters into force |
| ✅ **Aug 2, 2024** | Prohibited AI (Art. 5) starts applying |
| 🔄 **Aug 2, 2025** | GPAI obligations (Art. 51–53) apply |
| ⏰ **Aug 2, 2026** | Full enforcement — ALL articles apply |

**August 2026 is the critical deadline.** All high-risk AI systems (Annex III) must be compliant. Fines up to **€35,000,000 or 7% of global annual turnover** for prohibited AI violations.

---

## ⚖️ Disclaimer

*This repository provides technical pattern mappings and observational rules derived from the text of Regulation (EU) 2024/1689 (the EU AI Act). It does not constitute formal legal advice. The rules represent technical interpretations only. Always consult qualified legal counsel for EU AI Act compliance decisions. GuardAI assumes no liability for compliance decisions made based on these rules.*

---

## 📄 License

This work is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).

You are free to use, share, and adapt these rules — including commercially — provided you give appropriate credit to GuardAI.

---

*Maintained by [GuardAI](https://guardai.dev) | Last updated: April 2026*
