# StructureSaverInterfaces
Public interfaces for Structure Saver.

These interfaces allows third party mods to integrate themselves with Structure Saver.

For a tutorial about interfaces, check ZenRowe's [Wildcard Workshop #11: Blueprint Interfaces](https://survivetheark.com/index.php?/forums/topic/352467-wildcard-workshop-11-blueprint-interfaces/).

## Setting up

The public interface definition file can be found from (StructureSaver2/)
Download the interfaces you want to use, and copy them into your devkit under Mods/StructureSaver2/ -directory.

In your blueprint(s), add the interface into the Implemented Interfaces list, which can be found in the Components tab -> Edit Blueprint Options. Then imlement the functions and event handlers.

If the interface can not be found from the list when adding, try opening the interface file first.


## Interfaces

### ISaveableStructure_SS
This interface allows saving and restoring structure specific states, and perform some additional initialization upon restoration.

Example: https://steamcommunity.com/sharedfiles/filedetails/?id=1828127796


**StructureSave**

Inputs: 

Returns: JsonObject data

This is called when the structure is being saved. It should return a JSON object containing all the data that is needed when restoring the same structure. The contents can be almost anything, but references to other objects will not work, the JSON object is serialized and saved as a string.


**StructureRestore**

Inputs: JsonObject data

Returns:

This is called after the structure has been restored. The function will be given a JSON object containing the same data that was saved. What you do with that is up to you. You can also do extra initialization here, but for this function to be called, the JSON object must have been created and passed on in the *StructureSave* function.




### IStructureSaverMiddleware_SS
With this interface a third party mod can act as a bridge between Structure Saver and other mod(s) to provide support for saving and restoring structures located in the other mod(s).

Example: https://steamcommunity.com/sharedfiles/filedetails/?id=1886446337


**StructureSave**

Inputs: PrimalStructure structure

Returns: JsonObject data

This is called when the structure is being saved. It should return a JSON object containing all the data that is needed when restoring the same structure. The contents can be almost anything, but references to other objects will not work, the JSON object is serialized and saved as a string.


**StructureRestore**

Inputs: PrimalStructure structure, JsonObject data

Returns:

This is called after the structure has been restored. The function will be given a JSON object containing the same data that was saved. What you do with that is up to you. You can also do extra initialization here, but for this function to be called, the JSON object must have been created and passed on in the *StructureSave* function.


**StructureRestoreBeforeItems**

Inputs: PrimalStructure structure, JsonObject data

Returns: 

This is called after the structure is restored but before the inventory items for the structure are restored. The JSON object contains the data that was given by *StructureSave*.


**IsStructureSaveable**

Inputs: PrimalStructure structure

Returns: Boolean is_saveable

Return whether the given structure can be saved. Must return True for unknown (to this middleware) structures.


**IsStructureManaged**

Inputs: PrimalStructure structure

Returns: Boolean is_managed

Return True if this middleware manages saving and restoring of the given structure.


**IgnoreGroundCheckForStructure**

Inputs: PrimalStructure Class structure_class

Returns: Boolean ignore_ground_check

Return True if the ground check for the given structure should be ignored when restoring.



### Restoring

This is still considered beta, changes to the interface(s) are more than likely:

Files: IRestorer_SS, IRestoringCallback_SS, IStructureSaverManager_SS, Enum_TribeRankSource_SS

IStructureSaverManager_SS = manager = a singleton you obtain first and use for some global stuff

IRestorer_SS = restorer actor = an actor performing the restoring related operations, you can create them by using a function in the manager

IRestoringCallback_SS = callback object = your actor object that has the callbacks defined, you hand reference to this object when creating the restorer actor (it's mandatory)
