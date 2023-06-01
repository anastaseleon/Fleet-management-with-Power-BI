# Fleet-management-with-Power-BI
In this project, I leveraged Power BI to address several challenges related to fleet management within a transport company. The steps undertaken in this project include understanding the problem, gathering the requirements, creating a model, cleaning the data, and finally, creating measures and visualizations.

## Problem Statement
Our transport company faced a multitude of inefficiencies and challenges:

- **Inefficient Fleet Management**: Our vast fleet of trailers was being managed without the aid of real-time information and automated management mechanisms, leading to inefficient operations.

- **Inconsistent TMS Data**: Our Transport Management System (TMS) frequently contained inconsistencies, requiring constant verification and checks, thus draining our resources.

- **Over-reliance on Manual Methods**: Managers relied heavily on daily reports and manual yard checks to obtain clear information about the status of trailers and their freight, a process that was both inefficient and prone to human error.

- **Delayed Data Utilization**: Our approach did not allow for real-time use of data. The team had to wait for manually curated reports, hampering immediate decision-making.

- **Manual Analysis and Updates**: Our current process involved manual analysis of data, updating it into Excel spreadsheets, and having a coordinator fix inconsistencies in the TMS. Although functional, this method was extremely time-consuming.

- **Lack of Real-Time Information**: Despite our best efforts, our management team was unable to access real-time information, slowing down the decision-making process and negatively impacting operational efficiency.

## Approach
Since the management of the transport company needed a fast and modulable solution, We Borowed a few concept from the Agile methodologie.
We gather the folowing requirements :
| User Story # | User Story | Priority |
|--------------|------------|----------|
| User Story 1: VP of Operations | As a VP of Operations, I want to see high-level metrics about trailer utilization, so that I can make informed strategic decisions about our operations. | $$ |
| User Story 2: Director of Operations | As a Director of Operations, I want to be informed on the availability of equipment, so that I can effectively manage resources and plan operations. | $$$ |
| User Story 3: Warehouse Manager | As a Warehouse Manager, I want to have a view of the progress of work and amount of work that needs to be done, so that I can manage warehouse operations efficiently. | $$ |
| User Story 4: Warehouse Foreman | As a Warehouse Foreman, I want to know about the work my team needs to get done, so that I can assign tasks effectively and manage my team's workload. | $$$ |
| User Story 5: Customer Service Manager | As a Customer Service Manager, I want to know the overall service level and have a peek at work in progress for the customer, so that I can ensure high service standards and effectively manage customer expectations. | $$ |
| User Story 6: Customer Service Representative | As a Customer Service Representative, I want to be able to answer customers about the status of freight inside a trailer, so that I can provide timely and accurate information to customers. | $$$ |



## Data Cleaning and features engineering
i curently don<t have authorisation to share the raw data for the project  but i will mdo my best to create a similar dataset in the furtur
 Here is a description of  the data that we used for this project
                 
**TMS_Query**: This table contains a query from our database and holds information about all the company's equipment.       

  

