Reporter:
Shatakshi Malla | Cybersecurity Intern at Green Tick Nepal Pvt. Ltd.

Affected Product:
- Product: Axigen Mail Server
- Component: WebAdmin Branding
- Version: 10.6.15

Vulnerability Summary:
A **persistent Cross-Site Scripting (XSS)** vulnerability exists in the WebAdmin branding feature of Axigen Mail Server version 10.6.15. An authenticated administrator can inject arbitrary JavaScript code through the brand name field in the global settings that will execute whenever the dashboard page is loaded, potentially compromising the administrator’s session.

Vulnerability Type:
- Cross-Site Scripting (Stored/Persistent)
- CWE: CWE-79 (Improper Neutralization of Input During Web Page Generation)

Impact:
- Arbitrary JavaScript code execution in the context of an administrator’s session.
- Theft of session cookies.
- Potential for full administrative session compromise.
- Elevated risk of further server or application compromise through hijacked sessions.
 
Preconditions:
- Attacker must have valid administrator login credentials.
- Attacker must access Global Settings to modify branding.
- The administrator’s browser must visit the affected dashboard page after the malicious branding is saved.

Technical Details and Steps to Reproduce:

1. **Login** to Axigen WebAdmin as an administrator.
2. Navigate to **Global Settings**.
3. Click **Manage Branding**.
4. In the **Brand Name** field, insert the following payload (case insensitive):
   
   ```
   sfsdf</tExTaReA><sCrIpT>alert(document.cookie)</sCrIpT>
   ```
   <img width="1754" height="456" alt="image" src="https://github.com/user-attachments/assets/97f44468-3874-4646-a024-cb3c810f2571" />

5. **Save** the configuration.
6. Navigate to the **Dashboard**.
7. Observe that the injected script executes every time the dashboard is loaded, triggering an alert displaying the current document cookies, demonstrating persistent stored XSS.

Proof of Concept:
The injected payload executes JavaScript alert showing cookies, confirming that arbitrary scripts run in admin context upon visiting the dashboard.

Suggested Mitigations:
- Sanitize inputs in the branding fields before storing to prevent script injection.
- Implement proper HTML/JavaScript escaping here and in any user-controlled input fields.
