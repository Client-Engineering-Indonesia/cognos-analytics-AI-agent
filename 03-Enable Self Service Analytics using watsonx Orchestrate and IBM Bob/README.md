# Lab 03: Enable Self-Service Analytics using watsonx Orchestrate and IBM Bob

## Overview
This lab demonstrates how to build an end-to-end self-service analytics solution that enables business users to query databases using natural language. You'll create a multi-agent system where watsonx Orchestrate provides the user interface, IBM Bob creates the database integration, and AI agents collaborate to translate questions into SQL queries and generate insights.

## Prerequisites
- Completed [Lab 02: Advancing Cognos Analytics Capabilities with IBM Bob](../02-Advancing%20Cognos%20Analytics%20Capabilities%20with%20IBM%20Bob/README.md)
- IBM Bob installed and configured
- watsonx Orchestrate Developer Edition installed
- MSSQL database user created (`causer` / `P@ssw0rd123`)
- Access to D2G database

## Learning Objectives
By completing this lab, you will:
1. Use IBM Bob to create an MCP server for MSSQL database connectivity
2. Deploy the MCP server as a remote service using ngrok
3. Build a Text-to-SQL AI agent in watsonx Orchestrate
4. Create an Analyst AI agent that orchestrates the workflow
5. Enable business users to query databases using natural language

---

## Architecture Overview

```
Business User Question
        ↓
watsonx Orchestrate (UI)
        ↓
Analyst AI Agent (Orchestrator)
        ↓
Text-to-SQL Agent → Generates SQL Query
        ↓
MSSQL MCP Server → Executes Query
        ↓
Analyst AI Agent → Analyzes Results
        ↓
Business Insights (Natural Language)
```

---

## Step 1: Setup MCP Server for MSSQL Database

### 1.1 Recreate IBM Bob Conversation
Follow the conversation flow documented in [`bob-task-mssql-mcp-server.json`](./bob-task-mssql-mcp-server.json) to guide IBM Bob through creating the MSSQL MCP server.

### 1.2 Key Conversation Highlights

**Phase 1: Database Connection**
- Ensure IBM Bob can connect to the MSSQL database
- Verify access to the D2G database
- Test authentication with the `causer` credentials

**Phase 2: Database Analysis**
- Ask IBM Bob to scrape all tables in the D2G database
- Request analysis and understanding of the data structure
- Generate assessment files in markdown format for each table

**Phase 3: MCP Server Creation**
Request IBM Bob to create an MCP server with the following tools:
1. **list-database**: List all available databases
2. **list-tables-by-database-name**: List tables in a specific database
3. **execute-sql-query**: Execute SQL queries against the database
4. **read-file**: Read documentation files about table structures

**Phase 4: Documentation Generation**
- IBM Bob creates detailed documentation for each table
- Files are stored in the `D2G-docs/` directory
- Documentation includes table schemas, relationships, and business context

### 1.3 Deploy MCP Server with ngrok
Deploy the MCP server as a remote service:
```
deploy mssql-mcp-server using ngrok service
```

**Important**: Save the generated ngrok URL - you'll need it to connect watsonx Orchestrate to the MCP server.

<img width="1523" height="569" alt="image" src="https://github.com/user-attachments/assets/2d1be8ef-a3b5-4dcd-86a1-24670fdf3051" />

---

## Step 2: Create AI Agents in watsonx Orchestrate

### 2.1 Access watsonx Orchestrate
Open watsonx Orchestrate in your browser:
```
http://localhost:3000
```

<img width="1576" height="875" alt="image" src="https://github.com/user-attachments/assets/4ad67841-acf6-4d66-9ddf-b06fee48c18c" />

---

## Step 3: Create Text-to-SQL Agent

### 3.1 Initialize New Agent
1. Click **Create New Agent**
2. Select **Create From Scratch**

<img width="756" height="527" alt="image" src="https://github.com/user-attachments/assets/b96f338a-e270-4c6c-925f-c6b3edf01989" />

### 3.2 Configure Agent Details
**Name**: `Text to SQL Agent`

