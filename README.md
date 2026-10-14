**Web Log Attack Hunter

Project Type: Security Monitoring and Web Log Threat Detection
Workflow Platform: n8n
AI Analysis: Google Gemini
Alerting Channel: Telegram
1. Project Overview
Web Log Attack Hunter is an automated workflow designed to inspect web
server access logs, identify requests that may indicate common web
application attacks, use AI-assisted analysis to assess suspicious
activity, and send a concise security alert through Telegram.
The project focuses on three attack categories: SQL Injection
(SQLi), Cross-Site Scripting (XSS), and Path Traversal. It
combines pattern-based screening with a decoding and AI review stage to
make the results easier for a security analyst to interpret.
The repository contains the n8n workflow export, sample web access logs,
a workflow diagram, and an example of the Telegram alert output.
2. Objective
The main objective is to demonstrate how a repeatable
security-monitoring workflow can help a security analyst:
- Screen web access logs for suspicious request patterns.
- Identify indicators associated with SQL injection, XSS, and path
  traversal.
- Decode URL-encoded request content to help reveal concealed
  payloads.
- Use an AI agent to review flagged requests and provide a verdict and
  severity rating.
- Validate the AI response before using it in an alert.
- Deliver a compact summary of findings to Telegram.
- Provide a fallback notification if the AI response does not meet the
  expected format after a retry.
3. Problem Statement
Web applications can receive large numbers of legitimate and malicious
requests. Manually reviewing every access-log entry can be
time-consuming, and suspicious payloads may be URL-encoded or disguised.
Analysts need a repeatable way to highlight potentially harmful
requests, summarize the indicators, and receive timely notifications.
This project demonstrates a workflow-based approach to that problem. It
is a detection and notification aid; it does not replace a full Security
Information and Event Management (SIEM) platform, an intrusion
prevention system, or a human investigation.
4. Attack Categories Covered
SQL Injection (SQLi)
The workflow looks for request patterns commonly associated with
attempts to manipulate database queries, such as suspicious SQL
expressions, union-based selection patterns, database metadata
references, time-delay functions, and destructive database commands.
Cross-Site Scripting (XSS)
The workflow looks for potentially unsafe browser-oriented content in
request parameters, including script-related markup, SVG or image event
handlers, and JavaScript URL patterns.
Path Traversal
The workflow looks for path-manipulation patterns that may attempt to
access files outside an intended directory. Examples in the supplied
test data include traversal sequences and references to sensitive
operating-system files.
These detections are based on patterns and indicators. A matching
request is evidence of suspicious activity, but it does not by itself
prove that the target application was exploited or that data was
accessed.
5. Workflow Architecture
The workflow is organized into the following stages:
1. Manual Trigger --- starts a test or demonstration execution.
2. Sample Logs --- supplies the web access-log entries to be
   examined.
3. Regex Tagger --- parses recognizable log entries, checks request
   paths and decoded content against detection patterns, counts
   indicators by category, and summarizes the most frequently flagged
   source IP addresses.
4. AI Agent --- reviews the pre-tagged findings and is instructed
   to assess the attack types, decide whether the activity is
   confirmed, suspicious, or benign, and assign a severity.
5. Google Gemini Chat Model --- provides the language model used by
   the AI agent.
6. Payload Decoder Tool --- decodes URL-encoded content and reports
   attack indicators, encoding depth, possible automated-tool
   signatures, and a severity hint.
7. Output Validation --- checks whether the AI result follows the
   expected structure and allowed values.
8. Decision and Retry --- sends valid results forward or allows a
   retry when the result fails validation.
9. Build Telegram Message --- formats the verdict, severity,
   summary, attack-category counts, and top source IPs into a
   notification.
10. Telegram --- sends the alert to the configured Telegram chat.
11. Fallback Message --- prepares a notification stating that the AI
    result could not be verified if validation still fails after the
    configured retry path.
