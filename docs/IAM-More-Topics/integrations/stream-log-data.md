# About log streaming

Our platform enables you to feed audit Logs into your SIEM (Security information and event management) or other log collection systems. This includes the following data:  
- [Admin activities]() 
- [User activities]()  

## Integration options  

To feed audit logs into your systems, you can take advantage of the following: 

- API polling techniques
- Prebuilt plugins for third-party systems:  
    * [Audit log connector for Splunk](stream-splunk.md)  
    * More plugins under development  

Prebuilt plugins are normally better optimized for our platform and the consumers. 

## Prebuilt plugins  

There are multiple patterns a plugin can use to share data with target systems: API polling, pub/sub, and so on. No matter which pattern is implemented, a prebuilt plugin is easier to set up and configure than starting from scratch.  

To feed data into Splunk, use [Security Events Plugin](stream-splunk.md). 

More plugins are on the way, so check back soon.  

## API polling  

You can set up a cron job or a custom scheduler and query Audit Logs API at specified intervals. When configuring the batch size and polling interval, think about the activity level on the portal. Consider setting a longer polling period for low activity levels and a shorter polling period for higher activity levels. 

Keep in mind that determining the right batch size and polling interval for your organization may require some trial and error.  

!!! note "Learn more"
    * [Feed audit logs to Splunk](stream-splunk.md)