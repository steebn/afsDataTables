# Steps to set up afsDatatables v2.0 #

## Basic initialization ##

1. Copy the 2 lines below and paste them right above the final table request in the fex. These need to be BEFORE the last table request.

```
-FINAL_REQUEST
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dt_Sets.fex
```

2. Copy the 11 lines below and paste at the end of the final table request (replace the END and any styling in the request if it already exists.)
```
ON TABLE HOLD FORMAT HTMTABLE
ON TABLE SET STYLE *
TYPE=REPORT, LINES-PER-PAGE=UNLIMITED, $
TYPE=ACROSSTITLE, CLASS='tblHead acrossTitle', $
TYPE=ACROSSVALUE, CLASS='tblHead acrossValue', $
TYPE=GRANDTOTAL, CLASS='tblFoot', $
TYPE=TITLE, CLASS='tblHead', $
TYPE=DATA, CLASS='tblData', $
ENDSTYLE
END
-RUN
```

3. For a basic report from an `HTMTABLE` format, copy the lines below and paste after the end of the final procedure:
```html
-SET_DATATABLES
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dtFields.fex

-HTMLFORM BEGIN NOEVAL
<body>
    <script>
        table = $('#afsbi_table').DataTable();
    </script>
</body>

-HTMLFORM END
-EXIT
```

4. below is an example complete report and can be run at this  [link](https://afsbi.afstores.com/ibi_apps/run/ibfs?IBFS_path=/EDA/EDASERVE/baseapp/datatables/dt_example.fex):
   
```html
-FINAL_REQUEST
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dt_Sets.fex

TABLE FILE car
SUM SALES/D12M 
    DEALER_COST/D12M
    RETAIL_COST/D12M
BY COUNTRY
BY CAR 
BY BODYTYPE
BY MODEL
ON TABLE HOLD FORMAT HTMTABLE
ON TABLE SET STYLE *
TYPE=REPORT, LINES-PER-PAGE=UNLIMITED, $
TYPE=ACROSSTITLE, CLASS='tblHead acrossTitle', $
TYPE=ACROSSVALUE, CLASS='tblHead acrossValue', $
TYPE=GRANDTOTAL, CLASS='tblFoot', $
TYPE=TITLE, CLASS='tblHead', $
TYPE=DATA, CLASS='tblData', $
ENDSTYLE
END
-RUN

-SET_DATATABLES
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dtFields.fex

-HTMLFORM BEGIN NOEVAL
<body>
    !IBI.FIL.HOLD;
    <script>
        table = $('#afsbi_table').DataTable();
    </script>
</body>
-HTMLFORM END
-EXIT
```

## Advanced Features ##

### There are additional features that can be added to the Basic Configuration. These will need to be placed right after the opening `<script>` tag in the `HTMLFORM` block. ###
```html
-HTMLFORM BEGIN NOEVAL
<body>
    !IBI.FIL.HOLD;
    <script>
        //TODO: Additional features
        table = $('#afsbi_table').DataTable();
    </script>
</body>
-HTMLFORM END
-EXIT
```



```

```html
-SET_DATATABLES
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dtFields.fex

-HTMLFORM BEGIN NOEVAL
<body>
    <script>
        // *Optional: This is the title that shows on the the browser tab. Default is the file name.
        reportTitle = 'Title'; 

        // *Optional*: Include this if you want a heading on your report. 
        afsHeading({ 

            // The row1 option will be the main title. 
            row1: 'string',

            // The row2 option will be displayed on a single line
            //   - A circle bullet ( • ) will be places in front of each item. 
            //   - Each item will wrap on top of the other as the screen size gets smaller. 
            //   - No limit on row2 items.
            row2: {
                item1: 'Report Title line 2 -> item 1', 
                item2: 'Report Title line 2 -> item 2', 
                item3: 'Report Title line 2 -> item 3', 
            }        
        });
        
        // *Optional: Include this if you would like a breadcrumb at the top of the report.
        afsBreadcrumb({

            //ONLY include this if the report is the breadcrumb root.
            level   : 0,

            //This is the text that will show on the breadcrumb   
            aText   : reportTitle,  
            bTextCol: 1,            //This parameter defines which column has the value that the breadcrumb should display in the next report.

        });

        // *Required: This is the call to create the datatable
        table = $('#afsbi_table').DataTable({
            // *Optional: Include any datatables specific options
            //   - These can be found here https://datatables.net/reference/option/
            data: data, //Required for JSON, leave out for HTMTABLE
            columns: afsColumns, //Required for JSON, leave out for HTMTABLE
        });
    </script>
</body>

-HTMLFORM END
-EXIT
```

-*********************************************************************************************************************************
-FINAL_REQUEST
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dt_Sets.fex

TABLE FILE car
SUM SALES 
BY COUNTRY
BY CAR 
BY MODEL 
ON TABLE HOLD FORMAT HTMTABLE
ON TABLE SET STYLE *
TYPE=REPORT, LINES-PER-PAGE=UNLIMITED, $
TYPE=TITLE, CLASS='tblHead', $
TYPE=DATA, CLASS='tblData', $
TYPE=GRANDTOTAL, CLASS='tblFoot', $
ENDSTYLE
END
-RUN

-SET_DATATABLES
-INCLUDE IBFS:/EDA/EDASERVE/baseapp/datatables/dtFields.fex

-HTMLFORM BEGIN NOEVAL

<body>
    <script>
        reportTitle = 'Car File Example'; 
        afsHeading({ 
            row1: reportTitle,
            row2: {
                item1: 'By Country', 
                item2: 'By Car', 
                item3: 'By Model', 
            }        
        });
        
        afsBreadcrumb({
            level   : 0,           
            aText   : reportTitle,  
            bTextCol: 1,          
        });

        table = $('#afsbi_table').DataTable();
    </script>
</body>

-HTMLFORM END
-EXIT
-*--------------------------------------------------------------------------------------------------------------------------------


-EXAMPLE
