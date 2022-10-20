Midterm
================
Kaila An
2022-10-19

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(data.table)
```

    ## 
    ## Attaching package: 'data.table'
    ## 
    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     transpose

``` r
library(dplyr)
library(dtplyr)
library(ggplot2)
```

## Introduction and Formulated Question

The COVID-19 pandemic had changed the world as we knew it like no other
occurrence before. It changed people’s lifestyles, their habits, and
overall created a “new normal”. People’s mindsets and priorities have
changed so drastically, that it would be interesting to note whether or
not it brought a change to criminal behavior and activity. For this
project, I chose to focus on Los Angeles City Data, specifically, ‘Crime
Data in LA from 2020 to present’ and ‘Crime Data in LA from 2010 to
2019’. These particular data sets document incidents of crime,
transcribed from crime reports. This data has been provided by the Los
Angeles Police Department. Based on these data sets, the question I
would like to address is:

How has the prevalence of certain types of crimes in Los Angeles changed
since the start of the COVID-19 pandemic?

## Methods

# Reading in the data

``` r
lacrime202022 <- data.table::fread("crimedata_2020topresent.csv")
lacrime201019 <- data.table::fread("crime data_2010to2019.csv")
```

``` r
str(lacrime202022)
```

    ## Classes 'data.table' and 'data.frame':   581764 obs. of  28 variables:
    ##  $ DR_NO         : int  10304468 190101086 200110444 191501505 191921269 200100501 200100502 200100504 200100507 201710201 ...
    ##  $ Date Rptd     : chr  "01/08/2020 12:00:00 AM" "01/02/2020 12:00:00 AM" "04/14/2020 12:00:00 AM" "01/01/2020 12:00:00 AM" ...
    ##  $ DATE OCC      : chr  "01/08/2020 12:00:00 AM" "01/01/2020 12:00:00 AM" "02/13/2020 12:00:00 AM" "01/01/2020 12:00:00 AM" ...
    ##  $ TIME OCC      : int  2230 330 1200 1730 415 30 1315 40 200 1925 ...
    ##  $ AREA          : int  3 1 1 15 19 1 1 1 1 17 ...
    ##  $ AREA NAME     : chr  "Southwest" "Central" "Central" "N Hollywood" ...
    ##  $ Rpt Dist No   : int  377 163 155 1543 1998 163 161 155 101 1708 ...
    ##  $ Part 1-2      : int  2 2 2 2 2 1 1 2 1 1 ...
    ##  $ Crm Cd        : int  624 624 845 745 740 121 442 946 341 341 ...
    ##  $ Crm Cd Desc   : chr  "BATTERY - SIMPLE ASSAULT" "BATTERY - SIMPLE ASSAULT" "SEX OFFENDER REGISTRANT OUT OF COMPLIANCE" "VANDALISM - MISDEAMEANOR ($399 OR UNDER)" ...
    ##  $ Mocodes       : chr  "0444 0913" "0416 1822 1414" "1501" "0329 1402" ...
    ##  $ Vict Age      : int  36 25 0 76 31 25 23 0 23 0 ...
    ##  $ Vict Sex      : chr  "F" "M" "X" "F" ...
    ##  $ Vict Descent  : chr  "B" "H" "X" "W" ...
    ##  $ Premis Cd     : int  501 102 726 502 409 735 404 726 502 203 ...
    ##  $ Premis Desc   : chr  "SINGLE FAMILY DWELLING" "SIDEWALK" "POLICE FACILITY" "MULTI-UNIT DWELLING (APARTMENT, DUPLEX, ETC)" ...
    ##  $ Weapon Used Cd: int  400 500 NA NA NA 500 NA NA NA NA ...
    ##  $ Weapon Desc   : chr  "STRONG-ARM (HANDS, FIST, FEET OR BODILY FORCE)" "UNKNOWN WEAPON/OTHER WEAPON" "" "" ...
    ##  $ Status        : chr  "AO" "IC" "AA" "IC" ...
    ##  $ Status Desc   : chr  "Adult Other" "Invest Cont" "Adult Arrest" "Invest Cont" ...
    ##  $ Crm Cd 1      : int  624 624 845 745 740 121 442 946 341 341 ...
    ##  $ Crm Cd 2      : int  NA NA NA 998 NA 998 998 998 998 NA ...
    ##  $ Crm Cd 3      : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ Crm Cd 4      : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LOCATION      : chr  "1100 W  39TH                         PL" "700 S  HILL                         ST" "200 E  6TH                          ST" "5400    CORTEEN                      PL" ...
    ##  $ Cross Street  : chr  "" "" "" "" ...
    ##  $ LAT           : num  34 34 34 34.2 34.2 ...
    ##  $ LON           : num  -118 -118 -118 -118 -118 ...
    ##  - attr(*, ".internal.selfref")=<externalptr>

``` r
str(lacrime201019)
```

    ## Classes 'data.table' and 'data.frame':   2119797 obs. of  28 variables:
    ##  $ DR_NO         : int  1307355 11401303 70309629 90631215 100100501 100100506 100100508 100100509 100100510 100100511 ...
    ##  $ Date Rptd     : chr  "02/20/2010 12:00:00 AM" "09/13/2010 12:00:00 AM" "08/09/2010 12:00:00 AM" "01/05/2010 12:00:00 AM" ...
    ##  $ DATE OCC      : chr  "02/20/2010 12:00:00 AM" "09/12/2010 12:00:00 AM" "08/09/2010 12:00:00 AM" "01/05/2010 12:00:00 AM" ...
    ##  $ TIME OCC      : int  1350 45 1515 150 2100 1650 2005 2100 230 2100 ...
    ##  $ AREA          : int  13 14 13 6 1 1 1 1 1 1 ...
    ##  $ AREA NAME     : chr  "Newton" "Pacific" "Newton" "Hollywood" ...
    ##  $ Rpt Dist No   : int  1385 1485 1324 646 176 162 182 157 171 132 ...
    ##  $ Part 1-2      : int  2 2 2 2 1 1 1 1 1 1 ...
    ##  $ Crm Cd        : int  900 740 946 900 122 442 330 230 230 341 ...
    ##  $ Crm Cd Desc   : chr  "VIOLATION OF COURT ORDER" "VANDALISM - FELONY ($400 & OVER, ALL CHURCH VANDALISMS)" "OTHER MISCELLANEOUS CRIME" "VIOLATION OF COURT ORDER" ...
    ##  $ Mocodes       : chr  "0913 1814 2000" "0329" "0344" "1100 0400 1402" ...
    ##  $ Vict Age      : int  48 0 0 47 47 23 46 51 30 55 ...
    ##  $ Vict Sex      : chr  "M" "M" "M" "F" ...
    ##  $ Vict Descent  : chr  "H" "W" "H" "W" ...
    ##  $ Premis Cd     : int  501 101 103 101 103 404 101 710 108 710 ...
    ##  $ Premis Desc   : chr  "SINGLE FAMILY DWELLING" "STREET" "ALLEY" "STREET" ...
    ##  $ Weapon Used Cd: int  NA NA NA 102 400 NA NA 500 400 NA ...
    ##  $ Weapon Desc   : chr  "" "" "" "HAND GUN" ...
    ##  $ Status        : chr  "AA" "IC" "IC" "IC" ...
    ##  $ Status Desc   : chr  "Adult Arrest" "Invest Cont" "Invest Cont" "Invest Cont" ...
    ##  $ Crm Cd 1      : int  900 740 946 900 122 442 330 230 230 341 ...
    ##  $ Crm Cd 2      : int  NA NA NA 998 NA NA NA NA NA 998 ...
    ##  $ Crm Cd 3      : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ Crm Cd 4      : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LOCATION      : chr  "300 E  GAGE                         AV" "SEPULVEDA                    BL" "1300 E  21ST                         ST" "CAHUENGA                     BL" ...
    ##  $ Cross Street  : chr  "" "MANCHESTER                   AV" "" "HOLLYWOOD                    BL" ...
    ##  $ LAT           : num  34 34 34 34.1 34 ...
    ##  $ LON           : num  -118 -118 -118 -118 -118 ...
    ##  - attr(*, ".internal.selfref")=<externalptr>

``` r
table(lacrime201019$`Crm Cd Desc`)
```

    ## 
    ##                                         ABORTION/ILLEGAL 
    ##                                                        7 
    ##                                                    ARSON 
    ##                                                     3531 
    ##             ASSAULT WITH DEADLY WEAPON ON POLICE OFFICER 
    ##                                                     1604 
    ##           ASSAULT WITH DEADLY WEAPON, AGGRAVATED ASSAULT 
    ##                                                    92730 
    ##                                        ATTEMPTED ROBBERY 
    ##                                                    11995 
    ##                                 BATTERY - SIMPLE ASSAULT 
    ##                                                   190569 
    ##                                 BATTERY ON A FIREFIGHTER 
    ##                                                      325 
    ##                                  BATTERY POLICE (SIMPLE) 
    ##                                                     4869 
    ##                              BATTERY WITH SEXUAL CONTACT 
    ##                                                    11430 
    ## BEASTIALITY, CRIME AGAINST NATURE SEXUAL ASSLT WITH ANIM 
    ##                                                       26 
    ##                                                   BIGAMY 
    ##                                                       14 
    ##                                  BIKE - ATTEMPTED STOLEN 
    ##                                                       39 
    ##                                            BIKE - STOLEN 
    ##                                                    14353 
    ##                           BLOCKING DOOR INDUCTION CENTER 
    ##                                                        3 
    ##                                            BOAT - STOLEN 
    ##                                                      284 
    ##                                               BOMB SCARE 
    ##                                                     1186 
    ##                                          BRANDISH WEAPON 
    ##                                                    15478 
    ##                                                  BRIBERY 
    ##                                                       32 
    ##                                           BUNCO, ATTEMPT 
    ##                                                      758 
    ##                                       BUNCO, GRAND THEFT 
    ##                                                     9146 
    ##                                       BUNCO, PETTY THEFT 
    ##                                                     5434 
    ##                                                 BURGLARY 
    ##                                                   147731 
    ##                                    BURGLARY FROM VEHICLE 
    ##                                                   162184 
    ##                         BURGLARY FROM VEHICLE, ATTEMPTED 
    ##                                                     2846 
    ##                                      BURGLARY, ATTEMPTED 
    ##                                                    12536 
    ##                                        CHILD ABANDONMENT 
    ##                                                      104 
    ##              CHILD ABUSE (PHYSICAL) - AGGRAVATED ASSAULT 
    ##                                                     1682 
    ##                  CHILD ABUSE (PHYSICAL) - SIMPLE ASSAULT 
    ##                                                     9351 
    ##                           CHILD ANNOYING (17YRS & UNDER) 
    ##                                                     5207 
    ##                           CHILD NEGLECT (SEE 300 W.I.C.) 
    ##                                                     5257 
    ##                                        CHILD PORNOGRAPHY 
    ##                                                      229 
    ##                                           CHILD STEALING 
    ##                                                     1125 
    ##                                               CONSPIRACY 
    ##                                                       55 
    ##                                        CONTEMPT OF COURT 
    ##                                                     3839 
    ##                                             CONTRIBUTING 
    ##                                                      184 
    ##                                              COUNTERFEIT 
    ##                                                      842 
    ##                    CREDIT CARDS, FRAUD USE ($950 & UNDER 
    ##                                                      306 
    ##                 CREDIT CARDS, FRAUD USE ($950.01 & OVER) 
    ##                                                      799 
    ##                                        CRIMINAL HOMICIDE 
    ##                                                     2775 
    ##                   CRIMINAL THREATS - NO WEAPON DISPLAYED 
    ##                                                    56662 
    ## CRM AGNST CHLD (13 OR UNDER) (14-15 & SUSP 10 YRS OLDER) 
    ##                                                     9104 
    ##                                       CRUELTY TO ANIMALS 
    ##                                                     1274 
    ##     DEFRAUDING INNKEEPER/THEFT OF SERVICES, $950 & UNDER 
    ##                                                     2158 
    ##     DEFRAUDING INNKEEPER/THEFT OF SERVICES, OVER $950.01 
    ##                                                      238 
    ##                           DISCHARGE FIREARMS/SHOTS FIRED 
    ##                                                     3793 
    ##                         DISHONEST EMPLOYEE - GRAND THEFT 
    ##                                                      175 
    ##                         DISHONEST EMPLOYEE - PETTY THEFT 
    ##                                                      133 
    ##                       DISHONEST EMPLOYEE ATTEMPTED THEFT 
    ##                                                       10 
    ##                                           DISRUPT SCHOOL 
    ##                                                       44 
    ##                                     DISTURBING THE PEACE 
    ##                                                     3698 
    ##                         DOCUMENT FORGERY / STOLEN FELONY 
    ##                                                    22864 
    ##                        DOCUMENT WORTHLESS ($200 & UNDER) 
    ##                                                       62 
    ##                      DOCUMENT WORTHLESS ($200.01 & OVER) 
    ##                                                      372 
    ##                     DRIVING WITHOUT OWNER CONSENT (DWOC) 
    ##                                                      474 
    ##                                        DRUGS, TO A MINOR 
    ##                                                       48 
    ##                                               DRUNK ROLL 
    ##                                                       37 
    ##                                     DRUNK ROLL - ATTEMPT 
    ##                                                        1 
    ##               EMBEZZLEMENT, GRAND THEFT ($950.01 & OVER) 
    ##                                                     8058 
    ##                 EMBEZZLEMENT, PETTY THEFT ($950 & UNDER) 
    ##                                                      597 
    ##                                                EXTORTION 
    ##                                                     2618 
    ##                                      FAILURE TO DISPERSE 
    ##                                                       20 
    ##                                         FAILURE TO YIELD 
    ##                                                      520 
    ##                                       FALSE IMPRISONMENT 
    ##                                                      955 
    ##                                      FALSE POLICE REPORT 
    ##                                                      407 
    ##                 FIREARMS RESTRAINING ORDER (FIREARMS RO) 
    ##                                                        2 
    ##  FIREARMS TEMPORARY RESTRAINING ORDER (TEMP FIREARMS RO) 
    ##                                                        1 
    ##                                GRAND THEFT / AUTO REPAIR 
    ##                                                       16 
    ##                            GRAND THEFT / INSURANCE FRAUD 
    ##                                                       73 
    ##                  HUMAN TRAFFICKING - COMMERCIAL SEX ACTS 
    ##                                                      491 
    ##                HUMAN TRAFFICKING - INVOLUNTARY SERVITUDE 
    ##                                                      100 
    ##                                          ILLEGAL DUMPING 
    ##                                                      509 
    ##             INCEST (SEXUAL ACTS BETWEEN BLOOD RELATIVES) 
    ##                                                       12 
    ##                                          INCITING A RIOT 
    ##                                                       14 
    ##                                        INDECENT EXPOSURE 
    ##                                                     3519 
    ##                    INTIMATE PARTNER - AGGRAVATED ASSAULT 
    ##                                                    15453 
    ##                        INTIMATE PARTNER - SIMPLE ASSAULT 
    ##                                                   114618 
    ##                                               KIDNAPPING 
    ##                                                     2004 
    ##                               KIDNAPPING - GRAND ATTEMPT 
    ##                                                      717 
    ##                  LETTERS, LEWD  -  TELEPHONE CALLS, LEWD 
    ##                                                    21325 
    ##                                             LEWD CONDUCT 
    ##                                                     1391 
    ##                          LEWD/LASCIVIOUS ACTS WITH CHILD 
    ##                                                      258 
    ##                                                 LYNCHING 
    ##                                                       45 
    ##                                     LYNCHING - ATTEMPTED 
    ##                                                       25 
    ##                                  MANSLAUGHTER, NEGLIGENT 
    ##                                                        7 
    ##                                          ORAL COPULATION 
    ##                                                     1984 
    ##                                            OTHER ASSAULT 
    ##                                                     4099 
    ##                                OTHER MISCELLANEOUS CRIME 
    ##                                                    20712 
    ##                                                PANDERING 
    ##                                                      335 
    ##                                              PEEPING TOM 
    ##                                                     1182 
    ##                                PETTY THEFT - AUTO REPAIR 
    ##                                                       24 
    ##                                               PICKPOCKET 
    ##                                                     1006 
    ##                                      PICKPOCKET, ATTEMPT 
    ##                                                       23 
    ##                                                  PIMPING 
    ##                                                      479 
    ##                                                  PROWLER 
    ##                                                      943 
    ##                                          PURSE SNATCHING 
    ##                                                     1183 
    ##                                PURSE SNATCHING - ATTEMPT 
    ##                                                       47 
    ##                                          RAPE, ATTEMPTED 
    ##                                                     1123 
    ##                                           RAPE, FORCIBLE 
    ##                                                    10645 
    ##                                         RECKLESS DRIVING 
    ##                                                      265 
    ## REPLICA FIREARMS(SALE,DISPLAY,MANUFACTURE OR DISTRIBUTE) 
    ##                                                       30 
    ##                                         RESISTING ARREST 
    ##                                                     3253 
    ##                                                  ROBBERY 
    ##                                                    83860 
    ##                SEX OFFENDER REGISTRANT OUT OF COMPLIANCE 
    ##                                                     1724 
    ## SEX,UNLAWFUL(INC MUTUAL CONSENT, PENETRATION W/ FRGN OBJ 
    ##                                                     4444 
    ##                      SEXUAL PENETRATION W/FOREIGN OBJECT 
    ##                                                     2902 
    ##                                    SHOPLIFTING - ATTEMPT 
    ##                                                      243 
    ##                 SHOPLIFTING - PETTY THEFT ($950 & UNDER) 
    ##                                                    48397 
    ##                 SHOPLIFTING-GRAND THEFT ($950.01 & OVER) 
    ##                                                     4724 
    ##                        SHOTS FIRED AT INHABITED DWELLING 
    ##                                                     2536 
    ##         SHOTS FIRED AT MOVING VEHICLE, TRAIN OR AIRCRAFT 
    ##                                                      296 
    ##  SODOMY/SEXUAL CONTACT B/W PENIS OF ONE PERS TO ANUS OTH 
    ##                                                     1572 
    ##                                                 STALKING 
    ##                                                     1844 
    ##                              TELEPHONE PROPERTY - DAMAGE 
    ##                                                       37 
    ##                       THEFT FROM MOTOR VEHICLE - ATTEMPT 
    ##                                                     1237 
    ##      THEFT FROM MOTOR VEHICLE - GRAND ($950.01 AND OVER) 
    ##                                                    31649 
    ##          THEFT FROM MOTOR VEHICLE - PETTY ($950 & UNDER) 
    ##                                                    88516 
    ##                              THEFT FROM PERSON - ATTEMPT 
    ##                                                      293 
    ##                                        THEFT OF IDENTITY 
    ##                                                   128967 
    ##                                    THEFT PLAIN - ATTEMPT 
    ##                                                     1658 
    ##                       THEFT PLAIN - PETTY ($950 & UNDER) 
    ##                                                   149910 
    ## THEFT-GRAND ($950.01 & OVER)EXCPT,GUNS,FOWL,LIVESTK,PROD 
    ##                                                    74651 
    ##                            THEFT, COIN MACHINE - ATTEMPT 
    ##                                                       26 
    ##             THEFT, COIN MACHINE - GRAND ($950.01 & OVER) 
    ##                                                       46 
    ##               THEFT, COIN MACHINE - PETTY ($950 & UNDER) 
    ##                                                      232 
    ##                                            THEFT, PERSON 
    ##                                                    14780 
    ##                          THREATENING PHONE CALLS/LETTERS 
    ##                                                     3115 
    ##                        THROWING OBJECT AT MOVING VEHICLE 
    ##                                                     1650 
    ##                                       TILL TAP - ATTEMPT 
    ##                                                        4 
    ##                  TILL TAP - GRAND THEFT ($950.01 & OVER) 
    ##                                                       20 
    ##                          TILL TAP - PETTY ($950 & UNDER) 
    ##                                                       94 
    ##                                           TRAIN WRECKING 
    ##                                                        2 
    ##                                              TRESPASSING 
    ##                                                    21592 
    ##                             UNAUTHORIZED COMPUTER ACCESS 
    ##                                                     1473 
    ##  VANDALISM - FELONY ($400 & OVER, ALL CHURCH VANDALISMS) 
    ##                                                   109465 
    ##                 VANDALISM - MISDEAMEANOR ($399 OR UNDER) 
    ##                                                    90442 
    ##                                 VEHICLE - ATTEMPT STOLEN 
    ##                                                     3350 
    ##  VEHICLE - MOTORIZED SCOOTERS, BICYCLES, AND WHEELCHAIRS 
    ##                                                       61 
    ##                                         VEHICLE - STOLEN 
    ##                                                   159903 
    ##                                 VIOLATION OF COURT ORDER 
    ##                                                    20038 
    ##                           VIOLATION OF RESTRAINING ORDER 
    ##                                                    19326 
    ##                 VIOLATION OF TEMPORARY RESTRAINING ORDER 
    ##                                                     1443 
    ##                               WEAPONS POSSESSION/BOMBING 
    ##                                                      175

``` r
table(lacrime202022$`Crm Cd Desc`)
```

    ## 
    ##                                                    ARSON 
    ##                                                     1690 
    ##             ASSAULT WITH DEADLY WEAPON ON POLICE OFFICER 
    ##                                                      808 
    ##           ASSAULT WITH DEADLY WEAPON, AGGRAVATED ASSAULT 
    ##                                                    34468 
    ##                                        ATTEMPTED ROBBERY 
    ##                                                     3185 
    ##                                 BATTERY - SIMPLE ASSAULT 
    ##                                                    46445 
    ##                                 BATTERY ON A FIREFIGHTER 
    ##                                                      153 
    ##                                  BATTERY POLICE (SIMPLE) 
    ##                                                     1700 
    ##                              BATTERY WITH SEXUAL CONTACT 
    ##                                                     2608 
    ## BEASTIALITY, CRIME AGAINST NATURE SEXUAL ASSLT WITH ANIM 
    ##                                                        5 
    ##                                                   BIGAMY 
    ##                                                        4 
    ##                                  BIKE - ATTEMPTED STOLEN 
    ##                                                        6 
    ##                                            BIKE - STOLEN 
    ##                                                     5527 
    ##                           BLOCKING DOOR INDUCTION CENTER 
    ##                                                        4 
    ##                                            BOAT - STOLEN 
    ##                                                       78 
    ##                                               BOMB SCARE 
    ##                                                      267 
    ##                                          BRANDISH WEAPON 
    ##                                                     9214 
    ##                                                  BRIBERY 
    ##                                                        7 
    ##                                           BUNCO, ATTEMPT 
    ##                                                      214 
    ##                                       BUNCO, GRAND THEFT 
    ##                                                     3522 
    ##                                       BUNCO, PETTY THEFT 
    ##                                                     1314 
    ##                                                 BURGLARY 
    ##                                                    35539 
    ##                                    BURGLARY FROM VEHICLE 
    ##                                                    36911 
    ##                         BURGLARY FROM VEHICLE, ATTEMPTED 
    ##                                                      424 
    ##                                      BURGLARY, ATTEMPTED 
    ##                                                     2414 
    ##                                        CHILD ABANDONMENT 
    ##                                                       17 
    ##              CHILD ABUSE (PHYSICAL) - AGGRAVATED ASSAULT 
    ##                                                      396 
    ##                  CHILD ABUSE (PHYSICAL) - SIMPLE ASSAULT 
    ##                                                     2143 
    ##                           CHILD ANNOYING (17YRS & UNDER) 
    ##                                                      631 
    ##                           CHILD NEGLECT (SEE 300 W.I.C.) 
    ##                                                      723 
    ##                                        CHILD PORNOGRAPHY 
    ##                                                      161 
    ##                                           CHILD STEALING 
    ##                                                      284 
    ##                                               CONSPIRACY 
    ##                                                       12 
    ##                                        CONTEMPT OF COURT 
    ##                                                     1811 
    ##                                             CONTRIBUTING 
    ##                                                       21 
    ##                                              COUNTERFEIT 
    ##                                                       83 
    ##                    CREDIT CARDS, FRAUD USE ($950 & UNDER 
    ##                                                       62 
    ##                 CREDIT CARDS, FRAUD USE ($950.01 & OVER) 
    ##                                                       78 
    ##                                        CRIMINAL HOMICIDE 
    ##                                                     1062 
    ##                   CRIMINAL THREATS - NO WEAPON DISPLAYED 
    ##                                                    12149 
    ## CRM AGNST CHLD (13 OR UNDER) (14-15 & SUSP 10 YRS OLDER) 
    ##                                                     1063 
    ##                                       CRUELTY TO ANIMALS 
    ##                                                      159 
    ##     DEFRAUDING INNKEEPER/THEFT OF SERVICES, $950 & UNDER 
    ##                                                      193 
    ##     DEFRAUDING INNKEEPER/THEFT OF SERVICES, OVER $950.01 
    ##                                                       49 
    ##                           DISCHARGE FIREARMS/SHOTS FIRED 
    ##                                                     1749 
    ##                         DISHONEST EMPLOYEE - GRAND THEFT 
    ##                                                       24 
    ##                         DISHONEST EMPLOYEE - PETTY THEFT 
    ##                                                       13 
    ##                                           DISRUPT SCHOOL 
    ##                                                        7 
    ##                                     DISTURBING THE PEACE 
    ##                                                      929 
    ##                         DOCUMENT FORGERY / STOLEN FELONY 
    ##                                                     1940 
    ##                        DOCUMENT WORTHLESS ($200 & UNDER) 
    ##                                                       20 
    ##                      DOCUMENT WORTHLESS ($200.01 & OVER) 
    ##                                                       42 
    ##                     DRIVING WITHOUT OWNER CONSENT (DWOC) 
    ##                                                      119 
    ##                                        DRUGS, TO A MINOR 
    ##                                                        9 
    ##                                               DRUNK ROLL 
    ##                                                       16 
    ##               EMBEZZLEMENT, GRAND THEFT ($950.01 & OVER) 
    ##                                                     2223 
    ##                 EMBEZZLEMENT, PETTY THEFT ($950 & UNDER) 
    ##                                                       62 
    ##                                                EXTORTION 
    ##                                                     1118 
    ##                                      FAILURE TO DISPERSE 
    ##                                                        2 
    ##                                         FAILURE TO YIELD 
    ##                                                      865 
    ##                                       FALSE IMPRISONMENT 
    ##                                                      210 
    ##                                      FALSE POLICE REPORT 
    ##                                                       88 
    ##       FIREARMS EMERGENCY PROTECTIVE ORDER (FIREARMS EPO) 
    ##                                                        4 
    ##                 FIREARMS RESTRAINING ORDER (FIREARMS RO) 
    ##                                                        4 
    ##                                GRAND THEFT / AUTO REPAIR 
    ##                                                        1 
    ##                            GRAND THEFT / INSURANCE FRAUD 
    ##                                                        4 
    ##                  HUMAN TRAFFICKING - COMMERCIAL SEX ACTS 
    ##                                                      297 
    ##                HUMAN TRAFFICKING - INVOLUNTARY SERVITUDE 
    ##                                                       68 
    ##                                          ILLEGAL DUMPING 
    ##                                                       75 
    ##             INCEST (SEXUAL ACTS BETWEEN BLOOD RELATIVES) 
    ##                                                        3 
    ##                                          INCITING A RIOT 
    ##                                                        1 
    ##                                        INDECENT EXPOSURE 
    ##                                                      741 
    ##                    INTIMATE PARTNER - AGGRAVATED ASSAULT 
    ##                                                     8190 
    ##                        INTIMATE PARTNER - SIMPLE ASSAULT 
    ##                                                    30091 
    ##                                               KIDNAPPING 
    ##                                                      507 
    ##                               KIDNAPPING - GRAND ATTEMPT 
    ##                                                      153 
    ##                  LETTERS, LEWD  -  TELEPHONE CALLS, LEWD 
    ##                                                     4863 
    ##                                             LEWD CONDUCT 
    ##                                                      438 
    ##                          LEWD/LASCIVIOUS ACTS WITH CHILD 
    ##                                                       59 
    ##                                                 LYNCHING 
    ##                                                       13 
    ##                                     LYNCHING - ATTEMPTED 
    ##                                                        9 
    ##                                  MANSLAUGHTER, NEGLIGENT 
    ##                                                        8 
    ##                                          ORAL COPULATION 
    ##                                                      451 
    ##                                            OTHER ASSAULT 
    ##                                                     2793 
    ##                                OTHER MISCELLANEOUS CRIME 
    ##                                                     4262 
    ##                                                PANDERING 
    ##                                                       70 
    ##                                              PEEPING TOM 
    ##                                                      246 
    ##                                PETTY THEFT - AUTO REPAIR 
    ##                                                        7 
    ##                                               PICKPOCKET 
    ##                                                      889 
    ##                                      PICKPOCKET, ATTEMPT 
    ##                                                        3 
    ##                                                  PIMPING 
    ##                                                       98 
    ##                                                  PROWLER 
    ##                                                      121 
    ##                                          PURSE SNATCHING 
    ##                                                       72 
    ##                                PURSE SNATCHING - ATTEMPT 
    ##                                                        8 
    ##                                          RAPE, ATTEMPTED 
    ##                                                      197 
    ##                                           RAPE, FORCIBLE 
    ##                                                     2310 
    ##                                         RECKLESS DRIVING 
    ##                                                      163 
    ## REPLICA FIREARMS(SALE,DISPLAY,MANUFACTURE OR DISTRIBUTE) 
    ##                                                       10 
    ##                                         RESISTING ARREST 
    ##                                                      586 
    ##                                                  ROBBERY 
    ##                                                    20574 
    ##                SEX OFFENDER REGISTRANT OUT OF COMPLIANCE 
    ##                                                      584 
    ## SEX,UNLAWFUL(INC MUTUAL CONSENT, PENETRATION W/ FRGN OBJ 
    ##                                                      632 
    ##                      SEXUAL PENETRATION W/FOREIGN OBJECT 
    ##                                                      843 
    ##                                    SHOPLIFTING - ATTEMPT 
    ##                                                       61 
    ##                 SHOPLIFTING - PETTY THEFT ($950 & UNDER) 
    ##                                                    10141 
    ##                 SHOPLIFTING-GRAND THEFT ($950.01 & OVER) 
    ##                                                     1962 
    ##                        SHOTS FIRED AT INHABITED DWELLING 
    ##                                                     1139 
    ##         SHOTS FIRED AT MOVING VEHICLE, TRAIN OR AIRCRAFT 
    ##                                                      338 
    ##  SODOMY/SEXUAL CONTACT B/W PENIS OF ONE PERS TO ANUS OTH 
    ##                                                      334 
    ##                                                 STALKING 
    ##                                                      376 
    ##                              TELEPHONE PROPERTY - DAMAGE 
    ##                                                        4 
    ##                       THEFT FROM MOTOR VEHICLE - ATTEMPT 
    ##                                                      673 
    ##      THEFT FROM MOTOR VEHICLE - GRAND ($950.01 AND OVER) 
    ##                                                    18748 
    ##          THEFT FROM MOTOR VEHICLE - PETTY ($950 & UNDER) 
    ##                                                    23835 
    ##                              THEFT FROM PERSON - ATTEMPT 
    ##                                                       75 
    ##                                        THEFT OF IDENTITY 
    ##                                                    33861 
    ##                                    THEFT PLAIN - ATTEMPT 
    ##                                                      316 
    ##                       THEFT PLAIN - PETTY ($950 & UNDER) 
    ##                                                    30193 
    ## THEFT-GRAND ($950.01 & OVER)EXCPT,GUNS,FOWL,LIVESTK,PROD 
    ##                                                    17966 
    ##                            THEFT, COIN MACHINE - ATTEMPT 
    ##                                                        5 
    ##             THEFT, COIN MACHINE - GRAND ($950.01 & OVER) 
    ##                                                        5 
    ##               THEFT, COIN MACHINE - PETTY ($950 & UNDER) 
    ##                                                       15 
    ##                                            THEFT, PERSON 
    ##                                                     2655 
    ##                          THREATENING PHONE CALLS/LETTERS 
    ##                                                      334 
    ##                        THROWING OBJECT AT MOVING VEHICLE 
    ##                                                      526 
    ##                  TILL TAP - GRAND THEFT ($950.01 & OVER) 
    ##                                                        7 
    ##                          TILL TAP - PETTY ($950 & UNDER) 
    ##                                                       15 
    ##                                              TRESPASSING 
    ##                                                     7752 
    ##                             UNAUTHORIZED COMPUTER ACCESS 
    ##                                                      355 
    ##  VANDALISM - FELONY ($400 & OVER, ALL CHURCH VANDALISMS) 
    ##                                                    36870 
    ##                 VANDALISM - MISDEAMEANOR ($399 OR UNDER) 
    ##                                                    17230 
    ##                                 VEHICLE - ATTEMPT STOLEN 
    ##                                                     1656 
    ##  VEHICLE - MOTORIZED SCOOTERS, BICYCLES, AND WHEELCHAIRS 
    ##                                                      815 
    ##                                         VEHICLE - STOLEN 
    ##                                                    63711 
    ##                                 VIOLATION OF COURT ORDER 
    ##                                                     4135 
    ##                           VIOLATION OF RESTRAINING ORDER 
    ##                                                     7579 
    ##                 VIOLATION OF TEMPORARY RESTRAINING ORDER 
    ##                                                      565 
    ##                               WEAPONS POSSESSION/BOMBING 
    ##                                                       24

``` r
table(lacrime201019$`Vict Sex`)
```

    ## 
    ##             -      F      H      M      N      X 
    ## 196766      1 891687     73 976010     17  55243

``` r
table(lacrime202022$`Vict Sex`)
```

    ## 
    ##             F      H      M      X 
    ##  76951 214238     65 243401  47109

``` r
table(lacrime201019$`Vict Descent`)
```

    ## 
    ##             -      A      B      C      D      F      G      H      I      J 
    ## 196812      3  51218 335924   1074     24   2582     85 727540    951    419 
    ##      K      L      O      P      S      U      V      W      X      Z 
    ##   9203     18 203393    351     32    192    212 511348  78280    136

## Preliminary Results

## Conclusion
