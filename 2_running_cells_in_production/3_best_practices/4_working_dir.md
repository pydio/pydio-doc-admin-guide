
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




By default, application data is stored under the standard OS application dir:

- Linux: ${USER_HOME}/.config/pydio/cells
- Darwin: ${USER_HOME}/Library/Application Support/Pydio/cells
- Windows: ${USER_HOME}/ApplicationData/Roaming/Pydio/cells

You can customize the various storage locations with the following ENV variables:

- CELLS_WORKING_DIR: replace the whole standard application dir
- CELLS_DATA_DIR: replace the location for storing default datasources (default CELLS_WORKING_DIR/data)
- CELLS_LOG_DIR: replace the location for storing logs (default CELLS_WORKING_DIR/logs)
- CELLS_SERVICES_DIR: replace location for services-specific data (default CELLS_WORKING_DIR/services)

When running in production mode, we generally advise to setup at least CELLS_WORKING_DIR to a standard Linux layout folder, typically `/var/cells`.