The workflow therefore combines deterministic pattern matching with
AI-assisted interpretation and output validation before notification.
6. How the Project Works
When an execution is started, the workflow reads the supplied sample log
entries. It attempts to parse each entry and inspect the request path
for known indicators. URL decoding helps expose payloads that may not be
obvious in their encoded form.
Suspicious entries are grouped by detected category. The workflow also
records source IP addresses associated with flagged requests and ranks
them by the number of detections in the processed sample.
The AI agent is then given a summary of the screening results and
suspicious requests. It is instructed to use the payload-decoding tool,
assess the likely attack category, and return a structured verdict and
severity. The validation stage checks the response format and permitted
values. If validation succeeds, the workflow prepares a Telegram alert.
If validation fails, the workflow follows its retry path and can send a
fallback alert instead.
7. Alert Contents
The Telegram message is designed to include:
- Project identifier and execution reference.
- Timestamp in Indian Standard Time (IST).
- Verdict and severity assessment.
- A short summary of the finding.
- Counts for SQL injection, XSS, and path traversal indicators.
- The four source IP addresses with the highest recorded hit counts in
  the processed sample.
These values are generated from the workflow execution. They should be
interpreted in the context of the specific logs supplied to that
execution.
8. Repository Contents
  File                                Purpose
  web log attack hunter.json        Exported n8n workflow definition,
                                      including nodes, connections, and
                                      workflow settings.
  sample.csv                        Sample web access-log data used to
                                      demonstrate detection. Despite the
                                      filename, the supplied content is a
                                      block of raw log entries rather
                                      than a conventional comma-separated
                                      table.
  workflow.png                      Visual representation of the
                                      workflow stages and their
                                      connections.
  send msg.jpeg                     Example screenshot showing the
                                  Telegram security notification.
9. Tools and Technologies
- n8n: Visual workflow automation and orchestration.
- JavaScript within n8n Code nodes: Log parsing, pattern
  screening, payload decoding, result preparation, and validation.
- Google Gemini: AI-assisted review of suspicious log findings.
- Telegram Bot integration: Delivery of security notifications.
- Regular expressions: Initial identification of known suspicious
  request patterns.
10. Expected Benefits
- Reduces the need to manually scan every line in a small test log.
- Applies the same initial detection patterns consistently.
- Makes some encoded payload indicators easier to inspect.
- Produces a compact alert for quicker review.
- Separates initial pattern detection from AI-assisted assessment.
- Checks AI output before using it in the normal alert path.
- Demonstrates a practical integration of log analysis, AI,
  validation, and messaging.
11. Scope and Limitations
- The supplied workflow uses sample log data for demonstration; the
  repository does not, by itself, establish that a live production log
  source is connected.
- Pattern matching can produce false positives and can miss attacks
  that do not match the configured patterns.
- AI-generated verdicts and severity ratings are assessments, not
  proof of successful exploitation.
- The source IP ranking reflects flagged requests in the processed
  input, not a verified attribution of an attacker.
- The workflow does not itself block requests, patch an application,
  investigate a host, or confirm data exfiltration.
- The supplied workflow export is marked inactive. It must be
  configured and activated in the intended n8n environment before it
  can run as an operational workflow.
- The Telegram node contains a placeholder chat identifier. A valid
  destination and appropriate Telegram credentials must be configured
  before notifications can be delivered.
- The sample data and screenshot illustrate a test run. Results can
  differ when the input logs, configuration, model response, or
  execution settings change.
12. Setup Overview
To prepare the workflow for a demonstration or test environment:
1. Import the workflow definition into n8n.
2. Review the workflow nodes and connections.
3. Configure the Google Gemini credentials required by the AI model
   node.
4. Configure Telegram bot credentials and replace the placeholder chat
   identifier with the intended destination.
5. Review the supplied sample logs and use authorized test data.
6. Execute the workflow manually and inspect the node outputs.
7. Confirm that the Telegram destination receives the expected
   notification.
8. Validate the detections against known benign and suspicious test
   cases before relying on the workflow for monitoring.
9. Activate the workflow only after the required configuration and
   testing are complete.
Exact configuration steps may vary with the installed n8n version and
the credentials available in the environment.
13. Security and Responsible Use
Use this project only with web logs that you own or are authorized to
analyze. Treat IP addresses, request paths, user identifiers, and other
log details as potentially sensitive information. Avoid sending
confidential production data to external AI services unless your
organization's data-handling policies permit it.
The project is intended for defensive monitoring, educational
demonstrations, and authorized security testing.
14. Conclusion
Web Log Attack Hunter demonstrates an end-to-end security alerting
workflow: sample log intake, pattern-based screening, payload decoding,
AI-assisted assessment, response validation, and Telegram notification.
Its purpose is to help highlight suspicious web requests and summarize
them in a readable format for further investigation.
The workflow should be treated as a prototype or demonstration until its
credentials, alert destination, data source, detection quality, and
operational behavior have been configured and tested for the intended
environment.**
