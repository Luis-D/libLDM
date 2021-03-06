# libLDMat
The fast, low level and non-bloated (*maybe*) math library.

###### This library is conformant with:
 *  **x86-32**:
    * CDECL
 *  **x86-64**:
    * System V AMD64 ABI (*nix systems)
    * Microsoft x64 calling convention (Windows) 
 *  **ARMv7-A**:
    * Procedure Call Standard for the ARM Architecture
 
### Algebra tutorial:

#### Data types:
  The next datatypes are the ones currently handled by the library:
>(Matrices are Row Major).
* **SS**: *float*. IEEE-754 single precision floating point number.
* **DS**: *double* . IEEE-754 double precision floating point number.
* **ANGLE**: A *float*. It stands for "*angle in radians*".
* **V2**: *Vector 2*. An array of two IEEE-754 single precision floating point numbers.
* **V3**: *Vector 3*. An array of three IEEE-754 single precision floating point numbers.
* **V4**: *Vector 4*. An array of four IEEE-754 single precision floating point numbers.
* **QUAT**: *Quaternion*. An array of four IEEE-754 single precision floating point numbers.
* **UQUAT**: *Unitary Quaternion*. The same as a *Quaternion*.
* **EULER**: *EULER ANGLE*. A *Vector 3* plus a contiguous Scalar, tecnically a *Vector 4* too.
* **M2**: *Matrix 2*, *2x2 Matrix*. An array of four IEEE-754 single precision floating point numbers.
* **M4**: *Matrix 4*, *4x4 Matrix*. An array of 16 IEEE-754 single precision floating point numbers.
* **AM4**: *Affine transformation from Matrix 4*. Is a Matrix 4, but only the upper 3x3 is used.

#### Operations:
* **ADD**: Addition.
* **SUB**: Substraction.
* **MUL**: Multiplication.
* **DIV**: Divition.
* **DOT**: Dot Product.
* **CROSS**: Cross Product.
* **LERP**: Linear Interpolation.
* **NLERP**: Normalized Linear Interpolation.
* **NORM**: Norm.
* **NORMALIZE**: Normalization.
* **DISTANCE**: Distance between two operands.
* **INV**: Invert an Operand.
* **DIFF**: Difference of two operands, not the same as a raw substraction.
* **ANGLE**: Calculate angle (in randians) of operand(s).
* **xTOy**: Convertion from x to y.
* **ROT**: Rotation.
* **MAKE**: Construct data.
* **IDENTITY**: It makes an identity matrix, it uses **MAKE** with a factor of *1.0*.
* **DET**: Determinant.

> Given an operation **OP**, and operand(s) of type **x**, then the operation is **xOP**, for example: **M2MUL**, which is a multiplicacion between two *2x2 Matrices*.
> Given an operation **OP** and operands of different types, **x** and **y**, then the operation is **xOPy**, for example: **M2MULV2**, which is a multiplicacion between a *2x2 Matrix* and a *Vector 2*.

#### Valid Operations:
* On **SS**: *LERP*.
* On **DS**: *LERP*.
* On **V2**: *ADD, SUB, MUL, DIV, DOT, LERP, NORM, DISTANCE, CROSS, ANGLE*.
   * By **SS**: *MUL, DIV*.
   * Special operations: **V2V2ANGLE** (Angle between two 2D points.)
* On **V3**: *ADD, SUB, MUL, DIV, DOT, LERP, NORM, DISTANCE, CROSS*.
   * By **SS**: *MUL, DIV*.
* On **V4**: *ADD, SUB, MUL, DIV, DOT, LERP, NORM, DISTANCE*.
   * By **SS**: *MUL, DIV*.
* On **QUAT**: *MUL, NORMALIZE (TOUQUAT), TOM4*.
   * On **V3**: *ROT* (**QUATROTV3**).
* On **UQUAT**: *The same as QUAT's*, *INV, DIFF, NLERP*.
* On **M2**: *MAKE, IDENTITY, MUL, DET, INV, ADD, SUB*.
   * On **V2**: *MUL* (**M2MULV2**).
   * By **SS**: *MUL, DIV*
* On **M4**: *MAKE, IDENTITY, MUL, INV*.
   * On **V4**: *MUL*
   * On **V3**: *MUL*
   * Special operations: **AM4MULV3, PERSPECTIVE, ORTHO, LOOKAT**.
* On **EULER**: *TOQUAT*.
* On **ANGLE**: *RADTODEG, DEGTORAD, TOM2*
   * On **V2**: *ROT* (**ANGLEROTV2**).




#### Usage:
Most binary operations uses this format: 
```C
void OPERATION(void * Destiny, void * SourceOperand1, void * SourceOperand2);
```
Most unary operations uses this format:
```C
void OPERATION(void * Destiny, void * Source);
```
Scalar parameters are passed as value.
Most scalar only operations uses this format:
```C
float OPERATION(float Source);
double OPERATION(double Source);
```
For more complex operations refer to the respective definition files.


##### C/C++:
###### Example:
```C
#include "LDMat/LDMat.h"
#include "stdio.h"

int main()
{
  float A[2] = {1,2};
  //The same as V2 A = {.x = 1, .y = 2};
  
  float B[2] = {3,4};
  //The same as V2 B = {.x = 3, .y = 4};
  
  float C[2];
  //The same as V3 C;
  
  V2ADD (C, A, B);
  //This could also be V2ADD(&C, &A, &B);
  //Because functions relies on pointers.
  
  printf("%f,%f\n",C[0], C[1]);
  //This could also be printf(%f,%f\n", C.x,C.y);
  
  return 1;
}

```

### Geometry tutorial:

#### 2D Data types:
* **CIRCLE2**: *Vec3*. Circle represented by a 2D point and a Radius.
* **AABB2**: *Vec4*. Generic Axis Aligned Bounding Box represented by two 2D points.
* **TRI2**: *3 Vec2*. A triangle represented by an array of three 2D points.
* **V2V2**: *Vec4*. Generic composition of two 2D points, a line.
* **RANGE2**: *Vec3*. A cone made by two 2D points (a line) and a angle (radians).

#### 3D Data types:
* **SPHERE**: *Vec4*. Sphere represented by a 3D point and a Radius.
* **AABB3**: *2 Vec3*. Generic Axis Aligned Bounding Box represented by two 3D points.
* **TRI3**: *3 Vec3*. A triangle represented by an array of three 3D points.
* **V3V3**: *2 Vec3*. Generic composition of two 3D points, a line.
* **RANGE3**: *2 Vec3*. A cone made by two 3D points (a line) and a angle (radians).
* **PLANE**: *2 Vec3*. A plane represented by a point and a normal.

#### Common operations (there are a lot of variants):
* **VS**: Interception of two objects.
* **CENTROID**: Centroid of an Object.
* **CLOSES**: Closest object to another object.
* **NORMAL**: Calculation of normal vector.
* **DISTANCE**: Distance between two objects.

#### Modes:
Due to the many ways to represent these data types, the algoritms use flags to set the proper representation.
(Refer to the header).
