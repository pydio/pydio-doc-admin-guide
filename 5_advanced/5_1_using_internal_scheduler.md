In this chapter we will talk about the scheduler and how it enables you to program redudant tasks with ease.

### Using The internal Scheduler

The scheduler in Pydio give you the ability to schedule and execute tasks without having to intervene, the point is for instance, if you want to clear the recycle bin every first day of the month you will just have to configure a task, instead of launching a script or else to clear it.

![](/images/5_advanced/scheduler_interface.png)

The main interface looks like this :
Label | Owner | Trigger | Last Execution | Status
--- | --- | --- | --- | --- | ---
it's the name of the task(usually its goal) | who launched the task ( for system jobs it's usually the system user) | the type of trigger either manual, an event or periodic | the last time this task ran | it's current status ( for instance those tasks were finished)



### System jobs

System jobs are default jobs registered inside the application, they usually are handling tasks such as syncing the datasources, indexing and more.
Their main job is to make sure that your Pydio Cells in running healthy and smoothly, you dont want to intervene on those but if push comes to shove you can disable the jobs or on the other hand force them to run.

As seen in this screenshot ( we took the sync DataSource pydiods1 job) :

![](/images/5_advanced/scheduler_example.png)

You can **Run Now** which will force the task to run right now ,
**Disable job** this will disable the task therefore it will no longer run or 
you can **Enable multiple** and **Clear All** which clears the history.


### User Jobs

User jobs are usually task that where launched by users such as compression, share etc ...
They are created and launched when an user create an event like clicking on the button compression it will then run the task which will compress the file/folder.

### Create your own jobs

With Pydio you have the ability to create and therefore auto-run tasks of your choice, the configuration is pretty advanced and it's up to you to chose which suits you the best, right now it's not enabled but it's on our roadmap and you can expect it soon to be released.