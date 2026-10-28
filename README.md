# ai-hr-analyst-n8n-hr-analytics
Project Overview

Human Resources teams regularly handle requests for workforce metrics, including employee headcount, attrition, absenteeism, recruitment, and employee performance indicators. Retrieving these figures manually can consume valuable time and delay decision-making.

This project uses workflow automation and artificial intelligence to simplify the HR reporting process. When an email with the subject KPI REQUEST arrives in the inbox, the workflow processes the request, uses an AI agent to identify the required metrics, retrieves relevant data from Google Sheets, and replies to the original sender.

Project Objectives

Automate routine HR KPI reporting requests.
Interpret natural-language questions using an AI agent.
Retrieve workforce metrics from a centralized Google Sheet.
Generate clear and concise email responses.
Reduce manual reporting effort and improve access to HR analytics.
Establish a foundation for scalable, AI-assisted HR reporting.
Key Features

Automated Gmail monitoring: Checks the inbox every minute for emails matching the configured subject filter.
AI-powered request processing: Uses an Anthropic chat model through an n8n AI Agent.
Google Sheets integration: Connects the agent to an HR KPI spreadsheet for data retrieval.
Automated email replies: Responds to the original sender with the generated report.
Clarification support: The agent is instructed to ask which KPIs are required when a request is unclear or empty.
Attachment downloading: The Gmail Trigger is configured to download attachments.
End-to-end automation: Connects email intake, AI processing, data lookup, and response delivery in one workflow.
Technology Stack

Technology

Purpose

n8n

Workflow automation and orchestration

Gmail

Receives KPI requests and sends responses

Google Sheets

Stores and provides access to KPI data

Anthropic Claude

Powers natural-language interpretation and response generation

n8n AI Agent

Coordinates the language model and spreadsheet tool

Google OAuth2

Authenticates access to Gmail and Google Sheets
Workflow Architecture

The workflow consists of five configured nodes.

1.      Gmail Trigger: Monitors the inbox every minute and filters messages using the subject query KPI REQUEST.

2.      AI Agent: Extracts the sender, subject, and email body, interprets the request, and uses the Google Sheets tool to look up the requested metrics.

3.      Anthropic Chat Model: Provides the language-model capabilities used by the AI Agent.

4.      Google Sheets Tool: Provides access to the selected spreadsheet and worksheet for KPI retrieval.

5.      Reply to a Message: Sends the generated plain-text response to the original sender.

Workflow Diagram

flowchart TD

    A[Gmail Trigger] --> B[AI Agent]

    C[Anthropic Chat Model] -.-> B

    D[Google Sheets Tool] -.-> B
     B --> E[Gmail Reply]

The Anthropic model and Google Sheets tool are connected to the AI Agent as supporting components. The main execution path runs from Gmail Trigger to AI Agent and then to Gmail Reply.
How the Workflow Works

Step 1: Receive a KPI Request

The Gmail Trigger checks the inbox every minute using the configured Gmail search query:

subject: KPI REQUEST

It captures the incoming email information, including the sender, subject, and message body. Attachment downloading is enabled.

Example incoming email:

Subject: KPI REQUEST

 

Hello HR Team,

 

Could you please provide the current employee

headcount and attrition rate?

 

Thank you.

Step 2: Process the Request with AI

The AI Agent receives the email details and uses the Anthropic chat model to understand which metrics the sender is requesting.

Its configured instructions direct it to use Google Sheets for KPI lookups and generate a concise plain-text response.

If the request is unclear or empty, the agent is instructed to ask the sender which available KPIs they need.

Step 3: Retrieve HR Data from Google Sheets

The Google Sheets tool is connected to the AI Agent to support KPI retrieval from the configured spreadsheet.

Examples of metrics that could be maintained in the spreadsheet include:

Total employee headcount
Employee attrition rate
Employee turnover rate
Absenteeism rate
Recruitment and hiring metrics
Average time to hire
Training and development indicators
The workflow's actual reporting capabilities depend on the data available in the connected worksheet and the effectiveness of the agent's lookup process.

Step 4: Generate an Email Response

The AI Agent uses the available data to prepare a concise response.

Illustrative response:

Hello,

 

Here are the requested HR KPIs:

 

Employee Headcount: 520

Attrition Rate: 12.5%

 

Please let me know if you require any additional

HR metrics.

 

Regards,

HR Analytics

The numbers above are fictional examples for demonstration purposes and are not retrieved results from the connected spreadsheet.

Step 5: Reply to the Original Sender

The Gmail Reply node uses the original message ID to reply to the incoming email. It sends the AI Agent's output as plain text and is configured to reply to the sender only.

This completes the automated reporting cycle.
HR KPI Data Structure

For consistent reporting, maintain a structured Google Sheet with clear column headings.

Example:

KPI Name

Value

Reporting Period

Unit

Employee Headcount

520

October 2026

Employees

Attrition Rate

12.5

October 2026

%

Absenteeism Rate

3.2

October 2026

%

Average Time to Hire

28

October 2026

Days

These are illustrative sample records. Replace them with validated organizational data before using the workflow.

Recommended data practices:

Use consistent KPI names and column headers.
Include the reporting period for every metric.
Document the formula and definition of each KPI.
Validate source data before using it in reports.
Keep percentage, currency, count, and time units consistent.
Installation and Configuration

Prerequisites

Before running the workflow, you need:

An active n8n instance.
A Gmail account with the necessary OAuth2 permissions.
A Google Sheets document containing HR KPI data.
A supported Anthropic chat model connection.
Access permissions for the required Gmail and Google Sheets resources.
1. Import the Workflow

1.      Open your n8n instance.

2.      Create a new workflow or use the workflow import option.

