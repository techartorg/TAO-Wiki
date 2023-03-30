This is when Bary loses his way home after a drink and can't seem to find his way back.

## Typical use
Barycentric Coordinates, as typically used in the industry, is a 2 components coordinate system that defines a position within a triangle. They are often returned as results of intersection operation on mesh or triangles, or for interpolation purposes.

## The boring stuff
Let `T` be a triangle defined by it's points `P0`, `P1` and `P2`  
The barycentric coordinates `U` and `V` defines a unique position `X` within that triangle with:  

`X = P0*W + P1*U + P2*V`  
where `W = 1-U-V`  

or  
 
`X = P0 + (P1-P0)*U + (P2-P0)*V`

## Backlinks
* [[Glossary of Terms]]
	* [[Barycentric Coordinates]]

