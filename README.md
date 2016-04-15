#NOTE: Before trying to use this for anything, have a look at [Hogar](https://github.com/leonjza/hogar) first.

![](http://i.imgur.com/3VQGAwl.png)

A pure python telegram-cli interface to send [Telegram](https://telegram.org/) messages.

## introduction
In short, PyTel interfaces with the [telegram-cli](https://github.com/vysheng/tg) allowing you to send telegram messages from python code as simple as `telegram.send_message('Bob', 'Hello bob!')`

## installation
The most complex part of the installation is getting the actual `telegram-cli` client up and running. `telegram-cli` will always have to run and open a socket connection for PyTel to connect to.

 - Follow the installation instructions for `telegram-cli` found [here](https://github.com/vysheng/tg#installation). We are assuming that you installed `tg` in `/home/telegram` for the purposes of this guide. This of course can be anywhere.
 - Start `telegram-cli`. The syntax will depend on which socket type you want to use.
   - Unix Domain Sockets: `/home/telegram/tg/bin/telegram-cli -k /home/telegram/tg/tg-server.pub -W -S /tmp/tg.sock`
   - TCP socket on port 2361: `/home/telegram/tg/bin/telegram-cli -k /home/telegram/tg/tg-server.pub -W -P 2361`
 - With the `telegram-cli` client ready, ensure that you provide the mobile number and password to login. This is the identity that will be used for messages.
 - Install PyTel with `pip install pytel`
 - Done!

## example
Usage is really simple, check it out!

```python
from pytel import tg

telegram = tg.Telegram('unix:///tmp/tg.sock') # For Unix Domain Socket
# telegram = tg.Telegram('tcp://127.0.0.1:4444') # For TCP socket

message = 'Dude! Whats mine say?'
telegram.send_message('Some Dude', message)

# We can also send an image! Of course, the image needs to be readable
# by the telegram-cli instance.
telegram.send_image('Some Dude', '/tmp/image.png')
```
## troubleshooting
If you are working on ArchLinux with an armv7, there is a problem with libraries that import PyTel:
```
Traceback (most recent call last):
  File "pytel-leonjza.py", line 1, in <module>
    from pytel import tg
  File "/usr/lib/python3.5/site-packages/pytel/tg.py", line 23, in <module>
    from utils import strings
ImportError: No module named 'utils'

```
I solved it installing "utils" with pip
```
pip install utils



Collecting utils
  Downloading utils-0.9.0.tar.gz
Installing collected packages: utils
  Running setup.py install for utils ... done
Successfully installed utils-0.9.0
```
Now when I run the example, it gives me this error:
```
Traceback (most recent call last):
  File "pytel-leonjza.py", line 1, in <module>
    from pytel import tg
  File "/usr/lib/python3.5/site-packages/pytel/tg.py", line 23, in <module>
    from utils import strings
ImportError: cannot import name 'strings'

```
So I tried to install "strings" using pip but I have the following error:
```
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-hsb7uu5z/strings/
```
