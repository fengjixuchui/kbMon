# kbMon 
windows 10 compatible<br>
Kernel Mode, driver only, ring O, remote UDP keylogger. 

# Application Security
i did not implemented any IoCtl, in order to avoid any Security Issues. in addition, i did not implemented any Revc-datagram method's for this exact same reason, however if you use this driver & see any security problems, please report them here as an issue.

# Note: 
Using this software is at your Own Risk.
The Author will not be held responsible by any circumstances.

# Tech
this project splits down into two parts:<br>
1 KeyBoard hook.<br>
2 Raw Networking (datagram socket manipulation).<br>

to monitor the key strokes we need to get in beetwin the keyboard device IRP & the PS/2 port.<br>
Much of the KeyBoard Hook implementation & code is borrowed from <html><a href="https://github.com/fdiskyou">fdiskyou</a></html>.<br>
while to implement this 'kernel man in the middle', we need to mimic the IRP function passed down from the physical device up to the operating system processing; this is done by installing a hook beetwin the keyboard device and passing down each IRP request to the next implementation level.<br><br>
![](pic/keyboard-driver-stack.png)<br><br>
Our hook (by the diagram above) will come in beetwin win32k.sys & KBDHID.sys, each request is cached by our hook, proccessed by Our driver and passed up to the next irql.<br>
The Second part of Our driver Operation is to log the keystrokes & send them back to our monitoring server.<br>
i have implemented a UDP-DataGram protocol, as we do not recieve or handle any data coming back from the server, and this also make's the monitoring process a lot simpler by the server side.<br>
Another Advantage to a udp implementation is that the port can be closed and opened constantly to make the #dfir work a lot harder, and udp is not a stream based connection so you can avoid traffic logs etc'.<br>
to implement that i made use of the <html><a href="https://msdn.microsoft.com/library/windows/hardware/ff571083">Wsk</a></html>, (windows socket kernel), as to avoid any user-mode application.<br>

# Note:
you are encouraged to look at the source code your self.<br> 
so you can implement networking and hooks as you wish at the kernel level.
<html> <a href="https://github.com/akayn/kbMon/blob/master/Iliad/src/Driver.c">the major code and main logic</a><html>.<br><br>

# Usage
currently only the local keylogger is Generic and can be used W/O building the driver (as it simply logs the keystroke's to C:\\Windows\Logs\HomeGroup\klog.txt), but the remote udp based (that do not need to write any data to disk to run), needs to be build to your server address (or any other solution. this can be done with very minor code modifications. e.g let the driver read from the registry your ip and load it in /src/driver.c line 420).
# Install:
The driver is not signed, so you will have to disable code integrity:<br>
(From an elevated command prompt):<br>
  bcdedit /set testsigning on<br>
  shutdown /r -f -t 00<br><br>

  sc create kbMon type=kernel binpath="\path\to\your\driver.sys"<br>
  sc start kbMon<br>
# Uninstall:
  bcdedit /set testsigning off<br>
  sc stop kbMon<br>
  shutdown /r -f -t 00<br>
# if you encounter any problems simply restart your computer.
For any bugs comment an issue in this github repo.
enjoy!<br>

