# SharePoint Online Lab Exercises - LAB-06

> **Running scenario:** Contoso Consulting now has real sites, real external guests, and a Power App writing to a real list. Leadership just asked: "what stops this from spiraling out of control as we grow to 300 people?" That's tenant governance — the topic of this lab.

## Lab 1: Managing Tenant-Wide Access Settings
### Objective:
Learn how to configure the settings that apply to every site in the tenant at once, and understand why they live separately from per-site settings.

### Steps:
1. **Access tenant policies:**
   - Navigate to the **SharePoint Admin Center** > **Policies** in the left-hand navigation.
2. **Review sharing (recap from LAB-03):**
   - Under **Policies** > **Sharing**, confirm the external sharing level, default link type, and link expiration settings — these are the ceiling every site sharing setting is capped by.
3. **Configure idle session sign-out:**
   - Under **Policies** > **Access control** > **Idle session sign-out**, enable the setting and specify a timeout (e.g., 15 minutes).
   - Note the caveats: this applies to the *entire organization* — you cannot set it per site or per user — and it takes about 15 minutes to propagate, with no effect on sessions already open.
4. **Discuss the boundary:**
   - Ask: which of today's settings could Contoso's IT team delegate to individual site owners, and which absolutely must stay centralized? Idle session sign-out is a good example of "must stay centralized" — write down one more from what you've seen in earlier labs (hint: external sharing ceiling from LAB-03).

### Knowledge check:
Why can't idle session sign-out be configured per site, unlike sharing settings?

---

## Lab 2: Managing SharePoint Storage Settings (Tenant Level)
### Objective:
Learn how storage is allocated across the tenant, and when to switch from automatic to manual control.

### Steps:
1. **View total consumption:**
   - In **Active Sites**, note the overall storage usage summary at the top of the page.
2. **Understand Automatic mode (the default):**
   - Go to **Settings** > **Site storage limits**.
   - In **Automatic** mode, every site draws from one shared tenant pool as needed (up to 25 TB per site) — you don't set individual quotas at all.
3. **Switch to Manual and see what changes:**
   - Switch to **Manual**. Notice every existing site's quota jumps to the 25 TB maximum by default — manual mode doesn't shrink anything automatically, it just *unlocks* per-site control.
   - Set a small, deliberate limit (e.g., 5 GB) on the `Fabrikam Project` site from LAB-03 so you can test the warning behavior in Lab 3.
4. **Monitor consumption:**
   - Go to the **Microsoft 365 Admin Center** > **Reports** > **Usage**, and review the SharePoint storage report — this is your early-warning source before individual sites start hitting limits.

### Knowledge check:
Contoso is growing fast and unpredictably. Would you recommend Automatic or Manual storage management, and why might that answer change once Contoso has a few sites with sensitive, cost-tracked data (e.g., a client-billed video archive)?

---

## Lab 3: Managing Individual Site Storage Settings
### Objective:
Apply storage limits at the site level and see the warning system in action.

### Steps:
1. **View a site's usage:**
   - In **Active Sites**, select `Fabrikam Project` and check the **Storage used** figure.
2. **Fill it up (safely):**
   - Upload a handful of larger test files (or duplicate an existing file repeatedly) until the site approaches the 5 GB limit you set in Lab 2.
3. **Observe the warning:**
   - Once near the threshold, check whether site owners/members see an in-product storage warning. Note what happens if you try to upload past 100% — new uploads should be blocked while existing content remains accessible.
4. **Raise the limit and confirm:**
   - Increase `Fabrikam Project`'s limit to 10 GB and confirm uploads succeed again immediately, without any propagation delay (unlike idle session sign-out in Lab 1).

### Knowledge check:
What's the practical difference in *user experience* between a site that hits its storage limit versus a user who gets idle-session signed out? Which one is more disruptive to a client-facing site like `Fabrikam Project`, and why?

---

## Lab 4: Managing Site Creation Settings
### Objective:
Control who can create sites and how, balancing self-service productivity against sprawl.

### Steps:
1. **Review current self-service settings:**
   - Navigate to **Settings** > **Site creation**.
   - Note the three broad postures available: allow everyone to self-serve, restrict creation to admins only, or route creation through an **approval workflow** (someone requests a site, an approver signs off before it's provisioned).
2. **Pick the right posture for Contoso:**
   - Contoso is past the "5 people, everyone knows everyone" stage. Configure the **approval workflow** option instead of fully open self-service, and set yourself as the approver.
3. **Test it:**
   - As a test user (or by simulating the request), submit a new site creation request and confirm it lands in your approval queue instead of provisioning immediately.
4. **Restrict by group (optional hardening):**
   - Under **Site creation**, restrict who can even *submit* a request to a specific security group (e.g., "Site Owners" group) rather than every employee.
5. **Set sane defaults for anything that does get created:**
   - Configure default storage limit, default time zone, and default sharing settings for new sites — this is what keeps a wave of approved sites from each drifting to inconsistent settings.

### Knowledge check:
What's the actual trade-off Contoso is making by moving from open self-service to an approval workflow? Name one thing they gain and one thing they lose.

---

## Lab 5: Advanced Tenant and Site Management
### Objective:
Tie tenant settings together with day-to-day health monitoring.

### Steps:
1. **Enable/disable tenant features:**
   - In **Settings**, review toggles such as OneDrive **Request Files**, sync client restrictions, and the site collection app catalog — decide with reasoning (not just "enable everything") which ones fit Contoso today.
2. **Explore available site templates/designs:**
   - In **Active Sites** > **Create**, review the current Team Site and Communication Site (Topic/Showcase/Blank) options — you did the deep dive on this in LAB-02 Lab 4; here just confirm nothing has drifted since then.
3. **Monitor tenant health:**
   - Navigate to the **Microsoft 365 Admin Center** > **Health** > **Service health**.
   - Check current SharePoint Online status and any active advisories.

### Challenge:
Write a one-page "Contoso SharePoint Governance Baseline" combining decisions from Labs 1–5 of this file: sharing ceiling, idle timeout, storage mode, site creation posture, and default new-site settings. This is the artifact a real SharePoint admin hands to leadership — not a screenshot tour, a decision record.

### Knowledge check:
If you had to justify this governance baseline to a skeptical department head who just wants "self-service, no red tape," what's your strongest one-sentence argument?

---

These exercises provide hands-on experience managing tenant-level and site-specific settings in SharePoint Online — and, more importantly, practice in deciding *which* setting belongs at which level.