```powerquery
let
    Source = Sql.Database("srvmsqltrsp1", "TMS", [Query="Select * from [TMS].[dbo].[Vw_Web_TrailersFrames]"]),
    #"Filtered Rows" = Table.SelectRows(Source, each ([IsTrailer] = 1) and ([DateLastUsed] <> null) and ([Rem_Actif] = 1) and ([AddressLastUsed] <> "Ongoing - 1015 GOLF LINKS RD, ANCASTER (ON)" and [AddressLastUsed] <> "Ongoing - 1070 REST ACRES RD, PARIS (ON)" and [AddressLastUsed] <> "Ongoing - 11250 Chemin de la Côte-de-Liesse, LACHINE (QC)" and [AddressLastUsed] <> "Ongoing - 11775 BRAMALEA RD, BRAMPTON (ON)" and [AddressLastUsed] <> "Ongoing - 1200 50e Avenue, LACHINE (QC)" and [AddressLastUsed] <> "Ongoing - 1200 BRANT ST UNIT A4, BURLINGTON (ON)" and [AddressLastUsed] <> "Ongoing - 145 CARRIER DRIVE, ETOBICOKE (ON)" and [AddressLastUsed] <> "Ongoing - 1735 KIPLING AVE, ETOBICOKE (ON)" and [AddressLastUsed] <> "Ongoing - 1800 46, LACHINE (QC)" and [AddressLastUsed] <> "Ongoing - 199 Summerlea Road, BRAMPTON (ON)" and [AddressLastUsed] <> "Ongoing - 200 Westcreek Blvd, BRAMPTON (ON)" and [AddressLastUsed] <> "Ongoing - 2054 Joyceville Rd, JOYCEVILLE (ON)" and [AddressLastUsed] <> "Ongoing - 206497 HWY 26, MEAFORD (ON)" and [AddressLastUsed] <> "Ongoing - 20775 RUE DAOUST, SAINTE ANNE DE BELLEVUE (QC)" and [AddressLastUsed] <> "Ongoing - 2190 BOUL DAGENAIS OUEST, LAVAL (QC)" and [AddressLastUsed] <> "Ongoing - 3200 Chemin de la baronne, VARENNES (QC)" and [AddressLastUsed] <> "Ongoing - 3353 BOUL DES SOURCES, DOLLARD DES ORMEAUX (QC)" and [AddressLastUsed] <> "Ongoing - 3465 WYECROFT RD, OAKVILLE (ON)" and [AddressLastUsed] <> "Ongoing - 3500 FAIRWAY 7275, LACHINE (QC)" and [AddressLastUsed] <> "Ongoing - 3500 RUE FAIRWAY, LACHINE (QC)" and [AddressLastUsed] <> "Ongoing - 3520 BOUL ST-JOSEPH EST, MONTREAL (QC)" and [AddressLastUsed] <> "Ongoing - 375 CHEMIN SAINT FRANCOIS OUEST, SAINT FRANCOIS DE MONTMAGNY (QC)" and [AddressLastUsed] <> "Ongoing - 381 BOUL DES LAURENTIDES, LAVAL (QC)" and [AddressLastUsed] <> "Ongoing - 4041 RUE WELLINGTON, VERDUN (QC)" and [AddressLastUsed] <> "Ongoing - 4145 Saint-Elzear Ouest, LAVAL (QC)" and [AddressLastUsed] <> "Ongoing - 5555 Royalmount, MONT ROYAL (QC)" and [AddressLastUsed] <> "Ongoing - 60 DRIVER RD, BRAMPTON (ON)" and [AddressLastUsed] <> "Ongoing - 666 WEST SAINT-MARTIN BOULEVARD, LAVAL (QC)" and [AddressLastUsed] <> "Ongoing - 700 AV BEAUMONT, MONTREAL (QC)" and [AddressLastUsed] <> "Ongoing - 7380 Bren Road, MISSISSAUGA (ON)" and [AddressLastUsed] <> "Ongoing - 9501 Highway 50, KLEINBURG (ON)")),
    #"Sorted Rows" = Table.Sort(#"Filtered Rows",{{"IdleTime", Order.Descending}}),
    #"Duplicated Column" = Table.DuplicateColumn(#"Sorted Rows", "Rem_Code", "Rem_Code - Copy"),
    RemoveList = {"Supertrailer ", "Straight 26 ","Straight 28 tai ","Straight 24 ","DOLLY ","PUP TANDEM ","FLATBED THD ","Straight 22 ","Cube 16 ","Straight 24 tai ","Straight 28 "}, // List of values to remove
    FilteredRows = Table.SelectRows(Source, each not List.Contains(RemoveList, [Ret_Chassis])),
    #"Renamed Columns" = Table.RenameColumns(#"Duplicated Column",{{"Rem_Code - Copy", "Rem_Code -connect"}}),
    #"Replaced Value" = Table.ReplaceValue(Table.ReplaceValue(Table.ReplaceValue(Table.ReplaceValue(#"Renamed Columns", "-thd", "",Replacer.ReplaceText, {"Rem_Code -connect"}), "-THD", "", Replacer.ReplaceText, {"Rem_Code -connect"}), "-TIP", "", Replacer.ReplaceText, {"Rem_Code -connect"}),"-THD", "", Replacer.ReplaceText, {"Rem_Code -connect"}),
    #"Merged Queries" = Table.NestedJoin(#"Replaced Value", {"Rem_Code -connect"}, Trailer_Issues, {"Trailer"}, "issues1", JoinKind.LeftOuter),
    #"Expanded issues1" = Table.ExpandTableColumn(#"Merged Queries", "issues1", {"LatestStatus", "Las_date_from-doc_inv"}, {"issues1.LatestStatus", "issues1.Las_date_from-doc_inv"}),
    #"Filtered Rows1" = Table.SelectRows(#"Expanded issues1", each not Text.StartsWith([Company], "Ongoing")),
    #"Changed Type" = Table.TransformColumnTypes(#"Filtered Rows1",{{"Empty", type text}}),
    #"Replaced Value1" = Table.ReplaceValue(#"Changed Type","1","Declared Empty",Replacer.ReplaceText,{"Empty"}),
    #"Replaced Value2" = Table.ReplaceValue(#"Replaced Value1","0","Not Declared Empty",Replacer.ReplaceText,{"Empty"}),
    #"Filtered Rows2" = Table.SelectRows(#"Replaced Value2", each true),
    #"Removed Columns" = Table.RemoveColumns(#"Filtered Rows2",{"issues1.Las_date_from-doc_inv", "issues1.LatestStatus"}),
    #"Merged Queries1" = Table.NestedJoin(#"Removed Columns", {"Rem_Code -connect"}, #"Dock Inventory", {"Trailer"}, "Dock Inventory", JoinKind.LeftOuter),
    #"Expanded Dock Inventory" = Table.ExpandTableColumn(#"Merged Queries1", "Dock Inventory", {"Last_Date_from_doc_inv", "LastStatus"}, {"Dock Inventory.Last_Date_from_doc_inv", "Dock Inventory.LastStatus"}),
    #"Merged Queries2" = Table.NestedJoin(#"Expanded Dock Inventory", {"Rem_Code -connect"}, Trailer_Issues, {"Trailer"}, "issues1", JoinKind.LeftOuter),
    #"Expanded issues2" = Table.ExpandTableColumn(#"Merged Queries2", "issues1", {"Trailer", "Last_Issue", "Last_Issue_Date"}, {"issues1.Trailer", "issues1.Last_Issue", "issues1.Last_Issue_Date"}),
    #"Renamed Columns1" = Table.RenameColumns(#"Expanded issues2",{{"Dock Inventory.Last_Date_from_doc_inv", "Last_Date_from_doc_inv"}, {"Dock Inventory.LastStatus", "LastStatus"}, {"issues1.Trailer", "Trailer"}, {"issues1.Last_Issue", "Last_Issue"}, {"issues1.Last_Issue_Date", "Last_Issue_Date"}}),
    #"Removed Columns1" = Table.RemoveColumns(#"Renamed Columns1",{"Trailer"}),
    #"Renamed Columns2" = Table.RenameColumns(#"Removed Columns1",{{"Rem_Code", "Trailer"}})
in
    #"Renamed Columns2"
  ```
  


