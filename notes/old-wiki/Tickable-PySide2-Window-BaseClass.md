Using the below baseclass, You can have a tickable window for PySide2 UI
To subscribe to the tick, it should be something like
```python
self.sig_tick.connect(self.my_func)
```
# BASE CLASS
```python
import unreal
from PySide2 import QtWidgets
from PySide2.QtCore import Signal

class QtUE_Window(QtWidgets.QMainWindow):
    # Signal that can be conneted to for tick functionality
    sig_tick = Signal(float)
    def __init__(self, parent=None):
        super(QtUE_Window, self).__init__(parent)
        self.tick_handle = unreal.register_slate_post_tick_callback(self.__QtAppTick__)
    
    def __QtAppTick__(self, delta_seconds):
        # This function will receive the tick from Unreal
        QtWidgets.QApplication.processEvents()
        self.sig_tick.emit(delta_seconds)

    def __QtAppQuit__(self):
        # This function will be called when the application is closing.
        unreal.unregister_slate_post_tick_callback(self.tick_handle)

    def closeEvent(self, event):
        super(QtUE_Window, self).closeEvent(event)
        self.__QtAppQuit__()
```
