---
# This page was generated from the add-on.
title: Active Scan Rules
type: userguide
weight: 1
cascade:
  addon:
    id: ascanrules
    version: 34.0.0
---

# Active Scan Rules

The following release quality active scan rules are included in this add-on:

## Buffer Overflow

Looks for indicators of buffer overflows in compiled code. It does this by putting out large strings of input text and look for code crash and abnormal session closure.

Latest code: [BufferOverflow.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/BufferOverflow.java)

## Code Injection

This rule submits PHP and ASP attack strings as values for URL parameters in a request and examines the response to see if those values have been evaluated by the server. First, requests are constructed and sent containing injected PHP instructions to print a token. In the event of a match between the token and the content of the response body, the scanner raises an alert and returns immediately. If there aren't any matches, the scanner will construct requests with several injected ASP strings that instruct the server to write the product of two randomly generated integers in the response. If the body of the response matches the product, an alert is raised.

Latest code: [CodeInjectionPlugin.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/CodeInjectionPlugin.java)

## Command Injection

This rule submits \*NIX and Windows OS commands as URL parameter values to determine whether or not the web application is passing unchecked user input directly to the underlying OS. The injection strings consist of meta-characters that may be interpreted by the OS as join commands along with a payload that should generate output in the response if the application is vulnerable. If the content of a response body matches the payload, the scanner raises an alert and returns immediately. In the event that none of the error-based matching attempts return output in the response, the scanner will attempt a blind injection attack by submitting sleep instructions as the payload and comparing the elapsed time between sending the request and receiving the response against a heuristic time-delay lower limit. If the elapsed time is greater than this limit, an alert is raised with medium confidence and the scanner returns immediately.   
Post 2.5.0 you can change the length of time used for the blind injection attack by changing the `rules.common.sleep` parameter via the Options 'Rule configuration' panel.

Latest code: [CommandInjectionPlugin.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/CommandInjectionPlugin.java)

## Cross Site Scripting (reflected)

This rule starts by submitting a 'safe' value and analyzing all of the locations in which this value occurs in the response (if any).   
It then performs a series of attacks specifically targeted at the location in which each of the instances occurs, including tag attributes, URL attributes, attributes in tags which support src attributes, html comments etc.   
Note:   
This rule only scans HTTP PUT requests at LOW threshold.  
If an XSS injection is located in a JSON response a LOW risk and LOW confidence alert is raised.

Latest code: [TestCrossSiteScriptV2.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestCrossSiteScriptV2.java)

## Cross Site Scripting (persistent)

This rule starts by submitting a unique 'safe' value and then spiders the whole application to find all of the locations in which the value occurs.  
It then performs a series of attacks in the same way that the 'reflected' version does but in this case checks all of the target locations in other pages.  
Note:   
This rule only scans HTTP PUT requests at LOW threshold.  
If an XSS injection is located in a JSON response a LOW risk and LOW confidence alert is raised.  

Latest code: [TestPersistentXSSPrime.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestPersistentXSSPrime.java),
[TestPersistentXSSSpider.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestPersistentXSSSpider.java),
[TestPersistentXSSAttack.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestPersistentXSSAttack.java)

## CRLF Injection

This rule submits various CRLF special characters preceding an injected "Set-Cookie" header as a parameter to the server. If the response contains an identical Set-Cookie header, an alert is raised and the scanner returns immediately.

Latest code: [TestInjectionCRLF.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestInjectionCRLF.java)

## Directory Browsing

This rule checks to see if a request will provide access to a directory listing by examining the response body for patterns used with Apache, IIS and other web server software.

Latest code: [TestDirectoryBrowsing.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestDirectoryBrowsing.java)

## External Redirect

This rule submits a variety of URL redirect strings as parameter values in a request, then examines the headers and bodies of responses to determine whether or not a redirect occurred and of what type. The cause of redirect is searched for in the "Location" and "Refresh" header fields, as well as by HTML meta tags and Javascript in the body of the response. An alert is raised including the redirection type and the scanner returns immediately.

Latest code: [TestExternalRedirect.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestExternalRedirect.java)

## Format String Error

Looks for indicators of format string handling errors in compiled code. It does this by putting out strings of input text based upon characters compiled C code anticipates to produce formatted output and look for code crash and abnormal session closures.

Latest code: [FormatString.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/FormatString.java)

## Parameter Tampering

This rule submits requests with parameter values known to cause errors to be displayed to the user if handled improperly. Responses are checked to make sure that they return a server error status code, then compared with a normal HTTP response to make sure it does not raise an alert if the bad parameter has no effect on output. Finally, the content of the reponse body is compared against various patterns that may be found in Java servlet, Microsoft VBScript, OLE DB, JET, PHP and Tomcat errors. If a match is found, an alert is raised and the scanner returns immediately.

Latest code: [TestParameterTamper.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestParameterTamper.java)

## Path Traversal

This rule attempts to access files and directories outside of the web document root by constructing various combinations of pathname prefixes and local file targets for Windows and \*NIX systems as well as Java servlets. If the body of the response matches a pattern corresponding to the current target file an alert is raised and the scanner returns immediately. If none of the common local file targets succeed, path traversal is attempted using the filename in the URL. As long as submitting an arbitrary filename does not return an OK status code but the real filename does, an alert is raised and the scanner returns immediately.

Latest code: [TestPathTraversal.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestPathTraversal.java)

## Remote File Include

This rule submits a series of requests with external URLs as parameter values and looks for a match between the response body and the title of the page hosted at those URLs. If there is a match between the expected string and the response body, and the header returned a status code of 200, an alert is raised and the scanner returns immediately.

Latest code: [TestRemoteFileInclude.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestRemoteFileInclude.java)

## Server Side Include

This rule checks to see what OS the server is running on, then sends requests with a corresponding HTML SSI directive as a parameter value. If the response body matches a pattern indicating the SSI was successful, an alert is raised and the scanner returns immediately.

Latest code: [TestServerSideInclude.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestServerSideInclude.java)

## Source Code Disclosure - /WEB-INF

Exploit the presence of an unprotected /WEB-INF folder to download and decompile Java classes, to disclose Java source code.  
**Note:** Currently not supported on Java 9 and above.

Latest code: [SourceCodeDisclosureWEBINF.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/SourceCodeDisclosureWEBINF.java)

## SQL Injection

This scanner scans for SQL Injection vulnerabilities in an RDBMS-independent fashion, by attacking url parameters and form parameters with fragments of valid and invalid SQL syntax, using error based, boolean based, Union based, and stacked query SQL Injection techniques.   
This scanner may be able to fingerprint the RDBMS if the application throws a known RDBMS specific SQL error message.   
This scanner does not exploit any RDBMS specific techniques, and so is the best SQL injection scanner to use as a starting point.

Latest code: [TestSQLInjection.java](https://github.com/zaproxy/zap-extensions/blob/master/addOns/ascanrules/src/main/java/org/zaproxy/zap/extension/ascanrules/TestSQLInjection.java)