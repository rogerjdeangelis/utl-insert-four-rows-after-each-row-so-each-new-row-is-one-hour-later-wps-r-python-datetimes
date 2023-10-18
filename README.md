# utl-insert-four-rows-after-each-row-so-each-new-row-is-one-hour-later-wps-r-python-datetimes
Insert four rows after each row so each new row is one hour later wps r python datetimes
    %let pgm=utl-insert-four-rows-after-each-row-so-each-new-row-is-one-hour-later-wps-r-python-datetimes;

    Insert four rows after each row so each new row is one hour later wps r python datetimes

      SOLUTIONS

         1 wps base

         2 wps r base no sql
           Changed
           mutate(A = A + hours(add))
           To
           mutate(DTETYM = DTETYM + add*3600)
           https://stackoverflow.com/users/903061/gregor-thomas

         3 wps sql w/o do_over
         4 wps r sql
         5 wps python sql

    github
    https://tinyurl.com/5n6drmrs
    https://github.com/rogerjdeangelis/utl-insert-four-rows-after-each-row-so-each-new-row-is-one-hour-later-wps-r-python-datetimes

    stackoverflow R
    https://tinyurl.com/2s3pnf5b
    https://stackoverflow.com/questions/77311561/how-to-insert-4-rows-after-each-row-in-a-dataframe-so-each-new-row-is-1-hour

    Convert an R or python datetime to a WPS datetime

       wps datetime = r datetime + 315532800;
       wps    origin 01JAN1960
       R      origin 01JAN1970
       Python origin 01JAN1970

       Computation of coversion factors
       data _null_;
         dif ='01JAN1960:00:00:00'dt + '01JAN1970:00:00:00'dt - 24*60*60; /* one day */
         put dif=;
       run;quit;

       dif=315532800

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
       format dteTym E8601DT19.;
       input dteTym anydtdtm20. age;
    cards4;
    2015-01-10 17:00:00 45
    2017-12-19 03:00:00 67
    2020-09-02 14:00:00 87
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                    |                        |                                                          */
    /*             INPUT                  |   PROCESS              |                OUTPUT                                    */
    /*                                    |                        |                                                          */
    /* Obs          DTETYM           AGE  |                        |    Obs          DTETYM           AGE                     */
    /*                                    |                        |                                                          */
    /*  1     2015-01-10T17:00:00     45  |   Repeat but increment |      1    2005-01-10T17:00:00     45                     */
    /*                                    |   hour by 1 hour       |      2    2005-01-10T18:00:00     45                     */
    /*                                    |                        |      3    2005-01-10T19:00:00     45                     */
    /*                                    |                        |      4    2005-01-10T20:00:00     45                     */
    /*                                    |                        |      5    2005-01-10T21:00:00     45                     */
    /*                                    |                        |                                                          */
    /*  2     2017-12-19T03:00:00     67  |                        |      1    2007-12-20T03:00:00     67                     */
    /*                                    |                        |      2    2007-12-20T04:00:00     67                     */
    /*                                    |                        |      3    2007-12-20T05:00:00     67                     */
    /*                                    |                        |      4    2007-12-20T06:00:00     67                     */
    /*                                    |                        |      5    2007-12-20T07:00:00     67                     */
    /*                                    |                        |                                                          */
    /*  3     2020-09-02T14:00:00     87  |                        |      1    2010-09-03T14:00:00     87                     */
    /*                                    |                        |      2    2010-09-03T15:00:00     87                     */
    /*                                    |                        |      3    2010-09-03T16:00:00     87                     */
    /*                                    |                        |      4    2010-09-03T17:00:00     87                     */
    /*                                    |                        |      5    2010-09-03T18:00:00     87                     */
    /*                                    |                        |                                                          */
    /**************************************************************************************************************************/

    /*                        _
    / | __      ___ __  ___  | |__   __ _ ___  ___
    | | \ \ /\ / / `_ \/ __| | `_ \ / _` / __|/ _ \
    | |  \ V  V /| |_) \__ \ | |_) | (_| \__ \  __/
    |_|   \_/\_/ | .__/|___/ |_.__/ \__,_|___/\___|
                 |_|
    */
    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    data sd1.want;
       set sd1.have;
       output;
       do hours=1 to 4;
           dteTym = intnx("hour", dteTym, 1);
           output;
       end;
       drop hours;
    run;quit;

    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs          DTETYM           AGE                                                                                     */
    /*                                                                                                                        */
    /*    1    2015-01-10T17:00:00     45                                                                                     */
    /*    2    2015-01-10T18:00:00     45                                                                                     */
    /*    3    2015-01-10T19:00:00     45                                                                                     */
    /*    4    2015-01-10T20:00:00     45                                                                                     */
    /*    5    2015-01-10T21:00:00     45                                                                                     */
    /*                                                                                                                        */
    /*    6    2017-12-19T03:00:00     67                                                                                     */
    /*    7    2017-12-19T04:00:00     67                                                                                     */
    /*    8    2017-12-19T05:00:00     67                                                                                     */
    /*    9    2017-12-19T06:00:00     67                                                                                     */
    /*   10    2017-12-19T07:00:00     67                                                                                     */
    /*                                                                                                                        */
    /*   11    020-09-02T14:00:00      87                                                                                     */
    /*   12    020-09-02T15:00:00      87                                                                                     */
    /*   13    020-09-02T16:00:00      87                                                                                     */
    /*   14    020-09-02T17:00:00      87                                                                                     */
    /*   15    020-09-02T18:00:00      87                                                                                     */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                _                                             _
    |___ \  __      ___ __  ___   _ __  | |__   __ _ ___  ___   _ __   ___   ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| | `__| | `_ \ / _` / __|/ _ \ | `_ \ / _ \ / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ | |    | |_) | (_| \__ \  __/ | | | | (_) |\__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |_|    |_.__/ \__,_|___/\___| |_| |_|\___/ |___/\__, |_|
                     |_|                                                            |_|

    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(dplyr);
    library(lubridate);
    have;
    want<-have |>
      cross_join(y = data.frame(add = 0:4)) |>
      mutate(DTETYM = DTETYM + add*3600) |>
      select(-add);
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;
    ');


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* Obs          DTETYM           AGE                                                                                      */
    /*                                                                                                                        */
    /*   1    2015-01-10T17:00:00     45                                                                                      */
    /*   2    2015-01-10T18:00:00     45                                                                                      */
    /*   3    2015-01-10T19:00:00     45                                                                                      */
    /*   4    2015-01-10T20:00:00     45                                                                                      */
    /*   5    2015-01-10T21:00:00     45                                                                                      */
    /*                                                                                                                        */
    /*   6    2017-12-19T03:00:00     67                                                                                      */
    /*   7    2017-12-19T04:00:00     67                                                                                      */
    /*   8    2017-12-19T05:00:00     67                                                                                      */
    /*   9    2017-12-19T06:00:00     67                                                                                      */
    /*  10    2017-12-19T07:00:00     67                                                                                      */
    /*                                                                                                                        */
    /*  11    020-09-02T14:00:00      87                                                                                      */
    /*  12    020-09-02T15:00:00      87                                                                                      */
    /*  13    020-09-02T16:00:00      87                                                                                      */
    /*  14    020-09-02T17:00:00      87                                                                                      */
    /*  15    020-09-02T18:00:00      87                                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                  _             __          _
    |___ /  __      ___ __  ___   ___  __ _| | __      __/ /__     __| | ___     _____   _____ _ __
      |_ \  \ \ /\ / / `_ \/ __| / __|/ _` | | \ \ /\ / / / _ \   / _` |/ _ \   / _ \ \ / / _ \ `__|
     ___) |  \ V  V /| |_) \__ \ \__ \ (_| | |  \ V  V / / (_) | | (_| | (_) | | (_) \ V /  __/ |
    |____/    \_/\_/ | .__/|___/ |___/\__, |_|   \_/\_/_/ \___/   \__,_|\___/___\___/ \_/ \___|_|
                     |_|                 |_|                               |_____|
              _ _   _                 _
    __      _(_) |_| |__   ___  _   _| |_
    \ \ /\ / / | __| `_ \ / _ \| | | | __|
     \ V  V /| | |_| | | | (_) | |_| | |_
      \_/\_/ |_|\__|_| |_|\___/ \__,_|\__|

    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    options validvarname=any;
    proc sql;
      create
         table sd1.want as
             select dtetym as dtetym format=E8601DT19. ,age from sd1.have union corr
             select dtetym + 1*3600 as dtetym ,age from sd1.have union corr
             select dtetym + 2*3600 as dtetym ,age from sd1.have union corr
             select dtetym + 3*3600 as dtetym ,age from sd1.have union corr
             select dtetym + 4*3600 as dtetym ,age from sd1.have
    ;quit;
    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;
    ');

    /*        _ _   _
    __      _(_) |_| |__
    \ \ /\ / / | __| `_ \
     \ V  V /| | |_| | | |
      \_/\_/ |_|\__|_| |_|

    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %array(_tz,values=1-4);

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    options validvarname=any;
    proc sql;
      create
         table sd1.want as
             select dtetym as dtetym  format=E8601DT19. ,age from sd1.have union corr
             %do_over(_tz,phrase=%str(
                select dtetym + ?*3600 as dtetym ,age from sd1.have),between=union corr)

      ;quit;
    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;
    ");

    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* Obs          DTETYM           AGE                                                                                      */
    /*                                                                                                                        */
    /*   1    2015-01-10T17:00:00     45                                                                                      */
    /*   2    2015-01-10T18:00:00     45                                                                                      */
    /*   3    2015-01-10T19:00:00     45                                                                                      */
    /*   4    2015-01-10T20:00:00     45                                                                                      */
    /*   5    2015-01-10T21:00:00     45                                                                                      */
    /*                                                                                                                        */
    /*   6    2017-12-19T03:00:00     67                                                                                      */
    /*   7    2017-12-19T04:00:00     67                                                                                      */
    /*   8    2017-12-19T05:00:00     67                                                                                      */
    /*   9    2017-12-19T06:00:00     67                                                                                      */
    /*  10    2017-12-19T07:00:00     67                                                                                      */
    /*                                                                                                                        */
    /*  11    020-09-02T14:00:00      87                                                                                      */
    /*  12    020-09-02T15:00:00      87                                                                                      */
    /*  13    020-09-02T16:00:00      87                                                                                      */
    /*  14    020-09-02T17:00:00      87                                                                                      */
    /*  15    020-09-02T18:00:00      87                                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                           _
    | || |   __      ___ __  ___   _ __   ___  __ _| |
    | || |_  \ \ /\ / / `_ \/ __| | `__| / __|/ _` | |
    |__   _|  \ V  V /| |_) \__ \ | |    \__ \ (_| | |
       |_|     \_/\_/ | .__/|___/ |_|    |___/\__, |_|
                      |_|                        |_|
    */


    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    libname sd1 "d:/sd1";

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);
    have;
    have$DTETYM <- have$DTETYM + 315532800;
    want <- sqldf('
             select dtetym as dtetym ,age from have union
             select dtetym + 1*3600 as dtetym ,age from have union
             select dtetym + 2*3600 as dtetym ,age from have union
             select dtetym + 3*3600 as dtetym ,age from have union
             select dtetym + 4*3600 as dtetym ,age from have
    ');
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;

    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;
    ");

    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* Obs          DTETYM           AGE                                                                                      */
    /*                                                                                                                        */
    /*   1    2015-01-10T17:00:00     45                                                                                      */
    /*   2    2015-01-10T18:00:00     45                                                                                      */
    /*   3    2015-01-10T19:00:00     45                                                                                      */
    /*   4    2015-01-10T20:00:00     45                                                                                      */
    /*   5    2015-01-10T21:00:00     45                                                                                      */
    /*                                                                                                                        */
    /*   6    2017-12-19T03:00:00     67                                                                                      */
    /*   7    2017-12-19T04:00:00     67                                                                                      */
    /*   8    2017-12-19T05:00:00     67                                                                                      */
    /*   9    2017-12-19T06:00:00     67                                                                                      */
    /*  10    2017-12-19T07:00:00     67                                                                                      */
    /*                                                                                                                        */
    /*  11    020-09-02T14:00:00      87                                                                                      */
    /*  12    020-09-02T15:00:00      87                                                                                      */
    /*  13    020-09-02T16:00:00      87                                                                                      */
    /*  14    020-09-02T17:00 00      87                                                                                      */
    /*  15    020-09-02T18:00:00      87                                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                     _   _                             _
    | ___|  __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    |___ \  \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) |  \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/    \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
                     |_|         |_|    |___/                                |_|
    */

    /*----                                                                   ----*/
    /*---- Looks like python may not recognize datetime format               ----*/
    /*---- E8601DT19. changes to 12.                                         ----*/
    /*----                                                                   ----*/

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
       *format dteTym E8601DT19.;
       format dteTym 12.;
       input dteTym anydtdtm20. age;
    cards4;
    2015-01-10 17:00:00 45
    2017-12-19 03:00:00 67
    2020-09-02 14:00:00 87
    ;;;;
    run;quit;

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;
    print(have);
    from os import path;
    import pandas as pd;
    import numpy as np;
    from pandasql import sqldf;
    mysql = lambda q: sqldf(q, globals());
    from pandasql import PandaSQL;
    pdsql = PandaSQL(persist=True);
    sqlite3conn = next(pdsql.conn.gen).connection.connection;
    sqlite3conn.enable_load_extension(True);
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll');
    mysql = lambda q: sqldf(q, globals());
    have['DTETYM'] = have['DTETYM'] - 315532800;
    want = pdsql('''
       select dtetym as dtetym ,age from have union
       select dtetym + 1*3600 as dtetym ,age from have union
       select dtetym + 2*3600 as dtetym ,age from have union
       select dtetym + 3*3600 as dtetym ,age from have union
       select dtetym + 4*3600 as dtetym ,age from have
    ''');
    print(want);
    endsubmit;
    import data=sd1.want python=want;
    run;quit;
    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;
    ");

    proc print data=sd1.want width=min;
    format dteTym E8601DT19.;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* Obs          DTETYM           AGE                                                                                      */
    /*                                                                                                                        */
    /*   1    2015-01-10T17:00:00     45                                                                                      */
    /*   2    2015-01-10T18:00:00     45                                                                                      */
    /*   3    2015-01-10T19:00:00     45                                                                                      */
    /*   4    2015-01-10T20:00:00     45                                                                                      */
    /*   5    2015-01-10T21:00:00     45                                                                                      */
    /*                                                                                                                        */
    /*   6    2017-12-19T03:00:00     67                                                                                      */
    /*   7    2017-12-19T04:00:00     67                                                                                      */
    /*   8    2017-12-19T05:00:00     67                                                                                      */
    /*   9    2017-12-19T06:00:00     67                                                                                      */
    /*  10    2017-12-19T07:00:00     67                                                                                      */
    /*                                                                                                                        */
    /*  11    020-09-02T14:00:00      87                                                                                      */
    /*  12    020-09-02T15:00:00      87                                                                                      */
    /*  13    020-09-02T16:00:00      87                                                                                      */
    /*  14    020-09-02T17:00 00      87                                                                                      */
    /*  15    020-09-02T18:00:00      87                                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
