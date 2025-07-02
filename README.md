# readme


# 🧩 Feasibility Assessment: Brevo Integration with Treasure Data CDP

**Client:** Visma  
**Goal:** Evaluate feasibility of integrating Brevo with Treasure Data CDP.

---

## 🎯 Scope

- ✅ Review Brevo’s API documentation  
- ⚠️ Identify potential integration risks and “danger zones”  
- 🔎 Confirm whether a native integration exists  
- 🚧 Highlight technical concerns or blockers  

---

## ✅ Summary

There is **no official/native integration** between Brevo and Treasure Data.  
A **custom integration is technically feasible** using Brevo’s REST API and Treasure Data’s ingestion/export tools (REST API or TD Toolbelt).  
However, several **considerations and potential blockers** must be addressed to ensure a reliable and maintainable solution.

---

## 🔗 References

- [📘 Brevo API Docs](https://developers.brevo.com/)
- [🧑‍💻 Brevo Developer Hub](https://developers.brevo.com/reference/getting-started-1)
- [📘 Treasure Data REST API](https://api-docs.treasuredata.com/)
- [🛠️ TD Toolbelt CLI](https://docs.treasuredata.com/display/public/PD/Installing+TD+Toolbelt)

---

## 🔍 1. Integration Capabilities

### 📥 Data Ingestion into Treasure Data

You can ingest Brevo data into TD using:

- 🐍 Custom Python scripts to pull from Brevo REST API (contacts, events, campaigns)
- 🧰 TD Toolbelt CLI or REST API to upload data in batch (JSON, CSV)
- ⏱️ Workflows for scheduled ETL/ELT inside TD

**Example Strategy:**
```bash
# Pull Brevo contacts via Python
# Save as CSV or JSON
# Push to TD
td import:auto \
  --format csv \
  --column-header \
  --time-column updated_at \
  brevo_contacts.csv brevo_db.contacts
