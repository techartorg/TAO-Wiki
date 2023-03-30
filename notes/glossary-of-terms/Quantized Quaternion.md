> This is when there is a tasty quater-ion and you only have so much of it.

Quantization = reducing the number of bits used by a number. A quantized quaternion is a quaternion that has its components quantized to take up less space in memory, typically to reduce network bandwidth or the size of animation data stored on disk or in memory.

Normally quaternions are stored as 32-bit floating point numbers but this precision is not always needed.
Since all components of a normalized (rotation-only) quaternion can only be in the -1..1 range, it is possible to take each component and move them for example to the 0..255 range through addition and multiplication, and then round them to an integer value, losing the decimal digits, which allows each component to only take a single byte.
This lets the quaternion take 4 bytes total, instead of the original 16 bytes, at the cost of significantly reduced accuracy.
A different compromise would be to use 16 bits per component (or 64 bits/8 bytes total) converting each value to the 0..65535 range.
It is also possible to use a number of bits that is not a multiple of 8, as well as a different number of bits per component.