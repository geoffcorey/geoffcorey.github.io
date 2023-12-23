---
layout: post
title:  "Snowflakes conversion of timestamp NTZ to UTC with offset"
date:   2023-12-23 10:18:29 -0500
categories:  snowflakes dst timestamp_ntz utc SQL
---

Had an issue where I found a chunk of code was trying to convert Snowflakes timestamp_ntz stored in local `America/New_York` format to a UTC time format with offset.   The chunk of code did not take in account Daylight Savings Time.

Here is example code of various attempts and eventual solution

```
ALTER SESSION SET TIMEZONE = 'America/New_York';
 SELECT
     TO_TIMESTAMP_NTZ('12/15/2023 01:02:03', 'mm/dd/yyyy hh24:mi:ss') as mytimestamp,
     mytimestamp::timestamp_tz as orig,
     TO_VARCHAR(mytimestamp, 'YYYY-MM-DDTHH24:MI:SS.FF3-TZHTZM')  as datetime_wrong,
     CONVERT_TIMEZONE('UTC','America/New_York',TO_TIMESTAMP_LTZ(mytimestamp)) AS converted_ts,
     DATEDIFF('hour', converted_ts, mytimestamp) as hourdiff,
     TO_VARCHAR(converted_ts) || '-0' || hourdiff || ':00' as datetime;
```
