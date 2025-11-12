# Standard Operating Procedure
## Onboarding a New Westcoast IT Engineer

**Document Control**
- **Document ID**: SOP-IT-001
- **Version**: 1.0
- **Effective Date**: November 1, 2025
- **Department**: Information Technology
- **Prepared By**: IT Systems Administrator
- **Approved By**: IT Director

---

## 1. PURPOSE

This Standard Operating Procedure (SOP) outlines the step-by-step process for onboarding new engineers to the Westcoast IT Active Directory infrastructure. This document ensures consistent account creation, proper security group assignment, and appropriate access provisioning.

## 2. SCOPE

This procedure applies to all new IT engineers joining Westcoast IT and must be completed within the first day of employment.

## 3. PREREQUISITES

- Access to Domain Controller (lab-vm)
- Administrator credentials for WestCoast.local domain
- Completed new hire documentation from HR
- Azure Bastion access configured

---

## 4. PROCEDURE

### 4.1 ACCESS THE DOMAIN CONTROLLER

**Step 1:** Connect to Azure Portal
- Navigate to https://portal.azure.com
- Sign in with administrative credentials

**Step 2:** Access Virtual Machine via Bastion
- Go to Virtual Machines → lab-vm
- Click "Connect" → Select "Bastion"
- Enter credentials:
  - Username: `AzureAdmin`
  - Password: [As configured]
- Click "Connect" to open RDP session

### 4.2 OPEN ACTIVE DIRECTORY TOOLS

**Step 3:** Launch Active Directory Users and Computers
- On the Domain Controller, open Server Manager
- Click "Tools" → "Active Directory Users and Computers"
- Expand "WestCoast.local" domain

**Step 4:** Navigate to IT Organizational Unit
- Expand "Users" folder
- Locate and select "IT" OU

### 4.3 CREATE NEW USER ACCOUNT

**Step 5:** Initiate User Creation
- Right-click on "IT" OU
- Select "New" → "User"

**Step 6:** Enter User Information
- **First name**: [Engineer's first name]
- **Last name**: [Engineer's last name]
- **Full name**: [Auto-populated]
- **User logon name**: [firstname initial + lastname]@WestCoast.local
  - Example: rlopez@WestCoast.local
- **User logon name (pre-Windows 2000)**: [Same as above]
- Click "Next"

**Step 7:** Set Password and Options
- **Password**: Set temporary password (minimum 12 characters)
- Check "User must change password at next logon"
- Uncheck "User cannot change password"
- Uncheck "Password never expires"
- Check "Account is disabled" (will be enabled after completion)
- Click "Next" → "Finish"

### 4.4 CONFIGURE USER PROPERTIES

**Step 8:** Set Organizational Information
- Right-click newly created user → "Properties"
- Navigate to "General" tab:
  - **Description**: IT Engineer
  - **Office**: [Location]
  - **Telephone**: [Extension]
  - **Email**: [firstname.lastname]@westcoastit.com

**Step 9:** Configure Organization Tab
- Click "Organization" tab:
  - **Title**: IT Engineer
  - **Department**: IT
  - **Company**: Westcoast IT
  - **Manager**: [Select IT Manager from directory]

**Step 10:** Set Account Options
- Click "Account" tab:
  - Verify **User logon name** is correct
  - Set **Account expires**: Never (unless temporary position)
  - Ensure "Account is disabled" is unchecked

### 4.5 ASSIGN SECURITY GROUPS

**Step 11:** Add to Required Groups
- Click "Member Of" tab
- Click "Add" button
- Enter the following groups (one at a time):
  - **Domain Users** (default)
  - **IT-Engineers**
  - **VPN-Users**
  - **Remote-Desktop-Users**
- Click "Check Names" → "OK"

### 4.6 CONFIGURE NETWORK ACCESS

**Step 12:** Set Logon Restrictions
- Click "Account" tab → "Logon Hours"
- Configure: Monday-Friday, 6:00 AM - 8:00 PM (or as per company policy)
- Click "OK"

**Step 13:** Assign Dial-in Permissions (VPN Access)
- Click "Dial-in" tab
- Select "Allow access" under Network Access Permission
- Click "Apply" → "OK"

### 4.7 PROVISION WORKSPACE

**Step 14:** Add Computer to Domain
- Navigate to "Computers" OU in AD Users and Computers
- Document assigned computer name (format: WC-ENG-[NUMBER])
- Ensure computer is joined to WestCoast.local domain

**Step 15:** Create Home Directory (Optional)
- Open File Explorer on Domain Controller
- Navigate to shared drive: `\\lab-vm\Users$`
- Create folder: [username]
- Right-click folder → "Properties" → "Security"
- Grant Full Control to: [username]@WestCoast.local
- Grant Read access to: IT-Managers group

---

## 5. VERIFICATION STEPS

**Step 16:** Test Account Login
- Log out of Domain Controller
- Attempt login with new credentials
- Verify password change prompt appears
- Confirm successful login

**Step 17:** Verify Group Memberships
- Open Command Prompt
- Run: `whoami /groups`
- Confirm all assigned groups are listed

**Step 18:** Test Network Resources
- Access shared IT resources: `\\lab-vm\IT-Share`
- Verify read/write permissions
- Test VPN connectivity (if applicable)

---

## 6. COMPLETION CHECKLIST

- [ ] User account created in IT OU
- [ ] All required properties configured
- [ ] Security groups assigned
- [ ] Network access permissions set
- [ ] Home directory created (if applicable)
- [ ] Computer assigned and documented
- [ ] Account login tested and verified
- [ ] Welcome email sent to new engineer
- [ ] IT Manager notified of completion
- [ ] Onboarding ticket closed in system

---

## 7. DOCUMENTATION REQUIREMENTS

**Step 19:** Update Records
- Document account details in IT asset management system
- Add entry to "New Hire Tracking" spreadsheet
- File physical documentation in personnel folder

**Step 20:** Communicate Completion
- Send welcome email to new engineer with:
  - Username and temporary password
  - Links to IT resources and documentation
  - Contact information for IT support
- Notify IT Manager and HR of completion

---

## 8. RELATED DOCUMENTS

- SOP-IT-002: Offboarding IT Personnel
- SOP-IT-003: Security Group Management
- Westcoast IT Security Policy v2.1
- Azure Bastion Connection Guide
- Active Directory Naming Conventions

## 9. REVISION HISTORY

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 11/11/2025 | IT Admin | Initial creation |

---

## 10. CONTACT INFORMATION

**IT Support**: support@westcoastit.com | Ext. 5500  
**IT Manager**: manager@westcoastit.com | Ext. 5501  
**Emergency Contact**: oncall@westcoastit.com | 24/7 Support

---

*This document is confidential and intended for internal use only.*
