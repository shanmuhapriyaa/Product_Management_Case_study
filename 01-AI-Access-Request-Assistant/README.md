# AI-Powered Access Request Assistant

*A Product Management Case Study | AI Agents | Workflow Automation*

## 1. Overview

Access requests are a routine part of managing applications within an organization. However, submitting a request with the correct permissions is not always straightforward.

Consider a mid-sized company, ABC Corp, where employees use an internal application called XYZ. Access requests involve multiple configuration steps, and the available options in one step determine which options are permitted in the next.

Selecting an incompatible combination can result in ticket-processing errors. In other cases, a request may be processed successfully even though the selected permissions do not match the intended configuration.

I explored how AI could simplify this process by helping users understand what went wrong and how to correct their requests.

The result was a Python-based AI agent that analyses access-request tickets, checks permission combinations against predefined mappings, identifies configuration issues, and provides step-by-step corrective guidance.

## 2. The Problem

At ABC Corp, employees raise tickets whenever they need access to the XYZ application.

The request involves multiple configuration steps:

- Step 1: Initiate an access request.
- Step 2: Select the required access category or permission group.
- Step 3: Select the functions associated with the chosen option.

The available choices are interconnected. A selection in Step 2 determines which functions can be selected in Step 3.

For example, selecting Permission Group A may allow Functions A, B, and C, whereas Permission Group B may allow only Functions A and C.

This creates a challenge for users who are unfamiliar with the available permission combinations.

### Key problems identified

**1. Incorrect permission combinations**

Users may select a combination that is not permitted by the existing configuration. This can cause the ticket to generate an error during processing.

**2. Incorrect but successfully processed requests**

Some permission sets are technically valid and can be processed successfully. However, selecting a valid permission set does not necessarily mean the intended access configuration is correct.

**3. Difficulty understanding configuration errors**

Users may not know which selection caused the problem or what they should choose instead.

**4. Repeated investigation and corrections**

Identifying the incorrect selection and understanding the available alternatives can require additional effort and communication.

The underlying problem was not simply an error during ticket submission. It was the lack of clear, actionable guidance to help users understand and correct their access requests.

## 3. Understanding the Users

The solution considers two primary user groups.

### Access Requestor

An employee who needs access to an application but may not understand all the available permission configurations.

Their needs include:

- Understanding which selections are incorrect.
- Identifying the reason for a failed or incorrect request.
- Knowing what to change in each configuration step.
- Receiving clear instructions without having to investigate permission mappings manually.

### Support and Review Teams

People who help investigate access-request issues and assist users with corrections.

Their needs include:

- Quickly understanding the configuration issue.
- Reducing repetitive investigation.
- Providing consistent guidance.
- Spending less time explaining the same configuration rules repeatedly.

## 4. Problem Discovery and Business Analysis

My familiarity with the existing access-request process helped me understand the problem from a practical perspective.

Before developing the agent, I focused on understanding how the process worked and where users encountered difficulties.

My approach included:

- Examining previously raised access-request tickets and understanding their configuration details.
- Using my prior experience with the application to understand the different selection steps.
- Discussing the existing workflow and its challenges with the application product owner and business analyst.
- Understanding the relationships between available permission groups and their corresponding functions.
- Collaborating with a tester to validate the proposed solution against existing test scenarios.

These discussions and observations helped me identify an opportunity to make ticket analysis easier through AI-assisted guidance.

## 5. Proposed Solution

The proposed solution was an AI-powered Access Request Assistant.

The agent accepts an access-request ticket URL, retrieves the available ticket information through an API, and analyses the selected configurations against a predefined permission mapping.

Rather than simply returning an error, it explains the issue and provides instructions on how to correct the selections.

### Example Scenario

An employee at ABC Corp raises an access request for the XYZ application.

The request contains:

- Step 2: Permission Group B
- Step 3: Function D

However, the predefined permission mapping indicates that Permission Group B supports only Functions A and C.

The agent identifies the mismatch and provides guidance:

**Issue identified:** The selected function is not supported by the selected permission group.

**Recommended correction:**

1. Review the selected permission group in Step 2.
2. Check the functions supported by that permission group.
3. Select the appropriate permitted function in Step 3.
4. Raise a new access-request ticket with the corrected configuration.

The actual implementation uses the available permission and function names rather than generic labels such as A, B, or C.

This helps users understand both the problem and the next steps.

## 6. How the AI Agent Works

The current implementation follows a simple workflow.

### Step 1: Ticket URL Input

The user provides the access-request ticket URL through a custom AI agent in GitHub Copilot Chat within Visual Studio Code.

### Step 2: Ticket Information Retrieval

The agent uses a site-specific API connection to retrieve the available information associated with the ticket.

### Step 3: Permission Mapping Retrieval

The agent uses a locally stored CSV file containing the predefined relationships between the available Step 2 and Step 3 options.

The latest CSV can be downloaded from its centrally maintained online location when the reference mapping is updated.

### Step 4: Configuration Analysis

The agent analyses the ticket details and checks the selected options against the available permission mappings.

It identifies combinations that are not supported by the predefined configuration.

### Step 5: Issue Identification

When a mismatch is found, the agent explains which permission or function selection is incorrect.

### Step 6: Corrective Guidance

The agent generates a structured response using bullet points.

It identifies the relevant configuration step and explains what the user should select or change.

### Step 7: User Action

The user follows the instructions and raises a corrected ticket.

