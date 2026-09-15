# Callimacus ↔ Salesforce B2C Commerce — setup files

The three files a Salesforce B2C Commerce instance needs to feed Callimacus. They use
**only standard SFCC capabilities**: six native export jobs write to your instance's Impex
area, and Callimacus pulls them over WebDAV. There is no cartridge to install and no
custom code to deploy.

Full walkthrough: **[docs.callimacus.ai/user-guide/studio/anteater/salesforce](https://docs.callimacus.ai/user-guide/studio/anteater/salesforce)**

| File | What it is | Where it goes in Business Manager |
|---|---|---|
| `callimacus-jobs.zip` | The six `Callimacus*` export jobs (catalog full + delta, prices, inventory, site config, promotions), packaged as an importable site archive. Identical for every customer — no instance-specific values. Source: `jobs.xml`. | Administration › Site Development › **Site Import & Export** (merge) |
| `webdav/client_permissions-template.json` | Read-only WebDAV grant on `/impex/src/instance/callimacus`, and nothing else. **Required.** | Administration › Organization › **WebDAV Client Permissions** |
| `ocapi/data-api-template.json` | Lets Callimacus trigger and monitor *only* the six Callimacus jobs. **Recommended**, optional. | Administration › Site Development › **Open Commerce API Settings** (type Data, context Global) |

In both JSON templates, replace `<CALLIMACUS_API_CLIENT_ID>` with the client ID of the API
client you created in your own Account Manager, and **merge** the entry into the existing
list — preserve any clients already there.

Upload `callimacus-jobs.zip` as-is — Site Import & Export takes a **site archive**, never a
bare `.xml`. If you edit `jobs.xml`, rebuild the archive (the zip and its top-level folder
must carry the same name):

```sh
mkdir -p callimacus-jobs && cp jobs.xml callimacus-jobs/ && zip -r callimacus-jobs.zip callimacus-jobs
```

The jobs ship with empty schedules on purpose: set cadences per environment in
Administration › Operations › Jobs. Recommended cadences are in the comments in `jobs.xml`.

Questions, or a change you need to the exports: talk to your Callimacus contact.
