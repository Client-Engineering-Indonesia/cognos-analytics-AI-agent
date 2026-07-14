# Enable Self Service Analytics using watsonx Orchestrate and IBM Bob

Here you will create use cases about self-service analytics where business user can simply asking question about certain data from MSSQL database, then he/she will get insights. 
To do this, watsonx orchestrate will be used as it has user-friendly UI for business user. 
But before doing that, you will use IBM Bob to create MCP Server to connect to local MSSQL database.

## Setup MCP Server to MSSQL Database

1. Redo conversation as shown in this [file](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/bob-task-mssql-mcp-server.json)

Key Highlights:
* Ensure IBM Bob can connect to MSSQL database
* Ask IBM Bob to scrape all tables in D2G database, understand the data, create assessment as .md files
* Ask IBM Bob to create MCP Server that has tools, such as: 1) list-database; 2) list-tables-by-database-name; 3) execute-sql-query; 4) read-file
* watsonx orchestrate will use this MCP server to interact with database
* read-file tool will be used to read files that contains summary of each table in the database

2. Deploy MCP Server as Remote MCP Server using Ngrok by typing `deploy mssql-mcp-server using ngrok service` -> remember that this process will generate MCP Url that will be used by watsonx orchestrate to communicate

<img width="1523" height="569" alt="image" src="https://github.com/user-attachments/assets/2d1be8ef-a3b5-4dcd-86a1-24670fdf3051" />

## Create AI Agent in watsonx orchestrate

Here you will create 2 agents.
* Text to SQL Agent: focus to create SQL statement by understanding human natural language to get relevant tables and columns
* Analyst Agent: act as main agent to collaborate with Text to SQL Agent -> execute SQL statement returned by Text to SQL Agent -> understand retrieved data and generate insights based on user query

1. Open watsonx orchestrate from this url: http://localhost:3000

<img width="1576" height="875" alt="image" src="https://github.com/user-attachments/assets/4ad67841-acf6-4d66-9ddf-b06fee48c18c" />

### Create Text to SQL Agent

2. Click Create New Agent -> select Create From Scratch

<img width="756" height="527" alt="image" src="https://github.com/user-attachments/assets/b96f338a-e270-4c6c-925f-c6b3edf01989" />

3. Set Name to `Text to SQL Agent` and Description to:

