### Reading the official Python API docs
For Python in Unreal start with [the official docs](https://docs.unrealengine.com/5.0/en-US/PythonAPI/),
e.g. to get all actors in Unreal:
- I can use the [EditorLevelLibrary docs](https://docs.unrealengine.com/5.1/en-US/PythonAPI/class/EditorLevelLibrary.html)
- The docs tell me that the `get_all_level_actors()` method returns an array of Actors. So I go check out the [docs for Actor](https://docs.unrealengine.com/5.0/en-US/PythonAPI/class/Actor.html?highlight=actor#unreal.Actor).
- The [docs for Actor](https://docs.unrealengine.com/5.0/en-US/PythonAPI/class/Actor.html?highlight=actor#unreal.Actor) show I can run a `get_name()` method
```python
import unreal
actors = unreal.EditorLevelLibrary.get_all_level_actors()
actor_names = [x.get_name() for x in actors]
```
It's recommended to start experimenting line by line, in the Unreal Python console, and just print the results.

### Other 
* A great resource for Unreal Python scripts, tips & tricks: https://unrealcommunity.wiki/
* [[How to make an actor that can Tick In Editor.]]
* [[Tickable PySide2 Window BaseClass]]
