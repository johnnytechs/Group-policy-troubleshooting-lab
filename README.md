# Active Directory Group Policy Troubleshooting Lab

**Scenario:** I troubleshot a Group Policy issue in my Active Directory lab where a domain user was still able to access Control Panel even though a GPO was configured to restrict it.

**1. Verify the User and Policy**

I verified that Marcus Johnson was located in the `HelpDesk-Lab` OU and confirmed that the `HelpDesk - Control Panel Restriction` GPO was configured to prohibit access to Control Panel and PC settings.

<img width="1456" height="1080" alt="01-marcus-helpdesk-ou png" src="https://github.com/user-attachments/assets/739349f6-3055-455e-95ae-c2bef0743a19" />

**2. Reproduce the Issue**

I logged into the Windows 11 domain client as Marcus and used `whoami` to verify the account. Control Panel was still accessible, confirming that the expected policy was not being enforced.

**3. Investigate with gpresult**

I ran `gpresult /r` and found `N/A` under Applied Group Policy Objects. This confirmed that the expected GPO was not reaching the user.

**4. Identify the Root Cause**

I inspected the `HelpDesk-Lab` OU in Group Policy Management and discovered that the GPO showed `Link Enabled: No`. The policy was configured correctly, but its disabled link prevented it from applying.

**5. Apply the Fix**

I re-enabled the `HelpDesk - Control Panel Restriction` GPO link and ran `gpupdate /force` on the Windows 11 client to refresh Group Policy.

**6. Verify the Resolution**

I ran `gpresult /r` again and confirmed that `HelpDesk - Control Panel Restriction` now appeared under Applied Group Policy Objects.

**Tools used:** Active Directory Users and Computers • Group Policy Management • Group Policy Management Editor • `gpresult /r` • `gpupdate /force` • `whoami` • Windows Server • Windows 11

**Result:** Successfully diagnosed and resolved a Group Policy application issue caused by a disabled GPO link.

## What I Learned

I learned that configuring a GPO correctly does not guarantee that it is actually reaching the intended user. Using `gpresult /r` helped me determine that the issue was with policy application rather than the policy setting itself.

This lab also reinforced the importance of troubleshooting in a structured order instead of immediately changing settings:

**Reproduce → Gather Evidence → Find the Root Cause → Fix → Verify**
