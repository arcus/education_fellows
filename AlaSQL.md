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
    alasql("INSERT INTO patients VALUES ('76982e06-f8b8-4509-9ca3-65a99c8650fe','1982-09-01',null,'999-21-5604','S99957470','X55072337X','Ms.','Christal240','Brown30',null,null,'S','white','nonhispanic','F','Bellingham  Massachusetts  US','1060 Hansen Overpass Suite 86','Boston','Massachusetts','Suffolk County',2118,42.2845984733578,-71.1344967487613);");
    alasql("INSERT INTO patients VALUES ('71ba0469-f0cc-4177-ac70-ea07cb01c8b8','2000-11-21','2012-11-21','999-28-2716',null,null,null,'Carmelia328','Konopelski743',null,null,null,'white','nonhispanic','F','Lee  Massachusetts  US','1025 Collier Arcade','Ashland','Massachusetts','Middlesex County',null,42.2919859634347,-71.4637238426449);");
    alasql("INSERT INTO patients VALUES ('bf35e4fa-ea4f-40a4-8fe6-1f2f26e0aa45','2000-11-21',null,'999-87-8860','S99917788',null,'Ms.','Cecila397','Feil794',null,null,null,'white','nonhispanic','F','Nahant  Massachusetts  US','873 Mueller Arcade Unit 96','Ashland','Massachusetts','Middlesex County',null,42.2138985577807,-71.503695110333);");
    alasql("INSERT INTO patients VALUES ('e3af2463-f4c9-4dbb-a8d2-d6a08c5b1460','2013-07-02',null,'999-82-6451',null,null,null,'Lorrie905','Leannon79',null,null,null,'white','nonhispanic','F','Winthrop  Massachusetts  US','813 Casper Street','Peabody','Massachusetts','Essex County',1940,42.4951616189433,-71.0071749067398);");
    alasql("INSERT INTO patients VALUES ('ddfa05a6-b8a6-4208-b609-28f9a6a8d79e','2001-12-27',null,'999-83-2956','S99936978',null,'Ms.','Joleen561','Quitzon246',null,null,null,'white','nonhispanic','F','Woburn  Massachusetts  US','976 Bode Parade Apt 52','Greenfield','Massachusetts','Franklin County',null,42.6425538261325,-72.6360572438334);");
    alasql("INSERT INTO patients VALUES ('df91aedf-3e17-44c3-b273-62e35f518475','2008-10-24',null,'999-97-9250',null,null,null,'Sean831','Casper496',null,null,null,'white','nonhispanic','M','Marblehead  Massachusetts  US','173 Leuschke Club Unit 65','Waltham','Massachusetts','Middlesex County',2453,42.428920338643,-71.2288547808303);");
    alasql("INSERT INTO patients VALUES ('0c5aa52a-3b1d-4289-af16-29f7a2d07fec','1984-06-24',null,'999-82-1441','S99919484','X25592388X','Mrs.','Elvira561','Navarro863',null,'Cabrera242','M','white','hispanic','F','Santiago de los Caballeros  Santiago  DO','451 Buckridge Harbor','Attleboro','Massachusetts','Bristol County',null,41.8458658446907,-71.3317871538724);");
    alasql("INSERT INTO patients VALUES ('2a88cb57-86fa-4259-93bf-3956e91b07f4','2017-12-23',null,'999-28-4976',null,null,null,'Mateo562','Palacios784',null,null,null,'white','hispanic','M','Santiago  Santiago Province  CL','841 McGlynn Knoll Suite 67','Taunton','Massachusetts','Bristol County',2780,41.959978467312,-71.0141637048784);");
    alasql("INSERT INTO patients VALUES ('71e13815-55fb-4734-bcac-6079160d82a0','1973-06-02',null,'999-94-8759','S99996780','X23275205X','Mrs.','Laticia649','Flatley871',null,'Rempel203','M','white','nonhispanic','F','Boston  Massachusetts  US','469 Gerhold Bay Unit 34','Waltham','Massachusetts','Middlesex County',2451,42.4312144913848,-71.2680783842969);");
    alasql("INSERT INTO patients VALUES ('852f4588-ba8f-4eeb-94fe-5c3c974ab114','1956-01-02',null,'999-60-6320','S99945237','X75083103X','Mrs.','Keren761','Kiehn525',null,'Jast432','M','white','hispanic','F','Whitman  Massachusetts  US','167 Bosco Boulevard Unit 9','Lynn','Massachusetts','Essex County',1904,42.4446755379841,-70.9866011586124);");
    alasql("INSERT INTO patients VALUES ('09616ead-22c8-4210-8cb9-2fdc28e043ca','1953-08-03',null,'999-68-5321','S99927707','X54034630X','Mrs.','Christena299','Lang846',null,'Reynolds644','M','white','nonhispanic','F','Boston  Massachusetts  US','124 Fadel Dam Apt 81','Attleboro','Massachusetts','Bristol County',2703,41.9761886591609,-71.3242228778937);");
    alasql("INSERT INTO patients VALUES ('f7d7b580-e670-4921-bc67-550ec468d506','1975-02-16',null,'999-29-4371','S99927874','X76480436X','Mrs.','Micki733','Witting912',null,'Turner526','M','white','nonhispanic','F','Chelsea  Massachusetts  US','589 Pfeffer Avenue','Tewksbury','Massachusetts','Middlesex County',null,42.59289752343,-71.1833293183252);");
    alasql("INSERT INTO patients VALUES ('24bca5cf-ba55-457f-8e80-49690202443c','1977-06-28',null,'999-31-8026','S99975475','X46617643X','Mr.','Lionel365','Fadel536',null,null,'M','white','nonhispanic','M','Dighton  Massachusetts  US','1015 Parisian Divide Unit 26','Fairhaven','Massachusetts','Bristol County',null,41.6529893063487,-70.8948756027136);");
    alasql("INSERT INTO patients VALUES ('f36775e8-bf5a-40a2-a223-679c05b46004','2008-01-18',null,'999-56-1320',null,null,null,'Scott935','Boyer713',null,null,null,'black','nonhispanic','M','Templeton  Massachusetts  US','203 Sporer Esplanade Unit 14','Oxford','Massachusetts','Worcester County',null,42.1336968374336,-71.8348235441901);");
    alasql("INSERT INTO patients VALUES ('8be68b2d-8054-4882-8715-65297d36767a','1988-05-12',null,'999-15-5114','S99941698','X21674254X','Mrs.','James276','McClure239',null,'Toy286','M','white','nonhispanic','F','Somerville  Massachusetts  US','385 DAmore Byway Unit 19','Westport','Massachusetts','Bristol County',null,41.5719802553537,-71.092897914972);");
    alasql("INSERT INTO patients VALUES ('59669e7c-0190-4cbf-9e27-4898f1a35d03','1986-01-31',null,'999-65-7058','S99967380','X42303726X','Mr.','Julio255','Juárez383',null,null,'M','white','hispanic','M','Caracas  Capital District  VE','1013 Skiles Trafficway Unit 29','North Brookfield','Massachusetts','Worcester County',1535,42.3016733122009,-72.0730981041963);");
    alasql("INSERT INTO patients VALUES ('4b92e3f1-b92b-48ec-9baa-2905409d1743','2015-09-18',null,'999-27-7943',null,null,null,'Demetrice140','Zieme486',null,null,null,'white','nonhispanic','F','Revere  Massachusetts  US','856 Yundt Harbor Suite 60','Belchertown','Massachusetts','Hampshire County',null,42.3164507288839,-72.3610840980465);");
    alasql("INSERT INTO patients VALUES ('9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','1999-05-24',null,'999-91-6914','S99935557','X43776419X','Ms.','Ardath226','Spinka232',null,null,null,'white','nonhispanic','F','Templeton  Massachusetts  US','934 Little Crossroad Apt 52','Fitchburg','Massachusetts','Worcester County',null,42.6195236351258,-71.8595947155867);");
    alasql("INSERT INTO patients VALUES ('8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','2015-08-06',null,'999-43-7577',null,null,null,'Latonia966','Watsica258',null,null,null,'white','nonhispanic','F','Grafton  Massachusetts  US','1092 Lowe Alley','Wareham','Massachusetts','Plymouth County',null,41.7824350691882,-70.7367243449479);");
    alasql("INSERT INTO patients VALUES ('128d5c93-dfce-49a1-8d08-e9b24abcb4db','2015-09-03',null,'999-19-4863',null,null,null,'Tifany477','Wilderman619',null,null,null,'black','nonhispanic','F','Worcester  Massachusetts  US','975 Murphy Tunnel Apt 27','Plymouth','Massachusetts','Plymouth County',2360,41.8801316490312,-70.6619548788536);");
    alasql("INSERT INTO patients VALUES ('eba0f292-1951-4af9-9f14-9d6e9db8e480','2007-08-06',null,'999-48-3220',null,null,null,'Eugena417','Kris249',null,null,null,'white','nonhispanic','F','Concord  Massachusetts  US','652 Swaniawski Crossroad','Rochester','Massachusetts','Plymouth County',null,41.7398459280911,-70.8018859892228);");
    alasql("INSERT INTO patients VALUES ('5846c531-ad71-4b34-9607-3a1022cddffa','2008-09-07',null,'999-46-6114',null,null,null,'Marty115','Abshire638',null,null,null,'white','nonhispanic','F','Springfield  Massachusetts  US','728 Lynch Crossing','Weymouth','Massachusetts','Norfolk County',2190,42.1606760073641,-70.916636029243);");
    alasql("INSERT INTO patients VALUES ('d00fc5dd-be0e-47aa-a52a-5cf9ee3a78b5','2002-11-27',null,'999-12-8726','S99977695',null,null,'Maxwell782','Reichel38',null,null,null,'white','nonhispanic','M','Braintree  Massachusetts  US','693 Greenfelder Annex Suite 77','Needham','Massachusetts','Norfolk County',2492,42.2858720986636,-71.2510457960187);");
    alasql("INSERT INTO patients VALUES ('7e4e6d32-15cd-4d5f-a4cc-e5c95ab35eb0','1962-12-18',null,'999-35-9139','S99942641','X7322252X','Mrs.','Ellamae709','Bins636',null,'Jacobs452','M','white','nonhispanic','F','Lynn  Massachusetts  US','1075 Stokes Mall Apt 14','Ware','Massachusetts','Hampshire County',1082,42.2926402063468,-72.2362947721484);");
    alasql("INSERT INTO patients VALUES ('841095eb-d29f-4492-8f0e-08011321e85d','2017-04-08',null,'999-81-1909',null,null,null,'Carlton317','Leffler128',null,null,null,'asian','nonhispanic','M','Ipswich  Massachusetts  US','344 Feest Camp Suite 73','Wakefield','Massachusetts','Middlesex County',1880,42.4780284069299,-71.0892699664186);");
    alasql("INSERT INTO patients VALUES ('e112cedd-a98e-489e-abb0-875420d40397','2013-09-08',null,'999-98-4107',null,null,null,'Bobby524','Robel940',null,null,null,'white','nonhispanic','F','Milford  Massachusetts  US','389 Beier Annex Unit 70','Brookline','Massachusetts','Norfolk County',null,42.2978552171746,-71.1677734007861);");
    alasql("INSERT INTO patients VALUES ('ab6a2662-f6d1-4da6-b3ce-3929d68650d7','1971-01-16',null,'999-76-3317','S99978505','X28929072X','Mrs.','Miesha237','Wyman904',null,'Jacobs452','M','white','nonhispanic','F','Harvard  Massachusetts  US','850 Thiel Road Unit 0','Westfield','Massachusetts','Hampden County',1086,42.0904434489837,-72.7927566478986);");
    alasql("INSERT INTO patients VALUES ('c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','1965-07-08',null,'999-43-1836','S99911853','X60743162X','Mrs.','Dannette613','Bartoletti50',null,'Murray856','M','white','nonhispanic','F','Hudson  Massachusetts  US','152 Heller Wynd Apt 16','Holyoke','Massachusetts','Hampden County',1040,42.1701286966339,-72.6412540036445);");
    alasql("INSERT INTO patients VALUES ('c4f221f2-611b-4cdd-a0b6-958bbbfcf346','1987-03-18',null,'999-30-7178','S99914950','X7202355X','Mr.','Homero668','Reyes140',null,null,'M','white','hispanic','M','La Paz  Baja California  MX','257 Gutmann Highlands Apt 89','Hopkinton','Massachusetts','Middlesex County',1748,42.2597000095724,-71.4923689837963);");
    alasql("INSERT INTO patients VALUES ('77a0cd86-92bb-4c6d-a91a-49ee66e353b9','1989-08-19',null,'999-62-5886','S99996704','X23047417X','Mrs.','Norma469','Mayer370',null,'Deckow585','M','white','nonhispanic','F','Medfield  Massachusetts  US','192 Wilderman Trafficway Unit 13','Canton','Massachusetts','Norfolk County',null,42.1352712126216,-71.1121525530781);");
    alasql("INSERT INTO patients VALUES ('ea8e6623-5590-4d01-bfc1-25d86b1b4491','1994-04-21',null,'999-70-5118','S99979218','X10796024X','Ms.','Winona266','Reinger292',null,null,null,'asian','nonhispanic','F','Brockton  Massachusetts  US','525 Mills Quay Apt 74','Douglas','Massachusetts','Worcester County',null,42.0434467134591,-71.7811000556293);");
    alasql("INSERT INTO patients VALUES ('1a289f28-b73b-4d3a-83ef-2216e8837bad','2005-10-15',null,'999-68-7312',null,null,null,'Earl438','Friesen796',null,null,null,'white','nonhispanic','M','Framingham  Massachusetts  US','820 Stehr Fort Suite 88','Quincy','Massachusetts','Norfolk County',2170,42.2884273095886,-71.000040216754);");
    alasql("INSERT INTO patients VALUES ('ca8803ac-66ef-4895-a8c4-290313fcee6f','1980-05-06',null,'999-49-8670','S99952852','X444716X','Ms.','Rowena386','Borer986',null,null,'S','white','nonhispanic','F','Rockland  Massachusetts  US','825 Waters Landing','Somerville','Massachusetts','Middlesex County',2143,42.4136427791832,-71.1018363493116);");
    alasql("INSERT INTO patients VALUES ('9ec6d974-df2b-44ec-acc2-77d96725f4f4','1955-03-31',null,'999-89-7709','S99978137','X68810143X','Mrs.','Shala169','Keeling57',null,'Spinka232','M','white','nonhispanic','F','Wilbraham  Massachusetts  US','653 White Dam Unit 20','Quincy','Massachusetts','Norfolk County',2170,42.2741014792848,-71.0416632956233);");
    alasql("INSERT INTO patients VALUES ('bab51ea9-2945-4f8a-8015-e430f80a908e','2013-03-26',null,'999-65-6656',null,null,null,'Brooks264','Hirthe744',null,null,null,'black','nonhispanic','M','Marion  Massachusetts  US','330 Klein Mews','Boston','Massachusetts','Suffolk County',2121,42.3927574250863,-71.0940017494868);");
    alasql("INSERT INTO patients VALUES ('1d4f63d2-ddc0-47bc-b2cc-6f58068a5901','2014-10-16',null,'999-78-4113',null,null,null,'Imogene688','Friesen796',null,null,null,'white','nonhispanic','F','Brockton  Massachusetts  US','1057 Davis Walk Suite 15','Hanson','Massachusetts','Plymouth County',null,42.0425599559534,-70.8794910837097);");
    alasql("INSERT INTO patients VALUES ('2174522f-1d23-47cf-b56c-4ce3193c5bab','2009-10-16',null,'999-49-6603',null,null,null,'Alyce744','Prohaska837',null,null,null,'white','nonhispanic','F','Dedham  Massachusetts  US','909 Skiles Run Unit 77','Southwick','Massachusetts','Hampden County',null,42.0210227015259,-72.7761888145871);");
    alasql("INSERT INTO patients VALUES ('5c520448-6728-42cb-8cf2-457bc7c2b1f1','2008-08-31',null,'999-19-1869',null,null,null,'Noble66','Pagac496',null,null,null,'white','nonhispanic','M','Boston  Massachusetts  US','123 Jaskolski Terrace','Arlington','Massachusetts','Middlesex County',2474,42.45897606462,-71.1355026869962);");
    alasql("INSERT INTO patients VALUES ('a694ecfc-e39e-46dc-876e-029b32ba0135','2006-08-15',null,'999-31-6511',null,null,null,'Teisha100','Lockman863',null,null,null,'white','nonhispanic','F','Worcester  Massachusetts  US','475 Wolf Hollow Suite 53','Chicopee','Massachusetts','Hampden County',1020,42.1484557774119,-72.5498374009455);");
    alasql("INSERT INTO patients VALUES ('d7d1f837-a2c8-4648-b498-a02278b91a08','1987-03-11',null,'999-58-8342','S99936824','X83937653X','Mr.','Haywood675','Jast432',null,null,'M','white','nonhispanic','M','Gloucester  Massachusetts  US','757 Jenkins Crossroad','Wakefield','Massachusetts','Middlesex County',null,42.4884045933728,-71.0396652735026);");
    alasql("INSERT INTO patients VALUES ('8b119fdd-0fea-46dd-9106-b5c7813e7260','2000-07-25',null,'999-88-6112','S99942965',null,'Mr.','Christian753','Williamson769',null,null,null,'white','nonhispanic','M','Chelsea  Massachusetts  US','348 Beier Walk Unit 18','Medford','Massachusetts','Middlesex County',null,42.402047492327,-71.1513611400876);");
    alasql("INSERT INTO patients VALUES ('25f2b770-8821-48f0-a7fe-460dfe3b15b8','2000-05-15',null,'999-25-3887','S99990531',null,'Mr.','Winford225','Hoeger474',null,null,null,'white','nonhispanic','M','Holliston  Massachusetts  US','1027 Morar Road','Swansea','Massachusetts','Bristol County',null,41.7944024270971,-71.2494160992204);");
    alasql("INSERT INTO patients VALUES ('26ca976d-0b5b-4662-af41-535ff670dd5a','2014-09-22',null,'999-70-4950',null,null,null,'Shanti441','Lesch175',null,null,null,'white','nonhispanic','F','Boston  Massachusetts  US','234 Sawayn Drive','Amherst','Massachusetts','Hampshire County',null,42.4102817384995,-72.4674140466375);");
    alasql("INSERT INTO patients VALUES ('65243e61-da11-42a6-826e-a43d960d1e84','1990-09-16',null,'999-29-3981','S99976520','X24497111X','Mrs.','Anja508','Lubowitz58',null,'Huels583','M','white','nonhispanic','F','Lowell  Massachusetts  US','635 Bruen Bypass','New Bedford','Massachusetts','Bristol County',2744,41.6994267318715,-70.9845945618785);");
    alasql("INSERT INTO patients VALUES ('08ea9043-5f84-46ab-9815-81d90024169a','1960-10-27',null,'999-55-8341','S99929107','X74835475X','Mrs.','Mui729','Kihn564',null,'Ullrich385','M','white','nonhispanic','F','Somerville  Massachusetts  US','763 Smitham Rue','Worthington','Massachusetts','Hampshire County',null,42.368206635543,-72.9164963896378);");
    alasql("INSERT INTO patients VALUES ('a3abc11b-0fd1-4eb9-a69a-9075b4737612','1911-11-19',null,'999-47-9209','S99947584','X12431671X','Mr.','Irvin970','Goodwin327',null,null,'M','white','nonhispanic','M','Framingham  Massachusetts  US','114 Cummerata Parade','West Tisbury','Massachusetts','Dukes County',null,41.4143885882757,-70.6248836943567);");
    alasql("INSERT INTO patients VALUES ('5a1848d9-9b49-4529-94f1-e463b502c73b','1965-02-12','2000-03-03','999-56-6320','S99966540','X651013X','Mr.','Harris789','Metz686',null,null,'M','black','nonhispanic','M','Lowell  Massachusetts  US','904 Blick Pathway Apt 45','Easthampton','Massachusetts','Hampshire County',1027,42.3040223726238,-72.7895477376863);");
    alasql("INSERT INTO patients VALUES ('f504b982-e99b-4064-ad25-9e5480e769cd','1957-04-28',null,'999-15-8346','S99921820','X72325996X','Mr.','Jerrold404','Purdy2',null,null,'S','white','nonhispanic','M','Sutton  Massachusetts  US','544 Luettgen View Unit 73','Holyoke','Massachusetts','Hampden County',1040,42.1673552322292,-72.6594823655034);");
    alasql("INSERT INTO patients VALUES ('076688b0-f0d5-4c45-8bc6-b206684fa9ac','1959-04-24',null,'999-81-5413','S99922421','X12417642X','Ms.','Manie910','Torp761',null,null,'S','white','nonhispanic','F','Methuen  Massachusetts  US','359 Powlowski Parade','South Hadley','Massachusetts','Hampshire County',null,42.222849492012,-72.6142782115898);");
    alasql("INSERT INTO patients VALUES ('8aeeef59-43b3-4983-8f1d-54f3e8d5ea92','1988-01-03',null,'999-16-7944','S99944313','X89775026X','Mr.','Vaughn909','Beatty507',null,null,'M','white','nonhispanic','M','Rome  Lazio  IT','843 Wyman Village','Essex','Massachusetts','Essex County',null,42.6424493599461,-70.7641867001207);");
    alasql("INSERT INTO patients VALUES ('13db4bb8-d1dc-4158-a820-d2d1b7084fc4','1946-05-11',null,'999-99-3770','S99960086','X86202856X','Mrs.','Dulce933','Keebler762',null,'Hirthe744','M','white','nonhispanic','F','Everett  Massachusetts  US','911 Lebsack Route Unit 85','Malden','Massachusetts','Middlesex County',2155,42.3795762890418,-71.1151716913763);");
    alasql("INSERT INTO patients VALUES ('e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','2008-07-16',null,'999-93-5743',null,null,null,'Frances376','Schumm995',null,null,null,'white','nonhispanic','M','Chelsea  Massachusetts  US','826 Hammes Mission Apt 1','Natick','Massachusetts','Middlesex County',null,42.2584672080661,-71.3444415391516);");
    alasql("INSERT INTO patients VALUES ('b4d1167c-9adf-40e0-8295-4c3f25fdb3b4','2010-04-06',null,'999-62-7550',null,null,null,'Yong583','Zulauf375',null,null,null,'white','nonhispanic','M','Peabody  Massachusetts  US','791 Schoen Rest Suite 25','Wellesley','Massachusetts','Norfolk County',null,42.3121457217313,-71.238329167143);");
    alasql("INSERT INTO patients VALUES ('a4222dad-09bb-4049-b3dc-01b79f41012f','1963-05-21',null,'999-66-9769','S99944670','X8075158X','Mr.','Colin861','Jacobson885',null,null,'M','white','nonhispanic','M','Essex  Massachusetts  US','756 Skiles Underpass','Worcester','Massachusetts','Worcester County',1606,42.2410954683905,-71.8685532504472);");
    alasql("INSERT INTO patients VALUES ('0e1ad739-81d1-4170-841d-414ddc97c93a','1997-07-30',null,'999-18-8253','S99982219','X314488X','Ms.','Vanda440','Cruickshank494',null,null,null,'white','nonhispanic','F','Worcester  Massachusetts  US','811 Hoppe Loaf','Boston','Massachusetts','Suffolk County',2114,42.3819756196788,-71.0440702038103);");
    alasql("INSERT INTO patients VALUES ('92709d5b-63d2-4e47-b857-dccb09724ea3','1962-11-22',null,'999-37-3365','S99965897','X2354917X','Mr.','Diego848','Witting912',null,null,'M','white','nonhispanic','M','Wellesley  Massachusetts  US','811 Becker Gardens Unit 23','Medford','Massachusetts','Middlesex County',2145,42.4281944325234,-71.0711909930217);");
    alasql("INSERT INTO patients VALUES ('22415347-d2fb-4357-aaeb-ddba23e5cdf0','2018-03-12',null,'999-12-3897',null,null,null,'Buford910','Schultz619',null,null,null,'white','nonhispanic','M','Newburyport  Massachusetts  US','725 Fadel Byway Unit 97','Newburyport','Massachusetts','Essex County',1951,42.8505747515068,-70.8432980226202);");
    alasql("INSERT INTO patients VALUES ('b37c9b35-6fea-4570-b7f6-379baf4c9399','1982-07-25',null,'999-24-2069','S99948585','X63010439X','Mrs.','Stephany248','Larson43',null,'Hauck852','M','white','nonhispanic','F','New Bedford  Massachusetts  US','164 Friesen Trail Unit 13','Seekonk','Massachusetts','Bristol County',null,41.7946147725916,-71.2749171006568);");
    alasql("INSERT INTO patients VALUES ('b1b10f6e-e97a-426a-9581-f1a5c29b6e05','1914-09-06','1998-10-10','999-22-8602','S99961447','X28933673X','Mr.','Richie600','Steuber698',null,null,'M','white','nonhispanic','M','Boston  Massachusetts  US','1025 Kreiger Pathway','Worcester','Massachusetts','Worcester County',null,42.2313390454105,-71.7745961879125);");
    alasql("INSERT INTO patients VALUES ('0aaa2164-8de6-4152-8674-14d254aae13a','2008-05-11',null,'999-38-9488',null,null,null,'Drew592','Kunze215',null,null,null,'white','nonhispanic','M','Boston  Massachusetts  US','536 Cassin Mall','Lynn','Massachusetts','Essex County',1907,42.4261038655636,-70.969762559067);");
    alasql("INSERT INTO patients VALUES ('1c2aa038-9366-4c7d-9a3e-52cb753a670f','1962-09-13',null,'999-19-8817','S99966954','X83180931X','Mr.','Homero668','Carrillo204',null,null,'M','white','hispanic','M','Gaudalajara  Jalisco  MX','627 Weissnat Fork','Boston','Massachusetts','Suffolk County',2128,42.3109346386431,-71.0700902117231);");
    alasql("INSERT INTO patients VALUES ('8bf8631f-2fd4-4dc7-af2b-c400499fedfc','2002-03-14',null,'999-50-9855','S99949403',null,'Ms.','Enriqueta274','Ferry570',null,null,null,'black','nonhispanic','F','Norwood  Massachusetts  US','255 Christiansen Way','Uxbridge','Massachusetts','Worcester County',null,42.1010545480247,-71.5971893773271);");
    alasql("INSERT INTO patients VALUES ('c5d6bdfa-5554-4927-8fa8-794e5514cb56','1978-10-26',null,'999-42-1582','S99912021','X572354X','Mr.','Edmundo94','Romaguera67',null,null,'M','white','nonhispanic','M','Chelsea  Massachusetts  US','1065 Hackett Ville Suite 4','Gloucester','Massachusetts','Essex County',1930,42.5899646892389,-70.6792943103089);");
    alasql("INSERT INTO patients VALUES ('2a6d1e58-88eb-4be0-b6b4-59a471257c2e','1964-10-10',null,'999-22-8704','S99976805','X66668021X','Ms.','Nikia872','Herzog843',null,null,'S','white','nonhispanic','F','Wareham  Massachusetts  US','679 Robel Junction Apt 36','Quincy','Massachusetts','Norfolk County',2169,42.2640821758816,-71.0518467413496);");
    JSON.stringify(@0);
