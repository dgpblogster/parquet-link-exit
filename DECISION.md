# Which exit do you take?

All claims verified against Microsoft Learn on **2026-10-28**. Every row here is a moving
target, so check the dates before you act on it.

## The two dates

| Date | What happens |
|---|---|
| **2026-10-15** (passed) | Delta Lake (Parquet) export in Azure Synapse Link is no longer available to **new customers**. |
| **December 2027** | Existing customers must have transitioned to CSV output in Azure Synapse Link, or to Link to Microsoft Fabric. After this the Parquet option is gone. |

## Four questions, in this order

**1. Does anything downstream read your Parquet?**

The two dependencies Microsoft names explicitly are audit data in Power BI through Azure
Synapse Link, and Dynamics 365 ingestion with Azure Databricks. Inventory yours before you
choose, not after.

**2. Do you export audit tables?**

If yes, your options are narrower than they look. Audit table export is supported **only with
Delta Lake profiles** today. CSV does not carry it. Link to Fabric does not carry it yet;
Microsoft says support is coming, which is a thing to plan around rather than depend on.

**3. Will you want Microsoft Fabric, now or later?**

If yes, **do not take the CSV route.** A Synapse Link profile whose data is saved as CSV
cannot be linked to Microsoft Fabric. Choosing CSV to beat the deadline closes the Fabric
door until you rebuild the link.

Also currently excluded from Fabric linking: profiles secured with managed identities,
formerly MSI.

**4. Is this organization eligible?**

Eligibility is evaluated at the **Dataverse organization ID**, not the tenant. A new
environment with a new organization ID is not eligible even when another organization in the
same tenant is. Unlinking an eligible organization does not forfeit it: you can create the
link again for that same organization ID.

## What each destination carries

| | Delta Lake (Parquet), today | CSV in Synapse Link | Link to Microsoft Fabric |
|---|---|---|---|
| Available to new customers | No, since 2026-10-15 | Yes | Yes |
| Works after Dec 2027 | **No** | Yes | Yes |
| Keeps Delta / Parquet format | Yes | No | Yes |
| Can be linked to Fabric later | Yes | **No** | n/a |
| Carries audit tables | Yes | No | Not yet |
| Storage | Your own | Your own | Dataverse built-in |

## The opt-in, for eligible existing organizations

Add `enableSynapseParquet=true` to the Link data URL. Exact spelling, lowercase `true`,
included once. Append with `&` if the URL already has a `?`.

```text
https://make.powerapps.com/environments/<environment-id>/linkdata?tab=other&enableSynapseParquet=true
```

Let the page finish loading, select **Create link to data lake** (not **Create a new Fabric
link**), then in the panel find the Link to Fabric card and select **Get Parquet format in a
Synapse workspace anyway**.

Two caveats straight from the documentation:

- The URL does not select Parquet. The panel starts on CSV, and the Parquet action appears
  only after the opt-in **and** your organization's eligibility are confirmed.
- If the action is not there, that is the answer. **Do not try other feature flags.** The
  parameter overrides neither eligibility nor either deadline.

`tab=other` opens **Data lake links**. `tab=fabric` opens Fabric Links and has no effect on
BYOS eligibility.

## Naming

Azure Synapse Link branding is retiring. In Power Apps the experience is **Link data**, and
the UX says **Link to Data lake**. Older runbooks will not match the labels on screen.

## Pre-move checklist

- [ ] List every downstream consumer of the Parquet output, by name and owner
- [ ] Confirm whether any of them read audit tables
- [ ] Record each affected environment's **organization ID** and whether it is eligible
- [ ] Decide Fabric or CSV **before** touching any profile, since CSV is hard to reverse
- [ ] Check whether any profile uses managed identities, which blocks Fabric linking today
- [ ] Re-verify this document's claims against Learn, because they change
