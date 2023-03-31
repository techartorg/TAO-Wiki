_Back to [[Houdini]]_

The naming convention of .hda and .shelf are based on the SideFX versioning architecture as described here.
http://www.sidefx.com/docs/houdini/assets/namespaces.html  

For a .hda and a .shelf there are four major names you need to know about:
* The filename on disk  - company.context__assetname__1.0.hda
* The operator name     - company::assetname::1.0
* The operator label    - Asset Name
* Tab submenu entry     - company\"collection"
Directory for HDAs & Shelfs - $HOUDINI_PATH/otls $HOUDINI_PATH/toolbar

The operator name convention is the most important. 

First is the namespace as identified by "company" in our case this represent the different studios. In some cases this could represent a codename of a project when a studio or group can not be identified at that time. This prevents libraries of hda from clashing when built at different studios. Also allowing external libraries to be used. You can even use this to define teams within the same project.

Second is the assetname it is the non space, non camel case, lower case name of your tool. For the label this is First word capitalized space separated asset name. The label is the human readable version.

Third is the major and minor version of your asset. The major version is a number that represent a significant non-backwards compatible change to the hda. If you rework the node 100% from scratch or change a significant function of the node you update the major number. A minor version is an additive process like an additional parameter or a bug fix would not over haul. It is important not to get this confused with source control. Edits and fixes are just check in and check out of the same .hda. If you have an .hda deployed in your system and you don't want to break everyones production tools this is a good reason to major or minor version control.

The double colon :: is the separate buffer used only in the operator name. For the file on disk this gets converted to an underscore. In advanced pipeline you can use a modified http file structure to store assets based on this.

The context in the file on disk is inherent in the operator name of the file so it does not need to be include. It is included in the file on disk name to separate .hdas based on their internal structures which are unique. Additionally you can share the same asset name of a file in different context. This helps in scripting and OPcustomize when you do not have to load up houdini to see the name of the .hda.

It is important to store one .hda or one .shelf tool per a file. This allows multiple people to work on separate tools at a time. This is very important for source control.

The tab submenu entry is how you organize your tools in the tab menu. You do not need to add extra modifiers to the asset name to compensate for this. Under the company name you can add an extra folder for any collection of tools like "Import", "Engine", "Terrain" etc. We do not put them into the regular submenu entries because we would not be able to find our own tools. If a asset name collides this submenu entry will be bracketed () to tell you the difference.

Further when creating your asset libraries use a standard $HOUDINI_PATH folder structure don't be original. This allows us to append multiple studios and libraries tools together. Like the gameshelf. Place all .hda in /otls and shelfs in /toolbar etc. 

Additionally do not save your .hda in exploded mode via the hotl.exe. In production environments all these separated files can cause havoc when they get out of sync on disk. You can work with the .hda like this but compile them about before distribution in your studio.

For an example of folder structure you can look here:
https://github.com/LaidlawFX/LaidlawFX 
https://github.com/sideeffects/GameDevelopmentToolset
or within 

_Back to [[Houdini]]_
## Backlinks
* [[Houdini]]
	* [[Houdini-HDA-Naming-Conventions]] without a SQL database.

