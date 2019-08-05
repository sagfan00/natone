* >Natural Source Header 000000
* :Mode S
* :CP
* :LineIncrement 10
* <Natural Source Header
**********************************************************************
* Program    : CDDCHELP
* System     : NATURAL-CONSTRUCT
* Application: All which need Direct Command line Help.
* Description: This helproutine decides what type of help is to
*            : be displayed for the direct command line.
*            :
*            : If a Natural Command Processor is currently active
*            : then help for the commands provided by the Natural
*            : Command Processor will be displayed.
*            :
*            : Otherwise help text for standard use of direct
*            : commands will be displayed.
*            :
* History
**********************************************************************
*
DEFINE DATA
  PARAMETER
  01 #DIRECT-CMD-LINE(A60)
  LOCAL USING CDNCPHA   /* Used to display Command Processor Help.
  LOCAL
  01 #HPARM(A65)
  01 REDEFINE #HPARM
    02 #TYPE(A1)
    02 #MAJOR(A32)
    02 #MINOR(A32)
    02 REDEFINE #MINOR
      03 #TWOCHAR(A2)
  01 #PASSIVE-HELP(L)   /* True if only Help Text is to be displayed.
END-DEFINE
*
CALLNAT 'CDNCPH' CDNCPHA
IF NOT CDNCPHA.#NCP-ACTIVE THEN
  /*
  /* No Natural Command Processor is active, so default help to
  /* passive Direct Command Help text.
  ASSIGN #PASSIVE-HELP = TRUE
ELSE
  ASSIGN #DIRECT-CMD-LINE = CDNCPHA.#DIRECT-COMMAND
END-IF
*
IF #PASSIVE-HELP = TRUE
  ASSIGN #HPARM.#TYPE  = 'F'
  ASSIGN #HPARM.#MAJOR = 'CS'
  ASSIGN #HPARM.#MINOR = 'NCST-DIRECT-COMMAND'
  CALLNAT 'CD-HELP' #HPARM
END-IF
*
END
