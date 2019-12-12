# utl-rename-all-variables-starting-with-the-same-preffix-to-a-different-prefix
Rename all variables starting with the same preffix to a different prefix
    Rename all variables starting with the same prefix to a different prefix

    Problem: Rename variables (suppose you have 1,000 variables)

           dr_joanne    dr_janey   dr_joan    dr_jane

         to

           md_joannne   md_janey   md_joan     md_jane

     respectively

      Method

           1. Use utl_varlist to select just the variables with the desired pefix

           2, Use  Bart's array macro to chop off the prefix
              Bartosz Jablonski
              yabwon@gmail.com
              Bart's array macro allows us to uese datasrtep arrayys and all of datastep funtionality.
              We use his array to chop off the prefix.
              This is a powerfull macro built with 'DOSUBL'

           3. Use do_over and proc datasets to reanme old prtefix to new prefix

    github
    https://tinyurl.com/r2mgorw
    https://github.com/rogerjdeangelis/utl-rename-all-variables-starting-with-the-same-preffix-to-a-different-prefix


    see github
    https://github.com/rogerjdeangelis/utl_rename_variables_using_coordinated_lists_renamel_macro
    https://tinyurl.com/v2fr64y
    https://github.com/rogerjdeangelis?utf8=%E2%9C%93&tab=repositories&q=rename+in%3Aname&type=&language=

    SAS Forum
    https://tinyurl.com/sfphgls
    https://communities.sas.com/t5/SAS-Programming/Rename-all-variables-starting-with-the-same-preffix-to-a/m-p/611152


    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories



    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
       retain dr_joanne dr_janey dr_joan dr_jane "doctors";
    run;quit;

    WORK.HAVE total obs=1

       DR_JOANNE    DR_JANEY    DR_JOAN    DR_JANE

        doctors     doctors     doctors    doctors

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    WORK.HAVE total obs=1

      MD_JOANNE    MD_JANEY    MD_JOAN    MD_JANE   ** new prefix

       doctors     doctors     doctors    doctors

     *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    %let vars=%utl_varlist(have,qstyle=DOUBLE,prx=/^dr/i);

    /*
    %put &=vars;

    VARS="DR_JOANNE" "DR_JANEY" "DR_JOAN" "DR_JANE"
    */

    %barray(v[1:%sysfunc(countc(&vars,_))] $32 (&vars),function=substr(v[_i_],3))

    /*
    %put &=v1 &=v2 &=v3 &=v1  &=v4 &=vn;

    V1=_JOANNE V2=_JANEY V3=_JOAN V1=_JOANNE  V4=_JANE VN=4
    */

    proc datasets lib=work;
      modify have;
      rename
        %do_over(v,phrase=%str(DR? = MD?))
      ;
    run;quit;


    NOTE: Renaming variable DR_JOANNE to MD_JOANNE.
    NOTE: Renaming variable DR_JANEY to MD_JANEY.
    NOTE: Renaming variable DR_JOAN to MD_JOAN.
    NOTE: Renaming variable DR_JANE to MD_JANE.
    463   run;

    NOTE: MODIFY was successful for WORK.HAVE.DATA.
    463 !     quit;

    NOTE: PROCEDURE DATASETS used (Total process time):
          real time           0.16 seconds

