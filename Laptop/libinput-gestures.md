# Libinput gestures

This is how you call i3-msg from the libinput-gestures config

```
# Browser go back (works only for Xorg, and Xwayland clients)
gesture swipe right     /usr/bin/i3-msg 'workspace next'
```
