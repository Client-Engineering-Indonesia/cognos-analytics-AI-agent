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

5. 
Scroll down to Behavior section then fill it with prompt from this [file](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/03-Discover%20Insights%20Through%20Database%20using%20watsonx%20Orchestrate/text_to_sql_prompt.txt)
