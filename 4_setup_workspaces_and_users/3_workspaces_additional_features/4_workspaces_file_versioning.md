<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

### Introduction
Files versioning allows you to keep track of file changes.

Disclaimer : If you installed Pydio by the linux repository ( apt / yum ), you must install the "pydio-plugin-meta.git" package.

The current implementation is based on Git for the sake of simplicity, and it has some drawbacks :

+ Only works for FS-based workspaces
+ Git is known for not very well scaling for huge files. Thus if you are planning to use only media files for example (videos), you should probably avoid this.
A part of that, it's working neatly, and implementing versioning through a git repository also allows you to interact with the versioning system through external application.

[:image-popup:4_setup_workspaces_and_users/workspaces_versioning.png]

### Installing
First enable this plugin go to **All Plugins > Available Plugins > Workspaces Meta Aspects > Git-based Versioning**.

To enable this feature go to **Workspaces & Users > Workspaces > Choose a workspace and edit it** and **Git-based Versioning** to the workspace.

Once installed, you will be able to see a list of actions of what happened on a given file, eventually download a given previous revision, or directly reverse the current revision to a previous one (thus creating a new revision). To install, simply make sure that the **_GIT_** command is accessible through the command-line on your server, and that the **_PEAR/VersionControl_Git_** client is installed as well.

Previous implementation was based on SVN instead of Git. As they share some client resources (history browser), the meta.svn plugin must be installed as well (it is by default).

### Troubleshooting
When switching for the first time to this workspace and making a file operation (upload, move, whatever), a "git init" operation will be launched. If you encounter strange errors, maybe you can manually initiate the workspace by applying the following command line at the root of your workspace path :

`$ git init .`

`$ git add .`

`$ git commit -a -m "Initial commit"`

### Alternatives
Other versioning systems should be introduced in future versions, either leveraging specific access drivers capacities (cloud storage generally directly provide versioning, accessible through an API), or by storing diffs and version in an additional database.
