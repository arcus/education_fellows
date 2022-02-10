# AlaSQL

<!--

author: Peter Camacho (camachop@chop.edu)

email: None

language: en

narrator: US English Female

logo:     https://jquery-plugins.net/image/plugin/alasql-javascript-sql-database-library.png

comment:  Macros for executing SQL code snippets with AlaSQL in LiaScript.

script:   https://cdn.jsdelivr.net/npm/alasql@0.6.5/dist/alasql.min.js

attribute: [AlaSQL](https://alasql.org)
           by [Andrey Gershun](agershun@gmail.com)
           & [Mathias Rangel Wulff](m@rawu.dk)
           is licensed under [MIT](https://opensource.org/licenses/MIT)


script:  https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.6.1/papaparse.min.js

attribute: [PapaParse](https://www.papaparse.com)
           by [Matthew Holt](https://twitter.com/mholt6)
           is licensed under [MIT](https://opensource.org/licenses/MIT)


script: https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js
attribute: [Jquery](https://api.jquery.com/)


@AlaSQL.eval
<script>
function addAllColumnHeaders(myList) {
     var columnSet = [];
     var headerTr$ = $('<tr/>');
     for (var i = 0 ; i < myList.length ; i++) {
         var rowHash = myList[i];
         for (var key in rowHash) {
             if ($.inArray(key, columnSet) == -1){
                 columnSet.push(key);
                 headerTr$.append($('<th/>').html(key));
             }
         }
     }
     $("#excelDataTable").append(headerTr$);
     return columnSet;
}
function buildHtmlTable() {
     var columns = addAllColumnHeaders(myList);
     for (var i = 0 ; i < myList.length ; i++) {
         var row$ = $('<tr/>');
         for (var colIndex = 0 ; colIndex < columns.length ; colIndex++) {
             var cellValue = myList[i][columns[colIndex]];
             if (cellValue == null) { cellValue = ""; }
             row$.append($('<td/>').html(cellValue));
         }
         $("#excelDataTable").append(row$);
     }
 }
 
 // Adds a header row to the table and returns the set of columns.
 // Need to do union of keys from all records as some records may not contain
 // all records


try {
    buildHtmlTable(JSON.stringify(alasql(`@input`), null, 3));
} catch(e) {
  let error = new LiaError(e.message, 1);
  try {
    let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
    error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
  } catch(e) {
  }
  throw error;
}
</script>
@end

@AlaSQL.eval_with_csv
<script>
let data = Papa.parse(`@input(1)`, {header: true});
let error = "";
if(data.errors.length != 0) {
    error = JSON.stringify(data.errors, null, 3)+"\n";
}
try {
  error += JSON.stringify(alasql(`@input`, [data.data]), null, 3);
} catch(e) {
  let error = new LiaError(e.message, 1);
  try {
    let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
    error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
  } catch(e) {}
  throw error ;
}
</script>
@end

-->



```sql
CREATE TABLE test (language INT, hello STRING);

-- insert dummy values
INSERT INTO test VALUES (1,'Hello!');
INSERT INTO test VALUES (2,'Aloha!');
INSERT INTO test VALUES (3,'Bonjour!');

SELECT * FROM test WHERE language > 1;
```
@AlaSQL.eval_with_csv