```
You are a Text-to-SQL AI agent. Your sole job is to translate the user's natural language question into a correct, executable T-SQL SELECT statement against the **D2G** database on SQL Server.

- Output ONLY the SQL query — no explanation, no prose, no markdown fences unless specifically asked.
- If you cannot generate a query with confidence, respond with a single clarifying question.
- Never use SELECT *. Always name the specific columns needed.
- Always qualify table names as `[D2G].[dbo].[TableName]`.
- Use meaningful column aliases where column names are cryptic (e.g. `[# Separation]` AS `Separation_Count`).
- Default to TOP 100 for open-ended queries unless the user asks for aggregated/summary results.
```

4. Scroll down to Agent Style section and select React

<img width="613" height="237" alt="image" src="https://github.com/user-attachments/assets/d222dbf5-029c-4eae-8439-bb724a30a59e" />

5. Scroll down to Knowledge section -> click Add Source +

<img width="627" height="422" alt="image" src="https://github.com/user-attachments/assets/e4720b2c-c6d1-4507-a210-e82175bd24c0" />

6. Click New Knowledge

<img width="629" height="238" alt="image" src="https://github.com/user-attachments/assets/7bd2607e-74a3-4a68-be96-c78d744a4564" />

7. Select "Upload files" in Select Source tab -> click Next button

<img width="1449" height="824" alt="image" src="https://github.com/user-attachments/assets/fa0c3d3a-1e06-4706-af6d-2c9cfccb0fc8" />

8. Download files below:

* [Call Center tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Call_Center.txt)
* [Customer Service table](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Customer_Service.txt)
* [Empty Placeholder tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Empty_Placeholder.txt)
* [Finance tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Finance.txt)
* [HR Workforce tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_HR_Workforce.txt)
* [IT BI Audit tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_IT_BI_Audit.txt)
* [Procurement tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Procurement.txt)
* [Recruiting tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Recruiting.txt)
* [Sales Order tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Sales_Orders.txt)
* [Sales Pipelines tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Sales_Pipeline.txt)
* [Training tables](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/D2G-docs/Merged_Training.txt)

9. Click "Drag and drop files here or click to upload" -> upload files in step 9 -> click Next button

<img width="1439" height="822" alt="image" src="https://github.com/user-attachments/assets/5dfc9f4e-971c-40de-87b0-743f5e1da933" />

10. Set Name to "MSSQL Table Summary", set Description to `This knowledge gives information about list of tables and columns in MSSQL database, including short description from each table.` -> click Save

<img width="1454" height="831" alt="image" src="https://github.com/user-attachments/assets/653764f4-942d-4d02-9f64-39236c9f6d92" />

11. Scroll down to Behavior section -> set prompt using 

<img width="627" height="307" alt="image" src="https://github.com/user-attachments/assets/ed48bed4-d5c2-47c4-b435-a830fd5d688e" />

12. Scroll down to Behavior section then fill it with prompt from this [file](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/text_to_sql_prompt.txt)

13. Test this agent by typing `show procurement compliance rate`

<img width="1560" height="782" alt="image" src="https://github.com/user-attachments/assets/2c6ab4cf-b5c2-42f1-ac43-0422ace2d602" />

### Create Analyst AI Agent

14. Create new Agent named "Analyst AI Agent" -> set description to:

```
You are a Senior Data Analyst AI agent. Your job is to answer business questions by retrieving data from the D2G SQL Server database and turning the results into clear, actionable insights.
You do NOT write SQL yourself. You delegate SQL generation to the Text2SQL collaborator agent, pass the generated query to the execute-query tool, and then interpret and present the results to the user.
```

15. Once it is created, set Agent style to React -> scroll down to Toolset section -> click Add tool -> select MCP Server

<img width="727" height="519" alt="image" src="https://github.com/user-attachments/assets/4707cefa-a5db-4782-813d-ee6fe915fa63" />

16. Click Add MCP Server -> select Remote MCP Server -> click Next button

<img width="1448" height="823" alt="image" src="https://github.com/user-attachments/assets/2aef9d38-fa10-4cdf-834a-51382f7639ab" />

17. Set Name to `MSSQL MCP Server` -> set MCP Server URL to previous ngrok that you have deployed -> click Connect

<img width="1461" height="829" alt="image" src="https://github.com/user-attachments/assets/c522f681-347b-4e60-a60b-3a43a3a901ec" />

18. Select tool to execute SQL statement that is automatically generated by IBM Bob -> ensure you have similar view like below

<img width="623" height="254" alt="image" src="https://github.com/user-attachments/assets/a954ac8d-5000-48f2-9bb8-9559717daef6" />

19. In Agents section, click Add Agent -> select Local Instance

<img width="754" height="285" alt="image" src="https://github.com/user-attachments/assets/3ed38e7a-e97a-45a0-9957-8c9cb6063dff" />

20. Select Text to SQL Agent -> click Add to Agent button -> ensure you have similar view as below

<img width="616" height="354" alt="image" src="https://github.com/user-attachments/assets/4b4c0c51-46a8-4d93-9135-c98251d42661" />

21. Scroll down to Behavor section -> fill it using prompt from this [file](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/ai_analyst_agent_prompt.txt) -> ensure you have similar view as below

<img width="637" height="316" alt="image" src="https://github.com/user-attachments/assets/2f988320-b146-4fa6-81b9-8bd3627b9290" />

22. Test this agent by running typing `how many employees for now?`

<img width="625" height="498" alt="image" src="https://github.com/user-attachments/assets/06ff43fc-1587-42aa-a5da-5ba7d0198b5f" />

