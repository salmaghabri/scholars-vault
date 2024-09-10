---
resource: https://portswigger.net/web-security/xxe#how-to-find-and-test-for-xxe-vulnerabilities
---

# Def
- web security vulnerability that allows an attacker to interfere with an application's processing of XML data.
- allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.
#  How do XXE vulnerabilities arise
- Some applications use the XML format to transmit data between the browser and the server.
- External Entity is a feature of XML that allows you to define custom data pieces (entities) in a separate file or from an external source like a URL. It's like saying, "Hey, instead of putting all this information here, let's grab it from that file or website over there."
- these external entities can be manipulated in a way that's harmful.
# types of XXE attacks
- [Exploiting XXE to retrieve files](https://portswigger.net/web-security/xxe#exploiting-xxe-to-retrieve-files), where an external entity is defined containing the contents of a file, and returned in the application's response.
- [Exploiting XXE to perform SSRF attacks](https://portswigger.net/web-security/xxe#exploiting-xxe-to-perform-ssrf-attacks), where an external entity is defined based on a URL to a back-end system.
- [Exploiting blind XXE exfiltrate data out-of-band](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-exfiltrate-data-out-of-band), where sensitive data is transmitted from the application server to a system that the attacker controls.
- [Exploiting blind XXE to retrieve data via error messages](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-retrieve-data-via-error-messages), where the attacker can trigger a parsing error message containing sensitive data.

