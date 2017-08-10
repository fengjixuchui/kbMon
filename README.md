# Keylog
windows 10 compatible<br>
Kernel Mode, driver only, ring O, remote UDP keylogger. 

# Tech
# this project splits down into two parts:<br>
# 1 KeyBoard hook.
# 2 Raw Networking (datagram socket manipulation).
# 1
to monitor the key strokes we need to get in beetwin the keyboard device IRP & the PS/2 port.<br>
Much of the KeyBoard Hook implementation & code is borrowed from <html><a href="https://github.com/fdiskyou">fdiskyou</a></html>.<br>

