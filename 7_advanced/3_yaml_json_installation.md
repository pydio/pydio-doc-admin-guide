As explained in the Cells Installation, a core set of config must be set using the `cells configure` command before being able to run Cells for the first time. As this command is interactive (either via CLI or Browser), it may be blocking an automatic deployment of Cells.

## Running 'configure' non-interactive

The `cells configure` command can be run in a non-interactive mode by passing all expected values inside a json or yaml file. Basically, all the questions asked during the interactive mode are filling a model that can be unmarshalled directly using the correct keys.

Given a correct YAML file (see below for keys), you can run 

    cells configure --yaml /path/to/install-file.yaml

or in the same way 

    cells configure --json /path/to/install-file.json

or even with an ENV variable

    export CELLS_INSTALL_YAML=/path/to/install-file.yaml
    cells configure

The latter is typically used in our Docker image where the YAML is mounted as an internal file in a volume, and the ENV passes the path to the yaml. 

## Installation Models

Models used are defined as protobuf, as described below. Each field declares its json key, YAML key is the **lowercase** version of the key.

### InstallConfig proto

```
// All DB Connection informations
DbConnectionType        string `protobuf:"bytes,1,opt,name=dbConnectionType,proto3" json:"dbConnectionType,omitempty"`
DbTCPHostname           string `protobuf:"bytes,2,opt,name=dbTCPHostname,proto3" json:"dbTCPHostname,omitempty"`
DbTCPPort               string `protobuf:"bytes,3,opt,name=dbTCPPort,proto3" json:"dbTCPPort,omitempty"`
DbTCPName               string `protobuf:"bytes,4,opt,name=dbTCPName,proto3" json:"dbTCPName,omitempty"`
DbTCPUser               string `protobuf:"bytes,5,opt,name=dbTCPUser,proto3" json:"dbTCPUser,omitempty"`
DbTCPPassword           string `protobuf:"bytes,6,opt,name=dbTCPPassword,proto3" json:"dbTCPPassword,omitempty"`
DbSocketFile            string `protobuf:"bytes,7,opt,name=dbSocketFile,proto3" json:"dbSocketFile,omitempty"`
DbSocketName            string `protobuf:"bytes,8,opt,name=dbSocketName,proto3" json:"dbSocketName,omitempty"`
DbSocketUser            string `protobuf:"bytes,9,opt,name=dbSocketUser,proto3" json:"dbSocketUser,omitempty"`
DbSocketPassword        string `protobuf:"bytes,10,opt,name=dbSocketPassword,proto3" json:"dbSocketPassword,omitempty"`
DbManualDSN             string `protobuf:"bytes,11,opt,name=dbManualDSN,proto3" json:"dbManualDSN,omitempty"`
DbUseDefaults           bool   `protobuf:"varint,37,opt,name=dbUseDefaults,proto3" json:"dbUseDefaults,omitempty"`

// NoSQL Document store (mongo)
DocumentsDSN             string         `protobuf:"bytes,38,opt,name=DocumentsDSN,proto3" json:"DocumentsDSN,omitempty"`
UseDocumentsDSN          bool           `protobuf:"varint,39,opt,name=UseDocumentsDSN,proto3" json:"UseDocumentsDSN,omitempty"`

// Datasources informations
DsName                   string         `protobuf:"bytes,12,opt,name=dsName,proto3" json:"dsName,omitempty"`
DsPort                   string         `protobuf:"bytes,13,opt,name=dsPort,proto3" json:"dsPort,omitempty"`
DsType                   string         `protobuf:"bytes,15,opt,name=dsType,proto3" json:"dsType,omitempty"`
DsS3Custom               string         `protobuf:"bytes,16,opt,name=dsS3Custom,proto3" json:"dsS3Custom,omitempty"`
DsS3CustomRegion         string         `protobuf:"bytes,17,opt,name=dsS3CustomRegion,proto3" json:"dsS3CustomRegion,omitempty"`
DsS3ApiKey               string         `protobuf:"bytes,18,opt,name=dsS3ApiKey,proto3" json:"dsS3ApiKey,omitempty"`
DsS3ApiSecret            string         `protobuf:"bytes,19,opt,name=dsS3ApiSecret,proto3" json:"dsS3ApiSecret,omitempty"`
DsS3BucketDefault        string         `protobuf:"bytes,20,opt,name=dsS3BucketDefault,proto3" json:"dsS3BucketDefault,omitempty"`
DsS3BucketPersonal       string         `protobuf:"bytes,21,opt,name=dsS3BucketPersonal,proto3" json:"dsS3BucketPersonal,omitempty"`
DsS3BucketCells          string         `protobuf:"bytes,22,opt,name=dsS3BucketCells,proto3" json:"dsS3BucketCells,omitempty"`
DsS3BucketBinaries       string         `protobuf:"bytes,23,opt,name=dsS3BucketBinaries,proto3" json:"dsS3BucketBinaries,omitempty"`
DsS3BucketThumbs         string         `protobuf:"bytes,35,opt,name=dsS3BucketThumbs,proto3" json:"dsS3BucketThumbs,omitempty"`
DsS3BucketVersions       string         `protobuf:"bytes,36,opt,name=dsS3BucketVersions,proto3" json:"dsS3BucketVersions,omitempty"`
DsFolder                 string         `protobuf:"bytes,14,opt,name=dsFolder,proto3" json:"dsFolder,omitempty"`

// Additional Frontend configuration
FrontendHosts            string         `protobuf:"bytes,24,opt,name=frontendHosts,proto3" json:"frontendHosts,omitempty"`
FrontendLogin            string         `protobuf:"bytes,25,opt,name=frontendLogin,proto3" json:"frontendLogin,omitempty"`
FrontendPassword         string         `protobuf:"bytes,26,opt,name=frontendPassword,proto3" json:"frontendPassword,omitempty"`
FrontendRepeatPassword   string         `protobuf:"bytes,27,opt,name=frontendRepeatPassword,proto3" json:"frontendRepeatPassword,omitempty"`
FrontendApplicationTitle string         `protobuf:"bytes,28,opt,name=frontendApplicationTitle,proto3" json:"frontendApplicationTitle,omitempty"`
FrontendDefaultLanguage  string         `protobuf:"bytes,33,opt,name=frontendDefaultLanguage,proto3" json:"frontendDefaultLanguage,omitempty"`

// Sites configuration
ProxyConfigs            []*ProxyConfig `yaml:"proxyconfigs",json:"proxyConfigs,omitempty"`

// Additional arbitrary configs, where key is config path and value can be any type
CustomConfigs           map[string]interface{} `yaml:"customconfigs",json:"customConfigs,omitempty"`
```
### ProxyConfig proto

