# Advancing Cognos Analytics Capabilities with IBM Bob

Here you will use IBM Bob to connect with Cognos Analytics API endpoint then create MCP Server automatically. This is helpful for new developer without sufficient knowledge and experience in developing Cognos Analytics.

## Create Summary of Cognos Analytics API endpoints

1. First you have to understand that Cognos Analytics has prebuilt swagger that can be found in this url: `http://ibmdemos:9300/api/api-docs`

<img width="1581" height="860" alt="image" src="https://github.com/user-attachments/assets/f7c8de26-db7c-4ce6-b770-2599eb91caa5" />

2. Type `create summary or md file to2learn about how to use Cognos3Analytics API endpoints by referring4to this swagger:5http://ibmdemos:9300/api/api-docs/` and press Enter -> just follow the thinking of IBM Bob -> if you find prompt to Approve, please select Approve.

<img width="232" height="772" alt="image" src="https://github.com/user-attachments/assets/50706873-4e23-44dc-8cd0-951d85fea0d0" />

3. IBM Bob has capability to self-thinking, act and reiterate by itself. In this stage, it will automatically crawl the web page, understand the content, and create summary file. 

<img width="232" height="623" alt="image" src="https://github.com/user-attachments/assets/44f3f377-4eb7-4b16-a256-da73ec04ca49" />

4. When thinking process has completed, it will create .md file that helps understanding Cognos Analytics API. You can find the sample here: [02-Enable Cognos Analytics MCP Server using IBM Bob/cognos-api-reference.md](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/02-Enable%20Cognos%20Analytics%20MCP%20Server%20using%20IBM%20Bob/cognos-api-reference.md)

## Enable Cognos Analytics MCP Server

1. Type `i want you to create MCP Server of Cognos Analytics with this detail.2url: http://ibmdemos:9300/bi/ ;3username: pm;4password: IBMDem0s; Namespace: Harmony LDAP` -> approve all prompt

<img width="1558" height="781" alt="image" src="https://github.com/user-attachments/assets/cf3aa966-4fbd-46a5-91ad-0001a7187826" />

2. You can find sample of thinking history in this [file](https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/02-Enable%20Cognos%20Analytics%20MCP%20Server%20using%20IBM%20Bob/bob-task-3b48aa8a9a4e97b42f3c185d46ff6b39-2026-07-13.json)

3. We will use Go Sales Data Module that we have created in Lab 01 to test this MCP. Type `show my favorites`. Here you will see that Go Sales Data Module is listed in the output.

<img width="963" height="263" alt="image" src="https://github.com/user-attachments/assets/b8811458-e351-4771-8ec8-33986fa224fa" />

4. Generate insight by typing `show what data inside Go Sales Data Module`. You will see tables linked to this asset.

<img width="953" height="545" alt="image" src="https://github.com/user-attachments/assets/3e978bcb-8d9c-4528-943b-7d367f7cedf9" />

5. Type `please drill deeper` to enrich IBM Bob response quality about reviewing data module

<img width="1120" height="587" alt="image" src="https://github.com/user-attachments/assets/68a43b77-3403-43c3-9860-796fde194611" />

Closing:
IBM Bob is powerful when it is used in the right motion. For your reference, here is samples of use cases that can be developed if Cognos Analytics and IBM Bob are met.
https://github.com/Client-Engineering-Indonesia/cognos-analytics-AI-agent/blob/main/02-Enable%20Cognos%20Analytics%20MCP%20Server%20using%20IBM%20Bob/ibm-bob-cognos-analytics-developer-use-cases.html
