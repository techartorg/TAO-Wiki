# Selection Sets:
Getting Named Selection Sets is a fairly straightforward procedure for Object Selection Sets, but it's a bit more "difficult" to find how to do it for object mode ones (Vert/Edge/Faces etc), so to save the hassle
**Getting SelSets From object modes (verts/edges/poly/etc)**
This is the "same" just change whatever mode you're looking for
* Get Selection Set names from Faces Sets:
```python
import pymxs
rt = pymxs.runtime
thing = rt.getCurrentSelection()[0]
print(thing.Faces.selSetNames)
#("TestSet", "TestSet2") # example output
```
Obviously then you can select a set by doing something like:
```python
thing.selectedFaces = thing.Faces["TestSet2"]
```

# Get All Materials
Does a check to see if you have multi material on your model, if it does, iterates and tries to do a face select to check if the material is actually on the model and returns a list of material ID's and what faces have it. 
```python
def get_all_mats(model):
    materials = []
    dupe = rt.copy(model)
    rt.resetXForm(dupe)
    rt.collapseStack(dupe)
    material = dupe.material
    if rt.classOf(material) == rt.Multimaterial:
        for matID in xrange(material.numsubs):
            #see if we can select any faces for each material
            dupe.selectByMaterial(matID+1)
            faces = dupe.GetSelection(rt.name("Face"))
            numSelected = faces.numberset
            if numSelected > 0:
                materials.append({material[matID]: numSelected})
    elif rt.classOf(material) == rt.StandardMaterial:
        # if we only have one material we assume that it is on every face
        materials.append({material.name: dupe.mesh.numFaces})
    rt.delete(dupe)
    return materials
```

# EZ Multi Import
Very small multi file importer script. Drives me nuts having to import multiple fbx's etc.
```python
from PySide2 import QtWidgets
import pymxs
rt = pymxs.runtime

def main():
    dialog = QtWidgets.QFileDialog()

    files = dialog.getOpenFileNames(None, "Multi Importer", "", "FBX (*.fbx);; OBJ (*.obj);; All Files (*.*)")
    for f in files[0]:
        rt.importFile(f, rt.name("noPrompt"))


if __name__ == "__main__":
    main()
```

# Get all Children + define specific type of child to get

Provide a node as a parent to search down from and optionally give a node type in Max to search for such as Lights, Geometry, FFD's etc. Get back a list of the children 
```python
def get_all_children(parent, node_type=None):
    """Handy function to get all the children of a given node

    Args:
        parent (3dsmax Node1): node to get all children of
        node_type (None, runtime.class): give class to check for
                                         e.g. rt.FFDBox/rt.GeometryClass etc.

    Returns:
        list: list of all children of the parent node
    """
    def list_children(node):
        children = []
        for c in node.Children:
            children.append(c)
            children = children + list_children(c)
        return children
    child_list = list_children(parent)

    return ([x for x in child_list if rt.superClassOf(x) == node_type]
            if node_type else child_list)
```

# Is Ancestor

Check back up the tree from a node to see if a given named node is an ancestor
```python
def is_ancestor(node, name):
    """Given the node, is there an ancestor named "name"

    Args:
        node (max Node): node to check for an ancestor on
        name (string): name of the ancestor to check for

    Returns:
        bool: True if the node is an ancestor
    """
    found = False
    top = False
    curr_ancestor = node.parent
    if curr_ancestor.name == name:
        return True

    while curr_ancestor.parent is not None:
        curr_ancestor = curr_ancestor.parent
        if curr_ancestor.name == name:
            return True
    return False
```