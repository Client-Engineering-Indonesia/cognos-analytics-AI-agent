# Lab 02: Advancing Cognos Analytics Capabilities with IBM Bob

## Overview
This lab demonstrates how IBM Bob extends Cognos Analytics capabilities by automatically creating MCP (Model Context Protocol) servers. You'll learn how IBM Bob can analyze API documentation, generate integration code, and enable advanced interactions with Cognos Analytics without requiring deep technical expertise.

## Prerequisites
- Completed [Lab 01: Understand AI Assistant in Cognos Analytics](../01-Understand%20AI%20Assistant%20in%20Cognos%20Analytics/README.md)
- IBM Bob installed and configured
- Access to Cognos Analytics VM
- Go Sales Data Module created and added to favorites

## Learning Objectives
By completing this lab, you will:
1. Use IBM Bob to analyze and document Cognos Analytics API endpoints
2. Automatically generate an MCP server for Cognos Analytics integration
3. Interact with Cognos Analytics data modules through IBM Bob
4. Understand the power of AI-assisted development for analytics platforms

---

## Step 1: Create Cognos Analytics API Documentation

### 1.1 Understand the Cognos Analytics API
Cognos Analytics provides a comprehensive REST API with Swagger documentation available at:
```
http://ibmdemos:9300/api/api-docs
```

<img width="1581" height="860" alt="image" src="https://github.com/user-attachments/assets/f7c8de26-db7c-4ce6-b770-2599eb91caa5" />

### 1.2 Generate API Summary with IBM Bob
In IBM Bob, enter the following prompt:
```
create summary or md file to learn about how to use Cognos Analytics API endpoints by referring to this swagger: http://ibmdemos:9300/api/api-docs/
```

Press **Enter** and approve any prompts that appear.

<img width="232" height="772" alt="image" src="https://github.com/user-attachments/assets/50706873-4e23-44dc-8cd0-951d85fea0d0" />

### 1.3 IBM Bob's Autonomous Processing
IBM Bob demonstrates advanced AI capabilities by:
- **Self-thinking**: Analyzing the task requirements
- **Acting**: Crawling the Swagger documentation
- **Understanding**: Extracting API endpoint information
- **Iterating**: Refining the documentation structure
- **Creating**: Generating a comprehensive markdown file

<img width="232" height="623" alt="image" src="https://github.com/user-attachments/assets/44f3f377-4eb7-4b16-a256-da73ec04ca49" />

### 1.4 Review Generated Documentation
Once complete, IBM Bob creates a markdown file documenting the Cognos Analytics API. 

**Reference**: View the sample output at [`cognos-api-reference.md`](./cognos-api-reference.md)

---

## Step 2: Create Cognos Analytics MCP Server

### 2.1 Generate MCP Server with IBM Bob
Enter the following prompt in IBM Bob:
```
i want you to create MCP Server of Cognos Analytics with this detail.
url: http://ibmdemos:9300/bi/
username: pm
password: IBMDem0s
Namespace: Harmony LDAP
```

Approve all prompts that appear during the process.

<img width="1558" height="781" alt="image" src="https://github.com/user-attachments/assets/cf3aa966-4fbd-46a5-91ad-0001a7187826" />

### 2.2 Understanding the MCP Server Creation
IBM Bob automatically:
1. Analyzes the Cognos Analytics API structure
2. Generates authentication handlers
3. Creates tool definitions for common operations
4. Implements error handling and logging
5. Packages everything as a functional MCP server

**Reference**: View the complete thinking process at [`bob-task-3b48aa8a9a4e97b42f3c185d46ff6b39-2026-07-13.json`](./bob-task-3b48aa8a9a4e97b42f3c185d46ff6b39-2026-07-13.json)

---

## Step 3: Test the MCP Server Integration

### 3.1 List Favorite Items
Test the MCP server by querying your favorites:
```
show my favorites
```

IBM Bob will use the MCP server to retrieve your favorite items, including the Go Sales Data Module created in Lab 01.

<img width="963" height="263" alt="image" src="https://github.com/user-attachments/assets/b8811458-e351-4771-8ec8-33986fa224fa" />

### 3.2 Explore Data Module Contents
Query the data module structure:
```
show what data inside Go Sales Data Module
```

IBM Bob will display the tables and relationships within the data module.

<img width="953" height="545" alt="image" src="https://github.com/user-attachments/assets/3e978bcb-8d9c-4528-943b-7d367f7cedf9" />

### 3.3 Deep Dive into Data Module
Request detailed information:
```
please drill deeper
```

IBM Bob will provide comprehensive details about:
- Table structures
- Column definitions
- Data types
- Relationships between tables
- Metadata information

<img width="1120" height="587" alt="image" src="https://github.com/user-attachments/assets/68a43b77-3403-43c3-9860-796fde194611" />

---

## Understanding IBM Bob's Value Proposition

### 🚀 Key Advantages

**1. Accelerated Development**
- Eliminates weeks of API learning and integration coding
- Automatically generates production-ready MCP servers
- Reduces time-to-value from weeks to minutes

**2. Democratized Development**
- Enables non-technical users to create integrations
- No deep programming knowledge required
- Natural language interface for complex operations

**3. Intelligent Automation**
- Self-learning from API documentation
- Adaptive error handling
- Context-aware responses

**4. Enterprise-Ready**
- Secure authentication handling
- Proper error management
- Logging and monitoring capabilities

### 💡 Use Case Examples

IBM Bob + Cognos Analytics enables powerful scenarios:

1. **Automated Report Generation**
   - Natural language queries to generate reports
   - Scheduled report distribution
   - Dynamic dashboard creation

2. **Data Governance**
   - Automated metadata extraction
   - Data lineage tracking
   - Access control management

3. **Self-Service Analytics**
   - Business users query data without SQL knowledge
   - AI-assisted data exploration
   - Automated insight generation

4. **Integration Workflows**
   - Connect Cognos Analytics with other enterprise systems
   - Automated data pipeline creation
   - Cross-platform analytics orchestration

**Reference**: Explore detailed use cases at [`ibm-bob-cognos-analytics-developer-use-cases.html`](./ibm-bob-cognos-analytics-developer-use-cases.html)

---

## Summary
You have successfully:
- ✅ Generated comprehensive API documentation using IBM Bob
- ✅ Created a functional MCP server for Cognos Analytics
- ✅ Tested the integration by querying data modules
- ✅ Explored advanced data module inspection capabilities
- ✅ Understood the transformative potential of AI-assisted development

**Key Takeaway**: IBM Bob transforms complex API integration tasks into simple natural language conversations, enabling rapid development and democratizing access to enterprise analytics platforms.

**Next Step**: Proceed to [Lab 03: Enable Self-Service Analytics using watsonx Orchestrate and IBM Bob](../03-Enable%20Self%20Service%20Analytics%20using%20watsonx%20Orchestrate%20and%20IBM%20Bob/README.md) to build end-to-end analytics workflows.
