Little class for programs that need a "I'm not frozen" animation for console printing
Has an animated character in the console like ->    | / - \
It should be noted that you use `cursor_anim.start()` to start it and `cursor_anim.stop(bool)` to stop it

**Do not use the run function to start it as it will just loop forever** (and that is called by the `cursor_anim.start()`

Setup is like 
```python
cursor_anim = CursorAnimation()
cursor_anim.wip_msg = "Doing a thing that takes a while..."
cursor_anim.success_msg = "Successfully did the long thing!"
cursor_anim.fail_msg = "Failed to do the thing!"
cursor_anim.start()
# do long code

# after code is done or if code has failed
# if True is passed in stop that means you've done it successfully 
# and the success_msg will be printed, else if False, the fail_msg
cursor_anim.stop(True)
```



```python
import threading
import time

class CursorAnimation(threading.Thread):

    """Threaded class to provide an animation in the console while syncing different parts/Files
    
    Attributes:
        fail_msg (str): Message to print at the end when Stop is called with False
        success_msg (str): Message to print at the end when Stop is called with True
        wip_msg (str): Message to print while doing the task
    """

    def __init__(self):
        self.flag = True
        self.animation_char = "|/-\\"
        self.idx = 0
        threading.Thread.__init__(self)
        self.wip_msg = "DOING STUFF"
        self.success_msg = "DID THE THING!"
        self.fail_msg = "WE DID NOT DO THE THING!"
        self.max_len = 0

    def run(self):
        self.max_len = len(max([self.wip_msg, self.success_msg, self.fail_msg], key=len)) + 1

        #skip to a new line so we know we're not going to get wonky print animations
        print("\n")
        while self.flag:
            print("{}{}".format(self.wip_msg, self.animation_char[self.idx % len(self.animation_char)]).ljust(self.max_len, " "), end="\r")
            self.idx += 1
            time.sleep(0.1)

    def stop(self, success):
        """success is a bool to trigger either success or fail message
        """
        self.flag = False
        if success:
            print("{}".format(self.success_msg.ljust(self.max_len, " ")), end="\r")
        else:
            print("{}".format(self.fail_msg.ljust(self.max_len, " ")), end="\r")
```
## Backlinks
* [[Python3 Snippets]]
	* [[Console-Threaded-WIP-animation]]