```
// A list of [host]:port to bind to
Binds           []string `protobuf:"bytes,1,rep,name=Binds,proto3" json:"Binds,omitempty"`

// Optional URL of reverse proxy exposing this site
ReverseProxyURL string `protobuf:"bytes,3,opt,name=ReverseProxyURL,proto3" json:"ReverseProxyURL,omitempty"`
// TLS configuration used for this site
// Types that are assignable to TLSConfig:
//	*ProxyConfig_SelfSigned
//	*ProxyConfig_LetsEncrypt
//	*ProxyConfig_Certificate

TLSConfig       isProxyConfig_TLSConfig `protobuf_oneof:"TLSConfig"`

// If TLS is set, whether to automatically redirect each http://host:port to https://host:port
SSLRedirect     bool `protobuf:"varint,2,opt,name=SSLRedirect,proto3" json:"SSLRedirect,omitempty"`

// If set, this site will be in maintenance mode
Maintenance     bool `protobuf:"varint,7,opt,name=Maintenance,proto3" json:"Maintenance,omitempty"`

// Append caddy directive to restrict maintenance mode
MaintenanceConditions []string `protobuf:"bytes,8,rep,name=MaintenanceConditions,proto3" json:"MaintenanceConditions,omitempty"`
```

## Examples

Below are some examples of YAML and JSON files

### YAML Basic Install

```
# Basic no-TLS config for the embedded gateway
proxyconfig:
  binds:
    - localhost:9090
  reverseproxyurl: http://localhost
frontendlogin: admin
frontendpassword: password
dbconnectiontype: tcp
dbtcphostname: localhost
dbtcpport: 3306
dbtcpname: cells
dbtcpuser: pydio
dbtcppassword: cells
```

### YAML Multi Site, custom configurations

```
# Pre-configure multi sites 
# Note that the proxy configurations defined here overwrite the values 
# that might have been passed by the --bind and --url flags.
proxyconfigs:
  - binds:
      - 0.0.0.0:8080
    reverseproxyurl: http://localhost:8080
  - binds:
      - 0.0.0.0:8081
    reverseproxyurl: https://localhost:8081
    tlsconfig:
      selfsigned:
        hostnames:
          - localhost
  - binds:
      - 0.0.0.0:8082
    reverseproxyurl: https://localhost:8082
    tlsconfig:
      selfsigned:
        hostnames:
          - localhost
```

### JSON with provided certificate

```
{
    "ProxyConfig": {
        "Binds": ["localhost:443"],
        "ReverseProxyURL": "https://localhost",
        "RedirectURLs": [
            "http://localhost"
        ],
        "TLSConfig": {
            "Certificate": {
                "CertFile": "/var/cells/certs/cert.pem",
                "KeyFile": "/var/cells/certs/key.pem"
            }
        }
    }
}
```