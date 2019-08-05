* >Natural Source Header 000000
* :Mode S
* :CP
* :LineIncrement 10
* <Natural Source Header
**Postgen changes
**SAG GENERATOR: CST-SHELL                        VERSION: 3.4.1
**SAG TITLE: Help Text utility
**SAG SYSTEM: NATURAL-CONSTRUCT
**SAG DESCS(1): This helproutine converts passed keys to a Help key
**SAG DESCS(2): and then invokes the proper help display program.
**SAG PROGRAM-TYPE: H
**SAG LOCAL-NAME(1): CDHLPTXL
**SAG LOCAL-NAME(2): CDPDA-M
**SAG LOCAL-NAME(3): CDASAA
**SAG LOCAL-NAME(4): CDLLANG
************************************************************************
* Program  : CD-HELPR
* System   : NATURAL-CONSTRUCT
* Title    : Help Text utility
* Generated: Jul 25,96 at 08:53 AM by SAG
* Function : This helproutine converts passed keys to a Help key
*            and then invokes the proper help display program.
*
*
* History
**SAG DEFINE EXIT CHANGE-HISTORY
* Changed on Jul 25,96 by SAG for release ____
* >
* >    Description: This helproutine converts passed keys to a Help
* >               : key and then invokes the proper help display
* >               : subprogram.
* >               :
* >               : This helproutine may do conversion of the passed
* >               : key value since it is possible to use the HE
* >               : notation (HE='CD-HELPR',=) to alleviate the need
* >               : to define a specific parameter value. In this
* >               : case this routine tries to make assumptions about
* >               : the type of information for which help is
* >               : requested.
* >               :
* >               : Here are the rules used to convert the passed key
* >               : value into the help file key value.
* >               :
* >               : 1. Passed value contains a '.'
* >               :    Example: EMPLOYEES.BIRTH(HE='CD-HELPR',=)
* >               :    Rules..: Type is assumed to be D for DB field
* >               :             Major is portion before .
* >               :             Minor is portion after .
* >               :    Result.: D/EMPLOYEES/BIRTH
* >               : 2. Passed value begins with a # or +
* >               :    Example: #CODE(HE='CD-HELPR',=)
* >               :    Rules..: Type is assumed to be F for field help
* >               :             Major is first character
* >               :             Minor is remainder
* >               :    Result.: F/#/CODE
* >               : 3. Key length is less than 32 characters long
* >               :    Example: INPUT(HE='CD-HELPR',=) #CODE
* >               :    Rules..: Type is assumed to be P for program.
* >               :             Major is portion is assumed to be
* >               :             the first 2 characters.
* >               :             Minor is entire passed value.
* >               :    Result.: P/GL/GLMAIN
* >               : 4. None of the above, use the passed key
* >               :    directly.
* >               :
* >               : If field level help is not found on the Natural
* >               : Construct help file, also try to find a PREDICT
* >               : extended description for the field.
* >               :
* >               : This routine is now language independent.
* >
* >
**SAG END-EXIT
************************************************************************
DEFINE DATA
**SAG DEFINE EXIT PARAMETER-DATA
  PARAMETER
  01 #PDA-HPARM(A65)
**SAG END-EXIT
  LOCAL USING CDHLPTXL
  LOCAL USING CDPDA-M
  LOCAL USING CDASAA
  LOCAL USING CDLLANG
  LOCAL
  01 #PROGRAM (A8)
**SAG DEFINE EXIT LOCAL-DATA
*
* Additional local variables
  01 #HPARM(A65)
  01 REDEFINE #HPARM
    02 #TYPE(A1)
    02 #MAJOR(A32)
    02 #MINOR(A32)
    02 REDEFINE #MINOR
      03 #TWOCHAR(A2)
    02 REDEFINE #MINOR
      03 #ONECHAR(A1)
      03 #REMAIN-CHAR(A31)
  01 #LANG(I1)
  01 #LENGTH(N2)
  01 #LOCAL-HPARM(A65)
  01 NCST-HELP VIEW OF NCST-HELP
    02 HELP-LINES-PER-PAGE
  01 #MSG(A79)
*
* Variables used for language independent text.
  01 #MESSAGE(A50)
  01 #LANGUAGE-SPECIFIC-HELP-KEY (A67)
  01 REDEFINE #LANGUAGE-SPECIFIC-HELP-KEY
    02 #LANGUAGE-SPECIFIC-HELP-KEY-1 (A65)
    02 #LANGUAGE-SPECIFIC-HELP-KEY-2 (N2)
  01 #HELP-TEXT-FOUND(L)
