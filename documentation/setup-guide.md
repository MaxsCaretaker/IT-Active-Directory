# Westcoast IT Active Directory - Quick Setup Guide


---

## Part 1: Azure Setup 

### Create Resource Group
1. Go to https://portal.azure.com
2. Search "Resource groups" → Create
3. Name: `WestcoastIT-RG`
4. Region: `West US 2`
5. Click "Review + create" → Create

### Create Virtual Network
1. Search "Virtual networks" → Create
2. Name: `lab-vnet`
3. Resource group: `WestcoastIT-RG`
4. Region: `West US 2`
5. IP address: `10.0.0.0/16`
6. Subnet: `default` - `10.0.0.0/24`
7. Click "Review + create" → Create

### Create Virtual Machine
1. Search "Virtual machines" → Create
2. **Basics**:
   - Name: `lab-vm`
   - Region: `West US 2`
   - Image: `Windows Server 2022 Datacenter`
   - Size: `Standard_B2s` (2 vCPU, 4GB RAM) (Recommended) 
   - Username: `labadmin`
   - Password: [Create strong password - SAVE THIS!]
   - Public inbound ports: None
3. **Networking**:
   - Virtual network: `lab-vnet`
   - Subnet: `default`
   - Public IP: None
4. Click "Review + create" → Create 

### Setup Azure Bastion (for secure connection)
1. Go to your `lab-vnet`
2. Click "Subnets" → Add subnet
   - Name: `AzureBastionSubnet` (exact name)
   - Address: `10.0.1.0/26`
3. Search "Bastions" → Create
   - Name: `lab-bastion`
   - Virtual network: `lab-vnet`
   - Create new public IP
4. Click Create (wait 10-15 min)

### Connect to VM
1. Go to Virtual machines → `lab-vm`
2. Click "Connect" → "Bastion"
3. Username: `labadmin`
4. Password: [your password]
5. Click "Connect"


---

## Part 2: Install Active Directory 

### Add Server Roles
1. Server Manager opens automatically
2. Click "Manage" → "Add Roles and Features"
3. Click "Next" until you reach "Server Roles"
4. Check these boxes:
   - ☑ Active Directory Domain Services (click "Add Features")
   - ☑ DHCP Server (click "Add Features")
   - ☑ DNS Server (click "Add Features")
   - ☑ File and Storage Services
   - ☑ Print and Document Services
   - ☑ Web Server (IIS) (click "Add Features")
5. Click "Next" through all screens
6. Click "Install" 
7. Click "Close"

### Make it a Domain Controller
1. Click the **yellow warning flag** in Server Manager (top right)
2. Click "Promote this server to a domain controller"
3. Select "Add a new forest"
4. Root domain: `WestCoast.local`
5. Click "Next"
6. Create DSRM password (SAVE THIS!)
7. Click "Next" through all screens (ignore warnings)
8. Click "Install"
9. **Server will restart automatically** 

### Reconnect After Restart
1. Use Bastion to reconnect
2. Username: `WESTCOAST\labadmin`
3. Password: [same password]


---

## Part 3: Create Organization Structure 

### Open AD Users and Computers
1. Server Manager → Tools → "Active Directory Users and Computers"
2. You'll see "WestCoast.local" domain

### Create Organizational Units (OUs)
1. Right-click "WestCoast.local" → New → Organizational Unit
2. Name: `Users` → OK
3. Right-click the "Users" OU → New → Organizational Unit
   - Create: `IT`
   - Create: `HR`
   - Create: `Sales`
4. Right-click "WestCoast.local" → New → Organizational Unit
   - Name: `Computers` → OK

**Your structure now looks like this:**
```
WestCoast.local
├── Users
│   ├── IT
│   ├── HR
│   └── Sales
└── Computers
```


---

## Part 4: Create User Accounts 

### Create John Thomas (HR)
1. Navigate to: Users → HR
2. Right-click HR → New → User
3. Fill in:
   - First name: `John`
   - Last name: `Thomas`
   - User logon name: `jthomas`
