# flair-examp
directory fluka\flair project
I tried computing 1)Energy deposited 2) Dose and 3) dose-Eq
!
! Program Celsius Table: Prints simple Fahrenheit-Celsius table
!
	program celsius_table
      USE DIALOGM
      IMPLICIT NONE
      INCLUDE 'resource.FD'
      TYPE (DIALOG)  dlg
	real           Fahrenheit, Celsius
	INTEGER        retint
	LOGICAL        retlog
	EXTERNAL       UpdateTemp 
! Initialize.
      IF ( .not. DlgInit(IDD_DIALOG1, dlg ) ) THEN
       WRITE (*,*) "Error: dialog not found"
      ELSE
! Set up temperature controls.
      retlog = DlgSet( dlg, IDC_SCROLLBAR1, 200, dlg_range)
      retlog = DlgSet( dlg, IDC_EDIT1, "100" )
      CALL UpdateTemp( dlg, IDC_EDIT1, dlg_change)
      retlog = DlgSetSub( dlg, IDC_EDIT1, UpdateTemp )
      retlog = DlgSetSub( dlg, IDC_EDIT2, UpdateTemp )
      retlog = DlgSetSub( dlg, IDC_SCROLLBAR1, UpdateTemp )
!  Activate the dialog.
      retint = DlgModal( dlg )
!  Release dialog resources.
      CALL DlgUninit( dlg )
      END IF 

	CALL DLGEXIT(dlg)

	print *,'  Fahrenheit     Celsius'	
	print *,'--------------------------'
	do Fahrenheit = 30, 400, 10
		Celsius = (5.0/9.0) * (Fahrenheit-32.0)
		print '(F13.0,F12.3)',Fahrenheit,Celsius
	end do
	end	program celsius_table
      SUBROUTINE UpdateTemp( dlg, control_name, callbacktype )
      USE DIALOGM
      IMPLICIT NONE
      TYPE (dialog) dlg
      INTEGER control_name
      INTEGER callbacktype
      INCLUDE 'resource.FD'
      CHARACTER(256)  text
      INTEGER cel, far, retint
      LOGICAL retlog
! Suppress compiler warnings for unreferenced arguments.
      INTEGER local_callbacktype
      local_callbacktype = callbacktype

      SELECT CASE (control_name)
      CASE (IDC_EDIT1)
! Celsius value was modified by the user so 
! update both Fahrenheit and Scroll bar values.
      retlog = DlgGet( dlg, IDC_EDIT1, text )
      READ (text, *, iostat=retint) cel
      IF ( retint .eq. 0 ) THEN
           far = (cel-0.0)*((212.0-32.0)/100.0)+32.0
           WRITE (text,*) far
      retlog = DlgSet( dlg, IDC_EDIT2, TRIM(ADJUSTL(text)))
      retlog = DlgSet( dlg, IDC_SCROLLBAR1, cel,dlg_position)
      END IF
      CASE (IDC_EDIT2)
! Fahrenheit value was modified by the user so 
! update both celsius and Scroll bar values.
        retlog = DlgGet( dlg, IDC_EDIT2, text )
        READ (text, *, iostat=retint) far
      IF ( retint .eq. 0 ) THEN
           cel = (far-32.0)*(100.0/(212.0-32.0))+0.0
           WRITE (text,*) cel
           retlog = DlgSet( dlg, IDC_EDIT1, TRIM(ADJUSTL(text)) )
      retlog = DlgSet(dlg, IDC_SCROLLBAR1, cel, dlg_position)
      END IF
      CASE (IDC_SCROLLBAR1)
! Scroll bar value was modified by the user so 
! update both Celsius and Fahrenheit values.
      retlog = DlgGet( dlg, IDC_SCROLLBAR1, cel,dlg_position)

                          far = (cel-0.0)*((212.0-32.0)/100.0)+32.0
       WRITE (text,*) far
       retlog = DlgSet( dlg, IDC_EDIT2, TRIM(ADJUSTL(text)) )
       WRITE (text,*) cel
       retlog = DlgSet( dlg, IDC_EDIT1, TRIM(ADJUSTL(text)) )
      END SELECT
      END SUBROUTINE UpdateTemp
