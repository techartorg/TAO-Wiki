### Why not use an Editor Utility Actor?
Editor Utility Actors don't quite get you the exact functionality that seems useful enough for ticking in the editor via an actor. It will tick in the preview windows of blueprint and potentially lead to lots of head scratching as you realize that the blueprint you were working on is logging when you are trying to use it in other places. Also, there are times where you actually do want the same actor runtime as well (It's rare but totally a valid case).

### The Solve.. Make your own actor class!
In your custom actor class you can use this to add a bool that toggles being able to tick in the editor. This way whenever you need this you can just simply check it on and get going. It also is separated so that you can have different logic happen in the editor vs in-game. Most of the time I end up with 1-1 anyway but its nice to have the option. 


header file
```c++
/** Allows Tick To happen in the editor viewport*/
virtual bool ShouldTickIfViewportsOnly() const override;

UPROPERTY(BlueprintReadWrite, EditAnywhere)
bool UseEditorTick = false;

/** Tick that runs ONLY in the editor viewport.*/
UFUNCTION(BlueprintImplementableEvent, CallInEditor, Category = "Events")
void BlueprintEditorTick(float DeltaTime);

```

cpp file
```c++
// Separated Tick functionality and making sure that it truly can only happen in the editor. Might be a bit overkill but you can easily consolidate if you'd like. 
void YourActor::Tick(float DeltaTime)
{
	if (GetWorld() != nullptr && GetWorld()->WorldType == EWorldType::Editor)
	{
#if WITH_EDITOR
		BlueprintEditorTick(DeltaTime);
#endif
	}
	else
	{
		Super::Tick(DeltaTime);
	}
}

// This ultimately is what controls whether or not it can even tick at all in the editor view port. But, it is EVERY view port so it still needs to be blocked from preview windows and junk.
bool YourActor::ShouldTickIfViewportsOnly() const
{
	if (GetWorld() != nullptr && GetWorld()->WorldType == EWorldType::Editor && UseEditorTick)
	{
		return true;
	}
	else
	{
		return false;
	}
}

```

Then when you are done you should be able to add the new BlueprintEditorTick to your event graph and get rolling!

![](https://media.discordapp.net/attachments/559801353073852433/695677686160556042/unknown.png)

### Final Notes
This is pretty dangerous if you aren't mindful with what nodes you use in here. 
Try not to do things like Add Component or any other nodes that would spawn objects into the world or if you do make sure you store them and clean them up. 
Delays might work but they could be a bit odd as well.
Just be aware that it can be fragile at times so make sure to give unreal some cake beforehand... ❤️ 

