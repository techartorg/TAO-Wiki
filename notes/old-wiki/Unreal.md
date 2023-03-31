### Reading the official Python API docs
for Python in Unreal start with the official docs: https://docs.unrealengine.com/5.0/en-US/PythonAPI/.
e.g. to get all actors in Unreal:
- I can use the [EditorLevelLibrary docs](https://docs.unrealengine.com/5.1/en-US/PythonAPI/class/EditorLevelLibrary.html)
- the get_all_level_actors returns an array of Actors. Both of them have docs too.
- e.g. the [docs for Actor](https://docs.unrealengine.com/5.0/en-US/PythonAPI/class/Actor.html?highlight=actor#unreal.Actor) show I can run a `get_name()` method
```python
import unreal
actors = unreal.EditorLevelLibrary.get_all_level_actors()
actor_names = [x.get_name() for x in actors]
```

### Other 
* A great resource for Unreal Python scripts, tips & tricks: https://unrealcommunity.wiki/
* [How to make an actor that can Tick In Editor.](https://github.com/techartorg/TAO-Wiki/wiki/%5BUnreal%5D-How-to-make-an-actor-that-can-Tick-In-Editor)
* [Tickable PySide2 Window BaseClass](https://github.com/techartorg/TAO-Wiki/wiki/Tickable-PySide2-Window-BaseClass)
