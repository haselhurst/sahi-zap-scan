# Sahi-Zap-Scan

## Synopsis

Sahi ZAP Scan is a simple API designed to be used with the [Sahi](http://sahipro.com/) automation tool in conjunction with [OWASP ZAP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project), a free security scanning tool.

There isn't really too much to it, you just include the zap-include.sah file into your sahi script and after updating some of the variables you can call different functions to trigger different actions, such as activeScan() to perform an active scan.

## Usage

There are two Sahi files within this repository - zap-include.sah and example.sah

The zap-include.sah file needs to be included within the Sahi script that you create. You can do this using the following code:

```sh
_inlcude("zap-include.sah");
```

You are then able to call any of the functions within that include file. You'll find a list of them further down on this page.

The example.sah gives you an example of how to call the different functions from within the include file, triggering different actions within the ZAP program.

It is worth noting that the ZAP scan that you trigger will not initiate unless ZAP is running, so you should make sure that you have the application open before running the automated script. Don't worry though - if you don't have ZAP running when the script is ran then an error will be logged informing you of this, as long as you call the isZapOpen() function at the start of your script.

### Configuring The Include File

A number of variables are contained towards the top of the zap-include.sah file. Some of the values will need to remain unchanged, but here are the variables that you'll have to manually change the values of:
* var $target = "http://example.target.com";
* var $contextName = "ContextName";
* var $children = "20";
* var $zapURL = "http://127.0.0.1:8080";
* var $zapAPIKey = "*****";

$target should be set to the URL of the site that you want to perform your scans on.
$contextName should be set to the name that you want to give to your context. This should be relevant to your target. For example, if you are scanning google.com (which realistically you shouldn't be doing) then you may want to call your context "Google".
$children specifies the depth of the spider that you want ZAP to undertake. If you set this as "0" then the homepage of the site that you target would be scanned but no further scanning would be performed. If you set this to "1" then each link from the homepage would be scanned. The larger this number is set to, the more results would show in the spider of the site, but this does increase the amount of time that the scan would take.
$zapURL is the URL for the ZAP API. Typically ZAP runs on your computer (http://127.0.0.1) on port 8080 (:8080). If ZAP is located on a different machine or you've configured it to run on a port other than 8080 then these values should be changed.
$zapAPIKey is the API key that allows your script to interact with ZAP. This can be found within ZAP by going to Tools > Options > API. Make sure that the "Enabled" checkbox is the only checkbox on this screen to be checked. If no API Key is shown then you'll need to click the "Generate Random Key" button before copying the API key from this screen into the $zapAPIKey variable.

## Code Example

### Example - Perform An Active Scan & Log Results
This example shows you how to perform an active scan within ZAP.

This script does the following:
* Checks to make sure that ZAP is open.
* Deletes any alerts that are already logged in ZAP and sets the scan mode to "Standard".
* Automates the process of creating a context.
* Adds the target URL to that context.
* Performs a spider of the target site.
* Performs an active scan of the target site.
* Generates a report of any alerts raised and saves these as a PNG image in the specified location (C:\zap\ZapScanResult.png).
* Shuts down the ZAP program.

```sh
//Call ZAP Include File
_include("zap-include.sah");

//Perform Scan
isZapOpen();
resetZap();
newContext();
includeInContext();
spider();
activeScan();
getScanReport("C:\\zap\\");
shutdownZap();;
```

## Functions
Below is a list of functions that can be used within your script to interact with ZAP.
* isZapOpen()
* resetZap()
* newContext()
* includeInContext()
* spider()
* activeScan()
* getScanReport()
* shutdownZap()

Note that the getScanReport() function can take an optional parameter of which directory to store the scan report in. The location needs to be escaped, as shown below:
```sh
getScanReport("C:\\dev\\");
```
If no location is specified then the scan report will be saved in the same directory that houses your script. To use this function without specifying a location, do the following:
```sh
getScanReport();
```

## License
[Sahi ZAP Scan](http://github.com/haselhurst/sahi-zap-scan/) - Copyright (c) 2016 Michael Haselhurst

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
