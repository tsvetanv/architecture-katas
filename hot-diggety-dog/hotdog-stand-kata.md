
# Kata: Hot Diggety Dog!
Reference: [Hot Diggety Dog](https://nealford.com/katas/kata?id=HotDiggetyDog)
Local hot dog stand merchant wants a point-of-sale system for his hot dog stand operators.

## Users
- Fifty or so hot dog stand operators
- Thousands of customers in the local area (via social media)

## Requirements
- Must be lightweight in size (laptop is too unwieldy to use efficiently when making hot dogs on the street)
- Allow for discounts
- Track sales by time and location
- Send inventory updates to mobile inventory-management staff (who drive to the location with supplies)
- Provide a social-media integration so customers can be notified when a hot dog stand is nearby
- Export information in a format importable by accounting tools

## Additional Context
- Forced into undertaking this development because the current hodgepodge ways of tracking sales requires too much manual effort
- Time to completion is important
- Building a solution that won't need replacement in 3 years is more important
- No serious budget constraints

***

# Implementation

## I. Design Scope

### Features (Functional Requirements)
- **Post Sale:** Order content, discount, tax, total price, timestamp, location
- **Get Discount**
- **Post Inventory**
- **Get Inventory**
- **Notify Nearby Stand:** Post location
- **Get Accounting Info**

### Architecture Characteristics (Non-functional Requirements)
- **Availability**
- **Upgradeability**
- **Interoperability**

## II. High-level Design

### 1. API Design
- **Architecture Style:** Microservice Architecture
- **Architecture API Style:** RESTful API
- **App General URL:** `/hotdog/app/api/v1/`
 
| Functional Requirement | API | Description |
|----------|-----------------------------|----------|
| Report sales | `POST /sale/{standId}/{saleId}` | Add a scale |
| Track sales by time and location | `GET /sales`<br>`GET /api/sales/filter-by-time?{start_time}&{end_time}`<br>`GET /api/sales/filter-by-stands?stand={stand_name}`<br>`GET /api/sales/filter-by-stands` | Get sales by different criteria |
|Allow discount in sales|`GET /discounts`|Get discounts|
|Send inventory updates|`GET /inventory/{standId}`<br>`POST /inventory/{standId}`|Get and add an inventory for target stand|
|Post Stand Location|`POST /location/{standId}`|Add location of stand|
|Get Accounting Info|`GET /accounting/info/{standId}`|Export accounting info|



### 2. High-level Design Diagram

Topics for later discussion: 
- **Choice of database**

### 3. Data Model and Schema
- **Data Access Patterns**
- **Read/Write Ratio**
- **QPS (Query per Second):** Sales QPS: 50, Inventory QPS: 50 per hour

- **Data Model**
  
**Sale** entity
| Field | Type | Description |
|----------|-----------------------------|----------|
| sale_id | String | Primary key, unique identifier for each sale. |
| hot_dog_stand_id | String | Foreign key referencing Hot Dog Stand table, indicates which stand made the sale. |
| sale_date | Datetime  | Date and time when the sale occurred. |
| hot_dog_count | int  | Number of hot dogs sold in the sale. |
| discount_id | String  | Foreign key referencing Discount table, optional if there is a discount applied to the sale. |
| total_amount | Double  | Total amount of the sale after applying any discounts. |

**Hot Dog Stand** entity
| Field | Type | Description |
|----------|-----------------------------|----------|
| stand_id | String | Primary key, unique identifier for each hot dog stand. |
| stand_name | String | Name or identifier of the hot dog stand. |
| latitude | Double | Latitude of hot dog stand's location. |
| longitude | Double | Longitude of hot dog stand's location. |

**Discount** entity
| Field | Type | Description |
|----------|-----------------------------|----------|
| discount_id | String | Primary key, unique identifier for each discount. |
| discount_name | String | Name or description of the discount. |
| discount_percentage | Integer | Percentage of discount applied (e.g., 10% off). |
| start_date | Datetime | Date when the discount becomes valid. |
| end_date | Datetime | Date when the discount expires. |

**Hot Dog Item** entity
| Field | Type | Description |
|----------|-----------------------------|----------|
| item_id | String | Primary key, unique identifier for each hot dog item. |
| item_name | String | Name of the hot dog item. |
| quantity | Integer | Quantity of the available item. |

### 4. Rough Estimations
- **Queries**
- **Database:** Entity size, whole DB - scaling

## III. Design Deep Dive
- Review the design & go into details (refer to the list for later discussion)
- Analyze the quality (non-functional) requirements
- Think about problems / bottlenecks. Follow the described approach
- Think about different options/solutions and evaluate their tradeoffs
- Choose a concrete solution for some components (e.g., DB, Load Balancer)