3.      Import the exported n8n workflow JSON.

4.      Review the node connections and configuration.

5.      Save the workflow.

Suggested repository structure:

hr-analytics-kpi-automation/

├── README.md

├── workflows/

│   └── hr-kpi-email-automation.json

├── docs/

│   ├── workflow-architecture.md

│   └── kpi-data-dictionary.md

├── samples/

│   └── sample-kpi-data.csv

└── .gitignore

2. Configure Gmail

1.      Create a Gmail OAuth2 credential in n8n.

2.      Grant the required permissions to read matching emails and send replies.

3.      Select the credential in the Gmail Trigger and Reply nodes.

4.      Verify that the trigger filters the intended messages.

Configured search query:

subject: KPI REQUEST

The trigger is configured to poll every minute.

3. Configure Google Sheets

1.      Create or select a Google Sheets OAuth2 credential.

2.      Select the spreadsheet containing your HR KPI data.

3.      Choose the appropriate worksheet.

4.      Verify that the Google Sheets node can retrieve the necessary rows.

5.      Test whether the AI Agent can find the requested metrics.

Use your own spreadsheet configuration and do not publish private spreadsheet identifiers in a public repository.

4. Configure Anthropic

1.      Set up the supported Anthropic connection in n8n.

2.      Select an available chat model.

3.      Connect the chat model to the AI Agent's language-model input.

4.      Test the agent using sample KPI requests.

The provided workflow references claude-sonnet-5. Confirm that this model identifier is available to your account and supported by your configured n8n integration. If it is not, select an available model.

5. Configure the AI Agent

The configured system instructions direct the agent to:

Interpret HR KPI requests received by email.
Use the Google Sheets tool to look up requested metrics.
Generate concise, plain-text email replies.
Ask for clarification when requests are unclear or empty.
For production deployment, extend these instructions to explicitly prohibit invented figures, distinguish missing data from zero values, and include reporting periods in responses.

6. Test and Activate the Workflow

1.      Send a test email with the subject KPI REQUEST.

2.      Verify that the Gmail Trigger captures the email.

3.      Inspect the AI Agent's execution output.

4.      Confirm that the Google Sheets lookup returns the expected values.

5.      Verify that the email reply is sent to the intended sender.

6.      Activate the workflow after successful testing.
Testing Scenarios

Test Scenario

Expected Result

Valid KPI request

Generates a response using retrieved spreadsheet data

Multiple KPI request

Attempts to retrieve and report each requested metric

Unclear request

Asks the sender to clarify the required KPIs

Empty request

Asks which available KPIs are needed

KPI not found

Should identify unavailable data rather than inventing a value

Spreadsheet access failure

Requires appropriate error handling and failure notification

Unrelated email

Should not match the configured subject query

Email with attachment

Attachment is downloaded; further processing requires additional logic if needed

Security and Data Privacy

HR information may include confidential employee records. Apply appropriate safeguards before deploying this workflow.

Store OAuth credentials and API keys in the n8n credential manager.
Never commit API keys, access tokens, passwords, or real employee records to GitHub.
Remove sensitive credential IDs, spreadsheet identifiers, internal instance metadata, and unnecessary webhook identifiers from public workflow exports.
Restrict spreadsheet access to authorized users.
Use aggregated HR metrics whenever individual-level data is unnecessary.
Validate requester identity and authorization before sharing confidential metrics.
Minimize the employee information sent to external AI services.
Review AI-generated responses before using them for sensitive or high-impact HR decisions.
Maintain appropriate audit logs while limiting the retention of sensitive email content.
Important: The supplied workflow JSON includes environment-specific identifiers. Sanitize the export before publishing it publicly.
imitations and Recommended Improvements

The current workflow provides a foundation for automated HR reporting. The following improvements would make it more reliable for production use.

1.      Data validation: Verify retrieved KPI values against the source spreadsheet.

2.      Accurate calculations: Standardize formulas for attrition, absenteeism, and other HR metrics.

3.      Controlled data retrieval: Configure explicit filters or a structured lookup method to retrieve relevant rows.

4.      Missing-data handling: Add fallback responses for unavailable metrics and empty results.

5.      Authorization controls: Restrict confidential KPI responses to approved requesters.

6.      Error handling: Add retry policies, failure notifications, and execution monitoring.

7.      Duplicate prevention: Track processed message IDs or implement an idempotency mechanism.

8.      Attachment processing: Add extraction and validation steps if attachments must be analyzed.

9.      Output validation: Confirm that reported figures and periods match the retrieved source data.

10.    Model compatibility: Confirm that the configured Anthropic model is supported by the active integration.
Business Benefits

Reduces repetitive manual reporting tasks.
Improves access to centralized HR analytics.
Speeds up delivery of routine workforce metrics.
Encourages consistent responses to common KPI requests.
Allows HR analysts to focus more on strategic workforce planning.
Creates a foundation for further reporting and analytics automation.
Actual benefits depend on data quality, request volume, workflow reliability, and the level of human review.
uture Enhancements

Potential extensions include:

Power BI, Tableau, or Looker Studio dashboard integration.
Scheduled weekly or monthly HR KPI summaries.
Historical trend comparisons and reporting-period analysis.
Slack or Microsoft Teams integration for authorized requests.
Role-based access controls for confidential reports.
Human approval for sensitive or high-impact reports.
Monitoring dashboards for workflow execution and response quality.
Conclusion

This project demonstrates how AI and workflow automation can streamline routine HR analytics reporting. By integrating Gmail, Google Sheets, an AI Agent, and Anthropic, it enables users to request workforce metrics through email and receive automated responses.

The solution provides a foundation for more efficient HR reporting, with data validation, authorization, error handling, and privacy controls recommended before production deploy
