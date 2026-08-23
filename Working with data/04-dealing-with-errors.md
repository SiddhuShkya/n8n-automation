# Checking failed workflows

When a workflow fails, n8n provides tools to help you figure out what went wrong. Every workflow execution can be recorded in the Executions log, which you can access from the left panel in the Editor UI or by switching to the Executions tab when a workflow is open.

The Executions log shows:

- The workflow name
- When the execution started
- The status (Succeeded, Failed, Running, 
- Waiting, or Cancelled)
- How long the execution took
- The execution ID

## Saving successful executions

By default, n8n only saves failed executions to the log. Successful executions are not saved, which means you won't see them in the Executions list unless you change this setting. To enable it, open Workflow Settings and set Save Successful Production Executions to Yes. This is useful when you want to verify that a workflow ran correctly or need to inspect the output data from a past execution.

## Inspecting failed executions

When you select a failed execution from the log, n8n opens a read-only view of the workflow showing exactly where the error occurred. Failed nodes are highlighted in red, and you can click on them to see the error details including the error message and the data that was being processed at the time of failure.

Use the Editor | Executions toggle at the top of the screen to switch between editing your workflow and reviewing past executions. This lets you move quickly between investigating a failure and fixing the problem.

<img src="../screenshots/failed-execution.png" alt="image" width="100%"/>

## Debugging failed executions

Once you have identified a failed execution, you can copy it into the editor for debugging. Click the Debug in editor button on a failed execution and n8n will load that execution's data into the workflow editor with all the node outputs pinned. This means you can re-run individual nodes or the entire workflow using the exact same data that caused the failure, without needing to trigger a new execution from scratch. Fix the problem, test it against the pinned data, and then unpublish the pinned data when you are satisfied the issue is resolved.

<img src="../screenshots/debug-failed-exec.png" alt="image" width="100%"/>

# Catching and handling errors