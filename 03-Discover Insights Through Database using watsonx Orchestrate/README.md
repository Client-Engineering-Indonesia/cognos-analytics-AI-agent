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

3. 
