# Active Directory Group Policy Troubleshooting Lab

**Scenario:** I troubleshot a Group Policy issue in my Active Directory lab where a domain user was still able to access Control Panel even though a GPO was configured to restrict it.

**1. Verify the User and Policy**

I verified that Marcus Johnson was located in the `HelpDesk-Lab` OU and confirmed that the `HelpDesk - Control Panel Restriction` GPO was configured to prohibit access to Control Panel and PC settings.

<img width="1456" height="1080" alt="01-marcus-helpdesk-ou png" src="https://github.com/user-attachments/assets/739349f6-3055-455e-95ae-c2bef0743a19" />

<img width="1590" height="989" alt="02-control-panel-gpo-enabled png" src="https://github.com/user-attachments/assets/7d6daa2c-b948-486b-a45d-5cd7e8d884e7" />

**2. Reproduce the Issue**

I logged into the Windows 11 domain client as Marcus and used `whoami` to verify the account. Control Panel was still accessible, confirming that the expected policy was not being enforced.

**3. Investigate with gpresult**

I ran `gpresult /r` and found `N/A` under Applied Group Policy Objects. This confirmed that the expected GPO was not reaching the user.

<img width="1172" height="925" alt="03-control-panel-still-accessible-annotated" src="https://github.com/user-attachments/assets/eab6c475-8907-46d6-934e-7b725f433945" />

**4. Identify the Root Cause**

I inspected the `HelpDesk-Lab` OU in Group Policy Management and discovered that the GPO showed `Link Enabled: No`. The policy was configured correctly, but its disabled link prevented it from applying.

<img width="1728" height="592" alt="04-gpresult-policy-not-applied png" src="https://github.com/user-attachments/assets/dadeaa7d-50e4-4665-9c92-6bd1d7b5362d" />

**5. Apply the Fix**

I re-enabled the `HelpDesk - Control Panel Restriction` GPO link and ran `gpupdate /force` on the Windows 11 client to refresh Group Policy.

<img width="1360" height="752" alt="05-gpo-link-disabled-root-cause png" src="https://github.com/user-attachments/assets/98cb6662-0477-45fe-8522-856bc8d19b3b" />

**6. Verify the Resolution**

<img width="1000" height="832" alt="06-gpo-link-enabled-fix png" src="https://github.com/user-attachments/assets/ca269917-f5ce-4ef3-b427-33d7cb734274" />

I ran `gpresult /r` again and confirmed that `HelpDesk - Control Panel Restriction` now appeared under Applied Group Policy Objects.

<img width="1455" height="1081" alt="07-control-panel-access-blocked png" src="https://github.com/user-attachments/assets/b3440e32-811c-4a48-9832-63abc69dcafb" />

**Tools used:** Active Directory Users and Computers • Group Policy Management • Group Policy Management Editor • `gpresult /r` • `gpupdate /force` • `whoami` • Windows Server • Windows 11

**Result:** Successfully diagnosed and resolved a Group Policy application issue caused by a disabled GPO link.

## What I Learned

I learned that configuring a GPO correctly does not guarantee that it is actually reaching the intended user. Using `gpresult /r` helped me determine that the issue was with policy application rather than the policy setting itself.

This lab also reinforced the importance of troubleshooting in a structured order instead of immediately changing settings:

**Reproduce → Gather Evidence → Find the Root Cause → Fix → Verify**
