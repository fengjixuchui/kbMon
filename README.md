# Keylog
windows 10 compatible<br>
Kernel Mode, driver only, ring O, remote UDP keylogger. 

# Tech
this project splits down into two parts:<br>
1 KeyBoard hook.<br>
2 Raw Networking (datagram socket manipulation).<br>

to monitor the key strokes we need to get in beetwin the keyboard device IRP & the PS/2 port.<br>
Much of the KeyBoard Hook implementation & code is borrowed from <html><a href="https://github.com/fdiskyou">fdiskyou</a></html>.<br>
while to implement this 'kernel man in the middle', we need to mimic the IRP function passed down from the physical device up to the operating system processing; this is done by installing a hook beetwin the keyboard device and passing down each IRP request to the next implementation level.<br>
![](pic/keyboard-driver-stack.png)<br>
Our hook (by the diagram above) will come in beetwin win32k.sys & KBDHID.sys, each request is cached by our hook, proccessed by Our driver and passed up to the next irql.<br>

