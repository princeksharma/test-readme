# Commercial Planner Frontend Documentation

This document walks you through the frontend code-base structure. This section include description on initiative creation and plan creation with a channel associated with it. Intiatives are to be created in a unique timeframe with a target EPEP. Target group is associated with initiatives. The plan is created and can be send for approvals.

Also this section talks on how to approve the plans.

### Documentation Chapter Releveance

The following process entailed in the document, is to walk user through the process of how he can create an intitiative and plan. Later he can associate initiatives to plan (many to one)approach developed. Later he can approve the plans.

## Table of Contents

1. [Project Approach](#Project-Approach)
2. [Plan Creation](#Plan-Creation)
   - [Channel Specific Plan Creation](#Channel-Specific-Plan-Creation)
   - [Associating Channel Specific Available Initiative](#Associating-Channel-Specific-Available-Initiative)
3. [Plan Library](#Plan-library)
   - [Listing all the plans](#Listing-all-the-plans)
   - [Search a plan with search input](#Search-a-plan-with-search-input)
   - [Filter the table by selecting particular filter options](#Filter-the-table-by-selecting-particular-filter-options)
   - [Reload the plan listing table](#Reload-the-plan-listing-table)
   - [Send the plans for approvals](#Send-the-plans-for-approvals)
   - [Change rejection status to exercise](#Change-rejection-status-to-exercise)
   - [User can Edit a plan](#User-can-Edit-a-plan)
   - [User can delete a plan](#User-can-delete-a-plan)
   - [Can view the initiatives associated with a plan](#Can-view-the-initiatives-associated-with-a-plan)
   - [Can view a comment](#Can-view-a-comment)
4. [Initiative Creation](#Initiative-Creation)
   - [Channel Specific Initiative Creation](#Channel-Specific-Plan-Creation)
   - [Associating Plan in Initiative](#Associating-Plan-in-Initiative)
5. [Initiative Library](#Initiative-library)
   - [Listing all the initiatives](#Listing-all-the-initiatives)
   - [Search a initiatives with search input](#Search-a-initiatives-with-search-input)
   - [Filter the table by selecting particular filter options](#Filter-the-table-by-selecting-particular-filter-options)
   - [Reload the initiatives listing table](#Reload-the-initiatives-listing-table)
   - [User can Edit a initiative](#User-can-Edit-a-initiative)
   - [User can delete a initiative](#User-can-delete-a-initiative)
6. [Plan Approval](#Plan-Creation)
   - [Approve a plan](#Approve-a-plan)
   - [Reject a the plan](#Reject-a-plan)

## Project Approach

The Commercial planner frontend is utilizing **MSAL (Microsoft Authentication Library )** and **Python's Multiprocessing module** because it enables developers to acquire security tokens from the Microsoft identity platform to authenticate users and access secured web APIs.

Also we are using libraries like ngx Daterangepicker material, ngx papa parse, angular material & router.

The relevance are given below:

**Relevance of using Microsoft Authentication Library (MSAL) library**

- It can be used to provide secure access to Microsoft Graph, other Microsoft APIs, third-party web APIs, or your own web API.
- MSAL supports many different application architectures and platforms including .NET, JavaScript, Java, Python, Android, and iOS.
- No need to directly use the OAuth libraries or code against the protocol in your application.
- Acquires tokens on behalf of a user or application (when applicable to the platform).
- Maintains a token cache and refreshes tokens for you when they're close to expiring. You don't need to handle token expiration on your own.
- Helps you specify which audience you want your application to sign in. The sign in audience can include personal Microsoft accounts, social         identities with Azure AD B2C organizations, work, school, or users in sovereign and national clouds.
- For our project we are using @azure/msal-angular & @azure/msal-browser library.

**Relevance of using Ngx Daterangepicker material**

Ngx Daterangepicker gives the functionality to pick date range from single component calendar or with double component calendar.

**Relevance of using Ngx Papa Parse Library**

Papa Parse wrapper for Angular. Fast parser-library for CSV to JSON and vice versa, with built in support for background-workers. 

**Relevance of using Angular Router(@angular/router) Library**

Defines the Route object that maps a URL path to a component, and the Router Outlet directive that you use to place a routed view in a template, as well as a complete API for configuring, querying, and controlling the router state. You have to import Router Module to use the Router service in your app

**Relevance of using Angular material (@angular/material) Library**

Angular material is a UI component library for angular. It is an in-built responsive design and has standard CSS. UI Components are buttons, loader, text fields, input fields, select dropdowns and many more.

**Relevance of using RxJs Library**

It is defined as a library for composing asynchronous and event-based programs by using observable sequences. It provides one core type, the Observable, satellite types (Observer, Schedulers, Subjects) and operators inspired by Array (map, filter, reduce, every, etc.) to allow handling asynchronous events as collections.

---

## Plan Creation

PAGE URL - {frontend-base-url}/plan-creation

#### Summary: This module is responsible for creation of new plan and associating initiatives with it.

Majorly it is a two step process.

- [Channel Specific Plan Creation](#Channel-Specific-Plan-Creation)
- [Associating Channel Specific Available Initiative](#Associating-Channel-Specific-Available-Initiative)

#### Channel Specific Plan Creation

On landing this page, user can create a plan with a channel via following steps 
Coming to this page channel API will fetch channels and their ID

|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_channel_data*|*{"code":200,"data":[{"channel":"HM", "id":"1"{"channel":"OP","id":"2"}],"status":"success"}*|

Then User has to input plan name first and it should be unique. 

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/unique_plan_name*|*{"plan_name": "test_plan_x"}*|*{ "code": 200, "data": "Unique Plan name", "status": "success"}*|

- User has to select channel from dropdown option. Either HM or OP.
- User can save the plan by clicking on save plan button.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/save_plan*|*{"plan_name": "test_plan_x","channel_id": 1}*|*{"code":200,"data":{"plan_id":93},"status":"success"}*|

#### Associating Channel Specific Available Initiative

On Saving the plan, It will open Initiative Table below based on above channel selection which can be added to plan but it's optional. User has the functionality to add initiatives to plan later also.

When initiative listing table is open below. Below are the api calls to fetch the data from the database.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/plan_initiative_library_filters*|*{"plan_id":"","mode":"new","channel_id":"2","filters":{}}*|*{ code: 200, data: { channels: [2], created_bys: ["Meghraj Tuwar"], created_ons: ["2023-01-03"], elemento_peps: ["M/MXKT/19/MCAJ/05/390"], initiatives: ["initiative_7", "initiative_6"], target_groups: ["OP Target Group 1"], timeframes: ["17-01-2023 - 28-01-2023", "15-01-2023 - 16-01-2023"] }, status: "success" }*|

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/get_list_of_initiatives*|*{"channel_id":"2","offset":0,"limit":10,"filters": {},"mode":"new","plan_id":""}*|*{ code: 200, data: [{ channel_id: 2, created_by: "Meghraj Tuwar", created_on: "2023-01-03", elemento_peps: "M/MXKT/19/MCAJ/05/390", id: "118", initiative_name:"initiative_7", target_group: "OP Target Group 1", timeframe: "17-01-2023 - 28-01-2023" } ], status: "success"}*|

To associate an initiative to the plan, user can select by selecting the checkbox and click on add to plan button, it will do this following api call.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/add_initiative_to_plan*|*{"plan_id": 1,"initiatives": [4,5]}*|*{"code": 200,"data": "Initiatives Added","status": "success"}*|

Then user will be redirected to plan library screen.

---

## Plan Library

PAGE URL - {frontend-base-url}/plan-library

Summary: This module is responsible for listing all the plans that has been created by users with its initiatives(if there is any) in the table.

We have different functionalities associated with plan listing table

- [Listing all the plans](#Listing-all-the-plans)
- [Search a plan with search input](#Search-a-plan-with-search-input)
- [Filter the table by selecting particular filter options](#Filter-the-table-by-selecting-particular-filter-options)
- [Reload the plan listing table](#Reload-the-plan-listing-table)
- [Send the plans for approvals](#Send-the-plans-for-approvals)
- [Change rejection status to exercise](#Change-rejection-status-to-exercise)
- [User can Edit a plan](#User-can-Edit-a-plan)
- [User can delete a plan](#User-can-delete-a-plan)
- [Can view the initiatives associated with a plan](#Can-view-the-initiatives-associated-with-a-plan)
- [Can view a comment](#Can-view-a-comment)

#### Listing all the plans

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/plans_list*|*{"offset":0,"limit":10,"filters":{}}*|*{"code":200,"data":[{"approval_by":"None","approval_status":"Exercise","channel":"OP","comments":"None","comments_flag":"None","created_by":"Prince Sharma","created_on":"2023-01-05","initiatives":[],"plan_id":93,"plan_name":"plan_test_11"},{"approval_by":"None","approval_status":"Approval_Pending","channel":"OP","comments":"None","comments_flag":"None","created_by":"Meghraj Tuwar","created_on":"2023-01-03","initiatives":[{"channel":"OP","created_by":"Meghraj Tuwar","created_on":"2023-01-03","elemento_peps":"M/MXKT/19/MCAJ/05/390","initiative_name":"initiative_5","target_group":"OP Target Group 1","timeframe":"12-01-2023 - 13-01-2023","type_of_sale":"Universal"}],"plan_id":92,"plan_name":"plan_2023_8"}],"status":"success"}*|

#### Search a plan with search input

a) As we type it will hit the partial search API on each input

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/searched_partial_plan*|*{"plan_name":"p","limit":10,"offset":0}*|*{ code: 200, data: [ "plan_test_11", "plan_2023_8" ], status: "success"}*|

b) When we press enter after the input is done or if we click on any plan which is listed in a 	popup (i.e. search list result after partial search), it will fetch the data.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/searched_full_plan*|*{"plan_name":"plan_2023_8","limit":100,"offset":0}*|*{ code: 200, data: { plan_2023_8: { approval_by: "None", approval_status: "Approval_Pending", channel: "OP", comments: "None",created_by: "Meghraj Tuwar", created_on: "2023-01-03",initiatives: [{ channel: "OP", created_by: "Meghraj Tuwar", created_on: "2023-01-03", elemento_peps: "M/MXKT/19/MCAJ/05/390", initiative_name: "initiative_5", target_group: "OP Target Group 1", timeframe: "12-01-2023 - 13-01-2023", type_of_sale: "Universal"},], plan_id: 92, plan_name: "plan_2023_8" } }, status: "success" }*|

#### Filter the table by selecting particular filter options

|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/plans/plan_library_filters*|*{"code":200,"data":{"approved_bys":["Meghraj Tuwar","None"],"created_bys":["Meghraj Tuwar","Prince Sharma"],"created_ons":["2023-01-05","2023-01-03"],"elemento_peps":["M/MXKT/19/MCAJ/05/390"],"initiatives":["initiative_3","initiative_4","initiative_5","initiative_8"],"statuses":["Approval_Pending","Approved","Exercise"],"target_groups":["HM Target group 1","OP Target Group 1"],"timeframes":["19-01-2023 - 20-01-2023","12-01-2023 - 13-01-2023","10-01-2023 - 11-01-2023","07-01-2023 - 09-01-2023"]},"status":"success"}*|

#### Reload the plan listing table

By clicking on reload icon, user can refresh the plan list page and filters back to default.

#### Send the plans for approvals

To send plans for approval, user has to select plans. User can select either one plan or multiple plans and then he has to click on send for approval button.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/send_email_for_approval*|*{"plan_id":[91, 90]}*|*{"code":200,"data":"Email sent successfully.","status":"success"}*|

#### Change rejection status to exercise

If user wants to change the rejection status to back to exercise then user has to click on rejection status then confirmation popup will appear on the screen. User has to click on yes.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/plan_status_change*|*{"plan_id":96}*|*{"code":200,"data":"Plan reject status changed into exercise successfully of plan_id:96.","status":"success"}*|

#### User can Edit a plan

User can edit a plan by clicking on edit icon. User will be redirected to plan creation page with all the values associated with a plan value and initiative lists which can be associated with a plan.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/plan_info*|*{"{"plan_id":96}*|*{"code":200,"data":{"channel_id":2,"channel_name":"OP","initiatives_id_list":[1, 2],"plan_id":99},"status":"success"}*|

Then user can select an initiative also and click on add to plan.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/plan_edit*|*{"plan_id":98,"initiative_id_list":[112, 137]}*|*{"code":200,"data":"Initiative added in plan.","status":"success"}*|

Then he can click on save button if there is any change or he can go back to plan library screen by clicking on back icon.

#### User can delete a plan

If user wants to delete a plan, user has to click on delete icon. It will ask for confirmation. Click yes to proceed.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/plan_delete*|*{"plan_id":95}*|*{"code":200,"data":"Plan deleted successfully of id:95.","status":"success"}*|

#### Can view the initiatives associated with a plan

Users have to click on plan row to view how many inititatives are associated with that plan. If it shows empty row then there is no initiatives associated with it.

#### Can view a comment

User has to click on message icon to see a comment. It show a popup which shows the comment.

---

## Initiative Creation

PAGE URL - {frontend-base-url}/initiative-creation

Summary: This module is responsible for creation of new initiative and associating plan with it. Majorly it has five required options to create an initiative while others are optional

Initiative Creation steps process.

- [Channel Specific Initiative Creation](#Channel-Specific-Plan-Creation)
- [Associating Plan in Initiative](#Associating-Plan-in-Initiative)

#### Channel Specific Initiative Creation

a) Initiative Name
To create an initiative, User has to fill the initiative name input field and it should be unique. Once he inputs the initiative name, Uniqueness checker API will check its uniqueness

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*/api/v1/initiatives/unique_initiative_name*|*{"initiative_name":"test_initiative"}*|*{"code":200,"data":"Unique Initiative name","status":"success"}*|

b) Target EPEP

To get the elemento peps data

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_elemento_pep_data*|*{"limit":1,"elemento_peps_name":""}*|*{code: 200, data: [{description: "211-(366) CC Clasica 355 ml",elemento_peps: 	"M/MXKT/19/MCAJ/05/390",id: "163605"}] status: "success"}*|

User can open the dropdown options and select one of the target EPEP and click on apply. Also User has to select Target EPEP from dropdown. User has the functionality to search EPEP also 	from input field.

To search an EPEP user has to input epep name

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_elemento_pep_data*|*{"elemento_peps_name":"M/MXKT/19/MCDB/06/490","limit":1}*|*{"elemento_peps_name":"M/MXKT/19/MCDB/06/490","limit":1}*|

c) Channel

To get the channel data

|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_channel_data*|*{"code":200,"data":[{"channel":"HM","id":"1"}, {"channel":"OP","id":"2"}],"status":"success"}*|

If plan is already selected before this process then channel will be updated according to the channel associated with that plan else user can open the dropdown options and select one of the Channel and click on apply.

d) Target group

To get the target group data

|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_channel_data*|*{"code":200,"data":[{"channel":"HM","id":"1"}, {"channel":"OP","id":"2"}],"status":"success"}*|

User has two ways to fill the target group.

- User can directly select target group from dropdown
- User can upload channel_name.csv file and then input an unique target group name (there will be an uniqueness checker API to check if target name is unique or not ) additional there is download button to download the sample channel file (file upload should 	have same column)

e) Time Frame

User can select the time frame. The start date and end Date should not clash with any previously created Target EPEP in a particular time frame  
When user will enter the end date. It will check via this API

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/epep_permissions*|*{"epep_name":"","start_date":"05-01-2023","end_date":"12-01-2023","flag":0,"initiative_id":0}*|*{"code":200,"data":"Allowed","status":"success"}*|

f) Notes

User has the functionality to add a note. It's optional.

g) Initiative flag (Optional, by default no is selected)

User has the option to update/toggle initiative flag {0: no, 1: yes}

i) Type of sale (Optional, by default Universal is selected)

To get the type of sale data

|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_type_of_sales*|*{"code":200,"data":[{"id":101,"type_of_sale":"Universal"}, {"id":121,"type_of_sale":"Specialized"}],"status":"success"}*|

User has the option to update/toggle type of sale {Either universal or specialized}

j) Priority (Optional, by default 3rd is selected)
To get the priority data
|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/get_priority*|*{"code":200,"data":[{"id":101,"priority":"1A"},{"id":111,"priority":"2A"}, {"id":121,"priority":"3A"},{"id":131,"priority":"4A"}],"status":"success"}*|

User has the functionality to set the priority from dropdown. Select one of priority options and then click on apply.

#### Associating Plan in Initiative

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/get_list_of_plans*|*{"flag":0,"channel_id":0}*|*{"code":200,"data":[{"channel_id":2,"id":114, "plan_name":"Plan_test"}],"status":"success"}*|

User can select a plan from dropdown before saving the initiative. User has another option to add a plan in initiative either by editing it or during plan creation as well as plan edit. 

Last step is to save the initiative.
Then finally user can save the initiative by clicking on save initiative button

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/save_initiative*|*{"initiative_name": "initiativee_2023_9","plan_name": "","channel_id": 	"1","target_group_id": "","target_group_name": "HM target group 14","file_link": 	"https://kofstoragepvp.blob.core.windows.net/kofcommplanner-uat/mexico/target_group/04-	01-2023/hm.csv","elemento_peps_id": "163605","timeframe_begin": "26-03-	2023","timeframe_end": "27-03-2023","type_of_sale": "Universal","type_of_sale_code": 	101,"priority": "3A","priority_code": 121,"initiative_flag": 0,"note": "created by test."}*|*{"code": 200, "status": "success", "data": "Initiative Saved"}*|

After successfully saving the initiative, user will be redirected to Initiative library screen 

---

## Initiative Library

PAGE URL - {frontend-base-url}/initiative-library

Summary: This module is responsible for listing all the initiatives that has been created by users in the table.

Here we have different functionalities associated with plan listing table.

- [Listing all the initiatives](#Listing-all-the-initiatives)
- [Search a initiatives with search input](#Search-a-initiatives-with-search-input)
- [Filter the table by selecting particular filter options](#Filter-the-table-by-selecting-particular-filter-options)
- [Reload the initiatives listing table](#Reload-the-initiatives-listing-table)
- [User can Edit a initiative](#User-can-Edit-a-initiative)
- [User can delete a initiative](#User-can-delete-a-initiative)

#### Listing all the initiatives

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/initiatives_list*|*{"offset":0,"limit":10,"filters":{}}*|*{"code":200,"data":[{"channel":"OP","created_by":"Meghraj Tuwar","created_on":"2023-01-03","elemento_peps":"M/MXKT/19/MCAJ/05/390","id":"119","initiative_flag":"0","initiative_name":"initiative_8","plan_name":"plan_2023_7","status":"1","target_group":"OP Target Group 1","timeframe":"19-01-2023 – 20-01-2023"},{"channel":"HM","created_by":"Meghraj Tuwar","created_on":"2023-01-03","elemento_peps":"M/MXKT/19/MCAJ/05/390","id":"115","initiative_flag":"0","initiative_name":"initiative_4","plan_name":"plan_2023_4","status":"2","target_group":"HM Target group 1","timeframe":"10-01-2023 - 11-01-2023"}],"status":"success"}*|

#### Search a initiatives with search input


   a) As we type it will hit the partial search API on each input

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/searched_partial_initiative*|*{"initiative_name":"in","limit":10,"offset":0}*|*{"code":200,"data":["initiative_8", "initiative_7"],"status":"success"}*|

b) When we press enter after the input is done or if we click on any initiative which is listed in a 	popup (i.e. search list result after partial search), it will fetch the data.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/searched_full_initiative*|*{"initiative_name":"initiative_8","limit":100,"offset":0}*|*{"code":200,"data":[{"channel":"OP","created_by":"Meghraj Tuwar","created_on":"2023-01-03","elemento_peps":"M/MXKT/19/MCAJ/05/390","initiative_id":119,"initiative_name":"initiative_8","plan_name":"plan_2023_7","status":2,"target_group":"OP Target Group 1","timeframe":"19-01-2023 - 20-01-2023"}],"status":"success"}*|

#### Filter the table by selecting particular filter options

|**Request URL(GET)**|**Response**|
|------------|--------------------|
|*{backend-base-url}/api/v1/initiatives/initiative_library_filters*|*{"code":200,"data":{"channels":["HM","OP"],"created_bys":["Meghraj Tuwar"],"created_ons":["2023-01-03"],"elemento_peps":["M/MXKT/19/MCAJ/05/390"],"initiatives": ["initiative_8","initiative_7","initiative_6","initiative_5","initiative_4","initiative_3","initiative_2","initiative_1"],"plans": [null,"plan_2023_3","plan_2023_4","plan_2023_7","plan_2023_8"],"target_groups":["OP Target Group 1","HM Target group 1"],"timeframes":["19-01-2023 – 20-01-2023","17-01-2023 - 28-01-2023","15-01-2023 - 16-01-2023","12-01-2023 - 13-01-2023","10-01-2023 - 11-01-2023","07-01-2023 - 09-01-2023","05-01-2023 - 06-01-2023","03-01-2023 – 04-01-2023"]},"status":"success"}*|

#### Reload the initiatives listing table

By clicking on reload icon, user can refresh the initiatives list page and filters back to default.

#### User can Edit a initiative


   User can edit a plan by clicking on edit icon. User will be redirected to plan creation page with all the values associated with a plan for edit. User can update the following fields

- Time frame
- Notes
- Initiative flag
- Type of sale
- Plan
   Then he can click on save button if there is any change or he can go back to initiative library screen by clicking on back icon.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/initiative_edit*|*{"initiative_name":"op_2_init_12","plan_name":"plan_2023_8","channel_id":2,"target_group_id":130,"target_group_name":"OP Target Group 3","file_link":"https://kofstoragepvp.blob.core.windows.net/kofcommplanner/mexico/target_group/13-01-2023/op.csv","elemento_peps_id":163605,"timeframe_begin":"03-04-2023","timeframe_end":"04-04-2023","type_of_sale":"Universal","type_of_sale_code":101,"priority":"3A","priority_code":121,"initiative_flag":1,"note":"created by abc. Edited by prince","initiative_id":138,"plan_id":92}*|*{"code":200,"data":"initiatives edited successfully of initiatives_id:138.","status":"success"}*|

#### User can delete a initiative

If user wants to delete an initiative, user has to click on delete icon. It will ask for confirmation. Click yes to proceed.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/initiatives/initiative_delete*|*{"initiatives_id":"134"}*|*{"code":200,"data":"initiatives deleted successfully of initiatives_id:134.","status":"success"}*|

---

## Plan Approval

PAGE URL - {frontend-base-url}/plan-approval

Summary: This module is responsible for listing all the plans for approval with status as approval pending. Only user which is listed in approver group can access this url. Users who are not listed in approver group will be redirected  back to main plan library screen.

Majorly it has two process.

- [Approve a plan](#Approve-a-plan)
- [Reject a plan](#Reject-a-plan)

#### Approve a plan

User can select a plan by clicking on checkbox and click on approve button if he wants to 		approve any plan. Once he clicks on approve button. A confirmation popup will appear where he can either click on No button if he by mistake clicks on approve button or he can click on Yes button to proceed further. After clicking on Yes button another popup will appear on the screen to fill a comment a reason for approving this plan.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/commercial_planner/approve_plan*|*{"plan_id":[30],"flag":1,"comments":"Approved. Plan is selected.","comments_flag":1}*|*{"code":200,"data":"Response recorded successfully.","status":"success"}*|

#### Reject a plan

User can select a plan by clicking on checkbox and click on reject button if he wants to 	reject 	any plan. Once he clicks on approve button. A confirmation popup will appear where he can 	either click on No button if he by mistake clicks on reject button or he can click on Yes button 	to proceed further. After clicking on Yes button another popup will appear on the screen 	to fill a 	comment a reason for rejecting this plan.

|**Request URL(POST)**|**Payload**|**Response**|
|------------|----------|--------------------|
|*{backend-base-url}/api/v1/plans/save_reject_plan_comments*|*{"plan_id":29,"flag":2,"comments":"Rejected. Plan is not successful. ","comments_flag":1}*|*{"code":200,"data":"Comments of rejected plan saved.","status":"success"}*|

---