**Trailer Issues**: This table, derived from an Excel sheet, is where coordinators compile issues about trailers. For example, it contains updates about when safety inspections are needed.
```powerquery
  let
    Source = Excel.Workbook(File.Contents("C:\Users\didierl\OneDrive - L TRANSPORT \Yard Audit.xlsm"), null, true),
    issues_Table = Source{[Item="issues",Kind="Table"]}[Data],
    #"Changed Type" = Table.TransformColumnTypes(issues_Table,{{"DATE", type date}, {"Trailer", type text}, {"Status", type text}}),
    #"Changed Case" = Table.TransformColumns(#"Changed Type", {{"Trailer", Text.Upper, type text}}),
    #"Grouped Rows" = Table.Group(#"Changed Case", {"Trailer"}, {{"MaxDate", each List.Max([DATE]), type date}, {"LatestStatus", each [Status]{List.PositionOf([DATE], List.Max([DATE]))}, type text}}),
    #"Reordered Columns" = Table.ReorderColumns(#"Grouped Rows",{"Trailer", "LatestStatus", "MaxDate"}),
    #"Renamed Columns" = Table.RenameColumns(#"Reordered Columns",{{"MaxDate", "Las_date_from-doc_inv"}})
in
    #"Renamed Columns"
  ```
  
