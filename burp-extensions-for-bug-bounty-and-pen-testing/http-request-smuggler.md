# HTTP Request Smuggler

### Description

This is the go-to Burp extension, By using this you can easily detect and exploit a web application for[ HTTP Request Smuggling](../resources/web-app-pentest/http-request-smuggling.md). It detects whether you have a [CL.TE or TE.CL](https://portswigger.net/web-security/request-smuggling/finding) condition and reports it directly into Burp Suite’s Dashboard tab, under the Issue Activity menu where all the issues get listed.&#x20;

To use the HTTP request smuggler extension first you need to install a Turbo intruder extension, which sends a large number of requests to analyze the result.

### Steps to install Turbo Intruder

1. Start Burp Suite.
2. Move to the Extender tab.
3. Go to BApp Store.
4. Search Turbo intruder.
5. Hit Install.

### Steps to install HTTP Request Smuggler

1. Start Burp Suite.
2. Move to the Extender tab.
3. Go to BApp Store.
4. Search HTTP request smuggler.
5. Hit Install.

### References

{% embed url="https://github.com/PortSwigger/turbo-intruder" %}

{% embed url="https://github.com/portswigger/http-request-smuggler" %}