
# Oracle JSON 

## Introduction

JSON(JavaScript Object Notation) is a syntax for storing and exchanging data. When exchanging data between a browser and a server, the data can only be text.
JSON is text, and we can convert any JavaScript object into JSON, and send JSON to the server. We can also convert any JSON received from the server into JavaScript objects.
This way we can work with the data as JavaScript objects, with no complicated parsing and translations.


## Before You Begin

**What Do You Need?**

This lab assumes you have completed the following labs:
- Lab 1:  Login to Oracle Cloud
- Lab 2:  Generate SSH Key
- Lab 3:  Create Compute instance 
- Lab 4:  Environment setup
- Note :  All scripts for this lab are stored in the /u01/workshop/json folder and are run as the oracle user.

## Step 1: Insert a record.

  a. **Take a count of the rows in the json table-**

  ````
    <copy>
    select count(*) from purchase_order;
    </copy>
  ````
    
  ![](./images/insert_json.PNG " ")  

 b. Insert a record.

  ````
    <copy>
    INSERT INTO purchase_order
  VALUES (
    SYS_GUID(),
to_date('05-MAY-2020'),
    '{"PONumber"             : 10001,
      "Reference"            : "SBELL-20141017",
      "Requestor"            : "Sarah Bell",
      "User"                 : "SBELL",
      "CostCenter"           : "A50",
      "ShippingInstructions" : {"name"    : "Sarah Bell",
                  "Address" : {"street"  : "200 Sporting Green",
                              "city"    : "South San Francisco",
                              "state"   : "CA",
                              "zipCode" : 99236,
                        "country" : "United States of America"},
                        "Phone"   : "983-555-6509"},
      "Special Instructions" : "Courier",
      "LineItems"            : [{"ItemNumber" : 1,
             "Part"       : {"Description" : "Making the Grade",
                             "UnitPrice"   : 20,
                             "UPCCode"     : 27616867759},
             "Quantity"   : 8.0},
                              {"ItemNumber" : 2,
             "Part"       : {"Description" : "Nixon",
                                "UnitPrice"   : 19.95,
                                "UPCCode"     : 717951002396},
                                 "Quantity"   : 5},
                                {"ItemNumber" : 3,
          "Part"       : {"Description" : "Eric Clapton: Best Of 1981-1999",
                               "UnitPrice"   : 19.95,
                               "UPCCode"     : 75993851120},
                                 "Quantity"   : 5.0}
                                ]}');

    
             </copy>
  ````
   
The above insert query is also available as a sql file in the directory “/u01/workshop/json”.
The script is called as insert.sql. You can run this connecting to the SQL prompt.

Set your oracle environment and connect to PDB as **oracle** user.
 ````
    <copy>
. oraenv
ConvergedCDB
sqlplus appjson/Oracle_4U@JXLPDB
@insert.sql
</copy>
   ````
    


c. **Verify the count after insert.**

  
  ````
    <copy>
   Select * from purchase_order j where j.po_document.PONumber=10001;
    
    </copy>
   ````
    
    
  ![](./images/json.PNG " ")
   
  **Note:** Please copy the red highlighted id which we will use in our next section of update query.

## Step 2: Update a Table.
  We can use Oracle SQL function json-mergepatch or PL/SQL object-type method json-mergepatch() to update specific portions of a JSON document. In both cases we provide a JSON Merge Patch document, which declaratively specifies the changes to make to a a specified JSON document. JSON Merge Patch is an IETF standard.    
   
   **Note:** In the above update query replace the id which we copied in previous step.

   ````
    <copy>
    update purchase_order
     set    PO_DOCUMENT = json_mergepatch ( 
         PO_DOCUMENT,
         '{
           "Requestor" : "MSDhoni"
         }'
       )
    where id ='ID_copied_from_previous_step';

    </copy>
    
  ````
 ![](./images/json_lab7_6.PNG " ")





## Acknowledgements

- **Authors** - Balasubramanian Ramamoorthy, Arvind Bhope
- **Contributors** - Laxmi Amarappanavar, Kanika Sharma, Venkata Bandaru, Ashish Kumar, Priya Dhuriya, Maniselvan K.
- **Team** - North America Database Specialists.
- **Last Updated By** - Kay Malcolm, Director, Database Product Management, June 2020
- **Expiration Date** - June 2021   

**Issues-**
Please submit an issue on our [issues](https://github.com/oracle/learning-library/issues) page. We review it regularly.
      

