# utl_graphics_determine_us_state_from_latitude_and_longitude
Graphics determine uUS state from latitude and longitude.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Graphics determine uUS state from latitude and longitude

     Solution using WPS Proc R

    see github
    https://tinyurl.com/yd67acu5
    https://github.com/rogerjdeangelis/utl_graphics_determine_us_state_from_latitude_and_longitude

    StackOverFlow
    https://tinyurl.com/y77ed9u3
    https://stackoverflow.com/questions/51266193/how-do-i-automatically-determine-a-state-given-the-latitude-and-longitude-coor

    Pieca profile
    https://stackoverflow.com/users/5550835/pieca



    INPUT
    =====

      SD1.HAVE total obs=26

         ORIGIN_     ORIGIN_
          LAT         LON

        31.9618     -83.059
        44.8782     -69.472
        36.0485     -93.504
        33.7942     -84.202
        32.0749     -81.088
        31.0286     -97.612
        39.8359     -91.754
        35.9220     -80.537
        43.0720     -83.842
        39.2655     -76.494
        61.6303    -149.818
        39.9048     -75.295
        41.9050     -71.103
        42.3921     -97.475
        39.8359     -91.754
        35.8330     -90.697
        31.9618     -83.059
        33.8687     -84.335
        28.3051     -81.424
        40.5364     -89.189
        26.6048     -80.215
        39.0289     -95.209
        35.4074     -93.136
        39.1157     -94.627
        40.6344     -92.922
        41.2564     -73.211

    EXAMPLE OUTPUT
    --------------

    WORK.WANT total obs=26

      ORIGIN_     ORIGIN_
        LAT         LON      STATE

      31.9618     -83.059    Georgia
      44.8782     -69.472    Maine
      36.0485     -93.504    Arkansas
      33.7942     -84.202    Georgia
     ....


    PROCESS ( All the code)
    =======================

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk  sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    library(geojsonio);
    library(sp);
    have<-read_sas("d:/sd1/have.sas7bdat");
    head(have);
    usa <- geojson_read(
      "http://eric.clst.org/assets/wiki/uploads/Stuff/gz_2010_us_040_00_500k.json",
      what = "sp"
    );
    have$state <- NA;
    df<-have;
    for (i in 1:nrow(df)) {
      coords <- c(df$ORIGIN_LON[i], df$ORIGIN_LAT[i]);
      if(any(is.na(coords))) next;
      point <- sp::SpatialPoints(
        matrix(
          coords,
          nrow = 1
        );
      );
      sp::proj4string(point) <- sp::proj4string(usa);
      polygon_check <- sp::over(point, usa);
      df$state[i] <- as.character(polygon_check$NAME);
    };
    endsubmit;
    import r=df data=wrk.wantwps;
    run;quit;
    ');


    /*
    NOTE: Processing of R statements complete

    25        import r=df data=wrk.wantwps;
    NOTE: Creating data set 'WRK.wantwps' from R data frame 'df'
    NOTE: Column names modified during import of 'df'
    NOTE: Data set "WRK.wantwps" has 26 observation(s) and 3 variable(s)
    */


    OUTPUT
    ======

    WORK.WANT total obs=26

      ORIGIN_     ORIGIN_
        LAT         LON      STATE

      31.9618     -83.059    Georgia
      44.8782     -69.472    Maine
      36.0485     -93.504    Arkansas
      33.7942     -84.202    Georgia
      32.0749     -81.088    Georgia
      31.0286     -97.612    Texas
      39.8359     -91.754    Missouri
      35.9220     -80.537    North Carolina
      43.0720     -83.842    Michigan
      39.2655     -76.494    Maryland
      61.6303    -149.818    Alaska
      39.9048     -75.295    Pennsylvania
      41.9050     -71.103    Massachusetts
      42.3921     -97.475    Nebraska
      39.8359     -91.754    Missouri
      35.8330     -90.697    Arkansas
      31.9618     -83.059    Georgia
      33.8687     -84.335    Georgia
      28.3051     -81.424    Florida
      40.5364     -89.189    Illinois
      26.6048     -80.215    Florida
      39.0289     -95.209    Kansas
      35.4074     -93.136    Arkansas
      39.1157     -94.627    Kansas
      40.6344     -92.922    Iowa
      41.2564     -73.211    Connecticut

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input origin_lat origin_lon;
    cards4;
    31.9618 -83.0588
    44.8782 -69.4718
    36.0485 -93.5044
    33.7942 -84.2018
    32.0749 -81.0883
    31.0286 -97.6115
    39.8359 -91.7538
    35.922 -80.537
    43.072 -83.8424
    39.2655 -76.4935
    61.6303 -149.8181
    39.9048 -75.2946
    41.905 -71.1026
    42.3921 -97.4751
    39.8359 -91.7538
    35.833 -90.6965
    31.9618 -83.0588
    33.8687 -84.3351
    28.3051 -81.4242
    40.5364 -89.1885
    26.6048 -80.2149
    39.0289 -95.2086
    35.4074 -93.1355
    39.1157 -94.6271
    40.6344 -92.9219
    41.2564 -73.2111
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process



