<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v1. Looking for <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells v2 docs?</a></span>
</div>

In this section, we introduce the scheduler and show how you can optimize your workflow by scripting redundant tasks.

### Using The internal Scheduler

The scheduler in Pydio Cells let you define task that are to be done and program when they are to be executed, directly via the web interface.
For instance, you might want to configure a task to clear the recycle bin every first day of the month, instead of launching a script or else to clear it.

The main interface looks like this:

[:image-popup:5_advanced/scheduler_interface.png]

Label | Owner | Trigger | Last Execution | Status
--- | --- | --- | --- | ---
Human friendly name of the task (usually its goal) | Who launched the task ( for system jobs it's usually the system user) | Type of the trigger: manual, on-event or periodic | Last time it ran | Current status (in the figure above, all tasks were done)

### System jobs

System jobs are default jobs registered inside the application. They usually handle tasks such as syncing the datasources, indexing and more.
They mainly make sure that your Pydio Cells instance is running healthily. You do not want to intervene on those. Yet, you can disable the jobs or force them to run on demand.

As seen in this screenshot (we took the sync DataSource pydiods1 job) :

[:image-popup:5_advanced/scheduler_example.png]

You can:

- **Run Now**: force the task to run right now
- **Disable job**: disable the task that will therefore no longer run
- **Enable multiple** or **Clear All** which clears the history.

**You can also click on any job to have an history of it's events such as errors.**

### User Jobs

User jobs are usually tasks launched by users such as compression, share etc ...
They are created and launched asynchronously to keep end user experience fluid and smooth when executing heavy tasks without blocking the UI. Typically, when uploading or compressing large files or folders.

You can interact with user jobs in a similar manner as you do with system jobs, see above.

### Create your own jobs

With Pydio you can define, configure and schedule (and therefore auto-run) the task of your choice.  
This feature is not yet enabled but it's on our roadmap and you can expect it to be released soon.