4. Click "Next"
5. Set password → Check "User must change password at next logon"
6. Click "Next" → "Finish"

### Create Maria Garcia (Sales)
1. Navigate to: Users → Sales
2. Right-click Sales → New → User
3. Fill in:
   - First name: `Maria`
   - Last name: `Garcia`
   - User logon name: `mgarcia`
4. Click "Next" → Set password → "Next" → "Finish"

### Create Robert Lopez (IT)
1. Navigate to: Users → IT
2. Right-click IT → New → User
3. Fill in:
   - First name: `Robert`
   - Last name: `Lopez`
   - User logon name: `rlopez`
4. Click "Next" → Set password → "Next" → "Finish"


---

## Part 5: Configure DHCP 

### Setup DHCP Server
1. Server Manager → Tools → DHCP
2. Right-click your server → "Complete DHCP configuration"
3. Click "Commit" → "Close"

### Create DHCP Scope
1. Expand server → Right-click "IPv4" → "New Scope"
2. Click "Next"
3. Name: `WestCoast Network`
4. Click "Next"
5. IP range:
   - Start: `10.0.0.100`
   - End: `10.0.0.200`
   - Subnet mask: `255.255.255.0`
6. Click "Next" → "Next" (skip exclusions)
7. Leave lease duration default → "Next"
8. Select "Yes, configure options" → "Next"
9. Gateway: `10.0.0.1` → Add → "Next"
10. DNS should auto-fill → "Next"
11. Skip WINS → "Next"
12. Select "Yes, activate now" → "Next" → "Finish"

### Authorize DHCP
1. Right-click your server name → "Authorize"
2. Press F5 to refresh
3. Server should show **green checkmark**


---

## Part 6: Verification 

### Test DNS
1. Open PowerShell
2. Type: `nslookup WestCoast.local`
3. Should show your server IP

### Check Active Directory
1. Open "Active Directory Users and Computers"
2. Verify you see:
   - WestCoast.local domain
   - Users OU with IT, HR, Sales
   - Computers OU
   - Three user accounts

### Check Services
1. Open PowerShell as Administrator
2. Run these commands:
```powershell
Get-Service DNS
Get-Service DHCPServer
Get-ADUser -Filter * | Select Name, SamAccountName
```

All services should show "Running"


---

## Summary - What You Built

| Component | Details |
|-----------|---------|
| **Domain** | WestCoast.local |
| **Domain Controller** | lab-vm (10.0.0.4) |
| **Services** | AD DS, DNS, DHCP, File, Print, IIS |
| **OUs** | IT, HR, Sales, Computers |
| **Users** | jthomas (HR), mgarcia (Sales), rlopez (IT) |
| **DHCP Range** | 10.0.0.100 - 10.0.0.200 |

---

## Quick Commands Reference

```powershell
# List all AD users
Get-ADUser -Filter * | Select Name, Department

# Check domain controller
dcdiag /v

# Force group policy update
gpupdate /force

# Check services
Get-Service DNS, DHCPServer, ADWS
```

---

## Troubleshooting

**Can't connect to Bastion?**
- Make sure Bastion deployment finished (check notifications)
- Try a different browser

**DNS not working?**
- Restart DNS service: `Restart-Service DNS`

**DHCP not authorized?**
- Right-click server in DHCP console → Authorize
- Refresh with F5

**User can't login?**
- Verify account created in correct OU
- Check password requirements met
- Use format: `WESTCOAST\username` or `username@WestCoast.local`

---

## Next Steps

1. **Take screenshots** of your work for documentation
2. **Create more users** to practice
3. **Join a client computer** to the domain (requires another VM)
4. **Set up file shares** for departments
5. **Configure Group Policies** for security

---

**Important Notes:**
- Save all passwords securely
- Document IP addresses
- Take regular snapshots in Azure
- Turn off auto-shutdown if doing extended work

**Need Help?** Check Microsoft's official documentation:
- https://docs.microsoft.com/windows-server/identity/ad-ds/

---


You now have a working Active Directory environment in Azure.
