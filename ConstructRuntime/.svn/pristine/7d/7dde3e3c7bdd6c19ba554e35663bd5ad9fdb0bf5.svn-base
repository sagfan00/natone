* >Natural Source Header 000000
* :Mode S
* :CP
* :LineIncrement 10
* <Natural Source Header
**Postgen changes
**SAG GENERATOR: CST-SHELL                        VERSION: 3.4.1
**SAG TITLE: CST utility
**SAG SYSTEM: NATURAL-CONSTRUCT
**SAG DESCS(1): This helproutine receives an explicit logical parameter
**SAG PROGRAM-TYPE: H
************************************************************************
* Program  : CDHSET2
* System   : NATURAL-CONSTRUCT
* Title    : CST utility
* Generated: May 22,96 at 08:16 AM
* Function : This helproutine receives an explicit logical parameter
*
*
*
* History
**SAG DEFINE EXIT CHANGE-HISTORY
* Changed on Apr 29,96 by SAG for release ____
* >
* > Description: This helproutine receives an explicit logical parameter
* >            | The helproutine simply sets the received logical
* >            | parameter to indicate that HELP was requested for a
* >            | field. This logical parameter should be checked after
* >            | the INPUT statement (ie. if the flag is set, reset flag
* >            | and do the necessary help processing using CALLNATs).
* >            | Note: This technique solves the problem that the
* >            |       helproutines can not access data entered within
* >            |       the same screen transaction, for example,
* >            |       you can enter the file name (without pressing
* >            |       ENTER) and request HELP on field.
* >
* >
* >
**SAG END-EXIT
************************************************************************
DEFINE DATA
**SAG DEFINE EXIT PARAMETER-DATA
  PARAMETER
    01 #PDA-SET-HELP(L)
    01 #PDA-INDEX1(I2)
    01 #PDA-INDEX2(I2)
    01 #DUMMY(A32) /* Need this to satisfy naturals desire to pass the
                  /* field if you want the index values.
    01 #IMPLICITE-INDEX1(I2) /* Passed automatically by Natural
    01 #IMPLICITE-INDEX2(I2) /* Passed automatically by Natural
**SAG END-EXIT
END-DEFINE
*
**SAG DEFINE EXIT GENERATE-CODE
ASSIGN #PDA-SET-HELP = TRUE
ASSIGN #PDA-INDEX1 = #IMPLICITE-INDEX1
ASSIGN #PDA-INDEX2 = #IMPLICITE-INDEX2
SET CONTROL 'Q'    /* Bypass input screen.
**SAG END-EXIT
END
