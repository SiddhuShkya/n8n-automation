# Hands-on Exercise: Your First Workflow

### Overview

**Context:** You've just joined the n8n Academy. Your first task is to register your n8n instance with our training system by building a simple workflow that introduces itself to our servers. This is how we'll track your progress throughout the course.

**A Note on Credentials:** In this exercise, you'll use credentials for the first time. We're introducing them now because secure authentication is a best practice you should build into your workflows from the start. Don't worry if credentials feel unfamiliar, we'll cover them in full detail in Section 3. For now, just follow the steps and know that you're learning the right way to handle authentication.

## Learning Objectives Tested

| Skill | How It's Tested |
|---|---|
| Create a new workflow | Must create from scratch |
| Navigate the node panel | Must find and add specific nodes |
| Connect nodes properly | Trigger must connect to HTTP Request |
| Configure node parameters | Must set URL with query parameter |
| Name workflow following conventions | Grading checks workflow name in submission |
| Name nodes for self-documentation | Must rename nodes with clear names |
| Add tags to workflow | Must include required tag |
| Execute workflow manually | Must successfully call the endpoint |
| Understand node output | Must verify response data |

## Best Practice: Name Every Node

As your workflows grow, clear node naming becomes essential. A well-named workflow documents itself - anyone reading it can understand what each node does without opening it.

**Develop your own naming convention and use it consistently**. Here are some examples:

| Action | Prefix Examples |
|---|---|
| Getting data | Get, Fetch, Load |
| Setting/transforming data | Set, Transform, Format |
| Sending/submitting | Send, Submit, Post |
| Triggers | Trigger, Webhook, Schedule |

Why use camelCase or PascalCase? When you reference nodes in expressions later, consistent casing without spaces makes it easier:

- `$('SubmitRegistration').item.json` is cleaner than `$('HTTP Request').item.json`

Your convention is yours. The specific prefixes don't matter as much as consistency. Pick a pattern and stick with it across all your workflows.

# Exercise Instructions

## 📍 Step 1: Create Your Workflow

1. From the n8n home screen, click the Create Workflow button 
2. Name your workflow: Section 1 - Academy Registration
    - Click on "My workflow" at the top of the editor
    - Type the new name and press Enter
3. Add a tag to your workflow:
    - Click + Add Tag next to the workflow name
    - Create a new tag called n8n101
4. Save your workflow Ctrl/Cmd + S. It will auto save but its also good to get into the habit of saving manually. 

## 📍 Step 2: Add Your Trigger

1. Click Add first step or press N key to open the node panel by clicking the [+] button on the right.
2. Search for "Manual Trigger" it will be the first option
3. Select Manual Trigger to add it to your canvas
4. Rename it to TriggerManual (double-click the node title to rename)

**Why Manual Trigger?** For this exercise, you'll run the workflow yourself by clicking Execute. This is also useful during debugging. Later in the course, you'll learn about triggers that run automatically.

## 📍 Step 3: Set Up Authentication

To access the Academy training system, you need to authenticate your requests. This requires two things:

1. An API Key (shared by all students) - proves you're part of the course
2. Your Assessment ID (unique to you) - identifies who you are

We'll set up a credential for the API key, and add your Assessment ID as a header.

    1. Go to Credentials screen
    2. Click Create Credential
    3. Search for Header Auth
    4. Configure it:
        - Name: n8n Academy API Key
        - Header Name: X-API-Key
        - Header Value: pFT1jtDoVHAmVan$7eb6
    5. Save your workflow

## 📍 Step 4: Add the HTTP Request Node

1. Click the + on the right side of your TriggerManual node (or press Tab while the node is selected)
2. Search for "HTTP Request"
3. Select HTTP Request to add it and automatically connect it
4. Rename it to SubmitRegistration (double-click the node title to rename)

## 📍 Step 5: Configure the HTTP Request
Configure the SubmitRegistration node with these settings:

| Setting | Value |
|---|---|
| Method | GET |
| URL | https://learn.app.n8n.cloud/webhook/course/foundations/register |

