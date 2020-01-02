# utl-creating-zipcode-zcta-choropleth-maps-in-R-and-SAS
Creating zipcode choropleth maps in R and SAS
    SAS-L: Creating zipcode choropleth maps in R and SAS

    R zipcode map
    https://tinyurl.com/r7p3wzu
    https://github.com/rogerjdeangelis/utl-creating-zipcode-zcta-choropleth-maps-in-R-and-SAS/blob/master/r_zipcode.png

    SAS zipcode map
    https://tinyurl.com/sprfazj
    https://github.com/rogerjdeangelis/utl-creating-zipcode-zcta-choropleth-maps-in-R-and-SAS/blob/master/sas_zipcode.pdf


    github
    https://tinyurl.com/v98j7nd
    https://github.com/rogerjdeangelis/utl-creating-zipcode-zcta-choropleth-maps-in-R-and-SAS

            Two examples
                a. R
                b. SAS

    SAS
    Where to get census zcta shapefiles
    https://www2.census.gov/geo/tiger/TIGER2010/ZCTA5/2010/
    download  tl_2010_26_zcta510.zip      11-May-2011 12:46      8.7M
    Unzip to folder d:/shp/kent

    R
    Where to get R cloropeth packages, shapfiles and popuation data;
    https://arilamstein.com/creating-zip-code-choropleths-choroplethrzip/

    Related map
    https://github.com/rogerjdeangelis/utl_graphics_plotting_rivers_in_brazil_using_sharpefiles

    *
    __      ___ __  ___
    \ \ /\ / / '_ \/ __|
     \ V  V /| |_) \__ \
      \_/\_/ | .__/|___/
             |_|
    ;

    Almost worked

    Prior mapimport, annotate and maplabel macros seemed to work.

    I suspect this will work in WPS.
    I just don't have time to check out or work around these rrrors


    119       options orientation=landscape;
    120       goptions reset=all rotate=landscape;
                                       ^
    ERROR: Unknown graphics option rotate
    121       ods graphics on / reset height=640px width=800px;
    NOTE: Writing PDF file d:\pdf\mta_lntzip_kvtmap.pdf
    122       ODS pdf file="d:/pdf/mta_lntzip_kvtmap.pdf";
    123       PROC GMAP DATA = subzipnro map= subzipsrt all;
    124       TITLE1 height=2.5pct "Marketplace Enrollment Kent County Michigan by Zipcode(fake data
    124     ! )";
    125       TITLE2 height=2.5pct "Percent of Eligible Enrollees";
    126       legend1 shape=bar(.15in,.15in) frame cshadow=gray position=(top)
    127       label=(position=left height=2pct font="arial/bold")
                     ^
    ERROR: Found "position" when expecting one of ANGLE, COLOR, FONT, HEIGHT, JUSTIFY or ROTATE
    128       value=(height=2pct font="arial");
    129       PATTERN1 C = cxedf8fb;
    130       PATTERN2 C = cxb2e2e2;
    131       PATTERN3 C = cx66c2a4;
    132       PATTERN5 C = cx238b45;
    133       ID ZCTA5;
    134       FORMAT Percent PercentGroup.;
    135       CHORO Percent /levels=4 discrete annotate=anno legend=legend1;
    NOTE: Step processing stopped because of errors detected
    136       RUN;
    NOTE: Procedure GMAP step took :
          real time : 0.000
          cpu time  : 0.000

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;


    R

    https://www2.census.gov/geo/tiger/TIGER2010/ZCTA5/2010/


    SAS
    https://www2.census.gov/geo/tiger/TIGER2010/ZCTA5/2010/
    download

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    SAS zipcode map
    https://tinyurl.com/sprfazj

    R zipcode map
    https://tinyurl.com/r7p3wzu

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/
     ____
    |  _ \
    | |_) |
    |  _ <
    |_| \_\

    ;

    %utl_submit_r64('
    library(choroplethr);
    library(choroplethrZip);
    ?df_pop_zip;
    data(df_pop_zip);
    nyc_fips = c(36005, 36047, 36061, 36081, 36085);
    png("d:/png/kent.png",width = 800, height =640);
    zip_choropleth(df_pop_zip,
      county_zoom = nyc_fips,
      title       = "2012 New York City ZCTA Population Estimates",
      legend      = "Population");
    ');

    *
     ___  __ _ ___
    / __|/ _` / __|
    \__ \ (_| \__ \
    |___/\__,_|___/

    ;


    proc mapimport datafile="d:\shp\kent\tl_2010_26_zcta510.shp" out=kntzip;
    ;run;quit;

    data subzip;
    set
    kntzip( rename=ZCTA5CE10=zcta5 where=(zcta5 in (
    "48809", "48838", "49301", "49302", "49306",
    "49315", "49316", "49319", "49321", "49330", "49331",
    "49341", "49343", "49345", "49418", "49428", "49501",
    "49502", "49503", "49504", "49505", "49506", "49507",
    "49508", "49509", "49512", "49514", "49519", "49525",
    "49534", "49544", "49546", "49548")));
    ;run;quit;
    proc sort data=subzip out=subzipsrt;
    by zcta5;
    ;run;quit;


    data subzipnro;
       input ZCTA5$    P12I2    P12I26    PERCENT    ZCTA$;
    cards4;
    48809 5422 5593 51 48809
    48838 8790 9073 51 48838
    49301 9280 9421 50 49301
    49302 3898 3687 49 49302
    49306 4618 4626 50 49306
    49315 9891 10024 50 49315
    49316 9921 10153 51 49316
    49319 8072 8109 50 49319
    49321 7938 8303 51 49321
    49330 2746 2694 50 49330
    49331 8171 8456 51 49331
    49341 16658 17079 51 49341
    49343 2764 2725 50 49343
    49345 6441 6599 51 49345
    49418 13313 14104 51 49418
    49428 12320 13100 52 49428
    49503 18434 17506 49 49503
    49504 19864 20130 50 49504
    49505 15056 16160 52 49505
    49506 14926 16428 52 49506
    49507 17269 18190 51 49507
    49508 18519 20629 53 49508
    49509 13187 13533 51 49509
    49512 7571 7877 51 49512
    49519 13408 14016 51 49519
    49525 13128 14255 52 49525
    49534 10567 11003 51 49534
    49544 4337 4607 52 49544
    49546 14908 16972 53 49546
    49548 15163 15361 50 49548
    ;;;;
    run;quit;

    PROC FORMAT;
    VALUE PercentGroup
    48 <- 49 = "20% to 40% "
    49 <- 50 = "40% to 60% "
    50 <- 52 = "60% to 80% "
    52 <- HIGH = "Over 80% ";
    RUN;

    options sasautos="c:/oto");
    %ANNOMAC;
    %MAPLABEL ( subzipsrt, subzipnro, anno, zcta5,
    zcta5,font=Arial Black,color=black, size=2, hsys=3);


    options orientation=landscape;
    goptions reset=all rotate=landscape;
    ods graphics on / reset height=640px width=800px;
    ODS pdf file="d:/pdf/mta_lntzip_kvtmap.pdf";
    PROC GMAP DATA = subzipnro map= subzipsrt all;
    TITLE1 height=2.5pct "Marketplace Enrollment Kent County Michigan by Zipcode(fake data)";
    TITLE2 height=2.5pct "Percent of Eligible Enrollees";
    legend1 shape=bar(.15in,.15in) frame cshadow=gray position=(top)
    label=(position=left height=2pct font="arial/bold")
    value=(height=2pct font="arial");
    PATTERN1 C = cxedf8fb;
    PATTERN2 C = cxb2e2e2;
    PATTERN3 C = cx66c2a4;
    PATTERN5 C = cx238b45;
    ID ZCTA5;
    FORMAT Percent PercentGroup.;
    CHORO Percent /levels=4 discrete annotate=anno legend=legend1;
    RUN;quit;
    ods pdf close;
    ods graphics off;

