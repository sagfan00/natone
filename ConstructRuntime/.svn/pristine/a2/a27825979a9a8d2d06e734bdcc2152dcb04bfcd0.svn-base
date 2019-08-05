* >Natural Source Header 000000
* :Mode S
* :CP
* :LineIncrement 10
* <Natural Source Header
************************************************************************
* Program  : CDACTHL2
* System   : NATURAL-CONSTRUCT
* Title    : Helproutine for action field
* Generated: May 13,94
* Function : This program is used to display help for the valid action
*            values of a construct generated file maintenance program.
*            This is a driver program for CD-HELP.
*            To extend this helproutine to support customized action
*            codes, extend the DECIDE statement to include a WHEN
*            clause for each extra action defined in CDACTA.  It is
*            important that these be coded in the same sequence as
*            they are coded in CDACTA.
*
* Mod Date   Mod By    Description of Changes
*                      ______________________________________________
************************************************************************
DEFINE DATA
  PARAMETER
  01 #APPL-ACTIONS (A3/1:30,1:3)
  LOCAL USING CDENVIRA
  LOCAL USING CDKEYLDA
  LOCAL USING CDPDA-M
  LOCAL USING CDACTXL   /* Action code descriptions.
  LOCAL
  01 #MAX-CODES (I1) CONST <30>
*
* Define input fields.
  01 #INPUT
    02 #SELECTION(A3)                /* INPUT ACTION VALUE.
    02 REDEFINE #SELECTION
      03 #FIRST-CHAR-ON-SCREEN(A1)   /* Allow '.' to terminate.
*
* Value of the help keys for different actions
  01 #HELP-KEY-ARRAY(A65/1:#MAX-CODES)        /* KEY FIELD OF HELP.
  01 REDEFINE #HELP-KEY-ARRAY
    02 #HELP-KEY-STRUCTURE(1:#MAX-CODES)
      03 #HELP-KEY-TYPE(A1)          /* Type of help.
      03 #HELP-KEY-MAJOR(A32)        /* Help major.
      03 #HELP-KEY-MINOR(A32)        /* Help Minor.
  01 #I(I1)                          /* Temporary index.
  01 #LINES-PER-SCREEN (I1) CONST <10>
  01 #MESSAGE (A79)
  01 #NUMBER-OF-ACTIONS (I1)
  01 #PROGRAM (A8)
  01 #SECOND-INDEX (I1)
  01 #START (I1) INIT<1>
  01 #TABLE-CODE (A3/1:#MAX-CODES)
  01 #TABLE-DESC (A30/1:#MAX-CODES)
  01 #TABLE-INDEX(P3)                /* Index of Scroll Table.
  01 #TABLE-POINTER (I1) INIT <1>
  01 #VALID-CODES(A79)               /* USED IN REINPUT MSG.
  01 #VALID-ACTIONS(A79)             /* CURRENT VALID ACTIONS.
  01 #Y (I1)
  01 #Z (I1)
END-DEFINE
*
FORMAT PS=18 KD=ON LS=133
ASSIGN #PROGRAM = *PROGRAM
INCLUDE CCBEGIN
*
* Set PF-KEYS and their names.
RESET CDKEYLDA.#PFKEY(*)
RESET INITIAL CDKEYLDA.#RETURN-KEY
RESET INITIAL CDKEYLDA.#FORWARD-KEY
RESET INITIAL CDKEYLDA.#BACKWARD-KEY
INCLUDE CCSETKEY
*
* Get text strings for current language.
CALLNAT 'CDUTRANS' CDACTXL.CDUTRANS(*) CDACTXL-NAME MSG-INFO
*
FOR #I = 1 TO #MAX-CODES
  ASSIGN #HELP-KEY-MINOR(#I) = #APPL-ACTIONS(#I,3)
  IF NOT (#APPL-ACTIONS(#I,1) = ' ' OR = H'00') THEN
    COMPRESS #VALID-ACTIONS ',' #APPL-ACTIONS(#I,1) INTO #VALID-ACTIONS
      LEAVING NO
  END-IF
END-FOR
EXAMINE #VALID-ACTIONS FOR ',' GIVING NUMBER #NUMBER-OF-ACTIONS
COMPRESS '(' #VALID-ACTIONS ')' INTO #VALID-ACTIONS LEAVING NO
EXAMINE #VALID-ACTIONS FOR '(,' REPLACE WITH '('
ASSIGN #VALID-CODES = #VALID-ACTIONS
EXAMINE #VALID-ACTIONS FOR ')' DELETE
EXAMINE #VALID-ACTIONS FOR '(' DELETE
*
* Set window base and size.
SET CONTROL 'WB'     /* Initialize base so that size is not trunc.
SET CONTROL 'WF'     /* Frame.
SET CONTROL 'WL58'   /* Window length.
*
SET CONTROL 'WC23'
SET CONTROL 'WB1/22' /* Window base.
SET CONTROL 'YB'     /* Show PF keys.
*
* Build the help keys to be displayed.
MOVE 'F' TO #HELP-KEY-TYPE(*)
MOVE 'NCST-ACTION' TO #HELP-KEY-MAJOR(*)
*
********************* Start of Main Program Logic **********************
*
DECIDE FOR EVERY CONDITION
  WHEN NOT (#APPL-ACTIONS(1,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(1,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#ADD-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(2,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(2,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#BROWSE-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(3,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(3,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#CLEAR-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(4,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(4,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#DISPLAY-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(5,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(5,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#MODIFY-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(6,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(6,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#NEXT-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(7,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(7,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#PURGE-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(8,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(8,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#COPY-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(9,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(9,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#RECALL-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(10,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(10,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#REPLACE-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(11,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(11,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#SELECT-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(12,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(12,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#DETAIL-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NOT (#APPL-ACTIONS(13,1) = ' ' OR = H'00')
    ASSIGN #TABLE-CODE(#TABLE-POINTER) = #APPL-ACTIONS(13,1)
    ASSIGN #TABLE-DESC(#TABLE-POINTER) = CDACTXL.#FORMER-TXT
    ADD 1 TO #TABLE-POINTER
  WHEN NONE
    IGNORE
END-DECIDE
PROG.
REPEAT    /* Until Exit key is pressed.
  NEXT.
  REPEAT
    INPUT WITH TEXT #MESSAGE USING MAP 'CDACTHLM'
    RESET #MESSAGE
    IF *PF-KEY NE 'ENTR' THEN
      RESET #SELECTION
    END-IF
    IF *PF-KEY = CDKEYLDA.#RETURN-KEY OR #FIRST-CHAR-ON-SCREEN = '.'
      ESCAPE BOTTOM(PROG.) IMMEDIATE
    END-IF
    IF *PF-KEY = CDKEYLDA.#FORWARD-KEY THEN
      ADD #LINES-PER-SCREEN TO #START
      IF #START + #LINES-PER-SCREEN - 1 > #MAX-CODES THEN
        COMPUTE #START = #MAX-CODES - #LINES-PER-SCREEN + 1
      END-IF
      IF #TABLE-CODE(#START) = ' ' THEN
        SUBTRACT #LINES-PER-SCREEN FROM #START
        ASSIGN #MESSAGE = 'End of data reached'
      END-IF
      ESCAPE TOP
    END-IF
    IF *PF-KEY = CDKEYLDA.#BACKWARD-KEY THEN
      SUBTRACT #LINES-PER-SCREEN FROM #START
      IF #START <= 0 THEN
        ASSIGN #MESSAGE = 'Top of current scroll session reached'
        RESET INITIAL #START
      END-IF
      ESCAPE TOP
    END-IF
*
    IF *PF-KEY NE 'ENTR' THEN
      REINPUT *8003 ALARM  /* Invalid PF key pressed.
    END-IF
*
    IF #SELECTION = ' ' OR NOT (#SELECTION = #APPL-ACTIONS(*,1)) THEN
      /*  Enter a valid action code:1:to be described.
      REINPUT *8082  ,#VALID-CODES
    ELSE
      ESCAPE BOTTOM(NEXT.) IMMEDIATE
    END-IF
  END-REPEAT /* NEXT.
  /*
  /* Display the help
  EXAMINE #APPL-ACTIONS(*,1) FOR #SELECTION GIVING INDEX #TABLE-INDEX
    #SECOND-INDEX
*
  CALLNAT 'CD-HELP' #HELP-KEY-ARRAY(#TABLE-INDEX)
  ASSIGN #SELECTION = '.'
END-REPEAT /* PROG.
INCLUDE CCEND
END