**Description**:
```
You are a Text-to-SQL AI agent. Your sole job is to translate the user's natural language question into a correct, executable T-SQL SELECT statement against the **D2G** database on SQL Server.

- Output ONLY the SQL query — no explanation, no prose, no markdown fences unless specifically asked.
- If you cannot generate a query with confidence, respond with a single clarifying question.
- Never use SELECT *. Always name the specific columns needed.
- Always qualify table names as `[D2G].[dbo].[TableName]`.
- Use meaningful column aliases where column names are cryptic (e.g. `[# Separation]` AS `Separation_Count`).
- Default to TOP 100 for open-ended queries unless the user asks for aggregated/summary results.
```

### 3.3 Set Agent Style
Scroll to **Agent Style** section and select **React**

<img width="613" height="237" alt="image" src="https://github.com/user-attachments/assets/d222dbf5-029c-4eae-8439-bb724a30a59e" />

### 3.4 Add Knowledge Base

**Step 1**: Scroll to **Knowledge** section and click **Add Source +**

<img width="627" height="422" alt="image" src="https://github.com/user-attachments/assets/e4720b2c-c6d1-4507-a210-e82175bd24c0" />

**Step 2**: Click **New Knowledge**

<img width="629" height="238" alt="image" src="https://github.com/user-attachments/assets/7bd2607e-74a3-4a68-be96-c78d744a4564" />

**Step 3**: Select **Upload files** → Click **Next**

<img width="1449" height="824" alt="image" src="https://github.com/user-attachments/assets/fa0c3d3a-1e06-4706-af6d-2c9cfccb0fc8" />

**Step 4**: Download and upload the following table documentation files:

| Category | File |
|----------|------|
| Call Center | [Merged_Call_Center.txt](./D2G-docs/Merged_Call_Center.txt) |
| Customer Service | [Merged_Customer_Service.txt](./D2G-docs/Merged_Customer_Service.txt) |
| Empty Placeholder | [Merged_Empty_Placeholder.txt](./D2G-docs/Merged_Empty_Placeholder.txt) |
| Finance | [Merged_Finance.txt](./D2G-docs/Merged_Finance.txt) |
| HR Workforce | [Merged_HR_Workforce.txt](./D2G-docs/Merged_HR_Workforce.txt) |
| IT BI Audit | [Merged_IT_BI_Audit.txt](./D2G-docs/Merged_IT_BI_Audit.txt) |
| Procurement | [Merged_Procurement.txt](./D2G-docs/Merged_Procurement.txt) |
| Recruiting | [Merged_Recruiting.txt](./D2G-docs/Merged_Recruiting.txt) |
| Sales Orders | [Merged_Sales_Orders.txt](./D2G-docs/Merged_Sales_Orders.txt) |
| Sales Pipeline | [Merged_Sales_Pipeline.txt](./D2G-docs/Merged_Sales_Pipeline.txt) |
| Training | [Merged_Training.txt](./D2G-docs/Merged_Training.txt) |

**Step 5**: Upload files and click **Next**

<img width="1439" height="822" alt="image" src="https://github.com/user-attachments/assets/5dfc9f4e-971c-40de-87b0-743f5e1da933" />

**Step 6**: Configure knowledge base:
- **Name**: `MSSQL Table Summary`
- **Description**: `This knowledge gives information about list of tables and columns in MSSQL database, including short description from each table.`
- Click **Save**

<img width="1454" height="831" alt="image" src="https://github.com/user-attachments/assets/653764f4-942d-4d02-9f64-39236c9f6d92" />

### 3.5 Configure Agent Behavior
Scroll to **Behavior** section and paste the prompt from [`text_to_sql_prompt.txt`](./text_to_sql_prompt.txt)

<img width="627" height="307" alt="image" src="https://github.com/user-attachments/assets/ed48bed4-d5c2-47c4-b435-a830fd5d688e" />

### 3.6 Test Text-to-SQL Agent
Test the agent with a sample query:
```
show procurement compliance rate
```

The agent should generate a valid T-SQL query targeting the procurement tables.

<img width="1560" height="782" alt="image" src="https://github.com/user-attachments/assets/2c6ab4cf-b5c2-42f1-ac43-0422ace2d602" />

---

## Step 4: Create Analyst AI Agent

### 4.1 Initialize Analyst Agent
Create a new agent with the following configuration:

**Name**: `Analyst AI Agent`

**Description**:
```
You are a Senior Data Analyst AI agent. Your job is to answer business questions by retrieving data from the D2G SQL Server database and turning the results into clear, actionable insights.

You do NOT write SQL yourself. You delegate SQL generation to the Text2SQL collaborator agent, pass the generated query to the execute-query tool, and then interpret and present the results to the user.
```

### 4.2 Set Agent Style
Set **Agent Style** to **React**

### 4.3 Add MCP Server Tool

**Step 1**: Scroll to **Toolset** section → Click **Add tool** → Select **MCP Server**

<img width="727" height="519" alt="image" src="https://github.com/user-attachments/assets/4707cefa-a5db-4782-813d-ee6fe915fa63" />

**Step 2**: Click **Add MCP Server** → Select **Remote MCP Server** → Click **Next**

<img width="1448" height="823" alt="image" src="https://github.com/user-attachments/assets/2aef9d38-fa10-4cdf-834a-51382f7639ab" />

**Step 3**: Configure MCP Server connection:
- **Name**: `MSSQL MCP Server`
- **MCP Server URL**: [Your ngrok URL from Step 1.3]
- Click **Connect**

<img width="1461" height="829" alt="image" src="https://github.com/user-attachments/assets/c522f681-347b-4e60-a60b-3a43a3a901ec" />

**Step 4**: Select the SQL execution tool generated by IBM Bob

<img width="623" height="254" alt="image" src="https://github.com/user-attachments/assets/a954ac8d-5000-48f2-9bb8-9559717daef6" />

### 4.4 Add Text-to-SQL Agent as Collaborator

**Step 1**: In **Agents** section, click **Add Agent** → Select **Local Instance**

<img width="754" height="285" alt="image" src="https://github.com/user-attachments/assets/3ed38e7a-e97a-45a0-9957-8c9cb6063dff" />

**Step 2**: Select **Text to SQL Agent** → Click **Add to Agent**

<img width="616" height="354" alt="image" src="https://github.com/user-attachments/assets/4b4c0c51-46a8-4d93-9135-c98251d42661" />

### 4.5 Configure Analyst Behavior
Scroll to **Behavior** section and paste the prompt from [`ai_analyst_agent_prompt.txt`](./ai_analyst_agent_prompt.txt)

<img width="637" height="316" alt="image" src="https://github.com/user-attachments/assets/2f988320-b146-4fa6-81b9-8bd3627b9290" />

### 4.6 Test Analyst AI Agent
Test the complete workflow:
```
how many employees for now?
```

The agent should:
1. Delegate SQL generation to Text-to-SQL Agent
2. Execute the query via MCP Server
3. Analyze the results
4. Provide a natural language answer

<img width="625" height="498" alt="image" src="https://github.com/user-attachments/assets/06ff43fc-1587-42aa-a5da-5ba7d0198b5f" />

---

## Understanding the Multi-Agent Workflow

### 🔄 Execution Flow

1. **User Query**: Business user asks a question in natural language
2. **Analyst Agent**: Receives the query and determines it needs data
3. **Text-to-SQL Agent**: Translates the question into a SQL query
4. **MCP Server**: Executes the SQL query against the database
5. **Analyst Agent**: Receives the data and generates insights
6. **User Response**: Business user receives actionable insights

### 🎯 Agent Responsibilities

**Text-to-SQL Agent**:
- Specializes in SQL generation
- Uses knowledge base of table structures
- Produces syntactically correct T-SQL queries
- Handles complex joins and aggregations

**Analyst AI Agent**:
- Orchestrates the overall workflow
- Delegates SQL generation to specialist agent
- Executes queries via MCP tools
- Interprets results and generates insights
- Provides business context and recommendations

---

## Real-World Use Cases

### 📊 Business Intelligence Scenarios

**1. Executive Dashboards**
```
"What are our top 5 products by revenue this quarter?"
"Show me employee turnover trends by department"
"What's our procurement compliance rate?"
```

**2. Operational Analytics**
```
"How many open support tickets do we have?"
"Which vendors have the highest defect rates?"
"What's the average time to fill open positions?"
```

**3. Financial Analysis**
```
"Compare revenue vs. forecast for each region"
"Show me expense trends by cost center"
"What's our cash flow projection for next month?"
```

**4. HR Analytics**
```
"How many employees are eligible for promotion?"
"What's the average training completion rate?"
"Show me diversity metrics by department"
```

### 🚀 Advanced Scenarios

**Multi-Database Queries**
- Combine data from multiple databases
- Cross-reference information across systems
- Generate unified reports

**Predictive Analytics**
- Identify trends and patterns
- Forecast future metrics
- Recommend proactive actions

**Automated Reporting**
- Schedule regular report generation
- Distribute insights to stakeholders
- Alert on anomalies or thresholds

---

## Best Practices

### ✅ Do's
- Start with simple queries to test the system
- Provide clear, specific questions
- Review generated SQL queries for accuracy
- Iterate on prompts to improve results
- Document common query patterns

### ⚠️ Don'ts
- Avoid overly complex questions initially
- Don't assume the agent knows business context without providing it
- Don't skip testing with sample data first
- Avoid queries that could impact database performance

---

## Troubleshooting

### Common Issues

**Issue**: Agent generates incorrect SQL
- **Solution**: Review the knowledge base documentation
- **Solution**: Provide more specific table/column names in the question

**Issue**: MCP Server connection fails
- **Solution**: Verify ngrok URL is still active
- **Solution**: Check database credentials and connectivity

**Issue**: Query times out
- **Solution**: Add TOP clause to limit results
- **Solution**: Optimize query with proper indexes

---

## Summary
You have successfully:
- ✅ Created an MCP server for MSSQL database connectivity using IBM Bob
- ✅ Deployed the MCP server as a remote service with ngrok
- ✅ Built a specialized Text-to-SQL AI agent
- ✅ Created an orchestrating Analyst AI agent
- ✅ Enabled natural language database queries for business users
- ✅ Understood multi-agent collaboration patterns
- ✅ Explored real-world use cases for self-service analytics

**Key Achievement**: You've built a production-ready self-service analytics platform that democratizes data access, enabling business users to get insights without SQL knowledge or technical expertise.

**Next Steps**: 
- Expand the knowledge base with additional tables
- Create specialized agents for different business domains
- Integrate with other enterprise systems
- Build automated reporting workflows