**Dock Inventory**: This table contains information on when a trailer is unloaded, reloaded, and what type of loading it contains.
 ```powerquery let
    Source = Excel.Workbook(File.Contents("C:\Users\didierl\OneDrive - L TRANSPORT \Dock Inventory.xlsx"), null, true),
    doc_table = Source{[Item="2023 Freight on Dock",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(doc_table, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {
        {"DATE", type date},
        {"Trailer", type any},
        {"#of Skids", Int64.Type},
        {"Keep Van?", type text},
        {"Vendor 1", type text},
        {"Vendor 2", type text},
        {"Vendor 3", type text},
        {"Vendor 4", type text},
        {"Column9", type any},
        {"Column10", type any},
        {"Column11", type any}
    }),
    #"Removed Other Columns" = Table.SelectColumns(#"Changed Type", {"DATE", "Trailer", "Keep Van?"}),
    #"Renamed Columns" = Table.RenameColumns(#"Removed Other Columns", {{"Keep Van?", "Status"}}),
    #"Sorted Rows" = Table.Sort(#"Renamed Columns", {{"DATE", Order.Descending}}),
    #"Displayed Column Names" = Table.ColumnNames(#"Sorted Rows"),
    #"Grouped Rows" = Table.Group(#"Sorted Rows", {"Trailer"}, {
        {"LastDate", each List.Max([DATE])},
        {"LastStatus", (t) => Table.LastN(t, 1)[#"Status"]{0}, type text}
    }),
    #"Renamed Columns1" = Table.RenameColumns(#"Grouped Rows",{{"LastDate", "Last_Date_from_doc_inv"}, {"LastStatus", "LastStatus"}}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Renamed Columns1",{{"Last_Date_from_doc_inv", type date}}),
    #"Replaced Value" = Table.ReplaceValue(#"Changed Type1",null,"Empty",Replacer.ReplaceValue,{"LastStatus"}),
   
in
    #"Replaced Value" 
  ```
**Yard Inventory**: A yard inventory is performed every two days and is updated in this table using a Power app.
``` powerquery
  let
    Source = Excel.Workbook(File.Contents("C:\Users\didierl\OneDrive - L TRANSPORT \Documents\Yard check bck.xlsm"), null, true),
    Yardchecks_Sheet = Source{[Item="Yardchecks",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Yardchecks_Sheet, [PromoteAllScalars=true]),
  
    #"Removed Blank Rows" = Table.SelectRows(#"Promoted Headers", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Promoted Headers1" = Table.PromoteHeaders(#"Removed Blank Rows", [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers1",{{"3500 FAIRWAY", type text}, {"1800 46TH AVE", type text}, {"1212 32ND AVE", type any}, {"Column4", type any}, {"Column5", type any}, {"Column6", type any}, {"Column7", type any}, {"Column8", type any}}),
    #"Removed Columns" = Table.RemoveColumns(#"Changed Type",{"Column4", "Column5", "Column6", "Column7", "Column8"}),
    #"Replaced Value" = Table.ReplaceValue(#"Removed Columns",null,"",Replacer.ReplaceValue,{"3500 FAIRWAY", "1800 46TH AVE", "1212 32ND AVE"})
in
    #"Replaced Value"
  ```
**Locations**: This table provides a list of locations where trailers can be, including client yards, our yard, and vendor yards.
``` powerquery
  let
    Source = Excel.Workbook(File.Contents("C:\Users\didierl\OneDrive - L TRANSPORT \Location.xlsx"), null, true),
    Table1_Table = Source{[Item="Table1",Kind="Table"]}[Data],
    #"Changed Type" = Table.TransformColumnTypes(Table1_Table,{{"Company", type text}, {"Address", type text}, {"Approved drop location", type text}, {"Trailer Pool", Int64.Type}}),
    #"Removed Duplicates" = Table.Distinct(#"Changed Type", {"Company"}),
    #"Replaced Value" = Table.ReplaceValue(#"Removed Duplicates",null,"No",Replacer.ReplaceValue,{"Approved drop location"})
in
    #"Replaced Value"
  ```
## Data Modeling


## Visualization and Results


## Conclusion

  
 
