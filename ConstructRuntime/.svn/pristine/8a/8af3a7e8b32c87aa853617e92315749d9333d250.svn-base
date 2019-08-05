* >Natural Source Header 000000
* :Mode S
* :CP IBM01140
* :LineIncrement 10
* <Natural Source Header
************************************************************************
* Program  : CDACTHLP
* System   : NATURAL-CONSTRUCT
* Title    : Helproutine for action field
* Generated: Apr 15,88 at 01:11 PM
* Function : This program is used to display help for the valid action
*            values of a construct generated file maintenance program.
*            This is a driver program for CD-HELP.
*
* Mod Date   Mod By    Description of Changes
************************************************************************
DEFINE DATA
  PARAMETER
  01 #PDA-VAL-ACT(A18)
  LOCAL USING CDKEYLDA
  LOCAL USING CDPDA-M
  LOCAL USING CDACTXL   /* Action code descriptions.
  LOCAL USING CDACTA    /* Valid action codes.
  LOCAL USING CDENVIRA
  LOCAL
*
* Miscellaneous variables.
  01 #HEIGHT(P3)                     /* Window Height.
*
* Define input fields.
  01 #INPUT
    02 #SELECTION(A1)                /* Input action value.
    02 REDEFINE #SELECTION
      03 #FIRST-CHAR-ON-SCREEN(A1)   /* Allow '.' to terminate.
*
* Value of the help keys for different actions
  01 #HELP-KEY-ARRAY(A65/1:9)        /* Key field of help.
  01 REDEFINE #HELP-KEY-ARRAY
    02 #HELP-KEY-STRUCTURE(1:9)
      03 #HELP-KEY-TYPE(A1)          /* Type of help.
      03 #HELP-KEY-MAJOR(A32)        /* Help major.
      03 #HELP-KEY-MINOR(A32)        /* Help Minor.
  01 #I(P2)                          /* Temporary index.
  01 #SET-CONTROL(A5)                /* Set column height.
  01 #VALID-CODES(A30)               /* Used in reinput msg.
  01 #VALID-ACTIONS(A30)             /* Current valid actions.
  01 REDEFINE #VALID-ACTIONS
    02 #ACTION(A1/1:9)
  01 #NUMBER-OF-ACTIONS(P1)
  01 #TABLE-INDEX(P3)                /* Index of Scroll Table.
  01 #PROGRAM (A8)
END-DEFINE
*
FORMAT PS=18 KD=ON LS=133
ASSIGN #PROGRAM = *PROGRAM
INCLUDE CCBEGIN
*
* Set PF-KEYS and their names.
RESET CDKEYLDA.#PFKEY(*)
RESET INITIAL CDKEYLDA.#RETURN-KEY
INCLUDE CCSETKEY
*
* Get text strings for current language.
CALLNAT 'CDUTRANS' CDACTXL.CDUTRANS(*) CDACTXL-NAME MSG-INFO
*
*
* Get all valid file maintenance action codes.
ASSIGN CDACTA.#ADD(*) = 'X'
ASSIGN CDACTA.#BROWSE(*) = 'X'
ASSIGN CDACTA.#CLEAR(*) = 'X'
ASSIGN CDACTA.#DISPLAY(*) = 'X'
ASSIGN CDACTA.#MODIFY(*) = 'X'
ASSIGN CDACTA.#NEXT(*) = 'X'
ASSIGN CDACTA.#PURGE(*) = 'X'
ASSIGN CDACTA.#RECALL(*) = 'X'
CALLNAT 'CDACT' CDACTA
*
* Remove commas and brackets from actions allowed passed in.
ASSIGN #VALID-ACTIONS = #PDA-VAL-ACT
EXAMINE #VALID-ACTIONS FOR ':' DELETE /* Used in messages.
ASSIGN #VALID-CODES = #VALID-ACTIONS
EXAMINE #VALID-ACTIONS FOR ',' DELETE
EXAMINE #VALID-ACTIONS FOR ')' DELETE
EXAMINE #VALID-ACTIONS FOR '(' DELETE GIVING LENGTH #NUMBER-OF-ACTIONS
*
* Set window base and size.
SET CONTROL 'WB'           /* Initialize base so that size is not trunc.
SET CONTROL 'WF'     /* Frame.
SET CONTROL 'WL57'   /* Window length.
*
* Set column height.
COMPUTE #HEIGHT = 12 + #NUMBER-OF-ACTIONS
COMPRESS 'WC' #HEIGHT TO #SET-CONTROL LEAVING NO
SET CONTROL #SET-CONTROL
SET CONTROL 'WB1/23' /* Window base.
*
* Build the help keys to be displayed.
MOVE 'F' TO #HELP-KEY-TYPE(*)
MOVE 'NCST-ACTION' TO #HELP-KEY-MAJOR(*)
FOR #I = 1 TO #NUMBER-OF-ACTIONS
  ASSIGN #HELP-KEY-MINOR(#I) = #ACTION(#I)
END-FOR
*
********************* Start of Main Program Logic **********************
*
PROG.
REPEAT    /* Until Exit key is pressed.
  NEXT.
  REPEAT
    PRINT CDACTXL.#ACT-HELP-INFO-TXT1
    PRINT CDACTXL.#ACT-HELP-INFO-TXT2
    WRITE ' '
    /*
    /* Display selection list.
    FOR #I = 1 TO #NUMBER-OF-ACTIONS
      DECIDE FOR FIRST CONDITION
        WHEN CDACTA.#ADD(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#ADD-TXT
        WHEN CDACTA.#BROWSE(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#BROWSE-TXT
        WHEN CDACTA.#CLEAR(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#CLEAR-TXT
        WHEN CDACTA.#DISPLAY(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#DISPLAY-TXT
        WHEN CDACTA.#MODIFY(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#MODIFY-TXT
        WHEN CDACTA.#NEXT(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#NEXT-TXT
        WHEN CDACTA.#PURGE(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#PURGE-TXT
        WHEN CDACTA.#RECALL(1) EQ #ACTION(#I)
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#RECALL-TXT
        WHEN NONE
          PRINT 5X #ACTION(#I) (AD=I) CDACTXL.#UNKNOWN
       END-DECIDE
     END-FOR
     NEWPAGE
      /*
      /* Display headings and load the scrolling table with key and ISN.
      AT TOP OF PAGE
        WRITE NOTITLE(AD=I) *PROGRAM(AD=D)
                            CDACTXL.#ACT-HELP-HEADER-TXT
        / *DATX(EM=LLL^DD','YY AD=D) 34X  *TIMX(EM=^HH':'II^AP AD=D)
        SKIP 1
        /*
      END-TOPPAGE
      /*
      /* Input the screen and check users PF keys.
      AT END OF PAGE
       INPUT(AD=MITL'_' ZP=ON SG=OFF IP=OFF)
              CDACTXL.#ACT-HELP-PROMPT(AD=O) #SELECTION
*
        IF *PF-KEY = CDKEYLDA.#RETURN-KEY OR #FIRST-CHAR-ON-SCREEN = '.'
          ESCAPE BOTTOM(PROG.) IMMEDIATE
        END-IF
*
        IF *PF-KEY NE 'ENTR' THEN
          REINPUT *8003 ALARM  /* Invalid PF key pressed.
        END-IF
*
        IF #SELECTION = ' ' OR #VALID-ACTIONS NE SCAN #SELECTION THEN
          /*  Enter a valid action code:1:to be described.
          REINPUT *8082  ,#VALID-CODES
        ELSE
          ESCAPE BOTTOM(NEXT.) IMMEDIATE
        END-IF
      END-ENDPAGE /* AT END OF PAGE
  END-REPEAT /* NEXT.
  /*
  /* Display the help
  EXAMINE #ACTION(*) FOR #SELECTION GIVING INDEX #TABLE-INDEX
*
  CALLNAT 'CD-HELP' #HELP-KEY-ARRAY(#TABLE-INDEX)
  ASSIGN #SELECTION = '.'
END-REPEAT /* PROG.
INCLUDE CCEND
*
END
