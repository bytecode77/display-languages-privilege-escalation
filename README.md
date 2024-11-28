# Display Languages Volatile Environment LPE

| Exploit Information |                                   |
|:------------------- |:--------------------------------- |
| Date                | 28.06.2017                        |
| Patched             | Windows 10 RS3 (16299)            |
| Tested on           | Windows 7-10, x86 and x64         |

## Description

In lpksetup.exe (Display Languages), there is a DLL loading vulnerability that can be exploited by injecting an environment variable.

A load attempt to %SYSTEMROOT%\System32\ + either "rsaenh.dll" or "comsvcs.dll" (depending on OS version) will be performed from this auto-elevated process.

Redirecting %SYSTEMROOT% can be achieved through Volatile Environment. For this, we set HKEY_CURRENT_USER\Volatile Environment\SYSTEMROOT to a new directory, which we then populate with our hijacked payload DLL, along with *.clb files from C:\Windows\Registration as they are loaded from our new directory as well.

Then, as we execute lpksetup.exe, it will load our payload DLL and also the COM+ components. We need to copy those, too, because the process will otherwise crash. Note, that we need to specify the runas verb, as the executable's requestedExecutionLevel is asInvoker.

Our DLL is now executed with high IL. In this example, Payload.exe will be started, which is an exemplary payload file displaying a MessageBox.

## Expected Result

When everything worked correctly, Payload.exe should be executed, displaying basic information including integrity level.

![](https://bytecode77.com/images/pages/display-languages-privilege-escalation/result.webp)

## Downloads

Compiled binaries with example payload:

[![](http://bytecode77.com/public/fileicons/zip.png) DisplayLanguagesVolatileEnvironmentLPE.zip](https://downloads.bytecode77.com/DisplayLanguagesVolatileEnvironmentLPE.zip)
(**ZIP Password:** bytecode77)

## Project Page

[![](https://bytecode77.com/public/favicon16.png) bytecode77.com/display-languages-privilege-escalation](https://bytecode77.com/display-languages-privilege-escalation)