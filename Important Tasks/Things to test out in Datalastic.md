
### **Live Vessel Tracking**
- Real-time ship tracking API has 3 Endpoints to make sure our API is as flexible as possible:
	**1. Basic:** `/vessel` - very fast and has all the most popular real-time tracking data such as location, speed, course, destination, type, subtype, and more. The average API request response time is **0.005 sec**.
	
	**2. Pro:** `/vessel_pro` - provides all data from Basic version plus extended additional data apart. Pro version provides Estimated Time of Arrival, Actual Time of Departure, current draft, standardized destination name. The average API request response time is **0.9 sec**.
	
	**3. Bulk:** `/vessel_bulk` -  lets you call up to 100 vessels in a single string. It has all data as in the Basic real-time tracking.
	
	Choose if you need to have fast and the most essential data to achieve an excellent response time speed or you require extended data with the Pro version.


### **Location Tracking API**
Location Traffic Tracking API allows you to scan an area in the sea or ocean to see all the ships in your selected area. This API Endpoint lets you find and track all the vessels in the radius and monitor maritime traffic in your zone.

You can use Static or Dynamic Location Tracking.
- **Static Location Tracking** :  Choose a static point on the map for example select a port by its UN/LOCODE or by defining it longitude and latitude and select the radius around that point to see all vessels that are currently in your selected zone.
- **Dynamic Location Tracking** - Instead of defining the longitude and latitude of some point on earth, you can choose a vessel by its MMSI or IMO number to be the center point and select the radius around it. In this way you can track all vessels approaching your selected ship and see all vessels approaching the zone around it.

### **Historical Vessel Data API**

At this moment there are 2 endpoints with historical data:

- [**/vessel_history** - Ship History Data API](https://datalastic.com/api-reference/#historical-vessel-data-api). Search by vessel and timestamp. See vessel location and other events throughout your chosen dates.
- [**report "inradius_history"**](https://datalastic.com/api-reference/#historical-location-data-report) - Historical Location API. Choose a specific location and a timestamp and see all vessels that pass through your selected zone in the selected timeframe.
	```
	https://api.datalastic.com/api/v0/vessel_history?api-key={YOUR_API_KEY}&{PARAMETER}={PARAMETER_VALUE}&from={YYYY-MM-DD}&to={YYYY-MM-DD
	```
### **Ship History Data API**

Datalastic saves its data from Live Ship Tracking API and stores the data on its servers. You can access the data easily and see vessel historical movement, activities speed, course, heading, destination and more.
Historical data API records are available since 10th August 2021.

Specify the time frame you want to receive the data for your vessel and view past movements of your ship

> https://api.datalastic.com/api/v0/vessel_history?api-key={YOUR_API_KEY}&{PARAMETER}={PARAMETER_VALUE}&days={NUMBER_OF_DAYS}  

##### Request Parameters:

api-key - your API Key sent to you via email upon your successful registration

uuid– Vessel UUID number. Use as {PARAMETER}

mmsi– Vessel MMSI number. Use as {PARAMETER}

imo– Vessel IMO number. Use as {PARAMETER}

days– an optional filter, shows data for the last {NUMBER_OF_DAYS} from today. For example, days=7 will show you the vessel data for the last 7 days from today.

from to - an optional filter, define a specific time frame in the past when the data is required in YYYY-MM-DD format. 30 days maximum since from.

Note: To optimize data storage, we save only new data updates in historical endpoints. That means that if a vessel was docked for a few days and the data has not changed on that vessel, there are no new records for those dates as the data is the same.

```json
{
  "data": {
  "uuid": "9c4c0a78-0dca-312c-f851-65d59e55b8dc",
  "name": "PHENIX LEGEND",
  "mmsi": "431661000",
  "imo": "9595539",
  "eni": null,
  "country_iso": "JP",
  "type": "Tanker",
  "type_specific": "LPG Tanker",
  "positions": [
    {
      "lat": 34.560583333333334,
      "lon": 134.15901666666667,
      "speed": 0.1,
      "course": 114,
      "heading": 101,
      "destination": ">JP TNO OFF",
      "last_position_epoch": 1724972345,
      "last_position_UTC": "2024-08-29T22:59:05Z"
    },
    {
      "lat": 34.56061666666667,
      "lon": 134.15908333333334,
      "speed": 0.1,
      "course": 3,
      "heading": 104,
      "destination": ">JP TNO OFF",
      "last_position_epoch": 1724970723,
      "last_position_UTC": "2024-08-29T22:32:03Z"
    },
    {
      "lat": 34.5604,
      "lon": 134.15906666666666,
      "speed": 0,
      "course": 23,
      "heading": 96,
      "destination": ">JP TNO OFF",
      "last_position_epoch": 1724969102,
      "last_position_UTC": "2024-08-29T22:05:02Z"
      }
    ]
  },
  "meta": {
    "duration": 0.041392525,
    "endpoint": "/api/v0/vessel_history",
    "success": true
  }
}
```

## **Ship Specs Info**

There's two endpoints that allows you to get detailed data about a vessel specifications:

- [Ship Specs Info API](https://datalastic.com/api-reference/#ship-specs-info-api) "Vessel_info": Through a single API request you can retrieve all the detailed data about a vessel specifications.
- [Ship Specs Info](https://datalastic.com/api-reference/#vessel-report) "Vessel_list": Download a full list of vessels with the detailed specifications. You can check the [reports endpoint](https://datalastic.com/api-reference/#generate-reports) to follow the instructions and download your own report.

### **Ship Specs Info API**

Get detailed data about a vessel specifications. Find ships name, IMO, MMSI, UUID, Call Sign, country, type, subtype, draft, TEU unit of cargo capacity, deadweight, liquid gas capacity, length, breadth, average draught, maximum draught, average speed, maximum speed, year built, homeport, activity status.

https://api.datalastic.com/api/v0/vessel_info?api-key={YOUR_API_KEY}&mmsi=566093000

```json
{
 "data":
{
 "uuid":"b8625b67-7142-cfd1-7b85-595cebfe4191",
 "name":"MAERSK CHENNAI",
 "name_ais":"MAERSK CHENNAI",
 "mmsi":"566093000",
 "imo":"9525338",
 "eni":null,
 "country_iso":"SG",
 "country_name":"Singapore",
 "callsign":"9V9409",
 "type":"Cargo",
 "type_specific":"Container Ship",
 "gross_tonnage":50869,
 "deadweight":68898,
 "teu":"4500",
 "liquid_gas":null,
 "length":249.12,
 "breadth":37.4,
 "draught_avg":10.021295387634936,
 "draught_max":14.2,
 "speed_avg":7.4,
 "speed_max":20,
 "year_built":"2011",
 "is_navaid":false,
 "home_port":"SINGAPORE"
 },

"meta":{
"duration":0.002185789,
"endpoint":"/api/v0/vessel_info",
"success":true
 }
  }



```


###  **Vessel Finder API**

You can search and withdraw all vessels by type, similar names, draught, deadweight, year built, and more. You dont need to know vessel MMSI or IMO for this endpoint, just filter the search criteria and receieve the API response with all vessels matching your parameters.

There is overuse protection installed in a form of pagination. The Response retrieves 500 results maximum, to view the following 500 results you need to embed the "next" parameter in meta.

## **Vessel Finder API**

You can search and withdraw all vessels by type, similar names, draught, deadweight, year built, and more. You dont need to know vessel MMSI or IMO for this endpoint, just filter the search criteria and receieve the API response with all vessels matching your parameters.

There is overuse protection installed in a form of pagination. The Response retrieves 500 results maximum, to view the following 500 results you need to embed the "next" parameter in meta. You can find a detailed explanation below.

> **Imp** : https://datalastic.com/api-reference/#:~:text=API%20Key%20Usage%20and%20Credits%20Monitoring


To understand, the JSON files properties : 
> https://datalastic.com/api-reference/#:~:text=Maritime%20Values%20Legend

| Value               | Legend                                                                                                                                       | Example                  |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| name                | Vessel name                                                                                                                                  | MAERSK CHENNAI           |
| mmsi                | [Maritime Mobile Service Identity](https://en.wikipedia.org/wiki/Maritime_Mobile_Service_Identity) 9 digits number for a ship identification | 566093000                |
| eta_UTC             | Estimated time of arrival in UTC                                                                                                             | "2021-06-11T16:49:00Z"   |
| eta_epoch           | Estimated time of arrival                                                                                                                    | 1624127400               |
| draught_avg         | Current reported draft of the vessel                                                                                                         | 9.5                      |
| draught_max         | Maximum draught of the vessel                                                                                                                | 10.5                     |
| speed_max           | Maximum speed of the vessel                                                                                                                  | 46.3KM/H                 |
| speed_avg           | Current reported speed of the vessel                                                                                                         | 37.0KM/H                 |
| atd_UTC             | Actual time of departure                                                                                                                     | "2021-06-11T16:49:00Z"   |
| imo                 | [International Maritime Organization](https://en.wikipedia.org/wiki/IMO_number) 7 digits number for a ship identification                    | 9525338                  |
| eni                 | European Number of Identification                                                                                                            | 234456177                |
| country_iso         | Vessel country name                                                                                                                          | SG                       |
| country_name        | Full country name                                                                                                                            | Singapore                |
| type                | Vessel Type                                                                                                                                  | Cargo – Hazard A (Major) |
| type_specific       | Vessel subtype                                                                                                                               | Container Ship           |
| lat                 | Current vessel latitude                                                                                                                      | 6.303473                 |
| lon                 | Current vessel longitude                                                                                                                     | 3.201678                 |
| speed               | Current vessel speed in knots                                                                                                                | 5                        |
| navigational_status | Vessel activity status                                                                                                                       | Under way using engine   |
| course              | Current vessel course in degrees                                                                                                             | 326                      |
| heading             | Current vessel heading in degrees                                                                                                            | 226                      |
| destination         | Typically a port name of the next planned stop                                                                                               | ROTTERDAM                |
| callsign            | Vessels number for vessel identification                                                                                                     | 9V9409                   |
| gross_tonnage       | Gross Registered Tonnage, is a ship’s total internal volume expressed in “register tons”, each of which is equal to 100 cubic feet (2.83 m3) | 50869                    |
| deadweight          | A measure of how much weight a ship can carry in tonnage                                                                                     | 61614                    |
| teu                 | Container ship capacity is measured in twenty-foot equivalent units                                                                          | 4500                     |
| liquid_gas          | Ship liquid gas capacity                                                                                                                     | null                     |
| length              | Vessel length in meters                                                                                                                      | 249.12                   |
| breadth             | Vessel breadth in meters                                                                                                                     | 37.4                     |
| year_built          | Year the vessel was built                                                                                                                    | 2011                     |

# **What all is included in Add-ons**

-  Maritime Companies Report & API
    
- N Vessel Ownership Report & API
    
- N Inspections & Detention Report & API
    
- N Dry Dock Dates Report & API
    
- N Ship Demolitions Data Report & API
    
- N Ship Casualties Data Report & API
    
- N Vessel Engine Data Report & API
    
- N Classification Societies Report & API
    
- N Route Tracking API      
    
- N SAT-E Tracking API


## **Sea Routes API  **

Determine the optimal route between two points. Points can be specified using coordinates, port UUIDs, or UN/LOCODEs.

#####  Example for Sea Routes API Request:

> https://api.datalastic.com/api/ext/route?api-key=[API_KEY]&{PARAMETER}={PARAMETER_VALUE}

##### Request Parameters:

api-key - Your API Key sent to you via email upon your successful registration

lat_from, lon_from- Origin point latitude, longitude

lat_to, lon_to- Origin point latitude, longitude

port_uuid_from- Origin port UUID

port_unlocode_from- Origin port UNLOCODE

port_uuid_to- Destination port UUID

port_unlocode_to- Destination port UNLOCODE

Here's an example of an API request using the parameters:

`https://api.datalastic.com/api/ext/route?api-key=[API_KEY]&port_unlocode_from=FIHEL&port_unlocode_to=EETLL`


## **SAT-E API**

Get ship position data even when the vessel is out of AIS range as if tracked by satellite. Estimated positions are intelligently calculated using the last known location, destination port, and route-based ETA.

#####  Example for SAT-E API Request:

> https://api.datalastic.com/api/ext/vessel_pro_est?api-key=[API_KEY]&{PARAMETER}={PARAMETER_VALUE}

##### Request Parameters:

api-key- Your API Key sent to you via email upon your successful registration

uuid– Vessel UUID number

mmsi– Vessel MMSI number

imo– Vessel IMO number

Here's an example of an API request using the parameters:

`https://api.datalastic.com/api/ext/vessel_pro_est?api-key=[API_KEY]&imo=9156462`

##### SAT-E API Response Example:



# **Comparison between Experimenter and Developer Pro**
|Feature|Experimenter|Developer Pro+|
|---|---|---|
|**Monthly Credits**|80,000 credits/month [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|Unlimited credits [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|
|**Monthly Price**|₹60,883/month [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|₹72,653/month [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|
|**Annual Price**|₹81,748/month (billed annually) [](https://datalastic.com/pricing/)​|Not specified|
|**Standard APIs**|All API endpoints included [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|All API endpoints included [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|
|**Add-Ons Available**|₹90,843/month with add-ons [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|₹101,543/month with add-ons [](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​|

## Key Differences

**Credit Allocation**: The Experimenter plan provides 80,000 credits per month, which resets at the beginning of each billing cycle. Developer Pro+ offers unlimited credits with no monthly cap.[](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​

**Price Difference**: Developer Pro+ costs approximately ₹11,770 more per month (₹72,653 vs ₹60,883) but removes all credit limitations. For high-volume usage scenarios, the unlimited access can provide better value despite the higher base price.[](https://datalastic.com/blog/the-best-maritime-api-plan-compare-features-pricing/)​

**Use Case**: The Experimenter plan suits moderate-scale projects tracking a defined number of vessels or making predictable API calls. Developer Pro+ is ideal for applications requiring extensive vessel tracking, frequent historical data requests, or unpredictable usage patterns where credit limits would be restrictive.[](https://datalastic.com/pricing/checkout-all-in-developer-pro-yearly/)​


- References : 
  https://datalastic.com/api-reference/