The agent provides guidance; it does not independently approve or grant access.

## 7. Technology and Architecture

| Component | Technology / Approach |
|---|---|
| Programming Language | Python |
| Development Environment | Visual Studio Code |
| Agent Interface | GitHub Copilot Chat |
| Model Selection | Automatic model selection through GitHub Copilot |
| Ticket Retrieval | Site-specific API integration |
| Permission Reference | Locally stored CSV |
| Configuration Validation | Step 2 and Step 3 mapping |
| Agent Output | Issue identification and corrective guidance |

### Reference Data Approach

A predefined CSV file contains the permitted relationships between the configuration options.

The file is maintained at an online location and can be updated independently.

The current implementation uses a locally downloaded copy of the file. When the reference mapping changes, the latest version must be downloaded before it can be used by the agent.

This keeps the reference mapping separate from the agent's instructions and allows the available configuration rules to be maintained independently.

## 8. Key Product Decisions

During development, I focused on making the solution practical and easy to use.

### API-Based Ticket Retrieval

Rather than depending on browser-based navigation to inspect the ticket, the agent retrieves ticket information through the available API.

This provides a direct way to work with the ticket data.

### Structured Permission Mapping

The agent uses the predefined CSV mapping as its reference for evaluating configuration combinations.

This is important because the available permission combinations are established rules, not something the AI should invent.

### Actionable Guidance Instead of Error Messages

Simply telling a user that a permission selection is incorrect does not solve the problem.

The agent therefore explains the mismatch and provides step-by-step instructions for correcting the request.

### Human-Controlled Access Requests

The agent does not make access approval decisions or grant permissions. The user remains responsible for correcting and resubmitting the request through the existing process.

## 9. Testing and Validation

The solution was evaluated in collaboration with a tester who used an existing automation script and her established test scenarios.

The testing focused on whether the agent could:

- Retrieve and interpret ticket information.
- Identify incorrect permission combinations.
- Follow the predefined CSV mapping.
- Explain the identified issue.
- Generate relevant corrective instructions.

During testing, some hallucinated and inaccurate responses were identified.

To improve reliability, I refined the agent's instructions to require justification for its conclusions and added an additional verification step against the permission mapping.

The agent is currently designed to follow the CSV mapping strictly. However, further improvements to response consistency and reliability remain necessary.

## 10. Measuring Success

The current implementation does not have formally measured business impact or quantified accuracy results.

However, I identified the following metrics as useful ways to evaluate the solution in a broader rollout.

| Metric | What it measures |
|---|---|
| First-Time-Right Submission Rate | Percentage of requests submitted without requiring configuration corrections |
| Ticket Resubmission Rate | Number of requests that need to be corrected and submitted again |
| Validation Accuracy | How accurately the agent identifies incorrect permission combinations |
| False Positive Rate | Frequency of valid configurations incorrectly flagged by the agent |
| Average Resolution Time | Time taken to identify and correct an access-request issue |
| User Feedback | Whether users find the explanations and corrective guidance useful |

These are proposed success metrics, not measured outcomes from the current implementation.

## 11. Limitations and Considerations

The solution has certain limitations that need to be considered.

- Its validation depends on the accuracy and completeness of the available CSV mapping.
- The latest reference file must be downloaded manually when the online mapping changes.
- AI-generated explanations may still require improvement and verification.
- The current implementation validates configuration combinations rather than determining permissions based on an employee's business role.
- The agent currently operates through GitHub Copilot Chat in Visual Studio Code rather than a dedicated chatbot interface.

These limitations provide opportunities for further product development.

## 12. Future Enhancements

### Dedicated Chatbot Interface

Developing a standalone chatbot could make the assistant accessible to employees without requiring them to use a development environment.

### Automatic Reference Data Synchronization

The agent could retrieve the latest approved CSV automatically, reducing the possibility of using outdated mapping information.

### Enhanced Validation

Additional deterministic validation and expanded test scenarios could improve the consistency of permission-mismatch identification.

### Business-Role-Based Guidance

A future version could incorporate approved business-role-to-permission mappings to provide more contextual recommendations.

This would be an additional capability beyond the current Step 2 and Step 3 configuration validation.

### Improved AI Reliability

Further work could focus on reducing hallucinations, improving explanation quality, and validating recommendations before presenting them to users.

## 13. My Contribution and Key Learnings

I worked on this solution from understanding the problem through implementation and validation.

My contributions included:

- Analysing existing access-request tickets and understanding the configuration workflow.
- Discussing the existing challenges with the application product owner and business analyst.
- Identifying opportunities to simplify the ticket investigation process.
- Developing a Python-based AI agent using GitHub Copilot in Visual Studio Code.
- Implementing API-based ticket information retrieval.
- Integrating CSV-based permission mappings into the agent's analysis.
- Designing the agent's responses to explain configuration issues and provide corrective instructions.
- Collaborating with a tester to validate the solution and refine its behaviour.

### Key Takeaway

One of my biggest learnings from this project was that building an AI solution is not just about making a model generate responses.

The real value comes from understanding the problem, identifying what information the solution needs, using reliable reference data, and ensuring the output actually helps users take the next step.

This experience strengthened my interest in AI Product Management and in building practical AI solutions that simplify everyday workflows.


Note: ABC Corp and XYZ are fictional examples. This case study presents a generalized, fictionalized account of a workflow problem and solution I tried for learning. Scenarios, and configuration details have been adapted for public portfolio purposes.
