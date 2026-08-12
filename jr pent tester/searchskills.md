_**shodan**_
Shodan is often described as a search engine for the Internet of Things (IoT), but that undersells it. Shodan continuously scans the internet, searching for networking equipment, industrial control systems, traffic cameras, and virtually anything else with a public network connection to see what's running and where.

For example, searching apache 2.4.1 will return a list of servers advertising that version in their HTTP headers, broken down by country, organisation, and port. During a penetration test or vulnerability assessment, that kind of visibility is extremely useful, particularly when paired with a known CVE affecting that version.

Shodan also supports its own query filters, which let you narrow results significantly:

Filter	Description	Example
country	 Restrict results to a specific country code.	country:IE
port	Filter by a specific port number or a range.	port:22
org	Scope results in a named organisation or ASN Identifier (Who owns a range of IP addresses).	AS7224
(Amazon Web Services)
hostname	Match against a specific hostname or domain.	hostname:fakebank.thm


_**VirusTotal**_
VirusTotal collates results from over 70 antivirus engines and website scanners into a single interface. Submit a file, a URL, a domain, or a file hash. VirusTotal will tell you whether any of those engines have flagged it as malicious or not.

Whilst not foolproof, VirusTotal is a popular resource in the blue teaming community for obtaining a general consensus on suspicious files and links, as well as for gathering intelligence on new threats on the move.



_**CVE**_
The Common Vulnerabilities and Exposures (CVE) programme is the closest thing the industry has to a universal dictionary of known vulnerabilities.



Each confirmed vulnerability is assigned a unique identifier in the format CVE-YEAR-NUMBER, such as CVE-2025-55182. If the vulnerability is impactful enough, it may even get a moniker. You may have heard of vulnerabilities such as Heartbleed, React2Shell, and Log4Shell. These vulnerabilities are given a score (CVSS) based on a variety of factors, such as:

Impact - What damage can this vulnerability lead to?
Complexity - Is the vulnerability easy to exploit or not? 
Availability - How likely is it that someone can exploit this?
Organisations use scoring like this to prioritise their level of risk. Addressing the highest scoring first.
These identifiers function as a reference point among vendors, researchers, security tools, and documentation, ensuring that everyone discussing a vulnerability refers to the same issue. Websites like ExploitDB compile this information alongside "Proof of Concepts" (PoCs), which are scripts capable of demonstrating the vulnerability.



_**man nc(netcat)**_
Netcat (often called nc) is a powerful command-line tool used to read and write data across computer networks using TCP or UDP protocols. Known as the "Swiss Army knife" of networking, it helps users test connections, scan ports, transfer files, and debug networks.

