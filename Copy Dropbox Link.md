;----------------------------------------------------COPY DROPBOX LINK-----------------------------------------------------

;Welcome to the 'copy dropbox link' shortcut.  If you're running Windows 10 and you have 'copy dropbox link' in the context menu of you explorer, this code should work for you!  I work with dropbox a lot.  Specifically, I find myself needing to copy links and send them off to people in my organization.  It gets to be a pain when you're sending out tens of thousands of files a year using this method.  This code checks the file path of the active windows explorer [GetActiveExplorerPath()], then searches the retreived path [IfInString, DropboxPath, %Dropbox%] for the word dropbox.  If AHK detects you're in your Dropbox folder, it check to see if the highlighted item in the window is a folder or file by checking the file extension.  If there is no file extension, this script assumes you have a folder highlighted and sends the AppsKey to open the context menu, then sends 'l' to engadge the 'copy dropbox link' option.  If the script detects the highlighted item is a file, it sends 'l' twice since the 'copy dropbox link' is the second 'l' option in the context menu, then the script sends 'enter'.  Hope that's clear enough for any of you new guys out there.  I use this script in my job almost everyday.  Enjoy!

^+c::
Dropbox := "E:\Dropbox"
if WinActive("ahk_exe explorer.exe")
GetActiveExplorerPath()
Sleep, 300
DropboxPath := GetActiveExplorerPath()
Sleep, 300
IfInString, DropboxPath, %Dropbox%
	{
	Clipboard = 
	Send ^c
	ClipWait
	splitpath, clipboard,,, ext
	dir := clipboard
	FileGetAttrib, Attributes, % dir
;	MsgBox % ext
	If % ext = ""
		{
		Send, {Appskey}
		Send, l
		Return
		}
	Else
		{
		Send, {Appskey}
		Send, ll{Enter}
		Return
		}
	}
Else
MsgBox, Not Dropbox
Return



;You need the following function somewhere in this script.  I'm no expert in AHK but I have this up near the beginning of the script not enclosed by any braces.  If you read through the hotkey code at the beginning, you'll notice the limitations of this script.  It's just looking for the word "dropbox" in the path.  If you have that word anywhere in the path of your active explorer window, the hotkey will act as if it's in a dropbox folder on your computer.  Modify what is searched for to make this script work for your specific use case.

; https://www.autohotkey.com/boards/viewtopic.php?f=6&t=69925

GetActiveExplorerPath() 
{
    explorerHwnd := WinActive("ahk_class CabinetWClass")
    if (explorerHwnd)
    {
        for window in ComObjCreate("Shell.Application").Windows
        {
            if (window.hwnd==explorerHwnd)
                return window.Document.Folder.Self.Path
        }
    }
}