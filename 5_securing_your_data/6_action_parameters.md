
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>

In this section, we show how actions and parameters can help you micro manage every single thing about users/groups/roles, such as enabling/disabling an action from the **right-click menu**.

### Parameters and actions

Basically actions and parameters if chosen correctly can save you lot's of time and give you the custom experience that you wanted. You can control everything from for instance the buttons displaying and the action that users/groups are allowed to use, to even using the plugins such as the mailer, actions like compressing a file and more.

To do so go and edit your `user / group / role` and go to **Application Parameters** section:

- For instance let's click on **Add for** and select  `All Workspaces`:

[:image:5_securing_your_data/actions_parameters/action_parameters_1.png]

[:image:5_securing_your_data/actions_parameters/action_parameters_2.png]

[:image:5_securing_your_data/actions_parameters/action_parameters_3.png]

This is a list of every actions and parameters that you can configure.

#### Actions

With actions, you can either restrict particular actions or if it was already restricted, allow it for a user that was not supposed to.

As seen in this screen capture:

[:image:5_securing_your_data/actions_parameters/action_parameters_example_1.png]

[:image:5_securing_your_data/actions_parameters/action_parameters_example_2.png]

For instance in this screenshot, the action "create new Cells" was disabled for everyone, therefore I wished to have a user (in this case Admin) to have the action available and be able to create Cells.

You have a lot of choices but the principle is easy.
You can apply this for groups, roles. 
Choose the action and what it is targeting (for instance it could be the **core.conf**, and then you have a slider allowing to enable or disable it.

The choices are multiple and it is up to you to customize it.

#### Parameters

With **Parameters**, you can control the parameters affecting one workspace or many workspaces at once and override them, for instance you can override the default quotas of the workspace and more.

There is lots of parameters and customization available you can put specific values or on the other hand enable/disable them.