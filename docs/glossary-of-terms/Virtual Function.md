> The 'parties' we had to endure all throughout the pandemic

A function that can be overridden in a subclass. This allows different subclasses to do different things when calling a function with the same signature (name and parameters) from a base class pointer.

In C++ it is defined using the `virtual` keyword (before the declaration), which is only required for the topmost base class definition - subclasses make redefinitions of the same function virtual automatically, however it is recommended to mark it with the `override` keyword (at the end of the declaration) to catch mistakes.

The C++ compiler cannot see through virtual function calls and therefore will not optimize them so for performance reasons they should be avoided in inner loops if possible. When processing data, it should often be possible to pass many values for processing instead of one per call.

Virtual functions work by having each instance of a class refer to a table (array) of all such functions and use that table to find the right function to call.

It is possible to define abstract virtual functions - functions for which there is no definition at the base class level.

Note that virtual functions defined in subclasses will not be callable from a constructor or destructor of a base class. Pure virtual call errors happen when attempting to call abstract virtual functions from a constructor or destructor of a class where it is not yet defined.
## Backlinks
* [[Glossary of Terms]]
	* [[Virtual Function]]