</script>
@end

@AlaSQL.buildTable_allergies
<script>
    alasql("DROP TABLE IF EXISTS allergies;");
    alasql("create table allergies (start date,stop date,patient text,encounter text,description text);");
    alasql("INSERT INTO allergies VALUES ('1982-10-25',null,'76982e06-f8b8-4509-9ca3-65a99c8650fe','b896bf40-8b72-42b7-b205-142ee3a56b55','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('1982-10-25',null,'76982e06-f8b8-4509-9ca3-65a99c8650fe','b896bf40-8b72-42b7-b205-142ee3a56b55','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-01-25',null,'71ba0469-f0cc-4177-ac70-ea07cb01c8b8','7be1a590-4239-4826-9872-031327f3c368','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2002-01-25',null,'71ba0469-f0cc-4177-ac70-ea07cb01c8b8','7be1a590-4239-4826-9872-031327f3c368','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2002-01-25',null,'71ba0469-f0cc-4177-ac70-ea07cb01c8b8','7be1a590-4239-4826-9872-031327f3c368','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2002-01-25',null,'71ba0469-f0cc-4177-ac70-ea07cb01c8b8','7be1a590-4239-4826-9872-031327f3c368','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2002-01-25',null,'71ba0469-f0cc-4177-ac70-ea07cb01c8b8','7be1a590-4239-4826-9872-031327f3c368','Allergy to soya');");
    alasql("INSERT INTO allergies VALUES ('2002-01-25',null,'71ba0469-f0cc-4177-ac70-ea07cb01c8b8','7be1a590-4239-4826-9872-031327f3c368','Allergy to fish');");
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
    alasql("INSERT INTO allergies VALUES ('2002-08-03',null,'ddfa05a6-b8a6-4208-b609-28f9a6a8d79e','5c63f72a-7af4-4bde-b416-f8f22a36239f','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-11-30',null,'df91aedf-3e17-44c3-b273-62e35f518475','e171e3c8-3543-40cc-8946-9918e044fb36','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2009-11-30',null,'df91aedf-3e17-44c3-b273-62e35f518475','e171e3c8-3543-40cc-8946-9918e044fb36','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-11-30',null,'df91aedf-3e17-44c3-b273-62e35f518475','e171e3c8-3543-40cc-8946-9918e044fb36','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-11-30',null,'df91aedf-3e17-44c3-b273-62e35f518475','e171e3c8-3543-40cc-8946-9918e044fb36','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-11-30',null,'df91aedf-3e17-44c3-b273-62e35f518475','e171e3c8-3543-40cc-8946-9918e044fb36','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1985-11-28',null,'0c5aa52a-3b1d-4289-af16-29f7a2d07fec','bdccf0d7-f3c2-4fc2-97df-9f0d41ae84f2','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2019-05-31',null,'2a88cb57-86fa-4259-93bf-3956e91b07f4','647eb0bb-5a2d-4617-9c00-fc97f8111994','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2019-05-31',null,'2a88cb57-86fa-4259-93bf-3956e91b07f4','647eb0bb-5a2d-4617-9c00-fc97f8111994','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2019-05-31',null,'2a88cb57-86fa-4259-93bf-3956e91b07f4','647eb0bb-5a2d-4617-9c00-fc97f8111994','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2019-05-31',null,'2a88cb57-86fa-4259-93bf-3956e91b07f4','647eb0bb-5a2d-4617-9c00-fc97f8111994','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2019-05-31',null,'2a88cb57-86fa-4259-93bf-3956e91b07f4','647eb0bb-5a2d-4617-9c00-fc97f8111994','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1974-05-17',null,'71e13815-55fb-4734-bcac-6079160d82a0','9607667e-4c98-4087-9c59-0fd5b6331078','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1974-05-17',null,'71e13815-55fb-4734-bcac-6079160d82a0','9607667e-4c98-4087-9c59-0fd5b6331078','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1974-05-17',null,'71e13815-55fb-4734-bcac-6079160d82a0','9607667e-4c98-4087-9c59-0fd5b6331078','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1957-06-06',null,'852f4588-ba8f-4eeb-94fe-5c3c974ab114','aa997e7e-3421-4f40-8216-394f407de8c2','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1954-03-21',null,'09616ead-22c8-4210-8cb9-2fdc28e043ca','09e77dbd-a408-4459-9c75-8996d28bf972','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1975-12-23',null,'f7d7b580-e670-4921-bc67-550ec468d506','0e201aa3-271d-4fa0-b6db-d73aa17f776d','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1978-11-04',null,'24bca5cf-ba55-457f-8e80-49690202443c','1d475126-f3c0-41c9-a9ed-f4a0c9a955c4','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2009-04-29',null,'f36775e8-bf5a-40a2-a223-679c05b46004','bed7bbbf-9d73-4840-ae9a-039af0918b91','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2009-04-29',null,'f36775e8-bf5a-40a2-a223-679c05b46004','bed7bbbf-9d73-4840-ae9a-039af0918b91','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-04-29',null,'f36775e8-bf5a-40a2-a223-679c05b46004','bed7bbbf-9d73-4840-ae9a-039af0918b91','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-04-29',null,'f36775e8-bf5a-40a2-a223-679c05b46004','bed7bbbf-9d73-4840-ae9a-039af0918b91','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-04-29',null,'f36775e8-bf5a-40a2-a223-679c05b46004','bed7bbbf-9d73-4840-ae9a-039af0918b91','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-04-29',null,'f36775e8-bf5a-40a2-a223-679c05b46004','bed7bbbf-9d73-4840-ae9a-039af0918b91','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1989-02-22',null,'8be68b2d-8054-4882-8715-65297d36767a','d7d1a76c-65a9-499d-8a9f-5f65f1a3d797','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1989-02-22',null,'8be68b2d-8054-4882-8715-65297d36767a','d7d1a76c-65a9-499d-8a9f-5f65f1a3d797','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1986-12-09',null,'59669e7c-0190-4cbf-9e27-4898f1a35d03','d19a6b59-77a3-46ee-b8fd-4f59e8cee292','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2016-04-25',null,'4b92e3f1-b92b-48ec-9baa-2905409d1743','afb2659d-c3de-4ff9-aad0-b4283ff08443','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10','2015-11-30','9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10','2015-11-30','9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10','2015-11-30','9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10','2015-11-30','9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10','2015-11-30','9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10',null,'9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10',null,'9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-03-10',null,'9fda53d4-6fcc-4ef5-a1fe-16e007182ec2','da2fa819-307a-410d-9b95-6ea21a2f19ae','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2016-05-15',null,'8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','64599133-2dd7-4b23-b2eb-2e94a887a266','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2016-05-15',null,'8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','64599133-2dd7-4b23-b2eb-2e94a887a266','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2016-05-15',null,'8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','64599133-2dd7-4b23-b2eb-2e94a887a266','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2016-05-15',null,'8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','64599133-2dd7-4b23-b2eb-2e94a887a266','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2016-05-15',null,'8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','64599133-2dd7-4b23-b2eb-2e94a887a266','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2016-05-15',null,'8ba79a69-6f3f-4aa5-be2a-dfbd0119d3ea','64599133-2dd7-4b23-b2eb-2e94a887a266','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2016-04-12',null,'128d5c93-dfce-49a1-8d08-e9b24abcb4db','0a61339f-5663-4af2-a674-0ed5bcee8129','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2008-03-16',null,'eba0f292-1951-4af9-9f14-9d6e9db8e480','3a7465cc-5b2f-4e96-934c-7e74c9537cf8','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2009-10-12',null,'5846c531-ad71-4b34-9607-3a1022cddffa','3e655890-c6c1-47c7-8aa0-378936645135','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2004-01-29',null,'d00fc5dd-be0e-47aa-a52a-5cf9ee3a78b5','6eb061c1-d1f6-4109-8da9-397e102f5e7c','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2004-01-29',null,'d00fc5dd-be0e-47aa-a52a-5cf9ee3a78b5','6eb061c1-d1f6-4109-8da9-397e102f5e7c','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1964-04-22',null,'7e4e6d32-15cd-4d5f-a4cc-e5c95ab35eb0','479131ac-4344-4846-b8ca-71b4745224ef','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1964-04-22',null,'7e4e6d32-15cd-4d5f-a4cc-e5c95ab35eb0','479131ac-4344-4846-b8ca-71b4745224ef','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2018-03-20',null,'841095eb-d29f-4492-8f0e-08011321e85d','32622f63-734e-4433-8628-942ce1585e6a','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2014-04-24',null,'e112cedd-a98e-489e-abb0-875420d40397','d47a0669-53b1-4914-b18d-dd593f7e1767','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1971-03-07',null,'ab6a2662-f6d1-4da6-b3ce-3929d68650d7','603a0692-9302-459a-84b4-af631dc3aee8','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1966-02-10',null,'c844d8ac-d5bf-45eb-b8cf-84327c9a4e97','93f2e585-25a9-447e-9037-f2acb4533af7','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1987-09-24',null,'c4f221f2-611b-4cdd-a0b6-958bbbfcf346','c4bd5699-1d3f-421a-afff-b61489fc7304','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1990-08-01',null,'77a0cd86-92bb-4c6d-a91a-49ee66e353b9','ef0b7128-4a42-44af-b58d-4b9d79b3b949','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1990-08-01',null,'77a0cd86-92bb-4c6d-a91a-49ee66e353b9','ef0b7128-4a42-44af-b58d-4b9d79b3b949','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1995-06-24','2010-10-28','ea8e6623-5590-4d01-bfc1-25d86b1b4491','4d8f3b3f-c46e-464a-9298-97e713fdcbd3','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1995-06-24',null,'ea8e6623-5590-4d01-bfc1-25d86b1b4491','4d8f3b3f-c46e-464a-9298-97e713fdcbd3','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Allergy to soya');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2006-09-04',null,'1a289f28-b73b-4d3a-83ef-2216e8837bad','08c25a44-753e-4a0f-bd30-6bce5105bd71','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1981-03-11',null,'ca8803ac-66ef-4895-a8c4-290313fcee6f','6dae0a63-104c-468a-8796-028bd0991bcf','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('1981-03-11',null,'ca8803ac-66ef-4895-a8c4-290313fcee6f','6dae0a63-104c-468a-8796-028bd0991bcf','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1981-03-11',null,'ca8803ac-66ef-4895-a8c4-290313fcee6f','6dae0a63-104c-468a-8796-028bd0991bcf','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1981-03-11',null,'ca8803ac-66ef-4895-a8c4-290313fcee6f','6dae0a63-104c-468a-8796-028bd0991bcf','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1981-03-11',null,'ca8803ac-66ef-4895-a8c4-290313fcee6f','6dae0a63-104c-468a-8796-028bd0991bcf','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1981-03-11',null,'ca8803ac-66ef-4895-a8c4-290313fcee6f','6dae0a63-104c-468a-8796-028bd0991bcf','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1956-09-04',null,'9ec6d974-df2b-44ec-acc2-77d96725f4f4','d8789ff6-c8dc-474c-98e8-8225e749a9b0','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2014-06-07',null,'bab51ea9-2945-4f8a-8015-e430f80a908e','3bf855b5-6f5a-45b6-a248-870298609636','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2016-01-24',null,'1d4f63d2-ddc0-47bc-b2cc-6f58068a5901','82952a6b-28a4-443a-9dcf-bee496f1256d','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2016-01-24',null,'1d4f63d2-ddc0-47bc-b2cc-6f58068a5901','82952a6b-28a4-443a-9dcf-bee496f1256d','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2010-04-03',null,'2174522f-1d23-47cf-b56c-4ce3193c5bab','89685e6f-cb99-47aa-8304-e9bc4506e7cd','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-06-17',null,'5c520448-6728-42cb-8cf2-457bc7c2b1f1','318437d0-9900-460a-a870-8d3b259b488a','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2008-01-21',null,'a694ecfc-e39e-46dc-876e-029b32ba0135','33d7e56a-61f4-4123-beb1-91b0fbc9727b','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1988-02-20',null,'d7d1f837-a2c8-4648-b498-a02278b91a08','22f68bb1-06c5-4585-a74e-2924df8c6e93','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31',null,'8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31','2017-01-31','8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31','2017-01-31','8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31','2017-01-31','8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31','2017-01-31','8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31','2017-01-31','8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31',null,'8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2001-05-31',null,'8b119fdd-0fea-46dd-9106-b5c7813e7260','c0779655-785c-4ee7-9f19-afe8b8c96d4c','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01',null,'25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01','2016-11-21','25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01',null,'25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01','2016-11-21','25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01','2016-11-21','25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01','2018-04-11','25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01','2018-04-11','25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2000-11-01',null,'25f2b770-8821-48f0-a7fe-460dfe3b15b8','41baabeb-59ef-42d5-a69b-2a070d6f9acd','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','Latex allergy');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2015-07-28',null,'26ca976d-0b5b-4662-af41-535ff670dd5a','9ef70ad2-ee78-4685-b90f-aaae12dc2e7f','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1991-07-26',null,'65243e61-da11-42a6-826e-a43d960d1e84','4bd55d09-eca0-4e6e-b04d-a81b0906f8d8','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1962-03-31',null,'08ea9043-5f84-46ab-9815-81d90024169a','4d41baa1-4c54-4c57-be01-e67f0234d4fe','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1962-03-31',null,'08ea9043-5f84-46ab-9815-81d90024169a','4d41baa1-4c54-4c57-be01-e67f0234d4fe','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1912-07-29',null,'a3abc11b-0fd1-4eb9-a69a-9075b4737612','a2e3c7b6-8c3e-4f13-8937-c41fb5a802ef','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1966-01-27',null,'5a1848d9-9b49-4529-94f1-e463b502c73b','04e5b80a-5df7-45f4-a245-a15632dd0fd7','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1966-01-27',null,'5a1848d9-9b49-4529-94f1-e463b502c73b','04e5b80a-5df7-45f4-a245-a15632dd0fd7','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1966-01-27',null,'5a1848d9-9b49-4529-94f1-e463b502c73b','04e5b80a-5df7-45f4-a245-a15632dd0fd7','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1966-01-27',null,'5a1848d9-9b49-4529-94f1-e463b502c73b','04e5b80a-5df7-45f4-a245-a15632dd0fd7','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1966-01-27',null,'5a1848d9-9b49-4529-94f1-e463b502c73b','04e5b80a-5df7-45f4-a245-a15632dd0fd7','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1958-06-07',null,'f504b982-e99b-4064-ad25-9e5480e769cd','fb994807-4ced-47c8-a0ca-7687d22ad182','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1959-07-21',null,'076688b0-f0d5-4c45-8bc6-b206684fa9ac','7b116d0a-949d-492f-9294-8a170f5018c3','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1959-07-21',null,'076688b0-f0d5-4c45-8bc6-b206684fa9ac','7b116d0a-949d-492f-9294-8a170f5018c3','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1959-07-21',null,'076688b0-f0d5-4c45-8bc6-b206684fa9ac','7b116d0a-949d-492f-9294-8a170f5018c3','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1959-07-21',null,'076688b0-f0d5-4c45-8bc6-b206684fa9ac','7b116d0a-949d-492f-9294-8a170f5018c3','Allergy to nut');");
    alasql("INSERT INTO allergies VALUES ('1989-05-10',null,'8aeeef59-43b3-4983-8f1d-54f3e8d5ea92','3f6184c9-cdf7-4dae-96e3-9de0f8e4e2c1','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1989-05-10',null,'8aeeef59-43b3-4983-8f1d-54f3e8d5ea92','3f6184c9-cdf7-4dae-96e3-9de0f8e4e2c1','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1989-05-10',null,'8aeeef59-43b3-4983-8f1d-54f3e8d5ea92','3f6184c9-cdf7-4dae-96e3-9de0f8e4e2c1','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1947-07-14',null,'13db4bb8-d1dc-4158-a820-d2d1b7084fc4','df30f405-c6e2-404b-8847-7c12f8aa46bf','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('2009-01-22',null,'e188fafe-c1bb-45dc-9627-4ff4e4bc0ec0','5e4a49f2-47e7-4b76-9120-276a79f1766f','Allergy to peanuts');");
    alasql("INSERT INTO allergies VALUES ('2011-09-07',null,'b4d1167c-9adf-40e0-8295-4c3f25fdb3b4','1a2c3225-add3-422b-937b-0bf791e66bb4','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2011-09-07',null,'b4d1167c-9adf-40e0-8295-4c3f25fdb3b4','1a2c3225-add3-422b-937b-0bf791e66bb4','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1964-08-26',null,'a4222dad-09bb-4049-b3dc-01b79f41012f','51f66013-e9ba-442b-b720-4aa68fba0a9c','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1964-08-26',null,'a4222dad-09bb-4049-b3dc-01b79f41012f','51f66013-e9ba-442b-b720-4aa68fba0a9c','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1964-08-26',null,'a4222dad-09bb-4049-b3dc-01b79f41012f','51f66013-e9ba-442b-b720-4aa68fba0a9c','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1964-08-26',null,'a4222dad-09bb-4049-b3dc-01b79f41012f','51f66013-e9ba-442b-b720-4aa68fba0a9c','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17','2014-02-05','0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17',null,'0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17',null,'0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17','2014-02-05','0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17',null,'0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17','2015-10-05','0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('1998-03-17',null,'0e1ad739-81d1-4170-841d-414ddc97c93a','99ff05be-e932-4a00-b691-b2d974bc5851','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-08-04',null,'92709d5b-63d2-4e47-b857-dccb09724ea3','3da0bdc7-2715-4b76-8cc4-2272a920eab3','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1963-08-04',null,'92709d5b-63d2-4e47-b857-dccb09724ea3','3da0bdc7-2715-4b76-8cc4-2272a920eab3','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-08-04',null,'92709d5b-63d2-4e47-b857-dccb09724ea3','3da0bdc7-2715-4b76-8cc4-2272a920eab3','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1963-08-04',null,'92709d5b-63d2-4e47-b857-dccb09724ea3','3da0bdc7-2715-4b76-8cc4-2272a920eab3','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Allergy to dairy product');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('2019-07-17',null,'22415347-d2fb-4357-aaeb-ddba23e5cdf0','15951b86-bc15-4467-8d07-1f5177bfd59d','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Allergy to bee venom');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','House dust mite allergy');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Allergy to dairy product');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1983-04-09',null,'b37c9b35-6fea-4570-b7f6-379baf4c9399','cc65c8d0-1e3b-4c47-aa9f-43bd15bb79c6','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1915-08-19',null,'b1b10f6e-e97a-426a-9581-f1a5c29b6e05','31953388-58de-49eb-af31-437b88156a9e','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1915-08-19',null,'b1b10f6e-e97a-426a-9581-f1a5c29b6e05','31953388-58de-49eb-af31-437b88156a9e','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Allergy to dairy product');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Allergy to eggs');");
    alasql("INSERT INTO allergies VALUES ('2008-11-21',null,'0aaa2164-8de6-4152-8674-14d254aae13a','4c82c24d-7a02-49a9-ba7e-d1b002ab96dd','Allergy to fish');");
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
    alasql("INSERT INTO allergies VALUES ('2003-05-18',null,'8bf8631f-2fd4-4dc7-af2b-c400499fedfc','90ea604b-e2bf-4ca0-a46b-dc480f2f2a03','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('2003-05-18',null,'8bf8631f-2fd4-4dc7-af2b-c400499fedfc','90ea604b-e2bf-4ca0-a46b-dc480f2f2a03','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1979-07-10',null,'c5d6bdfa-5554-4927-8fa8-794e5514cb56','e42aa828-ad33-404f-af29-f1f9f17bd7f5','Allergy to fish');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to mould');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Dander (animal) allergy');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to grass pollen');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to tree pollen');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to wheat');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Shellfish allergy');");
    alasql("INSERT INTO allergies VALUES ('1965-09-23',null,'2a6d1e58-88eb-4be0-b6b4-59a471257c2e','f7ff5032-50cc-480e-90ca-848c85d6d014','Allergy to peanuts');");
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

Test 4.


```sql
SELECT * FROM patients
```
@AlaSQL.eval("#dataTable3")

<table id="dataTable2" border="1"></table><br>

<hr/><hr/>

@AlaSQL.buildTable_patients('`patients` table loaded!')

@AlaSQL.buildTable_allergies('`allergies` table loaded!')

