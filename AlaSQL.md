<!--

comment:  Macros for executing SQL code snippets with AlaSQL in LiaScript.

script: https://cdn.jsdelivr.net/npm/alasql@0.6.5/dist/alasql.min.js
script: https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.6.1/papaparse.min.js
script: https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js


@AlaSQL.eval
<script>
///////////
// USER NOTE: 
//   An element_id tag must be passed as an argument to this macro as a string. 
//   The results of his macro will then be rendered in any HTML element on this page referencing the specified element_id tag.
//////////
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
    $(@0).append(row$);
  }
  return "Query Execution Complete! (See Result Set Below)...";
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
  $(@0).append(headerTr$);
  return columnSet;
}
try {
    var myinput=`@input`
    myinput=myinput.replace(/;$/, ""); // remove trailing semi-colon
    var myStriptArray= myinput.split(';');
    var arrayLength = myStriptArray.length;
    console.clear();
    for (var i = 0; i < arrayLength; i++) {
        if((myStriptArray[i].trim()).length != 0) { // ignore blank queries.
            var myList=alasql(myStriptArray[i]);
        }
        if (myList != 1  & ((myStriptArray[i].trim()).length) != 0) { // If data is returned, format output as table.
            $(@0).html(""); // clear out existing data
            buildHtmlTable();
        } else {
            $(@0).html(""); // clear out existing data
            JSON.stringify("No Data to Return..");
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
</script>
@end

-->


<script>
alasql("CREATE TABLE test (language INT, hello STRING);")
alasql("INSERT INTO test VALUES (1,'Hello!');")
alasql("INSERT INTO test VALUES (2,'Aloha!');")
alasql("INSERT INTO test VALUES (3,'Bonjour!');")
</script>

# AlaSQL

Test HTML Table Output 21.

```sql
SELECT * FROM test;
```
@AlaSQL.eval("#dataTable1")

<table id="dataTable1" border="1"></table><br>

```sql
SELECT * FROM test where language>1;
```
@AlaSQL.eval("#dataTable2")

<table id="dataTable2" border="1"></table><br>
