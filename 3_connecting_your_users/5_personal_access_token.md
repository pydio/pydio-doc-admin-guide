@TODO



### Access the Personal Access Tokens administration interface

Browse to **Cells Console >> People >> (edit a user) >> Personal Access Tokens**.

[:image:3_connecting_your_users/pat/list_personal_access_token.png]

### Manage the Personal Access Tokens

You can view the status or/and remove a personal access token from this interface.

[:image:3_connecting_your_users/pat/manage_personal_access_token.png]

### Create a Personal Access Token

Hit the **+** and start creating a personal access token.

You will be presented with this interface, each field stands for:

- Label: name your token.
- Expiration Model: select one of the 2 modes.
  - Hard Limit: the token will be expired after the set time.
  - Auto Refresh: the token will be renewed if used before the set time.

[:image:3_connecting_your_users/pat/create_personal_access_token.png]


### Generate a Personal Access Token with the command line

Also available on **Cells Home**, you can generate a personal access token by using `./cells admin user token`.

```
TOKEN USAGE

  These token can be used in replacement of an OAuth2-based access token : they can replace the "Bearer" access
  token when calling any REST API. They can also be used as the password (in conjunction with username) for all
  basic-auth based APIs (e.g. webDAV).

TOKEN SCOPE

  By default, generated tokens grant the same level of access as a standard login operation. To improve security,
  it is possible to restrict these accesses to a specific file or folder (given it is accessible by the user in
  first place) with a "scope" in the format "node:NODE_UUID:PERMISSION" where PERMISSION string contains either "r"
  (read) or "w" (write) or both.

Usage:
  ./cells admin user token [flags]

Flags:
  -a, --auto string     Auto-refresh (number of seconds, see help)
  -e, --expire string   Expire after duration, format is 20u where u is a unit: s (second), (minute), h (hour), d(day).
  -h, --help            help for token
  -q, --quiet           Only return the newly created token value (typically useful in automation scripts with a short expiry time)
  -s, --scope strings   Optional scopes
  -u, --user string     User login (mandatory)
  ```