I've updated the management pack found here: http://www.systemcentercentral.com/pack-catalog/microsoft-network-policy-server-nps-2/ 

The update was needed to include to be able to discover NPS installed on Windows Server 2016 servers. 
The updated version info:

This management pack monitors the Network Policy and Access server (NPS) role on Windows Server 2008/2012 (R2)/2016.
**1.0.1.84** – Created a custom data source module to consolidate repeating event based on the “Trigger on Count, Sliding” module. The following rule is configured with the custom data source: “NPS: Authentication failed and the user was denied access”
**1.0.1.86** – Changed the “Network Policy Servers Discovery” back to discover only Server 2008 (R2) servers and renamed the discovery to “Network Policy Servers 2008 Discovery”. Added a new discovery “Network Policy Servers 2012 Discovery” to discover NPS on Server 2012 (R2).
(Note: The MP is not tested by me on a 2012 NPS Server)
**1.0.1.88** - Added a "Network Policy Server 2016 Discovery" to discover NPS on Server 2016 (and Server 1709)

Always test the management packs in a test environment before importing in production!

If you like what I'm doing consider buying me a beer:
- **Bitcoin address:** 3NVhdVDUg3T3gq55JMzAiymBCYspzJH7eS
- **Ripple address:** rLHzPsX6oXkzU2qL12kHCH8G8cnZv1rBJh (Destination tag: 4074302783)
