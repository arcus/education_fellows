<!--

comment:  Macros for executing SQL code snippets with AlaSQL in LiaScript.

script: https://cdn.jsdelivr.net/npm/alasql@0.6.5/dist/alasql.min.js
script: https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.6.1/papaparse.min.js
script: https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js


@AlaSQL.eval
<script>
try {
    var myinput=`@input`
    var myStriptArray= myinput.split(';');
    var arrayLength = myStriptArray.length;
    for (var i = 0; i < arrayLength; i++) {
        myList=alasql(myStriptArray[i])
        if (myList != 1 ) { // If data is returned, format output as table.
              buildHtmlTable();
        }
    }
} catch(e) {
  let error = new LiaError(e.message, 1);
  try {
    let log = e.message.match(/.*line (\d):.*\n.*\n.*\n(.*)/);
    error.add_detail(0, e.name+": "+log[2], "error", log[1] -1 , 0);
  } catch(e) {
  }
  throw error;
}
// Builds the HTML Table out of myList json data from Ivy restful service.
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
</script>
@end

-->

# AlaSQL

Test5 query

```sql
SELECT * FROM test WHERE language > 1;
```
@AlaSQL.eval

<table id="excelDataTable" border="1">
