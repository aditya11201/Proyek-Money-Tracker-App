# SHOULD BE CHANGED
# Proyek Money Tracker App
 This is a submission project learning path Become a Google Cloud Engineer on Modul dicoding platform.

 I use **App Engine** on **Google Cloud Platform** to run [Front-End](https://github.com/dicodingacademy/a133-gcp-labs/tree/money-tracker) and [Back-End](https://github.com/dicodingacademy/a133-gcp-labs/tree/money-tracker-api).

## Scenario
Let's say you are recruited as a Cloud Engineer at a startup that is developing a Money Tracker App. Currently the application is running on an on-premise infrastructure. To save expenses, especially upfront costs (server purchase and maintenance), your company plans to migrate to Google Cloud. The developed application has the following specifications:

- Front-End Web written using PHP with CodeIgniter framework version 3.1.10.
- Back-End API is written using Node.js.

The main feature of this application is to record income and expenses. Users can also upload images with attachments for each record, such as shopping receipts, pay slips, etc. The image is stored in object storage and its URL will be stored in the database. The form of the stored data has been determined so that it has a consistent schema.

This Money Tracker App is worked on by a team of many people, ranging from Project Managers, Developers, Cloud Architect, to Cloud Engineer (in this case, you). Luckily, the source code and cloud architecture design of the Money Tracker App have been completed and are ready for you to execute.

You can get the source code written by the Developer here:

- [Front-End](https://github.com/dicodingacademy/a133-gcp-labs/tree/money-tracker)
- [Back-End](https://github.com/dicodingacademy/a133-gcp-labs/tree/money-tracker-api)
  
Meanwhile, the cloud architecture that has been designed by Cloud Architect is as follows:
![Alt text](/asset/img/cloud-achitecture.png)

So, your job as a Cloud Engineer is to deploy the application to Google Cloud according to the cloud architecture design.

---

### TODO completion steps:

**(Criterion 1)**
- Create new project (format : submission-mgce-participantname.)
![Alt text](/asset/img/new-project.png)
-----

**(Criterion 2) Grant Access Rights to Reviewers**
```sh
> Main Menu -> IAM & Admin -> IAM -> +GRANT ACCESS
```
![Alt text](/asset/img/grant-access.png)

| Setting | Value |
| ------ | ------ |
|new principal | reviewer_googlecloud@dicoding.com|
|select a role | basic -> viewer|

-----
![Alt text](/asset/img/grantaccessreviewer.png)

**(Criterion 3) Deploy Money Tracker App**

**service account settings**
- create service account
```sh
> Main Menu -> IAM & Admin -> service account -> create service account
```
![Alt text](/asset/img/createserviceacc.png)

| setting | value |
| ------ | ------ |
| Service account name | name your project |
| Create and Continue -> Done|

![Alt text](/asset/img/serviceaccdetails.png)

```sh
> click your service account -> keys -> Add key -> create new key -> json
> open file download and copy all
```
![Alt text](/asset/img/privatekeysaved.png)
```sh
> open cloud shell -> open editor -> open terminal
> type git clone -b money-tracker-api https://github.com/dicodingacademy/a133-gcp-labs.git
> rename folder from "a133-gcp-labs" to "money-tracker-api" (to make it easier to identify)
```
it will be something like this

![Alt text](/asset/img/rename.png)

## Open the "money-tracker-api" folder, then Focus on the following file :
- serviceaccountkey. json
- modules -> imgUpload.js (Cloud Storage).
- routes -> record. js (Cloud SQL).
- create_table.sql (table schema).

```sh
> open serviceaccountkey.json and replace with your file download (ctrl+v)
```
**Before**
![Alt text](/asset/img/serviceaccjsonbefore.png)

**After**
![Alt text](/asset/img/serviceaccjsonafter.png)

```sh
> open modules -> imgUpload. js
```
![Alt text](/asset/img/imguploadjs.png)

> // TODO: Customize Storage configuration

> projectId : '<your_project_id>',

> // TODO: Add the name of the bucket used

> const bucketName = '<your_GCS_bucket_name>'

------
**Create bucket**
```sh
> Cloud Storage -> Buckets -> Create
```

| setting | value |
| ------ | ------ |
| Name Your Bucket | <Uniquee Name> |
| Choose where to store your data -> location type -> region | asia-southeast2
| **uncheck** Enforce public access prevention on this bucket |
| Access Control | Fine-grained |
| continue -> create |

name your bucket (here I use my project name) -> then select Region
Lowest latency within a single region -> asia-southeast2 (Jakarta) -> click continue 
![Alt text](/asset/img/asiase2jktname.png)

then do as pictured
![Alt text](/asset/img/createbucket.png)

------
**Add participant**
  
```sh
> menu -> Permissions -> +Grant Access
```
![Alt text](/asset/img/grantaccbucket.png)
  
| setting | value |
| ------ | ------ |
| new participant | allUsers |
| select a role | cloud storage -> storage object viewer |
| Save |

![Alt text](/asset/img/storageobjviewr.png)

  ```sh
> ### open routes -> record. js
```

> // TODO: Customize database configuration
 
>     const connection = mysql. createConnection({
>           host: 'public_ip_sql_your_instance',
>           user: 'root',
>           database: 'your_database_name',
>           password: 'your_sql_password'
>     })

------
**Create Cloud SQL**
 ```sh
> Main menu -> SQL -> create instance -> choose MySQL -> enable API
```
![Alt text](/asset/img/enableapisql.png)

| setting | value |
| ------ | ------ |
| Instance ID | please fill in what you want. |
| Password | fill in according to your wishes. |
| Database version | select MySQL 5.7. |
| Region | asia-southeast2. |
| Zonal availability | select Single zone only. |
| Then, click the arrow on Show Configuration Options |.
| Machine Type | select the machine type Shared core with the option of 1 vCPU, 0.614 GB. |
| Storage | select Storage type HDD and Storage capacity of 10 GB. |
| Connections | Add Network button ->  Name with Public and fill in Network with the range 0.0.0.0/0. |
| Then, click Done and select Save.|
| create|

the settings will be like this
![Alt text](/asset/img/sett-mysql.png)

wait for the build to finish (this will take a few minutes)
![Alt text](/asset/img/waitsqlcreate.png)

```sh
>  SQL -> Databases -> create a database
```
![Alt text](/asset/img/createdb.png)

  
| setting | value |
| ------ | ------ | 
| Database Name | please fill in according to your wishes |
| create |

![Alt text](/asset/img/createdbname.png)

```sh
> Replace //Todo according to the information that was previously made
```


**open website phpmyadmin.co**
| setting | value |
| ------ | ------ |
| server | Public ip your database |
| username | root |
| password | your password |

public ip you can check here
![Alt text](/asset/img/publicipdb.png)
**click go**

```sh
> open cloud shell editor -> create_table.sql -> copy all
> back to phpmyadmin.co click your database name -> SQL menu -> paste on blank board -> go
```
copy query
![Alt text](/asset/img/cpquery.png)

paste in here
![Alt text](/asset/img/imgrunsql.png)
**wait until checklist progress**
![Alt text](/asset/img/querydone.png)
 
```sh
> main menu -> app engine -> create application
```
  
| setting | value |
| ------ | ------ |
| select a region | asia-southeast2
| select service account | select your create service account
| next and wait until finish | 
| i'll do later |

![Alt text](/asset/img/createappeng.png)

```sh
> back to cloud shell editor -> open terminal -> cd /home/aditramadhan65/money-tracker-api -> gcloud app deploy and click 'Y'
```
if you find an error like this
![Alt text](/asset/img/errorconsole.png)

You must change the content of app.yaml -> "service : default"
![Alt text](/asset/img/appyamldefault.png)

wait the process
![Alt text](/asset/img/waituntildone.png)

```sh
> terminal -> gcloud app browse (check next step on your terminal)
> copy  back-end url
view on the backend tab will appear Response Success!
```


---
## Front-end
---
> //TODO
  
> //application -> config -> config.php (base URL of Front-End).
  
> //application -> models -> Record_model.php (base URL of Back-End)

```sh
> open cloud shell editor -> application -> config -> config.php
> Search TODO and replace with URL from Front-End, and the uncomment the code.
```


### deploy your front-end

```sh
> back to cloud shell editor -> open terminal -> git clone -b money-tracker https://github.com/dicodingacademy/a133-gcp-labs.git -> rename folder to "money-tracker" -> cd /home/aditramadhan65/money-tracker -> gcloud app deploy and click 'Y'
> terminal -> _gcloud app browse -s front-end_ (check next step on your terminal)
> copy front-end url and replace
```
![Alt text](/asset/img/changefelink.png)

```sh
> open open cloud shell editor -> application -> models -> Record_model.php
> Search TODO and replace with base URL Back-End
```
![Alt text](/asset/img/changebelink.png)


## LAST TASK

```sh
> back to cloud shell editor -> open terminal -> cd /home/aditramadhan65/money-tracker -> gcloud app deploy and click 'Y'
> terminal -> gcloud app browse -s front-end (check next step on your terminal)
> copy link -> open link in your browser -> check it is running perfectly ?
```
otherwise you'll get an error message like this
![Alt text](/asset/img/internalerror.png)


## Check Submit Permission project
## REFERENCE TUTOR https://github.com/suryaLuqman/submission2-Proyek-Money-Tracker-App.git
  
> ### Don't forget to delete or deactivate services that are no longer needed to prevent additional fees/billing from being required if they are no longer used. 
> ### Here are the steps you can take to Clean Up:
> - ### Click Read more in Notes from Reviewers 
