# Parquet link exit

A decision aid for getting off the **Delta Lake (Parquet)** export option in Azure Synapse
Link for Dataverse before it stops working, without walking into the one exit that closes a
door behind you.

Companion to [The Link With an Expiry Date](https://www.theworkbench.blog) on
[The Workbench](https://www.theworkbench.blog).

> **Two deadlines, one already passed.** Since **2026-10-15** the option is closed to new
> customers. By **December 2027** everyone else must be off it.

> **No script here, on purpose.** There is no documented API that reports a Synapse Link
> profile's output format, so a tool that claimed to inventory your links would be me
> guessing. What this repo carries instead is the decision, which is the hard part. Every
> claim is dated and sourced.

## Start here

**[DECISION.md](DECISION.md)** — the two dates, four questions in the order that matters, a
table of what each destination does and does not carry, the opt-in URL with its caveats, and
a pre-move checklist.

## The short version

Two routes off Parquet: **CSV output** in Azure Synapse Link, or **Link to Microsoft Fabric**.
Microsoft recommends Fabric, and it is the only route that keeps the Delta or Parquet format.

Three things decide it for you, and the order matters:

1. **A CSV profile cannot be linked to Microsoft Fabric.** Taking CSV because it is quick
   closes the Fabric route until you rebuild the link.
2. **Audit table export works only on Delta Lake profiles today.** CSV does not carry it, and
   Fabric does not carry it yet.
3. **Eligibility is per Dataverse organization ID, not per tenant.** A new environment with a
   new organization ID is not eligible, even inside an eligible tenant.

## Sources

Verified **2026-10-28** against:

- [Export Dataverse data in Delta Lake format](https://learn.microsoft.com/power-apps/maker/data-platform/azure-synapse-link-delta-lake)
- [Azure Synapse Link for Dataverse FAQ](https://learn.microsoft.com/power-apps/maker/data-platform/export-data-lake-faq)
- [Link to Microsoft Fabric](https://learn.microsoft.com/power-apps/maker/data-platform/fabric-link-to-data-platform)
- [Access audit data with Azure Synapse Link and Power BI](https://learn.microsoft.com/power-platform/admin/audit-data-azure-synapse-link)
- [Transition from legacy data integration services](https://learn.microsoft.com/power-apps/maker/data-platform/azure-synapse-link-transition-from-fno)

## Contributing

This repository is a publishing surface for the blog rather than a community project, so it
does not accept pull requests. If something here no longer matches the documentation, the
comments on the related blog post are the right place, and that is where corrections come
from.

## License

MIT. See [LICENSE](LICENSE).