**SAG END-EXIT
END-DEFINE
PROG.    /* to allow escape from routine.
REPEAT
*
PERFORM INITIALIZATIONS
**SAG DEFINE EXIT START-OF-PROGRAM
FORMAT LS=80 PS=24
**SAG END-EXIT
*
**SAG DEFINE EXIT GENERATE-CODE
  DECIDE ON FIRST VALUE OF #PDA-HPARM
    VALUE '#GDA-GEN-PROGRAM'
      ASSIGN #HPARM = #LOCAL-HPARM = '#PDA-GEN-PROGRAM'
    VALUE '#COMMAND'
      ASSIGN #HPARM = #LOCAL-HPARM = '#DIRECT-COMMAND'
    NONE
      ASSIGN #HPARM = #LOCAL-HPARM = #PDA-HPARM
  END-DECIDE
*
*
* Use current language to generate corresponding text/msg in pgm.
  CALLNAT 'CDUTRANS' CDHLPTXL.CDUTRANS(*) CDHLPTXL-NAME MSG-INFO
*
* Translate & characters to current language, also determine the length.
  ASSIGN #LANG = *LANGUAGE
  EXAMINE #HPARM FOR '&' REPLACE WITH CDLLANG.#LANG-CODE-CHAR(#LANG)
    GIVING LENGTH #LENGTH
*
* Passed Help key
* A) contains '.' eg. NCST-PROFILE.PROFILE-NAME
*    - Break at the '.', and assign TYPE = 'D'
*       key =    D/NCST-PROFILE/PROFILE-NAME
* B) begins with '+' or '#'  eg. #CODE  or  +MSG
*    - assign MAJOR = first char and assign TYPE = 'F'
*       key =    F/#/CODE        F/+/MSG
* C) Length le 32  eg. CSHEDTM1
*    - assign MAJOR = first 2 chars, and assign TYPE = 'P'
*       key =    P/CS/CSHEDTM1
* D) None of the above
*    - Use the input key directly
*
  DECIDE FOR FIRST CONDITION
    WHEN #HPARM EQ SCAN'.'
      SEPARATE #LOCAL-HPARM INTO #MAJOR #MINOR
        WITH DELIMITER '.'
      ASSIGN #TYPE = 'D'
    WHEN (#TYPE = '+' OR = '#') AND #LENGTH LE 32
      ASSIGN #MINOR = #MAJOR
      ASSIGN #MAJOR = #TYPE
      ASSIGN #TYPE = 'F'
    WHEN #LENGTH LE 8
      ASSIGN #MINOR = #HPARM
      ASSIGN #MAJOR = #TWOCHAR
      ASSIGN #TYPE = 'P'
    WHEN #LENGTH LE 33
      SET CONTROL 'WB'
      SET CONTROL 'WF'
      SET CONTROL 'WL80'
      SET CONTROL 'WC4'
      SET CONTROL 'WB3/0'
      SET KEY ALL
      SET KEY PA2 = OFF
*
* Convert to ISA-SAA standard prompts.
      ASSIGN CDASAA.#MSG-TXT = CDHLPTXL.#HELP-INVALID-PARAMETER
      ASSIGN CDASAA.#MAX-MSG-LENGTH = 50
      CALLNAT 'CDUSAA' CDASAA
      ASSIGN CDHLPTXL.#HELP-INVALID-PARAMETER = CDASAA.#MSG-TXT
*
      ASSIGN CDASAA.#MSG-TXT = CDHLPTXL.#HELP-PARAMETER-TXT
      ASSIGN CDASAA.#MAX-MSG-LENGTH = 15
      CALLNAT 'CDUSAA' CDASAA
      ASSIGN CDHLPTXL.#HELP-PARAMETER-TXT = CDASAA.#MSG-TXT
*
      INPUT(IP=OFF) WITH TEXT
        CDHLPTXL.#HELP-INVALID-PARAMETER(AD=P) 'CD-HELPR'(I)
        CDHLPTXL.#HELP-PARAMETER-TXT (AD=O) #HPARM(AL=33 AD=OI)
      PERFORM SET-ALTERNATE-PFKEYS
      ESCAPE ROUTINE
    WHEN NONE
      IGNORE
  END-DECIDE
*
  PERFORM FIND-HELP-TEXT
  IF NOT #HELP-TEXT-FOUND
    /* type D help text members are qualified members and could exist
    /* as not qualified members or exist in PREDICT
    IF #TYPE = 'D' /* No HELP TEXT found, try PREDICT field descriptions
      ASSIGN #LOCAL-HPARM = #HPARM
      ASSIGN #HPARM = #MINOR
      /* check to make sure that it is not a qualified program variable
      /* before checking PREDICT.
      IF (#TYPE = '+' OR = '#') AND #LENGTH LE 32
        ASSIGN #MINOR = #MAJOR
        ASSIGN #MAJOR = #TYPE
        ASSIGN #TYPE = 'F'
        PERFORM FIND-HELP-TEXT
        IF NOT #HELP-TEXT-FOUND
          NEWPAGE
          CALLNAT 'CD-HPRED' #PDA-HPARM (AD=O)
          PERFORM SET-ALTERNATE-PFKEYS
          ESCAPE ROUTINE
        END-IF
      ELSE
        NEWPAGE
        CALLNAT 'CD-HPRED' #PDA-HPARM (AD=O)
        PERFORM SET-ALTERNATE-PFKEYS
        ESCAPE ROUTINE
      END-IF
    ELSE
      COMPRESS #TYPE '/' #MAJOR '/' #MINOR INTO #MSG LEAVING NO
      SET CONTROL 'WB'
      SET CONTROL 'WF'
      SET CONTROL 'WL80'
      SET CONTROL 'WC4'
      SET CONTROL 'WB3/0'
      SET KEY ALL
      SET KEY PA2 = OFF
*
* Convert to ISA-SAA standard prompts.
      ASSIGN CDASAA.#MSG-TXT = CDHLPTXL.#HELP-ID-TXT
      ASSIGN CDASAA.#MAX-MSG-LENGTH = 15
      CALLNAT 'CDUSAA' CDASAA
      ASSIGN CDHLPTXL.#HELP-ID-TXT = CDASAA.#MSG-TXT
*
      INPUT(IP=OFF) WITH TEXT *8076
        CDHLPTXL.#HELP-ID-TXT(AD=O) #MSG(AD=O AL=50)
      PERFORM SET-ALTERNATE-PFKEYS
      ESCAPE ROUTINE
    END-IF
  END-IF
*
* If there are more than 18 lines per page then invoke CD-HELPL
* which obtains obtains 1:50 occurrences. Otherwise invoke CD-HELP
* which only obtains 1:18 lines.
  NEWPAGE
  IF NCST-HELP.HELP-LINES-PER-PAGE GT 18 THEN
    CALLNAT 'CD-HELPL' #HPARM
  ELSE
    CALLNAT 'CD-HELP' #HPARM
  END-IF
*
  PERFORM SET-ALTERNATE-PFKEYS
*
************************************************************************
DEFINE SUBROUTINE SET-ALTERNATE-PFKEYS
************************************************************************
*
  SET KEY PF13='%K1'  PF14='%K2' PF15='%K3' PF16='%K4' PF17='%K5'
  PF18='%K6'  PF19='%K7' PF20='%K8' PF21='%K9' PF22='%K10'
  PF23='%K11' PF24='%K12'
END-SUBROUTINE
**SAG END-EXIT
*
************************************************************************
DEFINE SUBROUTINE INITIALIZATIONS
************************************************************************
*
  ASSIGN #PROGRAM = *PROGRAM
*
END-SUBROUTINE /* INITIALIZATIONS
*
************************************************************************
DEFINE SUBROUTINE FIND-HELP-TEXT
************************************************************************
* Check Help file for existence.
  ASSIGN #LANGUAGE-SPECIFIC-HELP-KEY-1 = #HPARM
  ASSIGN #LANGUAGE-SPECIFIC-HELP-KEY-2 = *LANGUAGE
  FIND-HEADER. FIND NCST-HELP WITH HEADER-MAJOR-MINOR =
      #LANGUAGE-SPECIFIC-HELP-KEY
    ASSIGN #HELP-TEXT-FOUND = TRUE
  END-FIND
  /*
  /* If no help text for current language, check for corresponding help
  /* text member for language 1
  IF NOT #HELP-TEXT-FOUND
    ASSIGN #LANGUAGE-SPECIFIC-HELP-KEY-2 = 1
    FIND-HEADER2. FIND NCST-HELP WITH HEADER-MAJOR-MINOR =
        #LANGUAGE-SPECIFIC-HELP-KEY
      ASSIGN #HELP-TEXT-FOUND = TRUE
    END-FIND
  END-IF
*
END-SUBROUTINE /* find-help-text
*
ESCAPE BOTTOM(PROG.) IMMEDIATE
END-REPEAT  /* PROG.
END
