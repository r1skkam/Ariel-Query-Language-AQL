# Ariel Query Language (AQL)

The Ariel Query Language (AQL) is a structured query language that you use to communicate with the Ariel databases. 

Use AQL to query and manipulate event and flow data from the Ariel database. 

[Ariel Query Language - IBM Documentation ibm.com](https://www.ibm.com/docs/en/qsip/7.5?topic=aql-ariel-query-language)

**Notes: Replace the real QID numbers (example: QRadar's QID or Firewall's QID) in below queries ###**

*Query Last 24 hours logs of Offense Assigned Analyst*

```
SELECT QIDNAME(qid) as 'Event Name', LOGSOURCENAME(##) as 'Log Source', DATEFORMAT(starttime, 'dd MMM YYYY, HH:mm:ss') as 'Date/Time', sourceip as 'Source IP', username as 'Username' FROM events WHERE QIDNAME(qid) = 'Offense Assigned' AND logsourceid = '##' LAST 1440 MINUTES
```

*Query Last 24 hours log activities of AliceBob*

```
SELECT QIDNAME(qid) as 'Event Name', LOGSOURCENAME(###) as 'Log Source', DATEFORMAT(starttime, 'dd MMM YYYY, HH:mm:ss') as 'Date/Time', sourceip as 'Source IP', destinationip as 'Destination IP', destinationport as 'Destination Port', username as 'Username' FROM events WHERE QIDNAME(qid) = 'User authenticated successfully' OR QIDNAME(qid) = 'Session Allowed' OR QIDNAME(qid) = 'Session Denied' AND logsourceid = '###' AND username ILIKE '%AliceBob%' LAST 1440 MINUTES
```

**Credited and special thanks to [Bro Aung Ko Ko](https://www.linkedin.com/in/aung-ko-ko-02194621a/)**
