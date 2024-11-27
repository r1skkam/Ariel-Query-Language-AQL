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
*Last 10 VPN users login attempts 

```
SELECT DATEFORMAT("startTime", 'MMM dd yyyy hh:mm a') AS 'Start Time', sourceip as 'Source IP', sourceGeographicLocation as 'GeoLoaction', username as 'Username' from events where LOGSOURCENAME(logsourceid) = 'logsource name' AND CATEGORYNAME(category) = 'User Login Success' Last 5 HOURS
```

*Qradar Access Users

```
select sourceip as "Source IP", username as "User Name" from events where LOGSOURCENAME(logsourceid) = 'SIM Audit-2 :: logsourcename' and username is not null and (INCIDR('*.*.*.*/27', "sourceIP") OR INCIDR('192.168.1.0/24', "sourceIP")) group by username last 30 MINUTES 
```

*Heap Memory Usage

```
SELECT DATEFORMAT(starttime, 'yyyy-MM-dd') as "Date", "Hostname" as "QRadar Appliance Name", 
"Component Type", LONG(SUM("Value")/(1024*1024*1024)) as "Memory Usage Per Day (GB)"
FROM events 
WHERE (qid = 94000001)  AND ("Metric ID" = 'HeapMemoryUsed') 
GROUP BY "Date", "Hostname", "Component Type"
ORDER BY "Date", "Hostname", "Component Type"
```

**Credited and special thanks to [Bro Aung Ko Ko](https://www.linkedin.com/in/aung-ko-ko-02194621a/)**
