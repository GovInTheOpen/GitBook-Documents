# Project: Waze CCP Processor

Allows traffic teams and others within governments to store, analyze, visualize, and take action on Waze's CCP program data.

**Code**: [github.com/LouisvilleMetro/WazeCCPProcessor](https://github.com/LouisvilleMetro/WazeCCPProcessor)

_Collaborating Cities, States, Countries:_ Louisville, Denver, NYC, Joinville Brazil,  \([see all 60+ govs](https://github.com/LouisvilleMetro/WazeCCPProcessor/wiki/Waze-CCP-Collaborative-Processor)\)

* _Sponsors_: Amazon AWS, Slingshot
* _Promoters_: Waze
* _Potential Future Collaborators_: Microsoft Azure, Google Cloud

---

## Project Road Map

Waze provides government partners access to anonymized data that can be used for traffic, public safety, emergency response, infrastructure, and air pollution use cases. 

This road map shows what is needed to create a valuable end-to-end data processor and visualization solutions based on Louisville, KY’s existing use cases.

Everything is written in Terraform.io's infrastructure-as-code form for portability across cloud providers.

### **1\) End-to-end data processor**

[https://github.com/LouisvilleMetro/WazeCCPProcessor/projects/1](https://github.com/LouisvilleMetro/WazeCCPProcessor/projects/1)

* Gets the raw Waze CCP JSON data feed into a relational database.

* Base requirement for Waze data to be useful for governments.

_Technical:_ Cloudwatch alarm triggers Lambda every 2 minutes to look at the Waze endpoint and saves data to S3. Lambda adds record to SQS Queue to process S3 files into Aurora Postgres DB. Alerts and queues for failures and SNS Topics used to allow real-time hooks. DB enriched with extra columns need for analysis.

### 2\) API endpoints

[https://github.com/LouisvilleMetro/WazeCCPProcessor/projects/3](https://github.com/LouisvilleMetro/WazeCCPProcessor/projects/3)

* Allows easy access to chunks of data from the DB

* Proof of concept limited for use of Interactive Map

* Good to get developer buy-in, service integrations

Technical: HTTP endpoint would return GeoJSON file from DB. Parameters include start date/time, end date/time, lat/lon bounding box, Waze categories \(Jam types, Alert types, Irregularities\). Can run through AWS API Gateway or other API service like Kong.

_Examples:_

**List of reports**

`https://endpoint.com/api/waze/reports?startdate=2018-02-17&enddate=2018-02-19&starttime=1600&endtime=1800&minlat=37.9971&maxlat=38.38051&minlon=-85.948441&maxlon=-85.4051&jams=2,3,4&alerttypes=ACCIDENT_MAJOR,JAM_STAND_STILL_TRAFFIC,HAZARD_ON_SHOULDER_CAR_STOPPED&irregularity=0&roadtypes=3,5,8,9&streetname=&delay=&speed=&length=&format=json`

**Detailed data on a single report**

`https://endpoint.com/api/waze/reportdetail?reportid=FB6E701F-E8A6-3667-BC5A-434A593E5C76&format=json`

**Geodata \(line or point\) on a single report**

`https://endpoint.com/api/waze/segmentgeo?reportid=FB6E701F-E8A6-3667-BC5A-434A593E5C76&format=json`

### 3\) Interactive Map

[https://github.com/LouisvilleMetro/WazeCCPProcessor/issues/21](https://github.com/LouisvilleMetro/WazeCCPProcessor/issues/21)

* Overlays current snapshot of alerts, jams, irregularities, with filters

* Provides a date/time selector and slider to look back in time

* Good to get leadership/mayoral buy-in, business intelligence cases

_Technical: _A open source mapping platform \(OpenStreetMap base layer with Leaflet.js\) that has selectors that can pass XHR data into the API query string and return a clickable GeoJSON overlay of current or past conditions, running serverless to spin up only on demand. Allow JSON data export from map view \(eg, API call\).

### 4\) Traffic Study Tool

[https://github.com/LouisvilleMetro/WazeCCPProcessor/issues/22](https://github.com/LouisvilleMetro/WazeCCPProcessor/issues/22)

* Provides a traffic study replacement tool that allows comparison of a traffic area before and after a change \(signal retiming, road diet, reconfiguration, traffic events\)

* Working tool showing how Waze data can replace studies that can cost $25K - 75K

* Good to get traffic department, internal gov operations, data analytics buy-in

_Technical: _QuickSight could be used for this \(we welcome other AWS tool recommendations, maybe using Metabase in the cloud\). Louisville currently has a useful working version in Power BI that the traffic department uses that can be recreated in AWS. User selects area of geographic interest, times of day/week, date ranges, direction, and gets a before and after analysis to show effectiveness.

