As explained in the previous chapter, Security Policies are a set of rules that are evaluated at runtime to answer simple questions and return an "Allow" or "Deny" response.

## Comparators

Conditions are evaluated against various metadata (see next sections) using one of the following comparator:

| Comparator            | Description                                                                            | Value Example                                 |
|-----------------------|----------------------------------------------------------------------------------------|-----------------------------------------------|
| Boolean Value         | Evaluates value as a boolean (as Golang would with strconv.ParseBool)                  | `true`, `false`                               |
| String Matches        | Compares with value using a regular expression. Empty value always evaluates to false. | `localhost&#124;127.0.01`                     |
| String does not Match | Opposite of String Matches (empty values always evaluate to true)                      | `(?i)\.txt$`                                  |
| String equals         | Strict string equality                                                                 | `value`                                       |
| Glob Matcher          | Special Glob comparison only usable with FullPath field. See specific section below.   | `(ip)/path/to/folder/**`                      |
| Date Period           | Start/end dates in ISO8601 Format                                                      | `2018-02-01T00:00+0100/2018-04-01T00:00+0100` | 
| Date after            | Start date in ISO8601 Format                                                           | `2018-02-01T00:00+0100`                       |
| Office Hours          | Multiple recurring periods                                                             | Monday-Friday/09:00/18:30                     |
| CIDR Condition        | Network addresses comparisons                                                          | `192.168.0.1/16`                              |

### Glob Matcher

The Glob Matcher comparator is a specific manager performing "glob-like" comparison against files/folders paths. As such it's probably only useful in association with the FullPath field.

The expected pattern supports some specific flags and wildchars using the following format `([flags])/path/to/folder/[wildchars]`: 

 - Flags: 
   - `i` : for case-insensitive comparison
   - `p` : The p flag refers to 'parents'. It means that it applies similarly to all segments of the path
 - Wildchars: 
   - `*` : applies to all children first-level
   - `**` : applies to all children recursively

Using these flags, you can for example provide a Read/Write access only to a specific folder and everything below, by combining two rules: 

 - `(ip)datasource/path/to/folder => [R]` : open traversing (R) access to the folder's parents
 - `(i)datasource/path/to/folder{,/**} => [RW]` : allow creating any resource inside this folder and inside this folder chilren: braces are equivalent to `/folder` OR `/folder/**`

While the examples above match a fixed folder, Glob provides the ability to match a recurring pattern. A common use case can be to have "one folder per partner/client/collaborator" with a fixed structure of folders inside each, and to create rules to access only specific folders. Let's say you have

Master folder under `datasource/path` : 

 - ClientA
   - Commercial
   - Support
 - ClientB
   - Commercial
   - Support
 - ClientC
   - Commercial
   - Support
 - And so on... 

Create a profile that will only interact with the inner `Support` folders, the rules above can be changed to :

- `(ip)datasource/path/*/Support => [R]` : open traversing (R) access to this pattern (any clients)
- `(i)datasource/path/*/Support{,/**} => [RW]` : allow creating any resource inside `Support` and below


## Request Metadata

Every request sent to the server via Cells APIs carries a set of metadata that can be used in Conditions to restrict or authorize particular accesses. 

| Metadata       | Description                                                                                                                                                                                                  |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Server Host    | Hostname to which the user has connected. This is can be used in conjunction with cells "Sites" that allow Cells to bind on different network addresses, and grant an access depending on the Site accessed. |
| HTTP Method    | GET, PUT, POST, DELETE, etc.                                                                                                                                                                                 |
| Request URI    | If you specifically want to deny access to a certain API endpoint                                                                                                                                            |
| HTTP Protocol  | HTTP vs. HTTPS                                                                                                                                                                                               |
| User-Agent     | User-Agent header sent by Http client. Can be useful to prevent connection with Cells specific tools like Cells Client or Cells Sync, or with a specific Browser.                                            |
| Content-Type   | Content-Type header of the incoming request. Can be used to disable sync client access by excluding "application/json+grpc" requests.                                                                        |
| Cookies String | Cookie header sent by browser                                                                                                                                                                                |
| Time           | **Server** time when request is performed.                                                                                                                                                                   |

## Nodes Metadata

Many Cells REST API requests carry information about a specific "node", which is a file or folder, located in one of the authorized workspace. This node may or may not exist at the time of the request. 

Typically, an upload request will "PUT" an object to "/workspace/path/to/file.ext" that does not exist yet. On the other hand, a listing or stat request will generally point to an existing file that can expose its own advanced metadata. 

| Metadata           | Description                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------|
| Basename           | The basename of the node is the last segment of its path (e.g. file.ext)                        |
| Full Path          | Full path of the node inside the datasources global tree (.e.g pydiods1/path/to/file.ext)       |
| Node Type          | Whether it's a file (LEAF) or folder (COLLECTION).                                              |
| Extension          | Extension attached to file name                                                                 |
| Filesize           | Size in bytes, either from existing file, or from the Content-Length header for upload queries. |
| Modification Time  | Timestamp of the last modification date for an existing file.                                   |
| Custom Metadata... | Any other metadata that can be generated by Cells or by users (see below).                      |

Using `Custom Metadata` can be typically used to evaluate a condition against user-defined tags. For example, you can define a policy that hides any file tagged with a specific value (like "confidential"), or on the opposite _only show_ files that are tagged with a specific value (like "scanned-for-viruses")

Below are some condition examples: 

[:image:5_securing_your_data/rbac/security-policies-conditions-examples.png]