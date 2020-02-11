# cx-sca-search

cx-sca-search is a tool to search for a given open-source library name in the results of prior Checkmarx SCA scans. This search tool complements the search capability in the current SCA results User Interface, which limits searches to the current scan.

The search tool searches results that are:
  - in the scan history (prior scans)
  - in any state - not/vulnerable, not/at legal risk, not/outdated
 
# Features

  - Search library names in current and prior scans.
  - Reports matched libraries, even if they're not vulnerable/at legal risk/outdated.
  - Group results by Project or Team
  - Outputs search results to the console or as JSON to disk

# Execution

The tool is a PowerShell script and can be run from any Windows machine.
The search tool connects to the Checkmarx Database and therefore needs network connectivity to the database server. The tool supports both Integrated Authentication as well as SQLServer Authentication to the Checkmarx database.

## Usage
```
cx-sca-search.ps1 -lib <libraryName> [-dbUser <dbAccount>] [-dbPass <dbPassword>] [-group <Project|Team>] [-json] [-grid] 
```

| Parameter | Required? | Notes |
| ------ | ------ | ------ |
| lib | Yes | Library to search for. Can be full library name or partial using wildcards. *Examples: log4net, log4\*, \*unit\* etc.* |
| dbUser | No | Database account that can read from the Checkmarx database. Skip if using Integrated Authentication to the Checkmarx database |
| dbPass | Yes, if -dbUser is provided. Otherwise, not required. | Password for the database account used in the -dbUser parameter. Skip if using Integrated Authentication to the Checkmarx database |
| group | No | Grouping to use while displaying results on the console. Valid values are [Team or Project]. Ignored if used in conjunction with the -json flag. |
| json | No | If this flag is specified, search results will be written to a JSON file on disk. |
| grid | No | If this flag is specified, search results will be rendered in a Powershell Grid View. |

### Examples

```
PS C:\shared\workspace\checkmarx\cx-sca-search> .\cx-sca-search.ps1 -lib *log*

2/11/2020 7:31:24 PM: Loading configuration from C:\shared\workspace\checkmarx\cx-sca-search\cx_sca_search_config.json
2/11/2020 7:31:24 PM: -----------------------------------------
2/11/2020 7:31:24 PM: Checkmarx SCA Search
2/11/2020 7:31:24 PM: Database: localhost\SQLExpress
2/11/2020 7:31:24 PM: Using Integrated Authentication to SQLServer
2/11/2020 7:31:24 PM: -----------------------------------------
2/11/2020 7:31:24 PM: Found [8] results for library '*log*'.
2/11/2020 7:31:24 PM: Number of Teams that use library : [2]
2/11/2020 7:31:24 PM: Number of Projects that use library : [2]
2/11/2020 7:31:24 PM: Number of Libraries that matched : [6]

Team                                    Project     Library                             Version Found In Scan
----                                    -------     -------                             ------- -------------
\CxServer\Americas\Hardware\Engineering DVJA_master org.apache.logging.log4j:log4j-core 2.3     2/7/2020 9:31:14 PM
\CxServer\Americas\Hardware\Engineering DVJA_master org.slf4j:slf4j-log4j12             1.5.2   2/7/2020 9:31:14 PM
\CxServer\Americas\Hardware\Engineering DVJA_master log4j:log4j                         1.2.14  2/7/2020 9:31:14 PM
\CxServer\Americas\Hardware\Engineering DVJA_master org.apache.logging.log4j:log4j-api  2.3     2/7/2020 9:31:14 PM
\CxServer\Americas\Software\DevOps      DVNA_master npmlog                              4.1.2   2/7/2020 9:53:07 PM
\CxServer\Americas\Software\DevOps      DVNA_master npmlog                              4.1.2   2/11/2020 6:34:29 PM
\CxServer\Americas\Software\DevOps      DVNA_master logform                             2.1.2   2/7/2020 9:53:07 PM
\CxServer\Americas\Software\DevOps      DVNA_master logform                             2.1.2   2/11/2020 6:34:29 PM
```

```
PS C:\shared\workspace\checkmarx\cx-sca-search> .\cx-sca-search.ps1 -lib *log* -group Team
2/11/2020 7:33:16 PM: Loading configuration from C:\shared\workspace\checkmarx\cx-sca-search\cx_sca_search_config.json
2/11/2020 7:33:17 PM: -----------------------------------------
2/11/2020 7:33:17 PM: Checkmarx SCA Search
2/11/2020 7:33:17 PM: Database: localhost\SQLExpress
2/11/2020 7:33:17 PM: Using Integrated Authentication to SQLServer
2/11/2020 7:33:17 PM: -----------------------------------------
2/11/2020 7:33:17 PM: Found [8] results for library '*log*'.
2/11/2020 7:33:17 PM: Number of Teams that use library : [2]
2/11/2020 7:33:17 PM: Number of Projects that use library : [2]
2/11/2020 7:33:17 PM: Number of Libraries that matched : [6]
2/11/2020 7:33:17 PM: Grouping results by [Team]


   Team: \CxServer\Americas\Hardware\Engineering

Team                                    Project     Library                             Version Found In Scan
----                                    -------     -------                             ------- -------------
\CxServer\Americas\Hardware\Engineering DVJA_master org.apache.logging.log4j:log4j-core 2.3     2/7/2020 9:31:14 PM
\CxServer\Americas\Hardware\Engineering DVJA_master org.slf4j:slf4j-log4j12             1.5.2   2/7/2020 9:31:14 PM
\CxServer\Americas\Hardware\Engineering DVJA_master log4j:log4j                         1.2.14  2/7/2020 9:31:14 PM
\CxServer\Americas\Hardware\Engineering DVJA_master org.apache.logging.log4j:log4j-api  2.3     2/7/2020 9:31:14 PM


   Team: \CxServer\Americas\Software\DevOps

Team                               Project     Library Version Found In Scan
----                               -------     ------- ------- -------------
\CxServer\Americas\Software\DevOps DVNA_master npmlog  4.1.2   2/7/2020 9:53:07 PM
\CxServer\Americas\Software\DevOps DVNA_master npmlog  4.1.2   2/11/2020 6:34:29 PM
\CxServer\Americas\Software\DevOps DVNA_master logform 2.1.2   2/7/2020 9:53:07 PM
\CxServer\Americas\Software\DevOps DVNA_master logform 2.1.2   2/11/2020 6:34:29 PM
```

License
----

TBD
