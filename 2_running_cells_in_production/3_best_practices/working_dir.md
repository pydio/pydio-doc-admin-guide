By default, application data is stored under the standard OS application dir:

Linux: ${USER_HOME}/.config/pydio/cells
Darwin: ${USER_HOME}/Library/Application Support/Pydio/cells
Windows: ${USER_HOME}/ApplicationData/Roaming/Pydio/cells
You can customize the various storage locations with the following ENV variables:

CELLS_WORKING_DIR: replace the whole standard application dir
CELLS_DATA_DIR: replace the location for storing default datasources (default CELLS_WORKING_DIR/data)
CELLS_LOG_DIR: replace the location for storing logs (default CELLS_WORKING_DIR/logs)
CELLS_SERVICES_DIR: replace location for services-specific data (default CELLS_WORKING_DIR/services)
