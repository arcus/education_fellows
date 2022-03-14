<!--

author:   Peter EF Camacho

email:    camachop@chop.edu

version:  1.0.0

language: en

narrator: US English Female

logo:     https://liascript.github.io/img/bg-showcase-2.jpg

comment:  Macros for executing SQL code snippets with AlaSQL in LiaScript.

script: https://cdn.jsdelivr.net/npm/alasql@0.6.5/dist/alasql.min.js
attribute: [AlaSQL](https://alasql.org)
           by [Andrey Gershun](agershun@gmail.com)
           & [Mathias Rangel Wulff](m@rawu.dk)
           is licensed under [MIT](https://opensource.org/licenses/MIT)

script: https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.6.1/papaparse.min.js
attribute: [PapaParse](https://www.papaparse.com)
           by [Matthew Holt](https://twitter.com/mholt6)
           is licensed under [MIT](https://opensource.org/licenses/MIT)
           
script: https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js
attribute: [jQuery](https://jquery.com/)
           is licensed under [OpenJS Foundation](https://openjsf.org/)
           
@AlaSQL.eval
<script>
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// BUILD FUNCTIONS
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
function buildHtmlTable() {
  // Builds the HTML Table out of myList, and writes output to the id attribute assigned via the "@0" argument to this marco.
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
  try { // Error Handling for no null.
    var rowCount = document.getElementById(@0.substring(1)).rows.length - 1;
  } catch(err) {
    var cnt = 0
  }
  if (rowCount > 0) {
    var complete_message = "Query Execution Complete! (See Result Set Below)..."
  } else {
    var complete_message = "No Data to Return.."
  }
  return JSON.stringify(complete_message, null, 3);
}
function addAllColumnHeaders(myList) {
  // Creates and Returns Header Row From Array Data Provided as Input.
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
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
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
            JSON.stringify("No Data to Return..", null, 3);
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

@AlaSQL.buildTables
<script>
    alasql("DROP TABLE IF EXISTS greeting;");
    alasql("CREATE TABLE IF NOT EXISTS greeting (language_id INT, hello STRING);");
    alasql("INSERT INTO greeting VALUES (1,'Hello!');");
    alasql("INSERT INTO greeting VALUES (2,'Aloha!');");
    alasql("INSERT INTO greeting VALUES (3,'Bonjour!');");
    alasql("DROP TABLE IF EXISTS language;");
    alasql("CREATE TABLE IF NOT EXISTS language (language_id INT, language_name STRING);");
    alasql("INSERT INTO language VALUES (1,'English');");
    alasql("INSERT INTO language VALUES (2,'Hawaiian');");
    alasql("INSERT INTO language VALUES (3,'French');");
    JSON.stringify(@0);
</script>
@end

@AlaSQL.buildTable_patients
<script>
    alasql("DROP TABLE IF EXISTS patients;");
    alasql("create table patients (id text,birthdate date,deathdate date,ssn text,drivers text,passport text,prefix text,first text,last text,suffix text,maiden text,marital text,race text,ethnicity text,gender text,birthplace text,address text,city text,state text,county text,zip integer,lat real,lon real);");
    alasql("INSERT INTO patients VALUES ('bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','2000-11-21',null,'999-87-8860','S99917788',null,'Ms.','Cecila397','Feil794',null,null,null,'white','nonhispanic','F','Nahant  Massachusetts  US','873 Mueller Arcade Unit 96','Ashland','Massachusetts','Middlesex County',null,42.2138985577807,-71.503695110333);");
    alasql("INSERT INTO patients VALUES ('e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','2013-07-02',null,'999-82-6451',null,null,null,'Lorrie905','Leannon79',null,null,null,'white','nonhispanic','F','Winthrop  Massachusetts  US','813 Casper Street','Peabody','Massachusetts','Essex County',1940,42.4951616189433,-71.0071749067398);");
    alasql("INSERT INTO patients VALUES ('e061409e-4b85-4ec1-b1f7-02677d51f763','1997-09-11',null,'999-32-2366','S99995098','X50396137X','Ms.','Tabetha269','OHara248',null,null,null,'white','nonhispanic','F','Auburn  Massachusetts  US','1080 Sawayn Gateway Suite 9','Framingham','Massachusetts','Middlesex County',1701,42.3224819015944,-71.4003256055831);");
    alasql("INSERT INTO patients VALUES ('71e13815-55fb-4734-bcac-6079160d82a0','1973-06-02',null,'999-94-8759','S99996780','X23275205X','Mrs.','Laticia649','Flatley871',null,'Rempel203','M','white','nonhispanic','F','Boston  Massachusetts  US','469 Gerhold Bay Unit 34','Waltham','Massachusetts','Middlesex County',2451,42.4312144913848,-71.2680783842969);");
    alasql("INSERT INTO patients VALUES ('ca3330c5-bbbc-47e7-addb-302f2e069986','2003-06-23',null,'999-44-5854','S99912993',null,null,'Golden321','Pollich983',null,null,null,'white','nonhispanic','F','Carlisle  Massachusetts  US','792 OKon Byway','Springfield','Massachusetts','Hampden County',1105,42.0930726815877,-72.5803988374441);");
    alasql("INSERT INTO patients VALUES ('24bca5cf-ba55-457f-8e80-49690202443c','1977-06-28',null,'999-31-8026','S99975475','X46617643X','Mr.','Lionel365','Fadel536',null,null,'M','white','nonhispanic','M','Dighton  Massachusetts  US','1015 Parisian Divide Unit 26','Fairhaven','Massachusetts','Bristol County',null,41.6529893063487,-70.8948756027136);");
    alasql("INSERT INTO patients VALUES ('841095eb-d29f-4492-8f0e-08011321e85d','2017-04-08',null,'999-81-1909',null,null,null,'Carlton317','Leffler128',null,null,null,'asian','nonhispanic','M','Ipswich  Massachusetts  US','344 Feest Camp Suite 73','Wakefield','Massachusetts','Middlesex County',1880,42.4780284069299,-71.0892699664186);");
    alasql("INSERT INTO patients VALUES ('ee7f6c74-a8ed-4147-b8e2-4879c8657b0f','1950-04-11',null,'999-24-8407','S99984370','X70737069X','Mr.','Kelvin159','Powlowski563',null,null,'M','white','nonhispanic','M','Fall River  Massachusetts  US','623 Runolfsson Annex Suite 88','Revere','Massachusetts','Suffolk County',2151,42.4674386785624,-71.0070987954362);");
    alasql("INSERT INTO patients VALUES ('ab6a2662-f6d1-4da6-b3ce-3929d68650d7','1971-01-16',null,'999-76-3317','S99978505','X28929072X','Mrs.','Miesha237','Wyman904',null,'Jacobs452','M','white','nonhispanic','F','Harvard  Massachusetts  US','850 Thiel Road Unit 0','Westfield','Massachusetts','Hampden County',1086,42.0904434489837,-72.7927566478986);");
    alasql("INSERT INTO patients VALUES ('4440ff11-69ec-440b-a2bd-dc1c14105e8e','2001-11-20',null,'999-68-1710','S99968894',null,'Ms.','Ona426','Dooley940',null,null,null,'white','hispanic','F','Athol  Massachusetts  US','1048 Weimann Throughway','Northborough','Massachusetts','Worcester County',null,42.3661545339449,-71.6515505734496);");
    alasql("INSERT INTO patients VALUES ('1aa71b23-790e-4d22-92da-c689682c8993','1993-05-03',null,'999-84-7590','S99922416','X45217366X','Ms.','Jeannie478','VonRueden376',null,null,null,'white','nonhispanic','F','Ashburnham  Massachusetts  US','711 Williamson Dale','Ayer','Massachusetts','Middlesex County',null,42.5198988512541,-71.6009012317879);");
    alasql("INSERT INTO patients VALUES ('848e0227-5d5d-4bdf-8603-207cdea72e2a','1949-03-27',null,'999-87-5716','S99971093','X51980015X','Mrs.','Alda400','Kris249',null,'Satterfield305','M','white','nonhispanic','F','Mansfield  Massachusetts  US','1090 Wiegand Union','Attleboro','Massachusetts','Bristol County',null,41.9326094578451,-71.3272454816091);");
    alasql("INSERT INTO patients VALUES ('eafd1fd3-3778-423a-ba79-4584bd310eb4','2003-07-05',null,'999-39-2345','S99942603',null,null,'Buford910','Lynch190',null,null,null,'white','nonhispanic','M','Walpole  Massachusetts  US','332 Witting Mission','Malden','Massachusetts','Middlesex County',null,42.4569197038897,-71.0641138577902);");
    alasql("INSERT INTO patients VALUES ('0288abb6-633c-40c3-ba0c-66c7d957727e','1950-11-28',null,'999-18-7195','S99953954','X15598453X','Mrs.','Keva141','Shanahan202',null,'Reichel38','M','white','nonhispanic','F','Winchendon  Massachusetts  US','169 Witting Orchard Unit 98','Williamstown','Massachusetts','Berkshire County',null,42.7353013466036,-73.1923384461437);");
    alasql("INSERT INTO patients VALUES ('097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','2003-06-18',null,'999-43-8940','S99945945',null,null,'Maribeth658','DAmore443',null,null,null,'white','hispanic','F','Fall River  Massachusetts  US','238 Mills Hollow','Holyoke','Massachusetts','Hampden County',1040,42.1738356843755,-72.6457553208547);");
    alasql("INSERT INTO patients VALUES ('78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','1993-05-01',null,'999-75-7372','S99974220','X59022582X','Mr.','James276','Wyman904',null,null,null,'white','nonhispanic','M','Stoughton  Massachusetts  US','702 Stoltenberg Course Apt 16','Attleboro','Massachusetts','Bristol County',2703,41.9406317547512,-71.311136544988);");
    alasql("INSERT INTO patients VALUES ('c05478a7-a4df-4fd3-8d68-60b9452d4781','2010-10-14',null,'999-96-3194',null,null,null,'Brandon214','Hagenes547',null,null,null,'white','nonhispanic','M','Natick  Massachusetts  US','519 Thiel Annex Apt 55','Pittsfield','Massachusetts','Berkshire County',null,42.4079671056193,-73.3177862656724);");
    alasql("INSERT INTO patients VALUES ('e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','2008-07-16',null,'999-93-5743',null,null,null,'Frances376','Schumm995',null,null,null,'white','nonhispanic','M','Chelsea  Massachusetts  US','826 Hammes Mission Apt 1','Natick','Massachusetts','Middlesex County',null,42.2584672080661,-71.3444415391516);");
    alasql("INSERT INTO patients VALUES ('8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','2002-03-02',null,'999-93-1045','S99931036',null,'Ms.','Essie785','Kutch271',null,null,null,'white','nonhispanic','F','Cambridge  Massachusetts  US','219 Gorczany Gateway Unit 71','Chelmsford','Massachusetts','Middlesex County',null,42.5642627698721,-71.3448142130082);");
    alasql("INSERT INTO patients VALUES ('df7c1d66-eac2-49bd-9d12-ee17e8758f68','1979-11-19',null,'999-19-4886','S99963281','X82905419X','Mrs.','Iraida50','Oberbrunner298',null,'Sporer811','M','white','nonhispanic','F','Hanson  Massachusetts  US','903 Spencer Gate Suite 97','Springfield','Massachusetts','Hampden County',null,42.018977154431,-72.5783835627201);");
    alasql("INSERT INTO patients VALUES ('68878f91-5962-4ef2-83e7-43b8298c1708','1977-11-07',null,'999-40-6743','S99988503','X59484000X','Mr.','Ali918','Maggio310',null,null,'S','asian','nonhispanic','M','Taunton  Massachusetts  US','448 Rath Glen','Boston','Massachusetts','Suffolk County',2118,42.3425177110989,-71.1548454376564);");
    alasql("INSERT INTO patients VALUES ('1c2aa038-9366-4c7d-9a3e-52cb753a670f','1962-09-13',null,'999-19-8817','S99966954','X83180931X','Mr.','Homero668','Carrillo204',null,null,'M','white','hispanic','M','Gaudalajara  Jalisco  MX','627 Weissnat Fork','Boston','Massachusetts','Suffolk County',2128,42.3109346386431,-71.0700902117231);");
    alasql("INSERT INTO patients VALUES ('8d202c65-427d-4190-8c28-3aa27e1a9f4c','1986-10-24',null,'999-82-4546','S99932840','X66208297X','Mrs.','Mariam937','Bogisich202',null,'Hermann103','M','white','nonhispanic','F','Milton  Massachusetts  US','1032 McClure Extension Unit 88','Framingham','Massachusetts','Middlesex County',1701,42.3029088307893,-71.4025310847364);");
    alasql("INSERT INTO patients VALUES ('2a6d1e58-88eb-4be0-b6b4-59a471257c2e','1964-10-10',null,'999-22-8704','S99976805','X66668021X','Ms.','Nikia872','Herzog843',null,null,'S','white','nonhispanic','F','Wareham  Massachusetts  US','679 Robel Junction Apt 36','Quincy','Massachusetts','Norfolk County',2169,42.2640821758816,-71.0518467413496);");
    alasql("INSERT INTO patients VALUES ('e6ff4bf9-09c2-4976-aa84-cca142207cf8','1998-12-23',null,'999-91-5603','S99952608','X23816401X','Ms.','Corie618','Howe413',null,null,null,'white','nonhispanic','F','West Boylston  Massachusetts  US','580 Hickle Dam','Brookline','Massachusetts','Norfolk County',2215,42.3312937592791,-71.1672225619312);");
    JSON.stringify(@0);
</script>
@end

@AlaSQL.buildTable_encounters
<script>
    alasql("DROP TABLE IF EXISTS encounters;");
    alasql("create table encounters (id text,start date,stop date,patient text,organization text,provider text,encounterclass text,description text,reasondescription text);");
    alasql("INSERT INTO encounters VALUES ('a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','2002-01-24T20:46:46Z','2002-01-24T21:31:46Z','bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','24cb4eab-6166-3530-bddc-a5a8a14a4fc1','7bd4e666-a82d-3ad1-bc7c-b49eb726577b','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('469fbd8a-ec48-4da9-9165-027144ccf9a0','2014-12-04T23:28:40Z','2014-12-05T00:08:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','d692e283-0833-3201-8e55-4f868a9c0736','f4eb93d1-9187-3cfb-83a4-6d9cd77f7df6','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('022ad487-e41c-43ba-90f3-eb2d6711f4d3','1998-07-19T12:55:35Z','1998-07-19T13:38:35Z','e061409e-4b85-4ec1-b1f7-02677d51f763','465de31f-3098-365c-af70-48a071e1f5aa','0a8a9359-7b33-3256-a068-b5a7d18ebe4b','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('9607667e-4c98-4087-9c59-0fd5b6331078','1974-05-17T10:52:30Z','1974-05-17T11:07:30Z','71e13815-55fb-4734-bcac-6079160d82a0','6f122869-a856-3d65-8db9-099bf4f5bbb8','3180b739-e823-37a0-b307-52a6d67db4a5','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('d8f2b92b-5971-455f-a0b9-99da66d03899','2004-07-03T22:12:27Z','2004-07-03T22:57:27Z','ca3330c5-bbbc-47e7-addb-302f2e069986','60457c13-adb2-3415-82c5-86ab5dab5f93','47cb5349-d261-324a-9109-c888f4a0e966','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','1978-11-04T06:05:02Z','1978-11-04T06:51:02Z','24bca5cf-ba55-457f-8e80-49690202443c','ef6ab57c-ed94-3dbe-9861-812d515918b3','77a7881d-6dd5-32e1-9e18-521a59749572','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('32622f63-734e-4433-8628-942ce1585e6a','2018-03-20T11:48:11Z','2018-03-20T12:36:11Z','841095eb-d29f-4492-8f0e-08011321e85d','d692e283-0833-3201-8e55-4f868a9c0736','f4eb93d1-9187-3cfb-83a4-6d9cd77f7df6','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('0b7d2e65-a9df-4b74-84ed-25feffc23f62','1951-04-21T08:40:57Z','1951-04-21T08:55:57Z','ee7f6c74-a8ed-4147-b8e2-4879c8657b0f','d692e283-0833-3201-8e55-4f868a9c0736','f4eb93d1-9187-3cfb-83a4-6d9cd77f7df6','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('603a0692-9302-459a-84b4-af631dc3aee8','1971-03-07T16:13:43Z','1971-03-07T16:28:43Z','ab6a2662-f6d1-4da6-b3ce-3929d68650d7','ebc3f5c4-6700-34af-8323-85621c313726','eabb2bff-3216-34da-9f29-824dbca901c3','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('38de2a79-6bea-438e-963f-804823c1e32d','2002-05-31T06:08:11Z','2002-05-31T07:01:11Z','4440ff11-69ec-440b-a2bd-dc1c14105e8e','331f4c11-d298-308b-aaa1-d7825b29b57f','8ee28b4a-9018-3065-9f6b-0c9b69de7080','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('228c992b-3877-454c-920d-fa629bb8c5d9','1994-05-12T20:03:59Z','1994-05-12T20:47:59Z','1aa71b23-790e-4d22-92da-c689682c8993','ac8356a5-78f8-3a63-8a1e-59e832fd54e7','f6065151-bf86-330b-a526-ac86b53b440b','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('77427b07-f03b-49bc-9556-d69b4feed7ef','1950-01-07T13:40:23Z','1950-01-07T13:55:23Z','848e0227-5d5d-4bdf-8603-207cdea72e2a','5e765f2b-e908-3888-9fc7-df2cb87beb58','0359f968-d1a6-30eb-b1cc-e6cc0b4d3513','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('36279aee-15ff-48ad-a4a6-8ba334466278','2004-12-06T09:48:16Z','2004-12-06T10:36:16Z','eafd1fd3-3778-423a-ba79-4584bd310eb4','d692e283-0833-3201-8e55-4f868a9c0736','f4eb93d1-9187-3cfb-83a4-6d9cd77f7df6','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('a64c55df-b288-4f78-9996-d2ecf0b65c9d','1952-03-11T04:07:36Z','1952-03-11T04:22:36Z','0288abb6-633c-40c3-ba0c-66c7d957727e','4f3a530e-a2f7-3de0-9a09-c0a70a9ab894','3f15c687-0cfe-3bf2-9e62-34f3c85ff3cb','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('9c3c633f-c33c-426c-b771-b6117ba7d6fc','2004-04-26T14:03:38Z','2004-04-26T14:42:38Z','097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','5d4b9df1-93ae-3bc9-b680-03249990e558','af01a385-31d3-3c77-8fdb-2867fe88df2f','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','1994-06-07T13:13:50Z','1994-06-07T13:57:50Z','78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','5e765f2b-e908-3888-9fc7-df2cb87beb58','0359f968-d1a6-30eb-b1cc-e6cc0b4d3513','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','2011-10-24T09:24:08Z','2011-10-24T10:01:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','4f3a530e-a2f7-3de0-9a09-c0a70a9ab894','3f15c687-0cfe-3bf2-9e62-34f3c85ff3cb','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('5e4a49f2-47e7-4b76-9120-276a79f1766f','2009-01-22T22:23:00Z','2009-01-22T23:15:00Z','e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','465de31f-3098-365c-af70-48a071e1f5aa','0a8a9359-7b33-3256-a068-b5a7d18ebe4b','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('e75460f0-5f5c-4aa2-ab0b-200310a96c63','2003-06-13T09:58:22Z','2003-06-13T10:35:22Z','8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','b0e04623-b02c-3f8b-92ea-943fc4db60da','58b66cc1-2b86-377f-ad77-ad8164388e50','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('a232db22-565f-4559-bb56-edf9021b74b2','1981-01-29T12:47:12Z','1981-01-29T13:33:12Z','df7c1d66-eac2-49bd-9d12-ee17e8758f68','fd328395-ab1d-35c6-a2d0-d05a9a79cf11','1530e81b-106c-32d5-95d5-42a710c92068','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('95099931-0042-4524-b808-dd6b6447fc0e','1978-07-20T13:40:53Z','1978-07-20T14:17:53Z','68878f91-5962-4ef2-83e7-43b8298c1708','69176529-fd1f-3b3f-abce-a0a3626769eb','c9b3c857-2e24-320c-a79a-87b8a60de63c','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('c90b2536-b388-479c-aa7e-3406fe4c2211','1963-07-23T15:56:00Z','1963-07-23T16:11:00Z','1c2aa038-9366-4c7d-9a3e-52cb753a670f','ff9863d3-3fa3-3861-900e-f00148f5d9c2','e49edc61-6ba6-324c-bef7-b65f0e10799f','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('16bc6376-a1cc-4d63-8307-c5d7479dc021','1987-11-30T13:51:47Z','1987-11-30T14:41:47Z','8d202c65-427d-4190-8c28-3aa27e1a9f4c','465de31f-3098-365c-af70-48a071e1f5aa','0a8a9359-7b33-3256-a068-b5a7d18ebe4b','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('f7ff5032-50cc-480e-90ca-848c85d6d014','1965-09-23T13:40:01Z','1965-09-23T13:55:01Z','2a6d1e58-88eb-4be0-b6b4-59a471257c2e','12c9daf5-a29c-36c9-ac55-28972463e566','aa89beb2-7bc6-35fa-83f7-4b32039e84eb','ambulatory','Encounter for problem',null);");
    alasql("INSERT INTO encounters VALUES ('6c760807-a6b7-4af4-8d50-f32325803448','2000-01-03T07:32:25Z','2000-01-03T08:22:25Z','e6ff4bf9-09c2-4976-aa84-cca142207cf8','3d10019f-c88e-3de5-9916-6107b9c0263d','4b04cd2f-3f27-35bc-8069-f4ca6339529f','ambulatory','Encounter for problem',null);");
    JSON.stringify(@0);
</script>
@end

@AlaSQL.buildTable_providers
<script>
    alasql("DROP TABLE IF EXISTS providers;");
    alasql("create table providers (id text,name text,gender text,speciality text,address text,city text,state text,zip text,lat real,lon real);");
    alasql("INSERT INTO providers VALUES ('7bd4e666-a82d-3ad1-bc7c-b49eb726577b','Lonna614 Dietrich576','F','GENERAL PRACTICE','14 PROSPECT STREET','MILFORD','MA','01757',42.158692,-71.521419);");
    alasql("INSERT INTO providers VALUES ('f4eb93d1-9187-3cfb-83a4-6d9cd77f7df6','Vern731 Powlowski563','M','GENERAL PRACTICE','585 LEBANON STREET','MELROSE','MA','02176',42.455723,-71.059019);");
    alasql("INSERT INTO providers VALUES ('0a8a9359-7b33-3256-a068-b5a7d18ebe4b','Keri25 Schmidt332','F','GENERAL PRACTICE','115 LINCOLN STREET','FRAMINGHAM','MA','01701',42.307905,-71.436196);");
    alasql("INSERT INTO providers VALUES ('3180b739-e823-37a0-b307-52a6d67db4a5','Zana914 Considine820','F','GENERAL PRACTICE','41 & 45 MALL ROAD','BURLINGTON','MA','01803',42.503227,-71.201713);");
    alasql("INSERT INTO providers VALUES ('47cb5349-d261-324a-9109-c888f4a0e966','Mohammed454 Parisian75','M','GENERAL PRACTICE','759 CHESTNUT STREET','SPRINGFIELD','MA','01199',42.115454,-72.539978);");
    alasql("INSERT INTO providers VALUES ('77a7881d-6dd5-32e1-9e18-521a59749572','Phillip440 McCullough561','M','GENERAL PRACTICE','88 LEWIS BAY ROAD','HYANNIS','MA','02601',41.748854,-70.740536);");
    alasql("INSERT INTO providers VALUES ('eabb2bff-3216-34da-9f29-824dbca901c3','Óscar156 Mateo562','M','GENERAL PRACTICE','115 WEST SILVER STREET','WESTFIELD','MA','01085',42.138838,-72.755911);");
    alasql("INSERT INTO providers VALUES ('8ee28b4a-9018-3065-9f6b-0c9b69de7080','Malinda718 Cassin499','F','GENERAL PRACTICE','201 HIGHLAND STREET','CLINTON','MA','01510',42.411887,-71.690005);");
    alasql("INSERT INTO providers VALUES ('f6065151-bf86-330b-a526-ac86b53b440b','Tressa150 Kovacek682','F','GENERAL PRACTICE','200 GROTON ROAD','AYER','MA','01432',42.562221,-71.584844);");
    alasql("INSERT INTO providers VALUES ('0359f968-d1a6-30eb-b1cc-e6cc0b4d3513','Gaynell126 Streich926','F','GENERAL PRACTICE','211 PARK STREET','ATTLEBORO','MA','02703',41.931653,-71.294503);");
    alasql("INSERT INTO providers VALUES ('3f15c687-0cfe-3bf2-9e62-34f3c85ff3cb','Jesús825 Quiroz936','M','GENERAL PRACTICE','725 NORTH STREET','PITTSFIELD','MA','01201',42.452045,-73.26054);");
    alasql("INSERT INTO providers VALUES ('af01a385-31d3-3c77-8fdb-2867fe88df2f','Garth972 Wyman904','M','GENERAL PRACTICE','575 BEECH STREET','HOLYOKE','MA','01040',42.211656,-72.642448);");
    alasql("INSERT INTO providers VALUES ('58b66cc1-2b86-377f-ad77-ad8164388e50','Veda284 Pfeffer420','F','GENERAL PRACTICE','295 VARNUM AVENUE','LOWELL','MA','01854',42.638893,-71.322107);");
    alasql("INSERT INTO providers VALUES ('1530e81b-106c-32d5-95d5-42a710c92068','Wayne846 Mertz280','M','GENERAL PRACTICE','271 CAREW STREET','SPRINGFIELD','MA','01104',42.115454,-72.539978);");
    alasql("INSERT INTO providers VALUES ('c9b3c857-2e24-320c-a79a-87b8a60de63c','Suzette512 Monahan736','F','GENERAL PRACTICE','330 MOUNT AUBURN STREET','CAMBRIDGE','MA','02138',42.375967,-71.118275);");
    alasql("INSERT INTO providers VALUES ('e49edc61-6ba6-324c-bef7-b65f0e10799f','Carolyne559 Howell947','F','GENERAL PRACTICE','51 BLOSSOM STREET','BOSTON','MA','02114',42.33196,-71.020173);");
    alasql("INSERT INTO providers VALUES ('aa89beb2-7bc6-35fa-83f7-4b32039e84eb','Sanford861 Gottlieb798','M','GENERAL PRACTICE','199 REEDSDALE ROAD','MILTON','MA','02186',42.241589,-71.082651);");
    alasql("INSERT INTO providers VALUES ('4b04cd2f-3f27-35bc-8069-f4ca6339529f','Maile198 Frami345','F','GENERAL PRACTICE','2014 WASHINGTON STREET','NEWTON','MA','02462',42.331876,-71.208402);");
    JSON.stringify(@0);
</script>

@AlaSQL.buildTable_organizations
<script>
    alasql("DROP TABLE IF EXISTS organizations;");
    alasql("create table organizations (id text,name text,address text,city text,state text,zip text,lat real,lon real,phone text);");
    alasql("INSERT INTO organizations VALUES ('24cb4eab-6166-3530-bddc-a5a8a14a4fc1','MILFORD REGIONAL MEDICAL CENTER','14 PROSPECT STREET','MILFORD','MA','01757',42.158692,-71.521419,'5084731190');");
    alasql("INSERT INTO organizations VALUES ('d692e283-0833-3201-8e55-4f868a9c0736','HALLMARK HEALTH SYSTEM','585 LEBANON STREET','MELROSE','MA','02176',42.455723,-71.059019,'7819793000');");
    alasql("INSERT INTO organizations VALUES ('465de31f-3098-365c-af70-48a071e1f5aa','METROWEST MEDICAL CENTER','115 LINCOLN STREET','FRAMINGHAM','MA','01701',42.307905,-71.436196,'5083831000');");
    alasql("INSERT INTO organizations VALUES ('6f122869-a856-3d65-8db9-099bf4f5bbb8','LAHEY HOSPITAL & MEDICAL CENTER  BURLINGTON','41 & 45 MALL ROAD','BURLINGTON','MA','01803',42.503227,-71.201713,'7817445100');");
    alasql("INSERT INTO organizations VALUES ('60457c13-adb2-3415-82c5-86ab5dab5f93','BAYSTATE MEDICAL CENTER','759 CHESTNUT STREET','SPRINGFIELD','MA','01199',42.115454,-72.539978,'4137940000');");
    alasql("INSERT INTO organizations VALUES ('ef6ab57c-ed94-3dbe-9861-812d515918b3','CAPE COD HOSPITAL','88 LEWIS BAY ROAD','HYANNIS','MA','02601',41.748854,-70.740536,'5087711800');");
    alasql("INSERT INTO organizations VALUES ('ebc3f5c4-6700-34af-8323-85621c313726','NOBLE HOSPITAL','115 WEST SILVER STREET','WESTFIELD','MA','01085',42.138838,-72.755911,'4135682811');");
    alasql("INSERT INTO organizations VALUES ('331f4c11-d298-308b-aaa1-d7825b29b57f','CLINTON HOSPITAL ASSOCIATION','201 HIGHLAND STREET','CLINTON','MA','01510',42.411887,-71.690005,'9783683000');");
    alasql("INSERT INTO organizations VALUES ('ac8356a5-78f8-3a63-8a1e-59e832fd54e7','NASHOBA VALLEY MEDICAL CENTER','200 GROTON ROAD','AYER','MA','01432',42.562221,-71.584844,'9787849000');");
    alasql("INSERT INTO organizations VALUES ('5e765f2b-e908-3888-9fc7-df2cb87beb58','STURDY MEMORIAL HOSPITAL','211 PARK STREET','ATTLEBORO','MA','02703',41.931653,-71.294503,'5082225200');");
    alasql("INSERT INTO organizations VALUES ('4f3a530e-a2f7-3de0-9a09-c0a70a9ab894','BERKSHIRE MEDICAL CENTER INC - 1','725 NORTH STREET','PITTSFIELD','MA','01201',42.452045,-73.26054,'4134472000');");
    alasql("INSERT INTO organizations VALUES ('5d4b9df1-93ae-3bc9-b680-03249990e558','HOLYOKE MEDICAL CENTER','575 BEECH STREET','HOLYOKE','MA','01040',42.211656,-72.642448,'4135342500');");
    alasql("INSERT INTO organizations VALUES ('b0e04623-b02c-3f8b-92ea-943fc4db60da','LOWELL GENERAL HOSPITAL','295 VARNUM AVENUE','LOWELL','MA','01854',42.638893,-71.322107,'9789376000');");
    alasql("INSERT INTO organizations VALUES ('fd328395-ab1d-35c6-a2d0-d05a9a79cf11','MERCY MEDICAL CTR','271 CAREW STREET','SPRINGFIELD','MA','01104',42.115454,-72.539978,'4137489000');");
    alasql("INSERT INTO organizations VALUES ('69176529-fd1f-3b3f-abce-a0a3626769eb','MOUNT AUBURN HOSPITAL','330 MOUNT AUBURN STREET','CAMBRIDGE','MA','02138',42.375967,-71.118275,'6174923500');");
    alasql("INSERT INTO organizations VALUES ('ff9863d3-3fa3-3861-900e-f00148f5d9c2','SHRINERS HOSPITAL FOR CHILDREN - BOSTON  THE','51 BLOSSOM STREET','BOSTON','MA','02114',42.33196,-71.020173,'6177223000');");
    alasql("INSERT INTO organizations VALUES ('12c9daf5-a29c-36c9-ac55-28972463e566','BETH ISRAEL DEACONESS HOSPITAL-MILTON INC','199 REEDSDALE ROAD','MILTON','MA','02186',42.241589,-71.082651,'6176964600');");
    alasql("INSERT INTO organizations VALUES ('3d10019f-c88e-3de5-9916-6107b9c0263d','NEWTON-WELLESLEY HOSPITAL','2014 WASHINGTON STREET','NEWTON','MA','02462',42.331876,-71.208402,'6172436000');");
    JSON.stringify(@0);
</script>
@end


@AlaSQL.buildTable_allergies
<script>
    alasql("DROP TABLE IF EXISTS allergies;");
    alasql("create table allergies (start date,stop date,patient text,encounter text,description text);");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2002-01-24',null,'bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','a61f97fa-70c3-4366-90e1-7c6fdcba5cbb','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2014-12-04',null,'e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1998-07-19','2014-03-20','e061409e-4b85-4ec1-b1f7-02677d51f763','022ad487-e41c-43ba-90f3-eb2d6711f4d3','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1998-07-19','2014-03-20','e061409e-4b85-4ec1-b1f7-02677d51f763','022ad487-e41c-43ba-90f3-eb2d6711f4d3','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1998-07-19',null,'e061409e-4b85-4ec1-b1f7-02677d51f763','022ad487-e41c-43ba-90f3-eb2d6711f4d3','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1998-07-19',null,'e061409e-4b85-4ec1-b1f7-02677d51f763','022ad487-e41c-43ba-90f3-eb2d6711f4d3','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1974-05-17',null,'71e13815-55fb-4734-bcac-6079160d82a0','9607667e-4c98-4087-9c59-0fd5b6331078','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1974-05-17',null,'71e13815-55fb-4734-bcac-6079160d82a0','9607667e-4c98-4087-9c59-0fd5b6331078','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1974-05-17',null,'71e13815-55fb-4734-bcac-6079160d82a0','9607667e-4c98-4087-9c59-0fd5b6331078','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03','2019-12-30','ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Allergy to dairy product');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2004-07-03',null,'ca3330c5-bbbc-47e7-addb-302f2e069986','d8f2b92b-5971-455f-a0b9-99da66d03899','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1951-04-21',null,'ee7f6c74-a8ed-4147-b8e2-4879c8657b0f','0b7d2e65-a9df-4b74-84ed-25feffc23f62','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1951-04-21',null,'ee7f6c74-a8ed-4147-b8e2-4879c8657b0f','0b7d2e65-a9df-4b74-84ed-25feffc23f62','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31',null,'4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31',null,'4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31',null,'4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31',null,'4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31',null,'4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31','2020-03-21','4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31','2020-03-21','4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2002-05-31',null,'4440ff11-69ec-440b-a2bd-dc1c14105e8e','38de2a79-6bea-438e-963f-804823c1e32d','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1994-05-12','2011-02-03','1aa71b23-790e-4d22-92da-c689682c8993','228c992b-3877-454c-920d-fa629bb8c5d9','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('1994-05-12',null,'1aa71b23-790e-4d22-92da-c689682c8993','228c992b-3877-454c-920d-fa629bb8c5d9','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1994-05-12',null,'1aa71b23-790e-4d22-92da-c689682c8993','228c992b-3877-454c-920d-fa629bb8c5d9','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1950-01-07',null,'848e0227-5d5d-4bdf-8603-207cdea72e2a','77427b07-f03b-49bc-9556-d69b4feed7ef','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1950-01-07',null,'848e0227-5d5d-4bdf-8603-207cdea72e2a','77427b07-f03b-49bc-9556-d69b4feed7ef','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1950-01-07',null,'848e0227-5d5d-4bdf-8603-207cdea72e2a','77427b07-f03b-49bc-9556-d69b4feed7ef','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1950-01-07',null,'848e0227-5d5d-4bdf-8603-207cdea72e2a','77427b07-f03b-49bc-9556-d69b4feed7ef','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1950-01-07',null,'848e0227-5d5d-4bdf-8603-207cdea72e2a','77427b07-f03b-49bc-9556-d69b4feed7ef','Allergy to soya');");
    alasql("INSERT INTO allergies VALUES ('1950-01-07',null,'848e0227-5d5d-4bdf-8603-207cdea72e2a','77427b07-f03b-49bc-9556-d69b4feed7ef','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2004-12-06',null,'eafd1fd3-3778-423a-ba79-4584bd310eb4','36279aee-15ff-48ad-a4a6-8ba334466278','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1952-03-10',null,'0288abb6-633c-40c3-ba0c-66c7d957727e','a64c55df-b288-4f78-9996-d2ecf0b65c9d','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1952-03-10',null,'0288abb6-633c-40c3-ba0c-66c7d957727e','a64c55df-b288-4f78-9996-d2ecf0b65c9d','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1952-03-10',null,'0288abb6-633c-40c3-ba0c-66c7d957727e','a64c55df-b288-4f78-9996-d2ecf0b65c9d','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1952-03-10',null,'0288abb6-633c-40c3-ba0c-66c7d957727e','a64c55df-b288-4f78-9996-d2ecf0b65c9d','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1952-03-10',null,'0288abb6-633c-40c3-ba0c-66c7d957727e','a64c55df-b288-4f78-9996-d2ecf0b65c9d','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26','2019-12-25','097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26',null,'097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26',null,'097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26',null,'097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26',null,'097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Allergy to dairy product');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26',null,'097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Allergy to soya');");
    alasql("INSERT INTO allergies VALUES ('2004-04-26',null,'097079b1-ff8f-4ee0-8ce3-0ea744ecfa21','9c3c633f-c33c-426c-b771-b6117ba7d6fc','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07',null,'78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07',null,'78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07',null,'78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07',null,'78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07',null,'78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07','2011-03-09','78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','Allergy to dairy product');");
    alasql("INSERT INTO allergies VALUES ('1994-06-07',null,'78a9a8d6-b3b2-47dc-b4a0-867abec7c78f','7c0482a4-04fc-4cdc-9c2b-ff1f28f704db','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2011-10-24',null,'c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13','2018-09-08','8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13','2018-09-08','8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13','2018-09-08','8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13',null,'8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13',null,'8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13','2019-05-02','8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13','2019-05-02','8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2003-06-13',null,'8db0d104-4c3f-40d3-bcf5-f5eb81b7308f','e75460f0-5f5c-4aa2-ab0b-200310a96c63','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1981-01-29',null,'df7c1d66-eac2-49bd-9d12-ee17e8758f68','a232db22-565f-4559-bb56-edf9021b74b2','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1981-01-29',null,'df7c1d66-eac2-49bd-9d12-ee17e8758f68','a232db22-565f-4559-bb56-edf9021b74b2','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1981-01-29',null,'df7c1d66-eac2-49bd-9d12-ee17e8758f68','a232db22-565f-4559-bb56-edf9021b74b2','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1981-01-29',null,'df7c1d66-eac2-49bd-9d12-ee17e8758f68','a232db22-565f-4559-bb56-edf9021b74b2','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1981-01-29',null,'df7c1d66-eac2-49bd-9d12-ee17e8758f68','a232db22-565f-4559-bb56-edf9021b74b2','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1978-07-20',null,'68878f91-5962-4ef2-83e7-43b8298c1708','95099931-0042-4524-b808-dd6b6447fc0e','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1963-07-23',null,'1c2aa038-9366-4c7d-9a3e-52cb753a670f','c90b2536-b388-479c-aa7e-3406fe4c2211','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1987-11-30',null,'8d202c65-427d-4190-8c28-3aa27e1a9f4c','16bc6376-a1cc-4d63-8307-c5d7479dc021','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03','2016-06-25','e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2000-01-03',null,'e6ff4bf9-09c2-4976-aa84-cca142207cf8','6c760807-a6b7-4af4-8d50-f32325803448','Allergy to peanuts');");
    JSON.stringify(@0);
</script>
@end


@AlaSQL.buildTable_observations
<script>
    alasql("DROP TABLE IF EXISTS observations;");
    alasql("create table observations (observed date,patient text,encounter text,description text,observation_value text,units text,type text);");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','American house dust mite IgE Ab in Serum','26.0','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Cat dander IgE Ab in Serum','92.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Cladosporium herbarum IgE Ab in Serum','56.9','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Codfish IgE Ab in Serum','70.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Common Ragweed IgE Ab in Serum','93.7','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Cow milk IgE Ab in Serum','0.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Egg white IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Honey bee IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Latex IgE Ab in Serum','6.0','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Peanut IgE Ab in Serum','26.6','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Shrimp IgE Ab in Serum','0.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Soybean IgE Ab in Serum','0.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Walnut IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','Wheat IgE Ab in Serum','75.4','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2014-12-04T23:28:40Z','e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','469fbd8a-ec48-4da9-9165-027144ccf9a0','White oak IgE Ab in Serum','10.4','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','American house dust mite IgE Ab in Serum','63.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Cat dander IgE Ab in Serum','83.5','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Cladosporium herbarum IgE Ab in Serum','3.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Codfish IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Common Ragweed IgE Ab in Serum','58.7','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Cow milk IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Egg white IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Honey bee IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Latex IgE Ab in Serum','0.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Peanut IgE Ab in Serum','37.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Shrimp IgE Ab in Serum','85.0','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Soybean IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Walnut IgE Ab in Serum','89.9','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Wheat IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2018-03-20T11:48:11Z','841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','White oak IgE Ab in Serum','56.4','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','American house dust mite IgE Ab in Serum','42.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Cat dander IgE Ab in Serum','37.0','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Cladosporium herbarum IgE Ab in Serum','58.7','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Codfish IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Common Ragweed IgE Ab in Serum','42.0','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Cow milk IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Egg white IgE Ab in Serum','60.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Honey bee IgE Ab in Serum','61.8','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Latex IgE Ab in Serum','2.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Peanut IgE Ab in Serum','12.1','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Shrimp IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Soybean IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Walnut IgE Ab in Serum','0.3','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','Wheat IgE Ab in Serum','0.2','kU/L','numeric');");
    alasql("INSERT INTO observations VALUES ('2011-10-24T09:24:08Z','c05478a7-a4df-4fd3-8d68-60b9452d4781','6dbce8d2-3bb0-4ff9-8e9b-7152ff03cc0c','White oak IgE Ab in Serum','60.7','kU/L','numeric');");
    JSON.stringify(@0);
</script>
@end

-->

# AlaSQL

Test HTML Table Output 40.

```sql
SELECT * FROM greeting
```
@AlaSQL.eval("#dataTable1")

<table id="dataTable1" border="1"></table><br>

```sql
SELECT * 
FROM greeting
LEFT JOIN language
  ON language.language_id = greeting.language_id 
where
  greeting.language_id > 1
```
@AlaSQL.eval("#dataTable2")

<table id="dataTable2" border="1"></table><br>

<hr/><hr/>

@AlaSQL.buildTables('Note: Click the `</>` button under each text-box to run the SQL code.')

# AlaSQL - Page 2

Test 13.

```sql
SELECT * FROM patients
```
@AlaSQL.eval("#dataTable3")

<table id="dataTable3" border="1"></table><br>

<hr/><hr/>

<br/>
@AlaSQL.buildTable_patients('`patients` table queryable from this page!')
<br/>
@AlaSQL.buildTable_encounters('`encounters` queryable from this page!')
<br/>
@AlaSQL.buildTable_providers('`providers` queryable from this page!')
<br/>
@AlaSQL.buildTable_organizations('`organizations` queryable from this page!')
<br/>
@AlaSQL.buildTable_observations('`observations` queryable from this page!')
<br/>
@AlaSQL.buildTable_allergies('`allergies` queryable from this page!')
<br/>

