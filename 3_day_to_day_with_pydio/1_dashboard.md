The dashboard is the main page of Pydio Enterprise. This page allows you to quickly know what is going on your server.
You can easily change the way you want this page by adding, deleting and reordering widgets.

## Dashboard Builder

You can access Dashboard Builder by clicking on the bottom-right orange button.
This panel allows you to create/remove plugins.
I you want to get back the default settings, you can reset the dashboard layout.

Each Widget you add should be configured.

It's also possible to reorder the widget, just drag and drop the wanted widget.

[:image-popup:3_day_to_day_with_pydio/dashboard_builder.png]

## Widget types

When you add a widgets, you'll have some settings to fill depending the type.
For some widgets, you can apply a filter on a selected workspace.
It is also possible to filter the data by file extension (for example you just want to monitor downloads on .doc files).

Here are the different widgets attributes:

- Legend / Title
- Data type (Downloads, Uploads, ...)
- Sampling frequency
  - daily (last 10 days)
  - weekly (last 4 weeks)
  - monthly (last 12 months)
- Workspace filter [optional]
- Filename filter [optional]
  - "*.doc" (will monitor only .doc files)
  - "prefix*" (will monitor only files with this prefix)
- Refresh delay

### Small graph:
[:image-popup:3_day_to_day_with_pydio/dashboard_small_graph.png]

### Big graph:
[:image-popup:3_day_to_day_with_pydio/dashboard_big_graph.png]

###  Most active:
[:image-popup:3_day_to_day_with_pydio/dashboard_most_active.png]

### Recents events:
[:image-popup:3_day_to_day_with_pydio/dashboard_recent_events.png]

### Server status:
[:image-popup:3_day_to_day_with_pydio/dashboard_server_status.png]

### Quick access:
[:image-popup:3_day_to_day_with_pydio/dashboard_quick_access.png]

## Example

Let's say we want to create a small graph to monitor the ".docx" downloads daily on a selected workspace:
[:image-popup:3_day_to_day_with_pydio/dashboard_example.png]

- Click on the button to add a widget
- Choose Small Graph Widget
- Fill the legend with the name you want
- Select Downloads in the Chart combobox
- Pick Daily as sampling frequency
- Choose the workspace you want to filter
- Type "*.docx" to filter only ".docx" files
- Change the Refresh interval if you want
- Click on create widget

Here is the resulting widget:
[:image-popup:3_day_to_day_with_pydio/dashboard_example_result.png]
