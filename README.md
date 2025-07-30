# gnome-notify-python-server
The motivation for this repo is to have a very lean server-client implementation in python that works with GNOME, the Python Notify lib and systemd.

### The problem this repo tries to solve

By running `notify-send -a test "test notification" -a someapp` twice the notifications are not stacked. This is annoying as it does not take advantage of grouped notifications that can be used in GNOME 4x-something.

<img width="538" height="273" alt="image" src="https://github.com/user-attachments/assets/af71928e-c8ab-4a16-bfc9-bb0adc7f1cd3" />

What I'd like to achieve is this:

Grouped, closed:

<img width="537" height="153" alt="image" src="https://github.com/user-attachments/assets/db047a82-f7bf-44d0-ab33-d080dafd00a9" />

Grouped, expanded:

<img width="537" height="323" alt="image" src="https://github.com/user-attachments/assets/0e363e33-d386-410c-a307-da14e0812f89" />

Of course it was developed to workaround `neomutt`. What'd you expect? :D

### Solution

So how do you get around it? You have to have an app running that kinda "groups" things together by firing notifications from within the app itself, and if you don't (like in the case of `neomutt` which leverages on `notify-send`) you are getting your notifications ungroped. Now, if you start a service that will run as a notification server, an app will be running as part of it and it will take care of sending notifications to the system once it receives the data from the client.

This is how you start the server:

```bash
systemctl --user enable --now notify_server@appname
```

And this is how you send a notification by communicating to the server:

```bash
python notify_client.py -a appname -i /icon/path summary "body of the notification"
```
