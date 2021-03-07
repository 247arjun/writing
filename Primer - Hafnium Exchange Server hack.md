# Primer - HAFNIUM hack of on-prem Microsoft Exchange servers

![ProxyLogon is the name of the root cause vuln](https://proxylogon.com/images/logo-white.png)

## Summary
In March 2021, [Microsoft revealed] that it had detected multiple 0-day exploits being used to attack on-premises versions of Microsoft Exchange Server. It attributed these attacks to a [newly identified nation-state actor "HAFNIUM"] that operated from China. These 0-days were used to access on-premises Exchange servers, enabling the attackers to access email accounts and to also install additional malware to persist in the victim environments.

## Technology Breakdown

### Microsoft Exchange Server
In the days preceding the pervasiveness of cloud computing, organizations used to host their own internal IT infrastructure. This meant that email, calendaring and instant messaging was hosted on on-premises servers that each organization owned and operated. 

Microsoft Exchange Server was a mail and calendaring service that organizations would install on their on-premises Windows Server based servers, to host email and calendar services for their employees. Access to email used email protocols on supported email clients (on employee computers). 

Later on, Exchange Server allowed Outlook Web Access (OWA) that allowed employees to access their email/calendar information in a web browser.

### Server Side Request Forgery (SSRF)
Normal web requests from an external client to a server are handled by the server to respond to expected API calls. These generally have access control mechanisms to prevent abuse.

In an SSRF attack, the attacker can abuse functionality on the server to read or update internal resources by inducing the server-side application to issue web requests, under their control.

### Web Shell
When you view content on a webpage or interact with a web application, you are requesting resources that already exist on the web server. For static content, you are requesting a copy of the file to be served by the HTTP web server and for dynamic content you are probably using an API to make a query that the web server responds to.

A web shell is a way to run arbitrary queries on a web server, beyond those defined and controlled by the web server. Often, these are commands received by files on the web server that were placed there by attackers - think scripts that can be accessed by attackers that other users are unaware of.

## Attack Chain
From preliminary reports by [Volexity], the attackers exploited an SSRF 0-day in OWA ([CVE-2021-26855]) to bypass authentication and steal the contents of user mailboxes hosted on the Exchange servers.

Additionally, another Remote Code Execution (RCE) 0-day in the Offline Address Book feature ([CVE-2021-27065]) was used to write webshells (`.aspx` files) to disk. These webshells were then used to exfiltrate further data from the servers (e.g. `ntds.dit` which is the Active Directory environment data)


[Microsoft revealed]: https://www.microsoft.com/security/blog/2021/03/02/hafnium-targeting-exchange-servers/

[newly identified nation-state actor "HAFNIUM"]: https://blogs.microsoft.com/on-the-issues/2021/03/02/new-nation-state-cyberattacks/

[Volexity]: https://www.volexity.com/blog/2021/03/02/active-exploitation-of-microsoft-exchange-zero-day-vulnerabilities/

[CVE-2021-26855]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-26855

[CVE-2021-27065]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27065