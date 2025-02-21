
建一个下面的文件，然后修改后缀为.reg。然后双击执行

```BASH
Windows Registry Editor Version 5.00  
  
  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_here]  
  
@="open cmd here"  
"Icon"="cmd.exe"  
  
  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\background\shell\cmd_here\command]  
  
@="\"C:\\Windows\\System32\\cmd.exe\""  
 "Icon"="cmd.exe"  
  
  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\cmdPrompt]  
  
@="open cmd here"  
"Icon"="cmd.exe"   
  
  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\cmdPrompt\command]  
  
@="\"C:\\Windows\\System32\\cmd.exe\" \"cd %1\""  
"Icon"="cmd.exe"  
  
  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_here]  
  
@="open cmd here"  
"Icon"="cmd.exe"  

  
  
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\cmd_here\command]  
  
@="\"C:\\Windows\\System32\\cmd.exe\""  

[HKEY_CLASSES_ROOT\Directory\Background\shell\runas]
"ShowBasedOnVelocityId"=dword:00639bc8

 
[HKEY_CLASSES_ROOT\Directory\Background\shell\runas\command]
@="cmd.exe /s /k pushd \"%V\""

```

