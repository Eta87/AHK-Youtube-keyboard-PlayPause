# AHK-Youtube-keyboard-PlayPause

Found this script, after i got problems with Streamskeys, orginal is was shotcut for CTRL + space but i change it to media play pause key.

then i was thinking to share it since i would properly not be the only one to have that problem.

Thanks to harrymc for sharing: https://superuser.com/questions/1531362/control-the-video-behavior-in-another-window-using-the-keyboard

hes orginal post except for me editing the shortcut keys. : 



The following heavily-commented AutoHotkey script was taken from the post Script to Play/Pause Youtube from another Window.

The K character stops/starts YouTube video. This script is triggered by the key combination of Media_Play_Pause. It will find Chrome and its playing tab and send it the K character, all without changing the focused window (which I presume is Word).

After installing AutoHotKey, put the above text in a .ahk file and double-click it to test. You may stop the script by right-click on the green H icon in the traybar and choosing Exit. To have it run on login, place it in the Startup group at C:\Users\USER-NAME\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup.


	;============================== Start Auto-Execution Section ==============================
	
	; Keeps script permanently running
	#Persistent
	
	; Avoids checking empty variables to see if they are environment variables
	; Recommended for performance and compatibility with future AutoHotkey releases
	#NoEnv
	
	; Ensures that there is only a single instance of this script running
	#SingleInstance, Force
	
	;Determines whether invisible windows are "seen" by the script
	DetectHiddenWindows, On
	
	; Makes a script unconditionally use its own folder as its working directory
	; Ensures a consistent starting directory
	SetWorkingDir %A_ScriptDir%
	
	; sets title matching to search for "containing" isntead of "exact"
	SetTitleMatchMode, 2
	
	;sets controlID to 0 every time the script is reloaded
	controlID       := 0
	
	return
	
	;============================== Main Script ==============================
	#IfWinNotActive, ahk_exe chrome.exe
	
	Media_Play_Pause::
	    ; Gets the control ID of google chrome
	    ControlGet, controlID, Hwnd,,Chrome_RenderWidgetHostHWND1, Google Chrome
	
	    ; Focuses on chrome without breaking focus on what you're doing
	    ControlFocus,,ahk_id %controlID%
	
	    ; Checks to make sure YouTube isn't the first tab before starting the loop
	    ; Saves time when youtube is the tab it's on
	    IfWinExist, YouTube
	    {
	        ControlSend, Chrome_RenderWidgetHostHWND1, k , Google Chrome
	        return
	    }
	
	    ; Sends ctrl+1 to your browser to set it at tab 1
	    ControlSend, , ^1, Google Chrome
	
	    ; Starts loop to find youtube tab
	    Loop
	    {
	        IfWinExist, YouTube
	        {
	            break
	        }
	
	        ;Scrolls through the tabs.
	        ControlSend, ,{Control Down}{Tab}{Control Up}, Google Chrome
	
	        ; if the script acts weird and is getting confused, raise this number
	        ; Sleep, is measures in milliseconds. 1000 ms = 1 sec
	        Sleep, 150
	    }
	
	    Sleep, 50
	
	    ; Sends the K button to chrome
	    ; K is the default pause/unpause of YouTube (People think space is. Don't use space!
	    ; It'll scroll you down the youtube page when the player doesn't have focus.
	    ControlSend, Chrome_RenderWidgetHostHWND1, k , Google Chrome
	return
	
	#IfWinNotActive
	
	;============================== End Script ==============================
