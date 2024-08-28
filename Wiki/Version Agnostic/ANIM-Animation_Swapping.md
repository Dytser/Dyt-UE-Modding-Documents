___
# How Unreal handles animations.
Understanding how unreal handles animations is rather crucial in order to be able to effectively handle animation swapping.

Unreal Engine will effectively compile our animations information down to an array that keeps track of the bone based on their index during import into unreal.

Unreal takes the models and will give each bone an index starting at index 0 which almost always will be your root bone. 
Then for each bone in the list it will increment this by 1, 

So the bone named "Spine_01" is not stored as that name when the game then looks at the animation and plays it, instead it would be named "Bone_02" which normally is not a problem but it does become one when we are trying to mod animations.


# What happens when we cook the animations?
Because of the fact that the bones become indexed when we cook our animations they will be specifically referencing their index and the position of that bone.
While this can be fine if you happen to have a small skeleton which exports exactly the same way as it shows in the index (this is rare and likely not gonna happen).

The more common case is that when we export our models from things such as Fmodel it likely will contain things such as sockets and more which are not actually found on the bone index of the skeleton for that skeletal mesh.

Meaning for us to be able to effectively export our animations we need to recreate this bone index by making an armature that by all accounts will be completely scuffed BUT correctly cook the animations.

___
# Setting up a Bone Index Armature
In order to properly do this i suggest 3 tools for the job.
### Tools
**F Model**
**Visual Studio Code/Text editor of your choice**
**Blender**
**Unreal Engine** (Big reveal i know!)

## Procedure
### Create your BoneIndex text file
1. Open the game using F model and navigate to the model you aim to export and find its skeleton.
2. Right click the skeleton and choose "Save Properties (.json)"
3. Open said Json file in Visual Studio Code (or any preferred text editor)
4. Find the section named **"FinalNameToIndexMap"**
5. Now select the line that final name to index is on.
	1.  Go to Selection
	2.  Choose Expand Selection
	3. This should now have selected the entire bone index section of the code.
	4. Do Ctrl+X to cut
	5. Ctrl+ A to select all
	6. Backspace to delete
	7. Ctrl + V to paste the bone index back
6. You can now save your file, Congratulations this is your bone index you need to recreate.


### Recreate the BoneIndex in blender
Remember how unreal imports skeletons, it attributes a bone index number for each bone the way they end up showing up on the skeleton. 
So in order to force this index to be correct according to the bone index we exported from the games skeleton with the JSON file above we need to manually parent each bone one after the other so they appear like a long list the way the index says.

This is the time consuming part, You need to now parent each bone to the the previous one in the chain just the way the bone index is saying.

![Example of bone index recreated](/Assets/Version-Agnostic/ANIM-Bone_Index.png)

While this is not a big problem for smaller skeletons (smaller than 100-150 bones).
Some games has a single human skeleton where all the models have bones on which means these skeletons can easily balloon up to sizes 1000 bones big.