<!--

comment:  Macros for executing SQL code snippets with AlaSQL in LiaScript.

script: https://cdn.jsdelivr.net/npm/alasql@0.6.5/dist/alasql.min.js
script: https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.6.1/papaparse.min.js
script: https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js


@AlaSQL.eval
<script>
try {
    var myList=JSON.stringify(alasql(`@input`), null, 3);
} catch(e) {
  let error = new LiaError(e.message, 1);
  try {
    let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
    error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
  } catch(e) {
  }
  throw error;
}
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
buildHtmlTable()
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

# AlaSQL

Test2 query

```sql
CREATE TABLE test (language INT, hello STRING);

-- insert dummy values
INSERT INTO test VALUES (1,'Hello!');
INSERT INTO test VALUES (2,'Aloha!');
INSERT INTO test VALUES (3,'Bonjour!');

SELECT * FROM test WHERE language > 1;
```
@AlaSQL.eval
