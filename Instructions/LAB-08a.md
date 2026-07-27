# SharePoint Online Lab Exercises - LAB-08a

> **Running scenario:** Contoso Consulting has grown to 300 people, dozens of sites, and an active client engagement (`Fabrikam Project`). Basic sharing policies (LAB-03) and storage/creation governance (LAB-06) aren't enough anymore — leadership wants proof that only the right people can see HR and finance data, and that dead sites get cleaned up automatically. That's **SharePoint Advanced Management (SAM)**.

> ⚠️ **Structure note, current as of 2026:** Advanced Management features are split across two places in the SharePoint Admin Center left navigation — **Policies > Access control** (session/device/network/restricted-access controls, some free, some SAM-licensed) and a dedicated top-level **Advanced Management** node (governance reports, site lifecycle, restricted content discovery). If a menu label below doesn't match exactly what you see, use the admin center's search box — Microsoft reshuffles this area frequently, which is itself worth noting to your team.

## Lab 1: Overview and Licensing of SharePoint Advanced Management
### Objective:
Understand what SAM adds on top of standard SharePoint governance, and confirm Contoso is actually licensed to use it before planning around it.

### Steps:
1. **Define the scope:**
   - Discuss what SAM adds beyond what you've already configured: a second, independent layer of access restriction (**restricted access control**), oversharing visibility (**data access governance reports**), automated inactive-site handling (**site lifecycle management**), and control over how content surfaces in search/Copilot (**restricted content discovery**).
2. **Check licensing — don't assume:**
   - Navigate to the **Microsoft 365 Admin Center** > **Billing** > **Your products**.
   - Confirm: SAM is included automatically with a **Microsoft 365 Copilot** license; tenants without Copilot need it as a **standalone add-on**. Note which situation Contoso is in.
3. **Find the feature in the admin center:**
   - In the **SharePoint Admin Center** left navigation, locate the top-level **Advanced Management** item, and separately, **Policies > Access control**.

### Knowledge check:
Why does it matter, before you build a rollout plan, whether SAM is included via Copilot licensing versus purchased as a standalone add-on?

---

## Lab 2: Access Control Policies
### Objective:
Configure the access-control layer that sits *underneath* normal SharePoint permissions — it can say no even when a permission grant says yes.

### Steps:
1. **Recap what's already free/basic:**
   - Under **Policies** > **Access control**, revisit **Idle session sign-out** (configured in LAB-06) — this one doesn't require a SAM license.
2. **Configure network and device conditions:**
   - Under **Access control** > **Unmanaged devices**, set policy for the `Fabrikam Project` site: allow web-only access (block download/sync) from devices that aren't domain-joined or compliant.
   - Under **Access control** > **Network location**, restrict access to named locations if Contoso has fixed office IP ranges — otherwise, discuss why this control matters less for a mostly-remote workforce.
3. **Configure Restricted Access Control (the headline SAM feature):**
   - Create a site named `Contoso HR` (if you don't already have one).
   - Under **Access control** > **Restricted access control**, apply a policy limiting access to *only* an "HR Team" Microsoft 365 security group — even a Global Administrator without HR group membership should be blocked from browsing its content.
   - **Prove it, don't just configure it:** log in as (or simulate) a user who is a Site Owner but *not* in the HR security group, and confirm they're now denied, despite their SharePoint permission level saying otherwise.
4. **2026 update — delegate responsibly:**
   - Note the newer capability to delegate RAC management to site admins with a required justification note, instead of keeping it centralized with SharePoint admins only. Discuss whether Contoso is ready to delegate this, or should keep it centralized while the program is new.

### Knowledge check:
A user is a Site Owner on `Contoso HR` with full permissions granted in SharePoint. Restricted Access Control excludes them. Who wins, and why does this two-layer model exist instead of just fixing the SharePoint permission?

---

## Lab 3: Governance Reporting and Site Lifecycle
### Objective:
Move from reactive fixes to proactive, tenant-wide visibility and cleanup.

### Steps:
1. **Run a data access governance report:**
   - In the **Advanced Management** node, run a report on `Fabrikam Project` and `Contoso HR` looking for **oversharing** — links shared with "Anyone," or with people outside expected groups.
   - Identify at least one finding and remediate it (tighten a link, remove an unexpected external guest) using what you learned in LAB-03.
2. **Configure site lifecycle management:**
   - Set a policy to flag sites with no activity for 90+ days, notifying the site owner before any deletion, rather than deleting immediately.
   - Apply it and check what a "flagged for review" site looks like from an owner's perspective.
3. **Configure restricted content discovery:**
   - On `Contoso HR`, enable restricted content discovery so its content is excluded from tenant-wide search and Copilot answers for users outside the HR group — even if they'd technically have file-level access through some other path.

### Challenge:
Run the data access governance report a second time after your Lab 2 and Lab 3 remediations, and confirm the oversharing finding is gone. A report you never re-run isn't governance, it's a one-time cleanup.

### Knowledge check:
Why might a document be individually shareable with a user, and yet correctly excluded from that same user's Copilot/search results, once restricted content discovery is on? What real-world problem does that gap close?

---

### Benefits of SharePoint Advanced Management (what you just proved, hands-on)
- **Improved security** — Restricted Access Control blocked a legitimate Site Owner in Lab 2; that's a real second layer, not a talking point.
- **Enhanced compliance** — lifecycle and governance reports give evidence, not just intentions, for audits.
- **Streamlined management** — lifecycle policies remove the "who's going to remember to clean this up" problem.
- **Visibility and control** — the oversharing report in Lab 3 caught something a manual permissions review likely would have missed.

This lab focused on configuring and *proving* — not just describing — the value of SharePoint Advanced Management for securing and optimizing a growing SharePoint Online tenant.
