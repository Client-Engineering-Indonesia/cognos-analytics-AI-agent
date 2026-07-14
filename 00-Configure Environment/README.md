# Lab 00: Configure Environment

## Overview
This lab guides you through setting up the required environment for Cognos Analytics AI Agent development. You'll configure IBM TechZone, install essential tools, and prepare database access.

## Prerequisites
- IBM ID (register at [techzone.ibm.com](https://techzone.ibm.com/) if needed)
- Access to IBM Bob (eligibility required)

## Learning Objectives
By completing this lab, you will:
1. Reserve and access a TechZone environment with Cognos Analytics
2. Install and configure IBM Bob AI assistant
3. Set up watsonx Orchestrate Developer Edition for local development
4. Create MSSQL database user credentials for data access

---

## Step 1: Reserve TechZone Environment

### 1.1 Access IBM TechZone
Navigate to [IBM TechZone](https://techzone.ibm.com/) and log in with your IBM ID. If you don't have an IBM ID, register for one first.

<img width="1920" height="939" alt="image" src="https://github.com/user-attachments/assets/18d56f71-5a75-4d85-87d5-f4ec12040b3a" />

### 1.2 Provision the Environment
Access the [Cognos Analytics TechZone collection](https://techzone.ibm.com/collection/5f7229b4529ea0001e4c19b5) and provision the environment.

<img width="1920" height="767" alt="image" src="https://github.com/user-attachments/assets/0d7a15f8-9dff-4634-b774-b351ebd9cf4e" />

### 1.3 Access the Virtual Machine
Once your reservation is ready:
- Open the reservation details
- Scroll to the **Virtual Machine** section
- Click the **Console** button to access the VM with Cognos Analytics

<img width="1184" height="255" alt="image" src="https://github.com/user-attachments/assets/7e0ad163-e823-4b09-9ab1-68c1693cb9d9" />

---

## Step 2: Install IBM Bob

### 2.1 Download IBM Bob
From the Cognos Analytics VM, navigate to the [IBM Bob download page](https://bob.ibm.com/download/) and download the version appropriate for your environment.

<img width="1919" height="930" alt="image" src="https://github.com/user-attachments/assets/24598334-4d13-4043-b7ff-d47c178e6262" />

### 2.2 Install and Login
1. Run the installer
2. Log in with your IBM credentials (eligibility verification required)
3. Verify successful installation by confirming you see the IBM Bob interface

<img width="1920" height="625" alt="image" src="https://github.com/user-attachments/assets/a6340d85-db88-472f-a50a-7a987083b28d" />

---

## Step 3: Install watsonx Orchestrate Developer Edition

### 3.1 Install Agent Development Kit
From the Cognos Analytics VM, visit the [watsonx Orchestrate Agent Development Kit](https://developer.watson-orchestrate.ibm.com/getting_started/installing) and follow the installation instructions.

<img width="1920" height="976" alt="image" src="https://github.com/user-attachments/assets/808664ef-b762-4481-81dc-275a687ebbe9" />

### 3.2 Install Developer Edition
Install the [Developer Edition](https://developer.watson-orchestrate.ibm.com/developer_edition/wxOde_overview/) to enable local deployment of watsonx Orchestrate. This allows the AI agent to communicate with on-premises resources including:
- Applications
- Data sources
- Local files
- Other enterprise systems

<img width="1900" height="964" alt="image" src="https://github.com/user-attachments/assets/21ed1f2a-5fc8-49c1-8e97-b454737a44e7" />

### 3.3 Verify Installation
Run the following command to start watsonx Orchestrate:
```bash
orchestrate chat start
```
Confirm that the watsonx Orchestrate homepage opens successfully.

<img width="1583" height="878" alt="image" src="https://github.com/user-attachments/assets/ad934a11-9f03-4f53-869c-616b2c078575" />

---

## Step 4: Create MSSQL Database User

### 4.1 Open SQL Server Management Studio
1. From the Windows Start menu, launch **Microsoft SQL Server Management Studio**
2. Connect using **Windows Authentication**

<img width="1920" height="922" alt="image" src="https://github.com/user-attachments/assets/91b42721-66e8-4ee1-b7dd-2689ab85f8a9" />

### 4.2 Create New Login
1. In the Object Explorer, expand the **Security** folder
2. Right-click on **Logins**
3. Select **New Login**

<img width="339" height="201" alt="image" src="https://github.com/user-attachments/assets/aec51c32-c0cb-418d-90b4-6e91f9f61895" />

### 4.3 Configure Login Credentials
In the **General** tab, configure:
- **Login name**: `causer`
- **Password**: `P@ssw0rd123`
- Select **SQL Server authentication**
- Uncheck **Enforce password policy** (for lab purposes only)

<img width="684" height="623" alt="image" src="https://github.com/user-attachments/assets/9aaa6aa4-f404-42eb-92c0-649e9709d3be" />

### 4.4 Assign Server Roles
In the **Server Roles** tab, select:
- ☑ `public`
- ☑ `sysadmin`

<img width="691" height="629" alt="image" src="https://github.com/user-attachments/assets/923fd4c4-c55d-4ee3-99da-bc6231ecf35b" />

### 4.5 Configure Database Access
In the **User Mapping** tab:
1. Check the **D2G** database
2. In the database role membership section, select:
   - ☑ `db_owner`
   - ☑ `public`
3. Click **OK** to create the user

---

## Summary
You have successfully configured your development environment with:
- ✅ IBM TechZone environment with Cognos Analytics
- ✅ IBM Bob AI assistant
- ✅ watsonx Orchestrate Developer Edition
- ✅ MSSQL database user (`causer`) with appropriate permissions

**Next Step**: Proceed to [Lab 01: Understand AI Assistant in Cognos Analytics](../01-Understand%20AI%20Assistant%20in%20Cognos%20Analytics/README.md)

<img width="689" height="623" alt="image" src="https://github.com/user-attachments/assets/2d8ad13a-4151-49bc-a198-14c1ceff643f" />

