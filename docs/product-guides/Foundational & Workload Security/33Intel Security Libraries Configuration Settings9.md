# Workload Policy Manager 
-----------------------

## Installation Answer File Options

| Key                    | Sample Value                                            | Description                                                  |
| ---------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| KBS_BASE_URL           | https://\<IP address or hostname of the KBS\>:9443/v1/  | Required. Defines the baseurl for the Key Broker Service. The WPM uses this URL to request new encryption keys when encrypting images. |
| CMS\_TLS\_CERT\_SHA384 |                                                         | Required. SHA384 hash of the CMS TLS certificate             |
| CMS\_BASE\_URL         | https://\<IP address or hostname for CMS\>:8445/cms/v1/ | Required. Defines the base URL for the CMS owned by the image owner. Note that this CMS may be different from the CMS used for other components. |
| AAS\_API\_URL          | https://\<IP address or hostname for AAS\>:8444/aas/v1  | Required. Defines the baseurl for the AAS owned by the image owner. Note that this AAS may be different from the AAS used for other components. |
| BEARER\_TOKEN          | \<token\>                                               | Required; token from CMS with permissions used for installation. |
| WPM\_LOG\_LEVEL        | INFO (default), DEBUG                                   | Optional; defines the log level for the WPM. Defaults to INFO. |
| WPM\_SERVICE_PASSWORD  |                                                         | Defines the credentials for the WPM to use to access the KBS |
| WPM\_SERVICE_USERNAME  |                                                         | Defines the credentials for the WPM to use to access the KBS |

## Configuration Options

## Command-Line Options

The Workload Policy Manager supports several command-line commands that
can be executed only as the Root user:

Syntax:
```
wpm <command>
```
### create-image-flavor

Creates a new image flavor and encrypts a source image. Output is the
image flavor in JSON format and the encrypted image.
```
usage: wpm create-image-flavor [-l label] [-i in] [-o out] \[-e
encout] [-k key]

-l, --label image flavor label

-i, --in input image file path

-o, --out (optional) output image flavor file path

if not specified, will print to the console

-e, --encout (optional) output encrypted image file path

if not specified, encryption is skipped

-k, --key (optional) existing key ID

if not specified, a new key is generated
```
### create-software-flavor

Not currently supported; intended for future functionality.

### Uninstall

Removes the WPM.

### --help

Displays help text

### --version

Displays the WPM version

### Setup

usage : wpm setup [<tasklist>]

<tasklist>-space separated list of tasks

#### wpm setup

#### wpm setup CreateEnvelopeKey

#### wpm setup RegisterEnvelopeKey

#### wpm setup download_ca_cert [--force]

\- Download CMS root CA certificate

\- Option \[--force\] overwrites any existing files, and always
downloads new root CA cert

\- Environment variable CMS_BASE_URL=<url> for CMS API url

#### wpm setup download\_cert Flavor-Signing \[--force\]

\- Generates Key pair and CSR, gets it signed from CMS

\- Option \[--force\] overwrites any existing files, and always
downloads newly signed Flavor Signing cert

\- Environment variable CMS\_BASE\_URL=<url\> for CMS API url

\- Environment variable BEARER\_TOKEN=<token\> for authenticating with
CMS

\- Environment variable KEY\_PATH=<key\_path\> to override default
specified in config

\- Environment variable CERT\_PATH=<cert\_path\> to override default
specified in config

\- Environment variable WPM\_FLAVOR\_SIGN\_CERT\_CN=<COMMON NAME\> to
override default specified in config

\- Environment variable WPM\_CERT\_ORG=<CERTIFICATE ORGANIZATION\> to
override default specified in config

\- Environment variable WPM\_CERT\_COUNTRY=<CERTIFICATE COUNTRY\> to
override default specified in config

\- Environment variable WPM\_CERT\_LOCALITY=<CERTIFICATE LOCALITY\> to
override default specified in config

\- Environment variable WPM\_CERT\_PROVINCE=<CERTIFICATE PROVINCE\> to
override default specified in config

