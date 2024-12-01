# Enhanced Self-Healing Workflow for Kestra
## 🌟 Introduction
In complex systems, workflows are bound to fail occasionally. Whether due to transient issues like network timeouts or persistent bugs, handling these failures automatically can significantly improve system resilience.

This project is a self-healing workflow built with Kestra. It monitors and manages other workflows within Kestra, automatically identifying and addressing errors, retrying operations, and notifying developers when manual intervention is required.

**With this workflow, you can:**

- Detect and classify errors in real-time.
- Retry tasks for recoverable (transient) errors.
- Notify stakeholders about critical (persistent) issues.
- Generate logs and snapshots for analysis.
- Stay informed with Slack notifications and email alerts.

## 💡 Core Features
1. Monitor Other Workflows
The workflow begins by monitoring the execution of a target workflow using its ID. It fetches details such as execution status, errors, and logs to track progress.

     **Task:** `monitor-workflows`

    **Description:** Monitors a specified Kestra workflow and stores its execution details.

2. Validate Data Consistency
It checks if the monitoring task outputs critical data like workflow status and error messages. This ensures no incomplete data propagates through the pipeline.

    **Task:** `validate-output`

    **Description:** Validates that workflow monitoring outputs include all necessary information.

3. Analyze Errors
Using Python, the workflow categorizes errors into:

    **Transient Errors:** Short-term issues like network hiccups.

    **Persistent Errors:** Critical, system-level bugs requiring manual action.

    **Unknown Errors:** Errors that don’t match known patterns.

    **No Errors:** Successful execution.

    **Task:** `analyze-error`

    **Description:** Identifies the type of error and outputs it for classification.

4. Classify and Handle Errors
Depending on the error type, the workflow takes appropriate action:

    **Transient Errors:** Retries the failed task after a delay.

    **Persistent Errors:** Sends Slack notifications and logs error details for manual intervention.

    **Unknown Errors:** Sends email alerts for further analysis.
    **Task:** `handle-error`

    **Description:** Uses conditional branching to execute recovery or alert actions based on error type.

5. Capture Failure Snapshots
For every failure, it logs the workflow’s status, error messages, and execution logs into a file. These snapshots help developers debug and improve the system.

    **Task:** `capture-snapshot`

    **Description:** Captures and saves the current state of the monitored workflow during failures.

6. Summarize Results
Once the self-healing workflow completes, it sends a summary notification to Slack with the workflow status and snapshot details.

    **Task:** `send-summary`

    **Description:** Notifies stakeholders with an execution summary for transparency.

7. Global Error Handling
If the self-healing workflow itself encounters a critical failure, an email alert is sent to administrators for immediate action.

    **Error Task:** `notify-on-critical-error`

    **Description:** Sends an email on critical workflow errors.

## 🚀 How to Run
### Prerequisites
Kestra Installation

Install Kestra via Docker:
```
docker run -p 8080:8080 kestra/kestra:latest 
``` 
Verify that Kestra is running by visiting http://localhost:8080.

### Slack and Email Setup

Configure a Slack webhook and add the URL as a secret in Kestra.
Ensure email notifications are configured with valid SMTP credentials.

### GitHub Repository

Clone this repository:
```
git clone <repository-url>  
```

### Running the Workflow
Navigate to the Kestra UI and upload the YAML file (enhanced-self-healing-workflow.yml).
Provide the monitored_workflow ID as an input to start monitoring a specific workflow.
Trigger the workflow manually 

## 🧪 Demo Video
Video link : 