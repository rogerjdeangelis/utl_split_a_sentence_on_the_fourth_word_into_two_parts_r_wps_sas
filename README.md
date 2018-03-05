# utl_split_a_sentence_on_the_fourth_word_into_two_parts_r_wps_sas
Split a sentence on the fourth word into two parts R WPS SAS.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Split a sentence on the fourth word into two parts R WPS SAS

    github
    https://github.com/rogerjdeangelis/utl_split_a_sentence_on_the_fourth_word_into_two_parts_r_wps_sas

    https://tinyurl.com/ydblpyhv
    https://stackoverflow.com/questions/49073114/how-to-identify-the-nth-repetition-of-an-element-in-a-string

    profile
    https://stackoverflow.com/users/5508500/erocoar

      Two Solutions (same results SAS and WPS)

         1. WPS/SAS
         2. WPS/PROC R

    INPUT
    =====

     SD1.HAVE total obs=4

       COUNTING

       1_2_3_4_5_6_7_8_9;
       uno_dos_tres_quatro_cinco_seis_siete
       one_two_three_four_five
       ten_twenty_thirty_forty_fifty_sixty


     WANT

      BEFORE               AFTER

      1_2_3                4_5_6_7_8_9;
      uno_dos_tres         quatro_cinco_seis_siete
      one_two_three        four_five
      ten_twenty_thirty    forty_fifty_sixty


    PROCESS
    =======

     1. BASE WPS SAS

         data want;
           set have;
           call scan(counting,4,position,length,"_");
           before=substr(counting,1,position-2);
           after=substr(counting,position);
           keep before after;
         run;quit;

      2, WPS/PROC R (working code)

         want<-have %>% extract(COUNTING, into = c("a", "b"), regex = "^([^_]*_[^_]*_[^_*]*)_(.*)");


    OUTPUT
    ======

      WORK.WANT total obs=4

       BEFORE               AFTER

       uno_dos_tres         quatro_cinco_seis_siete
       one_two_three        four_five
       ten_twenty_thirty    forty_fifty_sixty
       1_2_3                4_5_6_7_8_9;

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;
    options validvarname=upcase;
    data sd1.have;
    input counting $60.;
    cards4;
    1_2_3_4_5_6_7_8_9;
    uno_dos_tres_quatro_cinco_seis_siete
    one_two_three_four_five
    ten_twenty_thirty_forty_fifty_sixty
    ;;;;
    run;quit;
    *
     ___  __ _ ___
    / __|/ _` / __|
    \__ \ (_| \__ \
    |___/\__,_|___/

    ;

    data want;
      set have;
      call scan(counting,4,position,length,"_");
      before=substr(counting,1,position-2);
      after=substr(counting,position);
      keep before after;
    run;quit;

    /*
      BEFORE               AFTER

      uno_dos_tres         quatro_cinco_seis_siete
      one_two_three        four_five
      ten_twenty_thirty    forty_fifty_sixty
      1_2_3                4_5_6_7_8_9;
    */

    *
     _ __
    | '__|
    | |
    |_|

    ;

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    library(dplyr);
    library(tidyverse);
    have<-read_sas("d:/sd1/have.sas7bdat");
    want<-have %>% extract(COUNTING, into = c("a", "b"), regex = "^([^_]*_[^_]*_[^_*]*)_(.*)");
    endsubmit;
    import r=want data=wrk.want;
    run;quit;
    ');

    > have<-read_sas("d:/sd1/have.sas7bdat")
    > want<-have %>% extract(COUNTING, into = c("a", "b"), regex = "^([^_]*_[^_]*_[^_*]*)_(.*)")

    NOTE: Processing of R statements complete

    13        import r=want data=wrk.want;
    NOTE: Creating data set 'WRK.want' from R data frame 'want'
    NOTE: Column names modified during import of 'want'
    NOTE: Data set "WRK.want" has 4 observation(s) and 2 variable(s)