**Add authentication:**

    1. Under Authentication, select Generic Credential Type
    2. Set Generic Auth Type to Header Auth
    3. Select your n8n Academy API Key credential

Now add your assessment ID as a query parameter:

1. Toggle Send Query Parameters to on.
2. Add the following to the first parameter: 
3. Set:
    - `Name:` assessment_id
    - `Value:` [Your Assessment ID] (find this below your name at the top of the page or in your Academy profile)

## 📍 Step 6: Add Workflow Information

We want to capture some information about your workflow in the request. Add two more query parameters: 

1. Click Add Query Parameter to add new parameters. 

| Name | Value |
|---|---|
| workflow_name | Section 1 - Academy Registration |
| tag | foundations-course |

**Pro Tip:** Later in the course, you'll learn how to pull this information dynamically using expressions. For now, typing it manually helps you understand what data you're sending. Be sure to copy and paste as we will check the exact text.

## 📍 Step 7: Execute Your Workflow

1. Make sure your workflow is saved (Ctrl/Cmd + S)
2. Click the Execute Workflow button (or press Ctrl/Cmd + Enter)
3. Watch the workflow execute - you should see green checkmarks on each node

## 📍 Step 8: Verify Your Results

1. Click on the SubmitRegistration node to see its output
2. You should see a response that includes:
    - Your assessment ID
    - A confirmation code
    - A congratulations message
    - A timestamp of when your request was received

**Example successful response:**

```json
{
    "status": "success",
    "assessment_id": "your-assessment-id",
    "confirmation_code": "N8N-2026-XXXX",
    "message": "Congratulations! You've successfully completed your first n8n workflow. Your registration has been recorded."
    "timestamp" : "2026-01-23T11:37:38.594+01:00"
}
```

## 📍 Step 9: Save Your Completed Workflow

Save your workflow one final time
Your submission has been automatically recorded when you executed the workflow


# Stretch Goal (Optional)

**Document your workflow for your future self**

Good automation builders always document their work. Practice this habit now:

1. Press Shift + S to add a Sticky Note to your canvas
2. Position it near your nodes
3. Write a brief description that includes:
    - What this workflow does
    - Your name and the date
    - Any notes for future reference
4. Sticky notes render Markdown so you can format your documentation as well

## Example:

```text
Academy Registration Workflow
Created by: [Your Name]
Date: [Today's Date]

This workflow registers my n8n instance with the
n8n Academy training system. It sends my assessment ID
and workflow info to the Academy API.

Part of: N8N101 Course, Section 1
```

`Why this matters:` When you return to a workflow weeks or months later, or when a teammate needs to understand what it does, documentation saves time and prevents confusion. Start this habit now - your future self will thank you.

# Grading Criteria

Your submission is automatically graded when you execute the workflow. We check:

| Criteria | Requirement |
|---|---|
| Request received | Your workflow successfully called our endpoint |
| Assessment ID present | Your Assessment ID was included in the request |
| Workflow name correct | The workflow_name parameter matches the required format |
| Tag included | The tag parameter contains foundations-course |

**Pass Requirement:** All four criteria must be met.

# Troubleshooting

- "I get an error when I execute"

    - Check that your URL is exactly correct (no typos)
    - Verify your assessment ID is correct (check your Academy profile)
    - Make sure you selected GET as the method

- "The node shows a red X"

    - Click on the node to see the error message
    - Common issues: incorrect URL, network connectivity, missing parameters

- "I don't see any output"

    - Make sure you clicked Execute Workflow, not just the play button on a single node
    - Check that your nodes are connected (there should be a line between them)

- "My confirmation code didn't appear"

    - Check that all query parameters are set correctly
    - Try executing again - the endpoint will return the same code for your student ID

# What You Just Learned

By completing this exercise, you've practiced:

- **Creating and naming workflows** following n8n conventions
- **Naming nodes** with clear, descriptive names for self-documenting workflows
- **Using tags** to organize your work
- **Adding and connecting nodes** on the canvas
- **Configuring node parameters** (URL, query parameters)
- **Executing workflows** manually and inspecting output
- **Reading node output** data to verify results

These fundamentals will be the foundation for everything you build in n8n.