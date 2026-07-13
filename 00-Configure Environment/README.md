# Configure Environment

Here you will: 
1. Reserve Techzone environment;
2. Install IBM Bob;
3. Install watsonx Orchestrate Developer Edition;
4. Create MSSQL user

## Reserve Techzone environment

1. Open https://techzone.ibm.com/ -> Login but if you don't have IBM ID, please register.

<img width="1920" height="939" alt="image" src="https://github.com/user-attachments/assets/18d56f71-5a75-4d85-87d5-f4ec12040b3a" />

2. This lab will be using Techzone environment in this url: https://techzone.ibm.com/collection/5f7229b4529ea0001e4c19b5. Please provision it by yourself.

<img width="1920" height="767" alt="image" src="https://github.com/user-attachments/assets/0d7a15f8-9dff-4634-b774-b351ebd9cf4e" />

3. After reservation is ready, open it -> scroll down until you find Virtual Machine section -> click Console button to open VM containing Cognos Analytics.

<img width="1184" height="255" alt="image" src="https://github.com/user-attachments/assets/7e0ad163-e823-4b09-9ab1-68c1693cb9d9" />

## Install & Login to IBM Bob

1. From Cognos Analytics VM, open IBM Bob download page https://bob.ibm.com/download/ and download based on your environment

<img width="1919" height="930" alt="image" src="https://github.com/user-attachments/assets/24598334-4d13-4043-b7ff-d47c178e6262" />

2. Install it -> login using your credential (you must be eligible before you are moving forward) -> ensure you have same view like image below

<img width="1920" height="625" alt="image" src="https://github.com/user-attachments/assets/a6340d85-db88-472f-a50a-7a987083b28d" />

## Install watsonx Orchestrate Developer Edition

1. From Cognos Analytics VM, open watsonx Orchestrate Agent Development Kit website https://developer.watson-orchestrate.ibm.com/getting_started/installing -> follow the page to have watsonx orchestrate installed in your local workspage

<img width="1920" height="976" alt="image" src="https://github.com/user-attachments/assets/808664ef-b762-4481-81dc-275a687ebbe9" />

2. Then, continue to install Developer Edition to localize watsonx Orchestrate by following this url: https://developer.watson-orchestrate.ibm.com/developer_edition/wxOde_overview/. Using this approach watsonx Orchestrate as IBM Agentic AI will be able to communicate to on-prem environment, including application, data sources, files, etc.

<img width="1900" height="964" alt="image" src="https://github.com/user-attachments/assets/21ed1f2a-5fc8-49c1-8e97-b454737a44e7" />

## Create MSSQL User

1. From Start menu, open Microsoft SQL Server Management Studio -> login using Windows authentication

<img width="1920" height="922" alt="image" src="https://github.com/user-attachments/assets/91b42721-66e8-4ee1-b7dd-2689ab85f8a9" />

2. Under Security folder, right click -> select New Login

<img width="339" height="201" alt="image" src="https://github.com/user-attachments/assets/aec51c32-c0cb-418d-90b4-6e91f9f61895" />

3. In General tab, set Login name: causer; password: P@ssw0rd123; 

<img width="684" height="623" alt="image" src="https://github.com/user-attachments/assets/9aaa6aa4-f404-42eb-92c0-649e9709d3be" />

4. In Server Role tab, select `public` and `sysadmin`

<img width="691" height="629" alt="image" src="https://github.com/user-attachments/assets/923fd4c4-c55d-4ee3-99da-bc6231ecf35b" />

5. In User Mapping tab, select D2G -> for Database and membership, select `db_owner` and `public` -> click OK

<img width="689" height="623" alt="image" src="https://github.com/user-attachments/assets/2d8ad13a-4151-49bc-a198-14c1ceff643f" />

