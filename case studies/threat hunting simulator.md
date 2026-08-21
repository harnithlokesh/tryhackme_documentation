**executive summary*

Issued by: TryDetectThis Intelligence

Classification: Internal – TLP:AMBER

TryDetectThis Intelligence has identified a coordinated supply chain attack campaign targeting open-source ecosystems, specifically, npm and Python package repositories. The campaign appears to be orchestrated by a threat actor leveraging long-term infiltration of neglected or low-profile projects to weaponize legitimate packages.

The attacker’s strategy involves contributing to moderately used but under-maintained libraries, gaining contributor or maintainer status through helpful commits. Once trusted, they publish malicious updates, embedding post-installation payloads or obfuscated backdoors within version releases that appear minor or maintenance-related.

These weaponized libraries often act as stagers for follow-on actions—such as downloading secondary payloads, establishing persistence, or exfiltrating tokens and credentials from developer machines. Due to their presence in tutorials, starter templates, or widely shared codebases, they have a high chance of spreading through organic adoption.




**Hypothesis**
An attacker may have leveraged a compromised third-party software package to gain initial access to the system and silently stage a payload for later execution. They likely established persistence to maintain access without immediate detection.





_what did i do_

in splunk i searched for install commands 

index=* (npm OR pip OR python OR node)
Complete 8 events (before 8/20/26 2:22:47.000 PM

index=* ("npm install" OR "npm i" OR "pip install" OR "pip3 install")
 0 events (before 8/20/26 2:25:00.000 PM



**finding the network activity**
 index=* "global-update.wlndows.thm"
QueryResults: ::ffff:127.0.0.1 _request was made on a local server_

we approve the hypothesis 


