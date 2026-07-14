# Lab 01: Understand AI Assistant in Cognos Analytics

## Overview
This lab introduces you to the embedded AI Assistant in Cognos Analytics. You'll learn how to leverage AI capabilities to create data modules and explore basic assistant features for data analysis and visualization.

## Prerequisites
- Completed [Lab 00: Configure Environment](../00-Configure%20Environment/README.md)
- Access to Cognos Analytics VM
- Login credentials: `pm` / `IBMDem0s`

## Learning Objectives
By completing this lab, you will:
1. Create a data module using Cognos Analytics
2. Explore AI Assistant features for data exploration
3. Generate visualizations using natural language queries
4. Understand the capabilities and limitations of the embedded AI Assistant

---

## Step 1: Access Cognos Analytics

### 1.1 Login to Cognos Analytics
1. From the Cognos Analytics VM, open **Firefox**
2. Navigate to: `http://ibmdemos:9300/bi/`
3. Enter credentials:
   - **Username**: `pm`
   - **Password**: `IBMDem0s`

<img width="1920" height="731" alt="image" src="https://github.com/user-attachments/assets/3014ed9c-3e68-440a-b208-3410123b5b7d" />

---

## Step 2: Create a Data Module

### 2.1 Start New Data Module
1. Click the **Hamburger menu** icon (☰)
2. Select **New** → **Data Module**

<img width="428" height="507" alt="image" src="https://github.com/user-attachments/assets/a0bf1e4f-306e-4ad8-8c39-03e2b345e12f" />

### 2.2 Select Data Source
1. Click the **Data Servers and Schemas** icon
2. In the search field, type: `Go Sales`
3. Select the **Go Sales** data source

<img width="1798" height="554" alt="image" src="https://github.com/user-attachments/assets/d77d81b9-98ff-475b-95b7-e7069981a49a" />

### 2.3 Choose Schema
Select **GOSALES/gosales** from the available schemas

<img width="1397" height="331" alt="image" src="https://github.com/user-attachments/assets/2d4a01f7-329a-4c65-8a70-dcea3770ce05" />

### 2.4 Select Tables
1. Click **Select Tables**
2. Click **Next**

<img width="1342" height="688" alt="image" src="https://github.com/user-attachments/assets/60b6d345-555a-4639-b0a9-e8b203a732b2" />

### 2.5 Add All Tables
1. Select **all tables** from the list
2. Click **OK**

<img width="1335" height="687" alt="image" src="https://github.com/user-attachments/assets/8b89c87a-7f76-4389-ac11-925e4f810df8" />

### 2.6 Save Data Module
1. Name the data module: `Go Sales Data Module`
2. Navigate to the **My Content** tab
3. Click **Save**

<img width="906" height="682" alt="image" src="https://github.com/user-attachments/assets/5e96c46d-8f9e-47a0-9d33-b35a1d4cd4e6" />

### 2.7 Add to Favorites
To easily access this data module in future labs:
1. Click the **three dots** icon (⋮) next to the data module
2. Select **Add to Favorites**

<img width="463" height="253" alt="image" src="https://github.com/user-attachments/assets/72cbd4e1-f4f9-42c5-af32-f24430b821e9" />

---

## Step 3: Explore AI Assistant Features

### 3.1 Open AI Assistant
1. Click the **Assistant** icon in the top-right corner
2. Click **Select My Data Source**

<img width="444" height="832" alt="image" src="https://github.com/user-attachments/assets/ea217e69-1fcc-435d-be9b-73e4bdc2e70d" />

### 3.2 Connect Data Source
1. Select **Go Sales Data Module**
2. Click **Open**

<img width="910" height="689" alt="image" src="https://github.com/user-attachments/assets/728bbaf7-4ed3-4128-a74f-bd20af32114b" />

### 3.3 Explore Data Fields
The AI Assistant displays all available fields from the data source. Try selecting different fields:

**Example 1: Select Unit Price**
- Click on **Unit Price** field
- Observe the suggested visualizations

<img width="422" height="542" alt="image" src="https://github.com/user-attachments/assets/ccd075a7-1593-4b5a-9431-89f4d32574c8" />

**Example 2: Select Ship Date**
- Click on **Ship Date** field
- Notice how the AI suggests time-based visualizations

<img width="413" height="562" alt="image" src="https://github.com/user-attachments/assets/2ca1bd9e-39f5-43b9-88e0-0257eba73d96" />

### 3.4 Generate Dashboard with Natural Language
Type the following query in the AI Assistant:
```
generate dashboard using this data source
```

The AI will analyze the data and suggest relevant charts and visualizations based on the available data.

<img width="1569" height="829" alt="image" src="https://github.com/user-attachments/assets/0d9b1360-b83d-4550-8e81-9e4fca4f4785" />

---

## Understanding AI Assistant Capabilities

### ✅ What the AI Assistant Can Do
- Suggest appropriate visualizations based on data types
- Generate basic dashboards from data sources
- Provide field-level insights
- Recommend chart types for specific data combinations

### ⚠️ Current Limitations
The basic AI Assistant in Cognos Analytics uses Natural Language Processing (NLP) with some limitations:
- May struggle with complex or ambiguous human language
- Limited understanding of business context
- Cannot perform advanced analytical reasoning
- Requires clear, simple queries for best results

---

## Summary
You have successfully:
- ✅ Created a data module in Cognos Analytics
- ✅ Connected the AI Assistant to your data source
- ✅ Explored field-level suggestions
- ✅ Generated visualizations using natural language
- ✅ Understood the capabilities and limitations of the embedded AI Assistant

**Next Step**: Proceed to [Lab 02: Advancing Cognos Analytics Capabilities with IBM Bob](../02-Advancing%20Cognos%20Analytics%20Capabilities%20with%20IBM%20Bob/README.md) to learn how IBM Bob extends these capabilities.
