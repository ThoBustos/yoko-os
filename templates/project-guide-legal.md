# Legal Folder Guide

This folder contains legal documents for {{project}}.

---

## Organization

### File Naming Convention
- Contracts: `contract-{{party}}-{{YYYY-MM-DD}}.pdf`
- Agreements: `agreement-{{topic}}-{{YYYY-MM-DD}}.md`
- Proposals: `proposal-{{client}}-{{YYYY-MM-DD}}.md`
- NDAs: `nda-{{party}}-{{YYYY-MM-DD}}.pdf`
- Terms: `terms-{{version}}-{{YYYY-MM-DD}}.md`

### Subfolders (optional for high volume)
```
Legal/
├── Contracts/
├── Proposals/
├── NDAs/
└── Archive/
```

---

## Document Status

Add status to the filename or track in this file:

| Document | Party | Status | Expiry | Notes |
|----------|-------|--------|--------|-------|
| | | draft/sent/signed/expired | | |

**Status values:**
- `draft` - Being written
- `sent` - Sent for review/signature
- `negotiating` - In discussion
- `signed` - Fully executed
- `expired` - Past expiration date
- `archived` - No longer active

---

## Review Reminders

- Review contracts 30 days before expiration
- Add expiration dates to `00_SYSTEM/IMPORTANT_DATES.md`
- Archive expired contracts, never delete signed documents

---

## Templates

Store reusable templates in `Legal/Templates/`:
- `template-contract.md`
- `template-proposal.md`
- `template-nda.md`

---

## Important Notes

- **Never delete signed contracts** - move to Archive/ instead
- Keep original signed PDFs, not just copies
- Note any verbal agreements in writing as memos
- Link to relevant decisions in `../Decisions/`

---

*Last reviewed: {{DATE}}*
