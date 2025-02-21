# utl-string-functions-to-parse-messy-four-word--string-into-name-grade-and-weight-sas-r-python-excel
String functions to parse messy four word string into name grade and weight sas r python excel 
    %let pgm=utl-string-functions-to-parse-messy-four-word--string-into-name-grade-and-weight-sas-r-python-excel;

    %stop_submission;

    String functions to parse messy four word string into name grade and weight sas r python excel

    github
    https://tinyurl.com/5t5cb3vp
    https://github.com/rogerjdeangelis/utl-string-functions-to-parse-messy-four-word--string-into-name-grade-and-weight-sas-r-python-excel

    stackoverflow R
    https://tinyurl.com/23t8r326
    https://stackoverflow.com/questions/79447615/splitting-one-column-to-three-columns-for-uneven-characters-in-r

    SOAPBOX ON
    I could not get any of the posted R solutions to work, some used the wrong input. Maybe my bad?
    All seem a little obscure, especially some of regular expressions non solutions?
    SOAPBOX OFF

        SOLUTIONS and NON SOLUTION

           1 sas load infile buffer (macro on end ane=d in https://tinyurl.com/y9nfugth)
             This allows you to use sas input/cards4 processing on any string in a permanent sas dataset.


           2 sas cards4 infile buffer
           3 sas datastep
           4 sas sql

           5 r failed stringr language (only works with the wrong input - could not get any of them to work - my bad?)
             WRONG INPUT
             Jhon Austin B 100kg
             Mick Gray C 110kg
             Tom Jef A 30kg    ==> should be Tom Jef A30kg

           6 r sql
           7 python sql
           8 excel sql
           9 loadinfile macro


    /****************************************************************************************************************************/
    /*                                   |                                                        |                             */
    /*                                   |                                                        |                             */
    /*       INPUT                       |   PROCESSS                                             |         OUTPUT              */
    /*       =====                       |   ========                                             |         =====               */
    /*                                   |                                                        |                             */
    /*                                   |                                                        |                             */
    /* SD1.HAVE                          |  1 SAS LOAD INFILE BUFFER                              |  SD1.WANT                   */
    /*                                   |  =========================                             |                             */
    /*         STR                       |                                                        |  FIRST  LAST   GRADE WEIGHT */
    /*                                   |  %utlfkil(%sysfunc(getoption(WORK))/tmp.txt);          |                             */
    /* Jhon Austin B 100kg               |                                                        |   Jhon  Austin   B   100kg  */
    /* Mick Gray C 110kg                 |  * save macro in autocall folder;                      |   Mick  Gray     C   110kg  */
    /* Tom Jef A30kg                     |  filename ft15f001 "c:/oto/loadinfile.sas";            |   Tom   Jef      A   30kg   */
    /*                                   |  parncards4;                                           |                             */
    /* options validvarname=upcase;      |  %macro loadinfile(str)                                |                             */
    /* libname sd1 "d:/sd1";             |    /des="load string into the infile buffer";          |                             */
    /* data sd1.have;                    |   %dosubl('                                            |                             */
    /*  str="Jhon Austin B 100kg";output;|     data _null_;                                       |                             */
    /*  str="Mick Gray C 110kg";output;  |       file "%sysfunc(getoption(WORK))/tmp.txt";        |                             */
    /*  str="Tom Jef A30kg";output;      |       put "*";                                         |                             */
    /* run;quit;                         |     run;quit;                                          |                             */
    /*                                   |     ');                                                |                             */
    /*                                   |    infile  "%sysfunc(getoption(WORK))/tmp.txt";        |                             */
    /*                                   |    input @;                                            |                             */
    /*                                   |    _infile_ = &str;                                    |                             */
    /*                                   |                                                        |                             */
    /*                                   |  %mend loadinfile;                                     |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc datasets lib=work nolist nodetails;              |                             */
    /*                                   |   delete want;                                         |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  data want ;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   |    set sd1.have;                                       |                             */
    /*                                   |                                                        |                             */
    /*                                   |    informat first last $8.                             |                             */
    /*                                   |            therest $24. grade $1.;                     |                             */
    /*                                   |                                                        |                             */
    /*                                   |    * load _infile_ buffer with variable str;           |                             */
    /*                                   |    %loadinfile(str);                                   |                             */
    /*                                   |                                                        |                             */
    /*                                   |    input first last therest & @;                       |                             */
    /*                                   |                                                        |                             */
    /*                                   |    grade=compress(therest);                            |                             */
    /*                                   |    weight=substr(compress(therest),2);                 |                             */
    /*                                   |                                                        |                             */
    /*                                   |    * restart the buffer;                               |                             */
    /*                                   |    input @1 @@;                                        |                             */
    /*                                   |                                                        |                             */
    /*                                   |    keep first last grade weight;                       |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  ------------------------------------------------------|                             */
    /*                                   |                                                        |                             */
    /*                                   |  2 SAS CARDS INFILE BUFFER                             |                             */
    /*                                   |  =========================                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc datasets lib=work nolist nodetails;              |                             */
    /*                                   |   delete want;                                         |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  data want; ;                                          |                             */
    /*                                   |    set sd1.have;                                       |                             */
    /*                                   |    informat first last $8. therest $24. class $1.;     |                             */
    /*                                   |    input @;                                            |                             */
    /*                                   |    _infile_ = str;                                     |                             */
    /*                                   |    input first last therest & @;                       |                             */
    /*                                   |    class=compress(therest);                            |                             */
    /*                                   |    pack=substr(compress(therest),2);                   |                             */
    /*                                   |    input @1 @@;                                        |                             */
    /*                                   |    keep first last class pack;                         |                             */
    /*                                   |  cards4;                                               |                             */
    /*                                   |  *                                                     |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  ------------------------------------------------------|---------------------------- */
    /*                                   |                                                        |                             */
    /*                                   |  3 SAS DATASTEP                                        |                             */
    /*                                   |  ==============                                        | NAME        CLASS    PACK   */
    /*                                   |                                                        |                             */
    /*                                   |  proc datasets lib=work nolist nodetails;              | Jhon Austin   B      100kg  */
    /*                                   |   delete want;                                         | Mick Gray     C      110kg  */
    /*                                   |  run;quit;                                             | Tom Jef       A      30kg   */
    /*                                   |                                                        |                             */
    /*                                   |  data want;                                            |                             */
    /*                                   |                                                        |                             */
    /*                                   |     length class $1; * force to length=1;              |                             */
    /*                                   |                                                        |                             */
    /*                                   |     set sd1.have;                                      |                             */
    /*                                   |                                                        |                             */
    /*                                   |     CALL SCAN(str, 2, position, length);               |                             */
    /*                                   |                                                        |                             */
    /*                                   |     * find position & length for second word           |                             */
    /*                                   |       and position after the name                      |                             */
    /*                                   |                                                        |                             */
    /*                                   |       consider "Jhon Austin B  100kg"                  |                             */
    /*                                   |       position  Length                                 |                             */
    /*                                   |            6    6                                      |                             */
    /*                                   |            |    |  length+position+1                   |                             */
    /*                                   |            |    | 13 position of B                     |                             */
    /*                                   |            |    | |                                    |                             */
    /*                                   |            |   1111                                    |                             */
    /*                                   |       1234567890123                                    |                             */
    /*                                   |       Jhon Austin B                                    |                             */
    /*                                   |                                                        |                             */
    /*                                   |       starting position of the rest of the string      |                             */
    /*                                   |       is length_position+1;                            |                             */
    /*                                   |                                                        |                             */
    /*                                   |     name=substr(str,1,position+length); *Jhon Austin;  |                             */
    /*                                   |                                                        |                             */
    /*                                   |     therest=compress(substr(str,position+length+1));   |                             */
    /*                                   |     * B100kg;                                          |                             */
    /*                                   |                                                        |                             */
    /*                                   |     class=therest; * forced to be length 1 B;          |                             */
    /*                                   |                                                        |                             */
    /*                                   |     pack=substr(therest,2); * 100kg skip over class;   |                             */
    /*                                   |                                                        |                             */
    /*                                   |     KEEP name class pack; * Jhon Austin B 100kg;       |                             */
    /*                                   |                                                        |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |--------------------------------------------------------|                             */
    /*                                   |                                                        |                             */
    /*                                   |  4 SAS SQL                                             |                             */
    /*                                   |  =========                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc datasets lib=work nolist nodetails;              |                             */
    /*                                   |   delete want;                                         |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc sql;                                             |                             */
    /*                                   |   create                                               |                             */
    /*                                   |     table want(drop=name_length therest) as            |                             */
    /*                                   |   select                                               |                             */
    /*                                   |     find(str,scan(str,2))+length(scan(str,2))          |                             */
    /*                                   |       as name_length                                   |                             */
    /*                                   |    ,substr(str,1,calculated name_length)               |                             */
    /*                                   |       as name                                          |                             */
    /*                                   |    ,compress(substr(str,calculated name_length+1))     |                             */
    /*                                   |       as therest                                       |                             */
    /*                                   |    ,calculated therest                                 |                             */
    /*                                   |       as class length=1                                |                             */
    /*                                   |    ,substr(calculated therest,2)                       |                             */
    /*                                   |       as pack                                          |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |     sd1.have                                           |                             */
    /*                                   |  ;quit;                                                |                             */
    /*                                   |                                                        |                             */
    /*                                   |--------------------------------------------------------|-----------------------------*/
    /*                                   |                                                        |                             */
    /*                                   |  5 R FAILED STRINGR LANGUAGE                           |With Correct input           */
    /*                                   |  ===========================                           |                             */
    /*                                   |                                                        |Tom Jef A30kg                */
    /*                                   |  %utl_rbeginx;                                         |                             */
    /*                                   |  parmcards4;                                           |Error separate_wider_regex() */
    /*                                   |  library(haven)                                        |Expected each value of `name */
    /*                                   |  library(tidyverse)                                    |to match the pattern,        */
    /*                                   |  library(stringr)                                      |the whle pattern,            */
    /*                                   |  source("c:/oto/fn_tosas9x.R")                         |and nothing but the pattern. */
    /*                                   |  have<-read_sas("d:/sd1/have.sas7bdat")                |                             */
    /*                                   |  print(have)                                           |                             */
    /*                                   |  # WRONG INPUT                                         |                             */
    /*                                   |  dat <- data.frame(                                    |                             */
    /*                                   |      name = c(                                         |                             */
    /*                                   |          "Jhon Austin B 100kg",                        |                             */
    /*                                   |          "Mick Gray C 110kg",                          |                             */
    /*                                   |          "Tom Jef A 30kg"                              |                             */
    /*                                   |      )                                                 |                             */
    /*                                   |  )                                                     |                             */
    /*                                   |  dat %>%                                               |                             */
    /*                                   |      separate_wider_regex(                             |                             */
    /*                                   |          name,                                         |                             */
    /*                                   |          patterns = c(name = ".*", " "                 |                             */
    /*                                   |         ,class = "\\w", " ", pack = "\\d+kg")          |                             */
    /*                                   |      )                                                 |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  %utl_rendx;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   |--------------------------------------------------------|                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc datasets lib=sd1 nolist nodetails;               |                             */
    /*                                   |   delete pywant;                                       |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |  %utlfkil(d:/xls/wantxl.xlsx);                         |                             */
    /*                                   |                                                        |                             */
    /*                                   |  %utl_rbeginx;                                         |                             */
    /*                                   |  parmcards4;                                           |                             */
    /*                                   |  library(openxlsx)                                     |                             */
    /*                                   |  library(sqldf)                                        |                             */
    /*                                   |  library(haven)                                        |                             */
    /*                                   |  have<-read_sas("d:/sd1/have.sas7bdat")                |                             */
    /*                                   |  wb <- createWorkbook()                                |                             */
    /*                                   |  addWorksheet(wb, "have")                              |                             */
    /*                                   |  writeData(wb, sheet = "have", x = have)               |                             */
    /*                                   |  saveWorkbook(                                         |                             */
    /*                                   |      wb                                                |                             */
    /*                                   |     ,"d:/xls/wantxl.xlsx"                              |                             */
    /*                                   |     ,overwrite=TRUE)                                   |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  %utl_rendx;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   |  %utl_rbeginx;                                         |                             */
    /*                                   |  parmcards4;                                           |                             */
    /*                                   |  library(openxlsx)                                     |                             */
    /*                                   |  library(sqldf)                                        |                             */
    /*                                   |  source("c:/oto/fn_tosas9x.R")                         |                             */
    /*                                   |   wb<-loadWorkbook("d:/xls/wantxl.xlsx")               |                             */
    /*                                   |   have<-read.xlsx(wb,"have")                           |                             */
    /*                                   |   addWorksheet(wb, "want")                             |                             */
    /*                                   |   want<-sqldf('                                        |                             */
    /*                                   |   with                                                 |                             */
    /*                                   |      poslen as (                                       |                             */
    /*                                   |   select                                               |                             */
    /*                                   |      str                                               |                             */
    /*                                   |     ,instr(substr(str,instr(str," ")+ 1)," ")          |                             */
    /*                                   |         +instr(str, " ")+1 as name_length              |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |      have                                              |                             */
    /*                                   |   )                                                    |                             */
    /*                                   |   select                                               |                             */
    /*                                   |     name_length                                        |                             */
    /*                                   |    ,substr(str,1,name_length-1) as name                |                             */
    /*                                   |    ,substr(replace(substr(str,name_length)," ",""),1,1)|                             */
    /*                                   |      as class                                          |                             */
    /*                                   |    ,replace(substr(str,name_length +1)," ","") as pack |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |      poslen                                            |                             */
    /*                                   |   ')                                                   |                             */
    /*                                   |   print(want)                                          |                             */
    /*                                   |   writeData(wb,sheet="want",x=want)                    |                             */
    /*                                   |   saveWorkbook(                                        |                             */
    /*                                   |       wb                                               |                             */
    /*                                   |      ,"d:/xls/wantxl.xlsx"                             |                             */
    /*                                   |      ,overwrite=TRUE)                                  |                             */
    /*                                   |  fn_tosas9x(                                           |                             */
    /*                                   |        inp    = want                                   |                             */
    /*                                   |       ,outlib ="d:/sd1/"                               |                             */
    /*                                   |       ,outdsn ="want"                                  |                             */
    /*                                   |       )                                                |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  %utl_rendx;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc print data=sd1.want(drop=rownames name_length);  |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /*                                   |--------------------------------------------------------|---------------------------- */
    /*                                   |                                                        |                             */
    /*                                   |  6 R SQL                                               | R                           */
    /*                                   |  =======                                               |                             */
    /*                                   |                                                        |        name  class  pack    */
    /*                                   |  proc datasets lib=sd1 nolist nodetails;               | Jhon Austin      B 100kg    */
    /*                                   |   delete want;                                         |   Mick Gray      C 110kg    */
    /*                                   |  run;quit;                                             |     Tom Jef      A  30kg    */
    /*                                   |                                                        |                             */
    /*                                   |  %utl_rbeginx;                                         | SAS                         */
    /*                                   |  parmcards4;                                           |                             */
    /*                                   |  library(haven)                                        | NAME        CLASS    PACK   */
    /*                                   |  library(sqldf)                                        |                             */
    /*                                   |  source("c:/oto/fn_tosas9x.r")                         | Jhon Austin   B      100kg  */
    /*                                   |  have<-read_sas("d:/sd1/have.sas7bdat")                | Mick Gray     C      110kg  */
    /*                                   |  want<-sqldf('                                         | Tom Jef       A      30kg   */
    /*                                   |   with                                                 |                             */
    /*                                   |      poslen as (                                       |                             */
    /*                                   |   select                                               |                             */
    /*                                   |      str                                               |                             */
    /*                                   |     ,instr(substr(str,instr(str," ")+ 1)," ")          |                             */
    /*                                   |         +instr(str, " ")+1 as name_length              |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |      have                                              |                             */
    /*                                   |   )                                                    |                             */
    /*                                   |   select                                               |                             */
    /*                                   |     name_length                                        |                             */
    /*                                   |    ,substr(str,1,name_length-1)  as name               |                             */
    /*                                   |    ,substr(replace(substr(str,name_length)," ",""),1,1)|                             */
    /*                                   |      as class                                          |                             */
    /*                                   |    ,replace(substr(str,name_length +1)," ","") as pack |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |      poslen                                            |                             */
    /*                                   |   ')                                                   |                             */
    /*                                   |  want                                                  |                             */
    /*                                   |  fn_tosas9x(                                           |                             */
    /*                                   |        inp    = want                                   |                             */
    /*                                   |       ,outlib ="d:/sd1/"                               |                             */
    /*                                   |       ,outdsn ="want"                                  |                             */
    /*                                   |       )                                                |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  %utl_rendx;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   |--------------------------------------------------------|-----------------------------*/
    /*                                   |                                                        |                             */
    /*                                   |  7 PYTHON SQL                                          | PYTHON                      */
    /*                                   |  ============                                          | ======                      */
    /*                                   |                                                        |           name class   pack */
    /*                                   | proc datasets lib=sd1 nolist nodetails;                | 0 Jhon Austin      B  100kg */
    /*                                   |  delete pywant;                                        | 1   Mick Gray      C  110kg */
    /*                                   | run;quit;                                              | 2     Tom Jef      A   30kg */
    /*                                   |                                                        |                             */
    /*                                   | %utl_pybeginx;                                         | SAS                         */
    /*                                   | parmcards4;                                            | ===                         */
    /*                                   | exec(open('c:/oto/fn_python.py').read());              | NAME        CLASS    PACK   */
    /*                                   | have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');  |                             */
    /*                                   | want=pdsql('''    \                                    | Jhon Austin   B      100kg  */
    /*                                   |  with             \                                    | Mick Gray     C      110kg  */
    /*                                   |     poslen as (   \                                    | Tom Jef       A      30kg   */
    /*                                   |  select           \                                    |                             */
    /*                                   |     str           \                                    |                             */
    /*                                   |    ,instr(substr(str,instr(str," ")+ 1)," ") \         |                             */
    /*                                   |        +instr(str, " ")+1 as name_length \             |                             */
    /*                                   |  from \                                                |                             */
    /*                                   |     have \                                             |                             */
    /*                                   |  )       \                                             |                             */
    /*                                   |  select  \                                             |                             */
    /*                                   |   name_length \                                        |                             */
    /*                                   |  ,substr(str,1,name_length-1) as name \                |                             */
    /*                                   |  ,substr(replace(substr(str,name_length)," ",""),1,1) \|                             */
    /*                                   |    as class \                                          |                             */
    /*                                   |  ,replace(substr(str,name_length +1)," ","") as pack \ |                             */
    /*                                   |  from \                                                |                             */
    /*                                   |    poslen                                              |                             */
    /*                                   |    ''')                                                |                             */
    /*                                   | print(want);                                           |                             */
    /*                                   | fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant')      |                             */
    /*                                   |                                                        |                             */
    /*                                   | ;;;;                                                   |                             */
    /*                                   | %utl_pyendx;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   | proc print data=sd1.pywant(drop=name_length);          |                             */
    /*                                   | run;quit;                                              |                             */
    /*                                   |                                                        |                             */
    /*                                   |--------------------------------------------------------|---------------------------- */
    /*                                   |                                                        |                             */
    /*                                   |  8 EXCEL SQL                                           |d:/xls/wantxl.xlsx)          */
    /*                                   |  ===========                                           |                             */
    /*                                   |                                                        |INPUT                        */
    /*                                   |  proc datasets lib=sd1 nolist nodetails;               |----------------+            */
    /*                                   |   delete pywant;                                       || A1| fx  |STR  |            */
    /*                                   |  run;quit;                                             |------------------------+    */
    /*                                   |                                                        |[_]|           A        |    */
    /*                                   |  %utlfkil(d:/xls/wantxl.xlsx);                         |------------------------|    */
    /*                                   |                                                        | 1 | STR                |    */
    /*                                   |  %utl_rbeginx;                                         | - |--------------------|    */
    /*                                   |  parmcards4;                                           | 2 |Jhon Austin B  100kg|    */
    /*                                   |  library(openxlsx)                                     | - |--------------------|    */
    /*                                   |  library(sqldf)                                        | 3 |Mick Gray   C  110kg|    */
    /*                                   |  library(haven)                                        | - |--------------------|    */
    /*                                   |  have<-read_sas("d:/sd1/have.sas7bdat")                | 4 |Tom Jef     A  30kg |    */
    /*                                   |  wb <- createWorkbook()                                | - |--------------------|    */
    /*                                   |  addWorksheet(wb, "have")                              |[HAVE]                       */
    /*                                   |  writeData(wb, sheet = "have", x = have)               |                             */
    /*                                   |  saveWorkbook(                                         |OUTPUT                       */
    /*                                   |      wb                                                |--------------+              */
    /*                                   |     ,"d:/xls/wantxl.xlsx"                              ||A| fx  |NAME |              */
    /*                                   |     ,overwrite=TRUE)                                   |--------------------------+  */
    /*                                   |  ;;;;                                                  |[]|    A      |  B |   C  |  */
    /*                                   |  %utl_rendx;                                           |--------------------------|  */
    /*                                   |                                                        |1 | NAME      |CLASS| PACK|  */
    /*                                   |  %utl_rbeginx;                                         |--|-----------+-----+-----|  */
    /*                                   |  parmcards4;                                           |2 |Jhon Austin|B    |100kg|  */
    /*                                   |  library(openxlsx)                                     |--|-----------+-----+-----|  */
    /*                                   |  library(sqldf)                                        |3 |Mick Gray  |C    |110kg|  */
    /*                                   |  source("c:/oto/fn_tosas9x.R")                         |--|-----------+-----+-----|  */
    /*                                   |   wb<-loadWorkbook("d:/xls/wantxl.xlsx")               |4 |Tom Jef    |A    |30kg |  */
    /*                                   |   have<-read.xlsx(wb,"have")                           |--|-----------+-----+-----|  */
    /*                                   |   addWorksheet(wb, "want")                             |[WANT]                       */
    /*                                   |   want<-sqldf('                                        |                             */
    /*                                   |   with                                                 |                             */
    /*                                   |      poslen as (                                       |                             */
    /*                                   |   select                                               |                             */
    /*                                   |      str                                               |                             */
    /*                                   |     ,instr(substr(str,instr(str," ")+ 1)," ")          |                             */
    /*                                   |         +instr(str, " ")+1 as name_length              |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |      have                                              |                             */
    /*                                   |   )                                                    |                             */
    /*                                   |   select                                               |                             */
    /*                                   |     name_length                                        |                             */
    /*                                   |    ,substr(str,1,name_length-1) as name                |                             */
    /*                                   |    ,substr(replace(substr(str,name_length)," ",""),1,1)|                             */
    /*                                   |       as class                                         |                             */
    /*                                   |    ,replace(substr(str,name_length +1)," ","") as pack |                             */
    /*                                   |   from                                                 |                             */
    /*                                   |      poslen                                            |                             */
    /*                                   |   ')                                                   |                             */
    /*                                   |   print(want)                                          |                             */
    /*                                   |   writeData(wb,sheet="want",x=want)                    |                             */
    /*                                   |   saveWorkbook(                                        |                             */
    /*                                   |       wb                                               |                             */
    /*                                   |      ,"d:/xls/wantxl.xlsx"                             |                             */
    /*                                   |      ,overwrite=TRUE)                                  |                             */
    /*                                   |  fn_tosas9x(                                           |                             */
    /*                                   |        inp    = want                                   |                             */
    /*                                   |       ,outlib ="d:/sd1/"                               |                             */
    /*                                   |       ,outdsn ="want"                                  |                             */
    /*                                   |       )                                                |                             */
    /*                                   |  ;;;;                                                  |                             */
    /*                                   |  %utl_rendx;                                           |                             */
    /*                                   |                                                        |                             */
    /*                                   |  proc print data=sd1.want(drop=rownames name_length);  |                             */
    /*                                   |  run;quit;                                             |                             */
    /*                                   |                                                        |                             */
    /************************************|********************************************************|******************************/


    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      str="Jhon Austin B 100kg";
        output;
      str="Mick Gray C 110kg";
         output;
      str="Tom Jef A30kg";
        output;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.HAVE                                                                                                               */
    /*                                                                                                                        */
    /*         STR                                                                                                            */
    /*                                                                                                                        */
    /* Jhon Austin B 100kg                                                                                                    */
    /* Mick Gray C 110kg                                                                                                      */
    /* Tom Jef A30kg                                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _                 _   _        __ _ _       _            __  __
    / |  ___  __ _ ___  | | ___   __ _  __| | (_)_ __  / _(_) | ___ | |__  _   _ / _|/ _| ___ _ __
    | | / __|/ _` / __| | |/ _ \ / _` |/ _` | | | `_ \| |_| | |/ _ \| `_ \| | | | |_| |_ / _ \ `__|
    | | \__ \ (_| \__ \ | | (_) | (_| | (_| | | | | | |  _| | |  __/| |_) | |_| |  _|  _|  __/ |
    |_| |___/\__,_|___/ |_|\___/ \__,_|\__,_| |_|_| |_|_| |_|_|\___||_.__/ \__,_|_| |_|  \___|_|
    */

    %utlfkil(%sysfunc(getoption(WORK))/tmp.txt);

    * save in autocall folder;
    filename ft15f001 "c:/oto/loadinfile.sas";
    parmcards4;
    %macro loadinfile(str)
      /des="load string into the infile buffer";
     %dosubl('
       data _null_;
         file "%sysfunc(getoption(WORK))/tmp.txt";
         put "*";
       run;quit;
       ');
      infile  "%sysfunc(getoption(WORK))/tmp.txt";
      input @;
      _infile_ = &str;

    %mend loadinfile;
    ;;;;
    run;quit;

    proc datasets lib=work nolist nodetails;
     delete want;
    run;quit;

    data want ;

      set sd1.have;

      informat first last $8.
              therest $24. grade $1.;

      * load _infile_ buffer with variable str;
      %loadinfile(str);

      input first last therest & @;

      grade=compress(therest);
      weight=substr(compress(therest),2);

      * restart the buffer;
      input @1 @@;

      keep first last grade weight;
    run;quit;

    proc print data=want;
     run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   FIRST    LAST      GRADE    WEIGHT                                                                                   */
    /*                                                                                                                        */
    /*   Jhon     Austin      B      100kg                                                                                    */
    /*   Mick     Gray        C      110kg                                                                                    */
    /*   Tom      Jef         A      30kg                                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                    _        __ _ _                          _      _            __  __
    |___ \   ___  __ _ ___  (_)_ __  / _(_) | ___    ___ __ _ _ __ __| |___ | |__  _   _ / _|/ _| ___ _ __
      __) | / __|/ _` / __| | | `_ \| |_| | |/ _ \  / __/ _` | `__/ _` / __|| `_ \| | | | |_| |_ / _ \ `__|
     / __/  \__ \ (_| \__ \ | | | | |  _| | |  __/ | (_| (_| | | | (_| \__ \| |_) | |_| |  _|  _|  __/ |
    |_____| |___/\__,_|___/ |_|_| |_|_| |_|_|\___|  \___\__,_|_|  \__,_|___/|_.__/ \__,_|_| |_|  \___|_|
    */

    proc datasets lib=work nolist nodetails;
     delete want;
    run;quit;

    data want;
      set sd1.have;
      informat first last $8. therest $24. class $1.;
      input @;
      _infile_ = str;
      input first last therest & @;
      class=compress(therest);
      pack=substr(compress(therest),2);
      input @1 @@;
      keep first last class pack;
    cards4;
    *
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   FIRST    LAST      GRADE    WEIGHT                                                                                   */
    /*                                                                                                                        */
    /*   Jhon     Austin      B      100kg                                                                                    */
    /*   Mick     Gray        C      110kg                                                                                    */
    /*   Tom      Jef         A      30kg                                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                       _       _            _
    |___ /   ___  __ _ ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
      |_ \  / __|/ _` / __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
     ___) | \__ \ (_| \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |____/  |___/\__,_|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                                                             |_|
    */

     proc datasets lib=work nolist nodetails;
      delete want;
     run;quit;

     data want;

        length class $1; * force to length=1;

        set sd1.have;

        CALL SCAN(str, 2, position, length);

        * find position & length for second word
          and position after the name

          consider "Jhon Austin B  100kg"
          position  Length
               6    6
               |    |  length+position+1
               |    | 13 position of B
               |    | |
               |   1111
          1234567890123
          Jhon Austin B

          starting position of the rest of the string
          is length_position+1;

        name=substr(str,1,position+length); *Jhon Austin;

        therest=compress(substr(str,position+length+1));
        * B100kg;

        class=therest; * forced to be length 1 B;

        pack=substr(therest,2); * 100kg skip over class;

        KEEP name class pack; * Jhon Austin B 100kg;

     run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  CLASS    NAME           PACK                                                                                          */
    /*                                                                                                                        */
    /*    B      Jhon Austin    100kg                                                                                         */
    /*    C      Mick Gray      110kg                                                                                         */
    /*    A      Tom Jef        30kg                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                               _
    | || |    ___  __ _ ___   ___  __ _| |
    | || |_  / __|/ _` / __| / __|/ _` | |
    |__   _| \__ \ (_| \__ \ \__ \ (_| | |
       |_|   |___/\__,_|___/ |___/\__, |_|
                                     |_|
    */
     proc datasets lib=work nolist nodetails;
      delete want;
     run;quit;

     proc sql;
      create
        table want(drop=name_length therest) as
      select
        find(str,scan(str,2))+length(scan(str,2))
          as name_length
       ,substr(str,1,calculated name_length)
          as name
       ,compress(substr(str,calculated name_length+1))
          as therest
       ,calculated therest
          as class length=1
       ,substr(calculated therest,2)
          as pack
      from
        sd1.have
     ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*      NAME        CLASS   PACK                                                                                          */
    /*                                                                                                                        */
    /*      Jhon Austin   B     100kg                                                                                         */
    /*      Mick Gray     C     110kg                                                                                         */
    /*      Tom Jef       A     30kg                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___            __       _ _          _       _        _                   _
    | ___|   _ __   / _| __ _(_) | ___  __| |  ___| |_ _ __(_)_ __   __ _ _ __ | | __ _ _ __   __ _ _   _  __ _  __ _  ___
    |___ \  | `__| | |_ / _` | | |/ _ \/ _` | / __| __| `__| | `_ \ / _` | `__|| |/ _` | `_ \ / _` | | | |/ _` |/ _` |/ _ \
     ___) | | |    |  _| (_| | | |  __/ (_| | \__ \ |_| |  | | | | | (_| | |   | | (_| | | | | (_| | |_| | (_| | (_| |  __/
    |____/  |_|    |_|  \__,_|_|_|\___|\__,_| |___/\__|_|  |_|_| |_|\__, |_|   |_|\__,_|_| |_|\__, |\__,_|\__,_|\__, |\___|
                                                                    |___/                     |___/             |___/
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(tidyverse)
    library(stringr)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    # WRONG INPUT
    dat <- data.frame(
        name = c(
            "Jhon Austin B 100kg",
            "Mick Gray C 110kg",
            "Tom Jef A30kg"
        )
    )
    dat %>%
        separate_wider_regex(
            name,
            patterns = c(name = ".*", " "
           ,class = "\\w", " ", pack = "\\d+kg")
        )
    ;;;;
    %utl_rendx;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Error in `separate_wider_regex()`:                                                                                     */
    /* ! Expected each value of `name` to match the pattern, the whole pattern,                                               */
    /*   and nothing but the pattern.                                                                                         */
    /*                                                                                                                        */
    /* WORKS WITH WRONG INPUT                                                                                                 */
    /*                                                                                                                        */
    /* Tom Jef A30kg  NEED TO CHANGE TO  Tom Jef A 30kg                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*__                      _
     / /_    _ __   ___  __ _| |
    | `_ \  | `__| / __|/ _` | |
    | (_) | | |    \__ \ (_| | |
     \___/  |_|    |___/\__, |_|
                           |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.r")
    have<-read_sas("d:/sd1/have.sas7bdat")
    want<-sqldf('
     with
        poslen as (
     select
        str
       ,instr(substr(str,instr(str," ")+ 1)," ")
           +instr(str, " ")+1 as name_length
     from
        have
     )
     select
       name_length
      ,substr(str,1,name_length-1)  as name
      ,substr(replace(substr(str,name_length)," ",""),1,1)
        as class
      ,replace(substr(str,name_length +1)," ","") as pack
     from
        poslen
     ')
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    /****************************************************************************************************************************/
    /*                                                                                                                          */
    /* R                                                                                                                        */
    /*                                                                                                                          */
    /*        name  class  pack                                                                                                 */
    /* Jhon Austin      B 100kg                                                                                                 */
    /*   Mick Gray      C 110kg                                                                                                 */
    /*     Tom Jef      A  30kg                                                                                                 */
    /*                                                                                                                          */
    /* SAS                                                                                                                      */
    /*                                                                                                                          */
    /* NAME        CLASS    PACK                                                                                                */
    /*                                                                                                                          */
    /* Jhon Austin   B      100kg                                                                                               */
    /* Mick Gray     C      110kg                                                                                               */
    /* Tom Jef       A      30kg                                                                                                */
    /*                                                                                                                          */
    /****************************************************************************************************************************/

    /*____               _   _                             _
    |___  |  _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
       / /  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
      / /   | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
     /_/    | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    want=pdsql('''    \
     with             \
        poslen as (   \
     select           \
        str           \
       ,instr(substr(str,instr(str," ")+ 1)," ") \
           +instr(str, " ")+1 as name_length \
     from \
        have \
     )       \
     select  \
      name_length \
     ,substr(str,1,name_length-1) as name \
     ,substr(replace(substr(str,name_length)," ",""),1,1) \
       as class \
     ,replace(substr(str,name_length +1)," ","") as pack \
     from \
       poslen
       ''')
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant')

    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant(drop=name_length);
    run;quit;

    /****************************************************************************************************************************/
    /*                                                                                                                          */
    /* PYTHON                                                                                                                   */
    /* ======                                                                                                                   */
    /*           name class   pack                                                                                              */
    /* 0 Jhon Austin      B  100kg                                                                                              */
    /* 1   Mick Gray      C  110kg                                                                                              */
    /* 2     Tom Jef      A   30kg                                                                                              */
    /*                                                                                                                          */
    /* SAS                                                                                                                      */
    /* ===                                                                                                                      */
    /* NAME        CLASS    PACK                                                                                                */
    /*                                                                                                                          */
    /* Jhon Austin   B      100kg                                                                                               */
    /* Mick Gray     C      110kg                                                                                               */
    /* Tom Jef       A      30kg                                                                                                */
    /*                                                                                                                          */
    /*                                                                                                                          */
    /****************************************************************************************************************************/

    /*___                      _             _
     ( _ )    _____  _____ ___| |  ___  __ _| |
     / _ \   / _ \ \/ / __/ _ \ | / __|/ _` | |
    | (_) | |  __/>  < (_|  __/ | \__ \ (_| | |
     \___/   \___/_/\_\___\___|_| |___/\__, |_|
                                          |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utlfkil(d:/xls/wantxl.xlsx);

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    library(haven)
    have<-read_sas("d:/sd1/have.sas7bdat")
    wb <- createWorkbook()
    addWorksheet(wb, "have")
    writeData(wb, sheet = "have", x = have)
    saveWorkbook(
        wb
       ,"d:/xls/wantxl.xlsx"
       ,overwrite=TRUE)
    ;;;;
    %utl_rendx;

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
     wb<-loadWorkbook("d:/xls/wantxl.xlsx")
     have<-read.xlsx(wb,"have")
     addWorksheet(wb, "want")
     want<-sqldf('
     with
        poslen as (
     select
        str
       ,instr(substr(str,instr(str," ")+ 1)," ")
           +instr(str, " ")+1 as name_length
     from
        have
     )
     select
       name_length
      ,substr(str,1,name_length-1) as name
      ,substr(replace(substr(str,name_length)," ",""),1,1)
        as class
      ,replace(substr(str,name_length +1)," ","") as pack
     from
        poslen
     ')
     print(want)
     writeData(wb,sheet="want",x=want)
     saveWorkbook(
         wb
        ,"d:/xls/wantxl.xlsx"
        ,overwrite=TRUE)
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want(drop=rownames name_length);
    run;quit;

    /*___    _                 _ _        __ _ _
     / _ \  | | ___   __ _  __| (_)_ __  / _(_) | ___  _ __ ___   __ _  ___ _ __ ___
    | (_) | | |/ _ \ / _` |/ _` | | `_ \| |_| | |/ _ \| `_ ` _ \ / _` |/ __| `__/ _ \
     \__, | | | (_) | (_| | (_| | | | | |  _| | |  __/| | | | | | (_| | (__| | | (_) |
       /_/  |_|\___/ \__,_|\__,_|_|_| |_|_| |_|_|\___||_| |_| |_|\__,_|\___|_|  \___/

    */

    * save in autocall folder;
    filename ft15f001 "c:/oto/loadinfile.sas";
    parncards4;
    %macro loadinfile(str)
      /des="load string into the infile buffer";
     %dosubl('
       data _null_;
         file "%sysfunc(getoption(WORK))/tmp.txt";
         put "*";
       run;quit;
       ');
      infile  "%sysfunc(getoption(WORK))/tmp.txt";
      input @;
      _infile_ = &str;

    %mend loadinfile;
    ;;;;
    run;quit;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
