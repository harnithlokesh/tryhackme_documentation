A fake banking application called FakeBank will launch. When the lab loads, you'll see the banking application running in your browser on the right, showing Mrs G. Benjamin's banking account.

Answer the questions below
What is the bank account number shown in the FakeBank application?
A:- 8881

**finding hidden pages**
Now we will find a weakness in the FakeBank website. One common thing Hackers look for on a website is pages that are not linked. Hackers are interested in these hidden pages because they can often lead to sensitive pages or information that the organisation did not intend for us to see.

We can use popular cyber security tooling to find these pages by using common words and phrases, such as "login", "admin", etc. One of those tools is dirb, which can be used in the terminal. 

The terminal is used to interact with the device and cyber security tools.

_dirb http://example.com_

---- Scanning URL: http://fakebank.thm/ ----

+ http://fakebank.thm/bank-transfer (CODE:200|SIZE:4663)
+ http://fakebank.thm/images (CODE:301|SIZE:179)


here, bank-transfer page is most likely to give us access to sensitive banking information 

dirb can we widely used on almost any website to reveal any hidden directory, if any, sometimes can give us access to sensitive systems 




To navigate to this hidden admin panel, we will add this newly discovered page to the search bar located at the top of the website. To do so, you will need to add the following: /bank-transfer. This demonstrates that just because something is hidden doesn't mean it isn't accessible and secure.



It looks like we can deposit money into Mr's G.Benjamin's account using this admin panel. Test this by selecting the account number you verified in task 2 (8881) and depositing at least 2,000 until your balance turns positive.



**how can we fix such a vulnerability?(How to defend against unauthorized web content enumeration)**

--------------------------------------x----------------------x----
What are we protecting?

Potentially:

Admin panels
Backup files
Configuration files
Development/staging pages
Internal documentation
Upload directories
Old/forgotten application endpoints
Sensitive files accidentally exposed through the web server
How would a company defend against this?

1. Remove unnecessary files and directories

If /backup-old/ or /test/ isn't needed in production, don't leave it sitting on the server.

This is probably the biggest lesson from DIRB:

Don't rely on obscurity. Remove what shouldn't be publicly accessible.

2. Enforce authentication and authorization

If /admin legitimately exists, don't assume that hiding it is security.

Instead:

Attacker → /admin → Authentication → Authorization → Application

Only authorized users should actually get access.

3. Don't expose sensitive files

Things like:

.env
backup.zip
database.sql
config.php
.git/

should never be publicly accessible.

This is particularly important because directory/file enumeration can reveal accidentally exposed sensitive resources.

4. Harden the web server

Configure the server so it doesn't unnecessarily expose directory listings.

For example:

/backup/
    file1.zip
    database.sql
    old-config.txt

You don't want an unauthenticated visitor simply browsing that directory and seeing everything inside.

5. Use a WAF / security controls

A WAF can help identify and potentially block suspicious automated request patterns.

For example:

GET /admin
GET /backup
GET /test
GET /uploads
GET /old
GET /config
...

A large number of sequential requests for nonexistent resources can be a useful indicator of automated enumeration.

But don't treat a WAF as the primary fix. If /backup.zip is publicly accessible, blocking DIRB doesn't solve the underlying exposure.

6. Monitor and detect enumeration

This is where you can connect your THM knowledge to a SOC.

Security teams can monitor:

Web server access logs
HTTP 404 spikes
Requests to sensitive paths
High request rates
Repeated directory/file discovery patterns
Source IP reputation
WAF alerts

A SIEM could correlate these events and generate an alert when the behavior looks suspicious.

DIRB demonstrates the importance of controlling web application attack surfaces. Organizations should minimize publicly accessible resources, enforce authentication and authorization, prevent exposure of sensitive files, disable unnecessary directory listing, and monitor web traffic for automated content-enumeration patterns.