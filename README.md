# Web Log Attack Hunter
 
**Project Type:** Web Security Monitoring and Threat Detection  
**Workflow Automation:** n8n  
**AI Model:** Google Gemini  
**Alerting Platform:** Telegram

## 📌 Project Overview

Web Log Attack Hunter is an automated security monitoring project designed to analyze web server access logs, detect suspicious activities, classify potential web application attacks, and send security alerts through Telegram.

The project combines regular expression-based detection, URL payload decoding, AI-assisted analysis, output validation, and automated notification to help identify potentially malicious web requests.

It focuses on three major web application attack categories:

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Path Traversal

## 🎯 Objectives

- Automate the initial analysis of web server access logs.
- Identify suspicious request patterns associated with common web attacks.
- Decode URL-encoded payloads to reveal hidden attack indicators.
- Use AI-assisted analysis to assess suspicious requests.
- Classify findings by verdict and severity.
- Validate AI-generated results before generating alerts.
- Send security notifications through Telegram.
- Provide fallback notifications when AI output fails validation.

## ❗ Problem Statement

Web applications frequently receive legitimate and suspicious HTTP requests. Manually examining large volumes of access logs can be time-consuming and may delay the identification of potential security threats.

Web Log Attack Hunter addresses this problem by automating the initial screening of access logs, highlighting suspicious requests, summarizing attack indicators, and delivering alerts to a configured Telegram chat.

The project supports security analysis and awareness but does not independently prove that an application has been compromised.

## 🛡️ Attack Categories

### 1. SQL Injection (SQLi)

Detects request patterns that may indicate attempts to manipulate database queries, access database information, or execute unauthorized database operations.

### 2. Cross-Site Scripting (XSS)

Identifies potentially malicious browser-oriented content, including suspicious script markup, event handlers, and JavaScript URL patterns.

### 3. Path Traversal

Identifies suspicious file paths and directory traversal patterns that may indicate attempts to access files outside an application's intended directory.

**Note:** Detection results indicate suspicious patterns. Further investigation is required to determine whether an attack was successful.

## ⚙️ Workflow Architecture

The project uses n8n to coordinate the following stages:

1. **Manual Trigger:** Starts the workflow execution.
2. **Sample Logs:** Supplies web access-log entries for analysis.
3. **Regex Tagger:** Parses log entries and identifies suspicious patterns associated with the supported attack categories.
4. **AI Agent:** Reviews the detected indicators and assesses the likely attack type, verdict, and severity.
5. **Google Gemini Chat Model:** Provides the AI model used by the agent.
6. **Payload Decoder:** Decodes URL-encoded request content and identifies additional attack indicators.
7. **Output Validation:** Checks whether the AI response follows the expected structure and permitted values.
8. **Retry Mechanism:** Allows another AI attempt when the output fails validation.
9. **Build Telegram Message:** Formats the findings into a security notification.
10. **Telegram Notification:** Sends the generated alert to the configured chat.
11. **Fallback Message:** Prepares an alternative notification if the AI response remains invalid after the configured retry.

## 🔄 How the Project Works

1. The workflow starts through the manual trigger.
2. Sample web access logs are supplied for analysis.
3. The log-processing stage examines recognizable entries and checks requests against predefined detection patterns.
4. URL decoding helps identify suspicious content that may be encoded.
5. Suspicious requests are grouped by attack category, and the source IP addresses associated with flagged requests are counted.
6. The AI agent reviews the findings and uses the payload decoder to assist its assessment.
7. The validation stage checks the AI response.
8. Valid results are formatted into a Telegram security alert.
9. If validation fails, the workflow follows its retry and fallback path.

## 📩 Telegram Security Alerts

The Telegram notification is designed to provide a concise summary of the analysis, including:

- Project identifier and execution reference.
- Execution timestamp in Indian Standard Time (IST).
- Security verdict and severity.
- Summary of the detected activity.
- SQL Injection detection count.
- XSS detection count.
- Path Traversal detection count.
- Top four source IP addresses by recorded suspicious-request count.

This notification format helps make the results easier to review without manually examining every processed log entry.

## 🧰 Technologies Used

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| JavaScript | Log processing, detection, decoding, and validation |
| Regular Expressions | Initial identification of suspicious request patterns |
| Google Gemini | AI-assisted security analysis |
| Telegram Bot | Automated security notifications |
| JSON | Workflow configuration and structured AI output |

## 📁 Repository Structure

| File | Description |
|---|---|
| `web log attack hunter.json` | Exported n8n workflow configuration |
| `sample.csv` | Sample web access-log data used for testing |
| `workflow.png` | Visual representation of the workflow architecture |
| `send msg.jpeg` | Screenshot of the Telegram security alert |
| `README.md` | Project documentation |

## ✅ Key Features

- Automated web access-log screening.
- Detection of SQL Injection, XSS, and Path Traversal indicators.
- URL-encoded payload inspection.
- AI-assisted threat assessment.
- Severity and verdict classification.
- Validation of AI-generated output.
- Retry and fallback handling.
- Automated Telegram alert generation.
- Summary of frequently flagged source IP addresses.

## 📊 Expected Benefits

- Reduces manual effort during initial log analysis.
- Helps identify suspicious web requests more consistently.
- Improves visibility into common web attack patterns.
- Makes encoded payloads easier to inspect.
- Provides readable summaries through Telegram.
- Demonstrates the integration of security monitoring, AI, and workflow automation.

## ⚠️ Scope and Limitations

- The supplied workflow uses sample logs; a live production log source is not established by the repository files alone.
- Pattern-based detection may produce false positives or miss attacks that do not match the configured rules.
- AI-generated verdicts and severity ratings require verification.
- Source IP counts represent activity in the analyzed input and do not establish the identity of an attacker.
- The workflow detects and reports suspicious activity; it does not automatically block attacks or remediate vulnerabilities.
- The exported workflow is marked inactive and requires configuration before operational use.
- Telegram credentials and a valid destination chat ID must be configured before notifications can be delivered.
- The workflow should be tested with benign and suspicious inputs before being used for security monitoring.

## 🚀 Setup Overview

1. Import the workflow JSON file into n8n.
2. Review the workflow nodes and their connections.
3. Configure the required Google Gemini credentials.
4. Configure Telegram bot credentials and the destination chat ID.
5. Review the sample log data.
6. Execute the workflow manually.
7. Inspect the detection, AI analysis, and validation results.
8. Confirm that the Telegram notification is delivered correctly.
9. Test the workflow with authorized test data before activating it.

## 🔐 Security and Responsible Use

Use this project only with logs from systems you own or are authorized to analyze.

Treat access logs, IP addresses, request paths, and other potentially sensitive information carefully. Ensure that sending log data to an external AI service complies with applicable security and privacy requirements.

This project is intended for educational demonstrations, defensive security monitoring, and authorized security testing.

## 📝 Conclusion

Web Log Attack Hunter demonstrates an automated approach to identifying suspicious web access-log entries and notifying analysts of potential security threats.

By combining pattern-based detection, payload decoding, AI-assisted analysis, output validation, and Telegram integration, the project provides a practical example of security workflow automation.

It serves as a foundation for further improvements in log monitoring, detection accuracy, alert management, and security operations.
