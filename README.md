~~~~~~~~~~~~~~~~

//***FILE 204 IS FROM KEN TOMIAK OF DOITT (DEPT OF INFORMATION      *   FILE 204
//*           TECHNOLOGY AND TELECOMMUNIATIONS) OF NEW YORK         *   FILE 204
//*           CITY  (FORMERLY CALLED CDCSA).  THIS PACKAGE          *   FILE 204
//*           CONTAINS THEIR MVS CROSS SYSTEM FACILITY.  THE        *   FILE 204
//*           FACILITY IS AN ISPF APPLICATION, WHICH HELPS TO       *   FILE 204
//*           MAINTAIN A SINGLE RES PACK OVER MANY SEPARATE         *   FILE 204
//*           LPARS.  MAINTENANCE LEVELS ARE KEPT, AND THE RES      *   FILE 204
//*           PACKS ARE PROPAGATED THROUGH AN ISPF-GENERATED        *   FILE 204
//*           CLONING PROCESS.                                      *   FILE 204
//*                                                                 *   FILE 204
//*                  CDCSA MVS CROSS SYSTEM FACILITY                *   FILE 204
//*                                                                 *   FILE 204
//*       OVERALL DESCRIPTION.                                      *   FILE 204
//*                                                                 *   FILE 204
//*            THE CDCSA MVS CROSS SYSTEM FACILITY IS AN ISPF       *   FILE 204
//*       APPLICATION WHICH IS DESIGNED TO HELP MAINTAIN A          *   FILE 204
//*       STANDARD MVS RESIDENCE PACK TO BE CLONED AND USED BY      *   FILE 204
//*       MANY SEPARATE LPARS.  THE APPLICATION IS CONSTRUCTED      *   FILE 204
//*       TO KEEP PROPER DOCUMENTATION OF THE MAINTENANCE LEVELS    *   FILE 204
//*       ON EACH SYSTEM THAT IS BEING RUN.  THERE ARE SOME         *   FILE 204
//*       OTHER ASPECTS OF THIS SYSTEM, AS YOU CAN DISCOVER         *   FILE 204
//*       WHILE YOU EXAMINE IT.                                     *   FILE 204
//*                                                                 *   FILE 204
//*            THIS SYSTEM MAKES LIFE MUCH EASIER IN OUR            *   FILE 204
//*       INTERNAL SERVICE BUREAU ENVIRONMENT THAT IS COMMONLY      *   FILE 204
//*       FOUND NOWADAYS IN STATE GOVERNMENTS, LARGE CITY           *   FILE 204
//*       GOVERNMENTS, AND CORPORATIONS THAT HAVE COMBINED          *   FILE 204
//*       SEPARATE DATA CENTERS.  THEY ARE NOW RUNNING LPARS        *   FILE 204
//*       INSTEAD, AT A SINGLE LARGE SITE.                          *   FILE 204
//*                                                                 *   FILE 204
//*            WE ALSO HAVE A CICS AND A DB2 ADAPTATION OF THIS     *   FILE 204
//*       SYSTEM.  THESE PACKAGES ARE NOW ON FILES 210 AND 211      *   FILE 204
//*       RESPECTIVELY.                                             *   FILE 204
//*                                                                 *   FILE 204
//*            SOME OF THE OVERALL PHILOSOPHY OF THIS SYSTEM        *   FILE 204
//*       IS DOCUMENTED IN THE SCRIPT FILE WHICH IS ON CBT          *   FILE 204
//*       TAPE FILE 205.                                            *   FILE 204
//*                                                                 *   FILE 204
//*            THE WAY WE HAVE IT HERE, EACH LPAR GETS A COMMON     *   FILE 204
//*       MVS RES PACK, AT A CERTAIN (TWO-DIGIT) MAINTENANCE        *   FILE 204
//*       LEVEL.  THIS RES PACK CAN BE "CLONED" FROM ANY ONE        *   FILE 204
//*       PACK TO ANY OTHER PACK.  FROM THE "XSYSALC" CLIST         *   FILE 204
//*       (WHICH CALLS UP PANEL "XSYSPNL"), THE OPTION M, FOR       *   FILE 204
//*       MIGRATIONS, WILL GENERATE THE RES-PACK CLONING JCL.       *   FILE 204
//*       AS PART OF THE CLONING PROCEDURE, NEW SMP/E TARGET        *   FILE 204
//*       ZONES ARE CREATED, WHICH REFLECT THE LEVELS OF THE        *   FILE 204
//*       CONTENTS OF ALL THE SYSTEM LIBRARIES ON THE PACK.         *   FILE 204
//*                                                                 *   FILE 204
//*            IT IS UP TO EACH INSTALLATION TO DECIDE WHICH        *   FILE 204
//*       DATASETS THEY WILL KEEP ON THE COMMON RES PACK, AND       *   FILE 204
//*       WHICH ONES WILL GO ON THE PARMLIB PACK THAT IS UNIQUE     *   FILE 204
//*       FOR EACH LPAR.  THE LIST OF DATASETS ON OUR COMMON RES    *   FILE 204
//*       PACK FOR THE MVS/ESA 4.3 SYSTEM, IS INCLUDED AS MEMBER    *   FILE 204
//*       RESPACKD ON THIS FILE.  THE LIST OF DATASETS ON THE       *   FILE 204
//*       PARMLIB PACK IS INCLUDED AS MEMBER PRMPACKD ON THIS       *   FILE 204
//*       FILE.  THIS MAY HELP GIVE GUIDELINES ON "WHAT TO PUT      *   FILE 204
//*       WHERE".  GENERALLY, COMMON SMP-MAINTAINED LIBRARIES GO    *   FILE 204
//*       ON THE RES PACK.                                          *   FILE 204
//*                                                                 *   FILE 204
//*            THE UNIQUENESS OF EACH LPAR IS PROVIDED BY A         *   FILE 204
//*       SEPARATE PACK (MAINTAINED "BY HAND") WHICH HAS            *   FILE 204
//*       SYS1.PARMLIB, SYS1.PROCLIB, THE SYSTEM MASTER CATALOG,    *   FILE 204
//*       THE IODF, ETC.  WE ARE, AT THIS WRITING, RUNNING          *   FILE 204
//*       MVS/ESA RELEASE 4.1 IN PRODUCTION, SOON TO GO TO          *   FILE 204
//*       RELEASE 4.3.  I HAVE INCLUDED A MEMBER CALLED PARMLIB     *   FILE 204
//*       WHICH CONTAINS A FEW SELECTED SYS1.PARMLIB MEMBERS.       *   FILE 204
//*       PLEASE NOTE THE ORDER OF THE LINK LIST AND LPA LIST       *   FILE 204
//*       CONCATENATIONS.  SYSTEM SPECIFICITY CAN STILL BE          *   FILE 204
//*       PROPAGATED ON A COMMON RES PACK, DEPENDING ON THE         *   FILE 204
//*       ORDER OF THESE CONCATENATIONS.                            *   FILE 204
//*                                                                 *   FILE 204
//*            EACH SERVICE LEVEL IS CREATED ON TEST RES PACKS,     *   FILE 204
//*       OF WHICH WE HAVE SEVERAL.  THESE ARE THE PACKS THAT       *   FILE 204
//*       THE SMP IS DONE TO.  WE IPL THEM AS TEST SYSTEMS UNDER    *   FILE 204
//*       VM.  ONCE A GIVEN MAINTENANCE LEVEL IS FROZEN, THE        *   FILE 204
//*       APPROPRIATE TEST PACK IS CLONED TO A PRODUCTION RES       *   FILE 204
//*       PACK THAT IS IPL'ED, POINTING TO THE PRODUCTION           *   FILE 204
//*       PARMLIB PACK FOR ITS UNIQUENESS.                          *   FILE 204
//*                                                                 *   FILE 204
//*            A WORD ABOUT NAMING CONVENTIONS:  MOST OF THE        *   FILE 204
//*       MEMBERS OF THIS PDS:  CHANGES, CLIST, ETC. ARE            *   FILE 204
//*       IEBUPDTE-UNLOADED PDS'ES THEMSELVES.  THEY CAN BE         *   FILE 204
//*       PROPERLY RESTORED USING THE PDSLOAD PROGRAM FROM FILE     *   FILE 204
//*       093 OF THIS TAPE.  A SAMPLE PDSLOAD JOB IS MEMBER         *   FILE 204
//*       $PDSLOAD ON THIS FILE.  PDSLOAD WILL RESTORE EACH         *   FILE 204
//*       MEMBER'S ISPF STATISTICS.  IF YOU USE IEBUPDTE, THE       *   FILE 204
//*       ISPF STATISTICS WILL NOT BE STOWED.  THE ORIGINAL NAME    *   FILE 204
//*       FOR EACH OF THESE PDS'ES WAS PREFIXED BY XSYS.MVSESA.     *   FILE 204
//*       THEREFORE, THE ORIGINAL NAME FOR THE LIBRARY WHOSE        *   FILE 204
//*       NAME HERE IS CLIST, WAS "XSYS.MVSESA.CLIST".  YOU GET     *   FILE 204
//*       THE POINT.  THESE FULL NAMES WILL BE MENTIONED            *   FILE 204
//*       THROUGHOUT THIS PACKAGE, AND YOU MUST MAKE GLOBAL         *   FILE 204
//*       CHANGES TO THE XSYS.MVSESA PREFIX TO ADAPT THE PACKAGE    *   FILE 204
//*       TO YOUR OWN SYSTEM'S NAMING CONVENTIONS.  MEMBER          *   FILE 204
//*       LEVLLIST CAME FROM A PS DATASET CALLED                    *   FILE 204
//*       XSYS.MVSESA.LEVEL.LIST, WHICH IS MAINTAINED BY HAND.      *   FILE 204
//*                                                                 *   FILE 204
//*            ALL 80-BYTE LRECL PDS'ES FROM THE PACKAGE HAVE       *   FILE 204
//*       BEEN MADE INTO MEMBERS ON THIS FILE.  THERE WAS ONE       *   FILE 204
//*       OTHER PDS, CALLED XSYS.MVSESA.SCRIPT, WHOSE LRECL IS      *   FILE 204
//*       147 AND WHICH WILL BE SEPARATELY PLACED IN FILE 205 OF    *   FILE 204
//*       THE CBT TAPE.                                             *   FILE 204
//*                                                                 *   FILE 204
//*            TO SET UP THIS PACKAGE, LOOK AT MEMBER XSYSALC IN    *   FILE 204
//*       THE CLIST LIBRARY.  THE CLIST "XSYSALC" SETS              *   FILE 204
//*       EVERYTHING ELSE IN MOTION.  THINGS START FROM THERE.      *   FILE 204
//*       IT SHOULD BE OBVIOUS HOW THE LIBRARIES OUGHT TO BE SET    *   FILE 204
//*       UP.  AS WE MENTIONED BEFORE, YOU HAVE TO MAKE GLOBAL      *   FILE 204
//*       CHANGES TO THE DATASET PREFIX NAMES WHEN YOU SET THIS     *   FILE 204
//*       UP ON YOUR OWN SYSTEM.  TO MY KNOWLEDGE, THESE NAMES      *   FILE 204
//*       ARE HARD CODED.  IF YOU THINK SOME OF THE INGREDIENTS     *   FILE 204
//*       ARE MISSING, PLEASE EMAIL SAM GOLOB AT                    *   FILE 204
//*          sbgolob@att.net        or  sbgolob@cbttape.org         *   FILE 204
//*                                                                 *   FILE 204
//*       (IF MY CONTACT INFORMATION BECOMES OBSOLETE, PLEASE       *   FILE 204
//*        CALL THE MEMBERSHIP OFFICER AT NASPA 414-768-8000        *   FILE 204
//*        WHERE I INTEND TO LEAVE MY NEW INFORMATION.   SG)        *   FILE 204
//*                                                                 *   FILE 204
//*            FROM THE XSYSPNL PANEL, OPTION D BROWSES A PDS       *   FILE 204
//*       CALLED XSYS.PGMDIR WHERE WE HAVE PUT OUR IPO1.PGMDIR      *   FILE 204
//*       MEMBERS.  OPTION E INVOKES ISPF 3.4 AGAINST PREFIX        *   FILE 204
//*       XSYS.MVSESA.*.  OPTION IVP INVOKES ISPF 3.4 AGAINST       *   FILE 204
//*       SOME DATASETS NAMED XSYS.---.IVPLIB, WHICH CONTAIN        *   FILE 204
//*       JOBSTREAMS TO TEST THE NEW SYSTEMS IN VARIOUS LPAR        *   FILE 204
//*       ENVIRONMENTS.  THESE DATASETS ARE NOT BEING INCLUDED      *   FILE 204
//*       HERE.                                                     *   FILE 204
//*                                                                 *   FILE 204
//*            I THINK THE MOST IMMEDIATELY INTERESTING PART OF     *   FILE 204
//*       THIS SYSTEM IS THE M OPTION TO GENERATE THE JCL THAT      *   FILE 204
//*       DOES THE RES PACK CLONING.  THE OTHER THINGS ARE          *   FILE 204
//*       CONVENIENT ADD-ONS IN MY OPINION, ALTHOUGH I AM GLAD      *   FILE 204
//*       THEY ARE THERE.                                           *   FILE 204
//*                                                                 *   FILE 204
//*            TO RUN THE CLONING JOBS, YOU HAVE TO SET UP THE      *   FILE 204
//*       CHANGES LIBRARY, BECAUSE THIS LIBRARY IS AUTOMATICALLY    *   FILE 204
//*       UPDATED EVERY TIME YOU RUN A CLONING JOB.                 *   FILE 204
//*                                                                 *   FILE 204
//*            THIS PACKAGE WAS WRITTEN BY JOEL PERLMAN AND KEN     *   FILE 204
//*       TOMIAK AT CDCSA (COMPUTER AND DATA COMMUNICATONS          *   FILE 204
//*       SERVICES AGENCY) OF NEW YORK CITY, WHILE THEY WERE        *   FILE 204
//*       WORKING FOR IBM AND UNDER CONTRACT TO NEW YORK CITY.      *   FILE 204
//*       ALL THE REQUISITE PERMISSIONS FOR INCLUSION ON THE CBT    *   FILE 204
//*       MVS UTILITIES TAPE, TO MY BEST KNOWLEDGE, HAVE BEEN       *   FILE 204
//*       GRANTED.                                                  *   FILE 204
//*                                                                 *   FILE 204
//*       IF YOU HAVE QUESTIONS, PLEASE CONTACT:                    *   FILE 204
//*                                                                 *   FILE 204
//*           Sam Golob             EMAIL:  sbgolob@att.net         *   FILE 204
//*                                         sbgolob@cbttape.org     *   FILE 204
//*                                                                 *   FILE 204
~~~~~~~~~~~~~~~~

