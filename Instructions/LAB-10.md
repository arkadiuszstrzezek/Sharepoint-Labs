# SharePoint Online Lab Exercises - LAB-10

> **Running scenario:** You've now built and governed most of Contoso Consulting's SharePoint environment across LAB-01 through LAB-09. The last skill isn't a feature — it's staying ahead of Microsoft's own release cadence, so you're never the last to know about an outage or a breaking change (like the classic-site retirement you documented back in LAB-02).

## Lab 1: Exploring the Microsoft 365 Health Center
### Objective:
Learn to monitor real-time service health, and to distinguish "Microsoft's problem, wait it out" from "my tenant's problem, go fix it."

### Steps:
1. **Access Service Health:**
   - Navigate to the **Microsoft 365 Admin Center** (https://admin.microsoft.com) > **Health** > **Service health**.
2. **Review current status:**
   - Check SharePoint Online's current status and any active incidents or advisories tenant-wide.
3. **Inspect an incident in detail (use a past/resolved one if nothing is active):**
   - Open an incident and record: Incident ID, impacted services, current status, and estimated time to resolution.
   - Note which fields tell you whether to open a support ticket (a *your-tenant-specific* issue) versus just wait (a *Microsoft-wide* incident already being worked).
4. **Set up notifications:**
   - Go to **Health** > **Message center**, and configure email notifications so Contoso's admins are alerted to service health changes without needing to check the dashboard manually.
5. **Run a mini incident drill:**
   - Imagine users report `Fabrikam Project` (LAB-03) is "completely down" right now. Using only Service Health, walk through how you'd confirm within 2 minutes whether this is a Microsoft-side outage or something local to your tenant/site — write down the exact sequence of clicks.

### Knowledge check:
A user says "SharePoint is down." Service Health shows all services green. What are two tenant-specific things you'd check next, given everything you've configured in LAB-01 through LAB-09 (hint: think about what you configured in LAB-06 and LAB-08a)?

---

## Lab 2: Understanding the SharePoint Roadmap
### Objective:
Learn to track upcoming SharePoint changes before they land on users' screens — and before they break something you built earlier in this course.

### Steps:
1. **Access the roadmap:**
   - Navigate to the **Microsoft 365 Roadmap** (https://www.microsoft.com/microsoft-365/roadmap) and filter for **SharePoint**.
2. **Explore upcoming features:**
   - Review items marked **In development** or **Rolling out**. Pick one that would actually matter to Contoso (e.g., anything touching hub sites, sharing, or Advanced Management) and summarize it in one sentence.
3. **Retroactively check yourself:**
   - Search the roadmap for the classic-site retirement and SharePoint Premium rebrand you documented back in LAB-02 and LAB-08. Confirm the roadmap entry existed *before* you would have been caught off guard by it in production — this is the entire value of the habit.
4. **Track launched features:**
   - Filter to **Launched**, and find one recent SharePoint feature you haven't used yet in this course. Try it once, briefly, on any test site.
5. **Subscribe to updates:**
   - Bookmark the roadmap filtered view (or set up an RSS/Message Center notification) so this becomes a recurring 10-minute weekly habit, not a one-time lab exercise.

### Knowledge check:
Name one change you documented earlier in this course (LAB-02's classic site retirement or LAB-08's Syntex→Premium rebrand) that a roadmap-watching admin would have seen coming months in advance. What's the cost, concretely, of finding out the hard way instead?

---

## Lab 3: Turning Monitoring Into Administrative Planning
### Objective:
Convert Health Center and Roadmap awareness into an actual operating habit for Contoso, not just something you look at when things break.

### Steps:
1. **Monitor proactively, not reactively:**
   - Using the Service Health dashboard, identify any advisory (even a minor one) and draft a two-sentence heads-up message you'd send to Contoso site owners *before* it affects them.
2. **Build a feature adoption plan:**
   - Take the roadmap feature you flagged as relevant in Lab 2, and sketch a rollout plan: who needs training, who needs to approve it, and how it connects to something you already governed (e.g., if it's a sharing-related feature, tie it back to your LAB-03 sharing policy).
3. **Produce a stakeholder report:**
   - Combine a Service Health summary and a roadmap summary into a single one-page update, written for Contoso's leadership — not for another admin. No jargon, no raw incident IDs, just "here's what happened, here's what's coming, here's what we're doing about it."

### Challenge:
Set a recurring 15-minute weekly slot (literally add it to a calendar) for "Service Health + Roadmap check" and define, in writing, what would trigger you to escalate something from that check into an actual change request for Contoso — this is the difference between admins who get surprised and admins who don't.

### Knowledge check:
Why is a stakeholder report that says "SharePoint had 2 incidents this month, both resolved, and 3 relevant features are rolling out next quarter" more valuable to leadership than raw access to the Service Health dashboard itself?

---

These exercises give hands-on experience monitoring Microsoft 365 service health and using the SharePoint roadmap for administrative planning — closing the loop on a course that started with "what does a SharePoint admin do" and ends with "how do you keep doing it well over time."
