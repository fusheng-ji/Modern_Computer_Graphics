---
typora-root-url: picture
---



# Modern Graphics

It's a notebook of games-101

class link https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html

## Course Topics

- Rasterization
  - Project geometry primitives (3D triangles/polygons) onto the screen
  - Break projected primitives into fragments (pixels)
  - Gold standard in video games (ream-time applications)
- Curves and Meshes
  - How to represent geometry in computer graphics
    - Bezier Curve
    - Catmull-Clark subdivision
- Ray Tracing
  - shoot rays from the camera though each pixel
    - calculate intersection and shading
    - continue to bounce the rays till they hit light sources
  - gold standard in animations/movies (off-line application)
- Animation/Simulation
  - key frame animation
  - mass-spring system

### Relations between computer graphics and computer vision?

```mermaid
  flowchart LR
  	a[model<br>Computer Graphics<br>modeling,simulation]--'computer Graphics <br> Rendering'-->b
  	b[image<br>computer vision<br>image processing<br>not comp. photography]--'computer vision'-->a
  	a-->a
  	b-->b
  	
```

## A Swift and Brutal Introduction to Linear Algebra

### Graphics' Dependencies

- Basic mathematics
  - Linear algebra,calculus,statistics
- Basic physics
  - optics, mechanics
- Misc
  - signal processing
  - numerical analysis

### Vectors

- usually written as $\vec{a}$ or in bold **a**
- or using start and end points $\vec{AB}=B-A$
- direction and length
- no absolute starting position

### Vector Normalization

- magnitude (length) og a vector written as $\left\| \vec{a} \right \|$
- unit vector
  - a vector with magnitude of 1
  - finding the unit vector of a vector (normalization) : $\hat{a}=\vec{a}/\left\| \vec{a} \right \|$
  - used to represent directions

### Vector Addition

- geometrically: parallelogram law & triangle law
- algebraically: simply add coordinates

### Vector Multiplication

- Dot product

  - $a\cdot b=\left\|a\right\|\left\|b\right\|cos \theta$
  - in graphics
    - find angle between two vectors (i.e. cosine of angle between light source and surface)
    - find projection of one vector on another
    - measures how close two directions are
    - decompose a vector
    - determine forward / backward
      - dot product > or < 0
  - dot product for projection
    - $\vec{b}_{\perp}$: projection of $\vec{b}$ onto $\vec{a}$
      - $\vec{b}_{\perp}$ must be along $\vec{a}$ (or along $\hat{a}$)
      - $\vec{b}_{\perp}=k \hat{a}$
    - what's its magnitude k?
      - $k=\left\|\vec{b}_{\perp}\right\|=\left\|\vec{b}\right\|cos \theta$

- Cross product

  - $a \times b=-b\times a$
  - $\vec{a}\times \vec{a}=\vec{0}$
  - $\vec{a}\times(\vec{b}+\vec{c})=\vec{a}\times\vec{b}+\vec{a}\times{c}$
  - $\vec{a}\times(k\vec{b})=k(\vec{a}\times\vec{b})$
  - $\left\|a\times b\right\|=\left\|a\right\|\left\|b\right\|sin\phi$
  - cross product is orthogonal to two initial vectors
  - direction determined by right-hand rule
  - useful in constructing coordinate systems (later)
  - properties (right handed coordinate)
    - $\vec{x}\times \vec{y}=+\vec{z}$
    - $\vec{y}\times \vec{x}=-\vec{z}$
    - $\vec{y}\times \vec{z}=+\vec{x}$
    - $\vec{z}\times \vec{y}=-\vec{x}$
    - $\vec{z}\times \vec{x}=+\vec{y}$
    - $\vec{x}\times \vec{z}=-\vec{y}$
  - Cartesian formula
  - $\vec{a}\times\vec{b}=\begin{pmatrix}y_az_b-y_bz_a\\z_ax_b-x_az_b\\x_ay_b-y_ax_b\end{pmatrix}=A*b=\underset{\text {dual matrix of vector a}}{\begin{pmatrix}0&-z_a&y_a\\z_a&0&-x_A\\-y_a&x_a&0\end{pmatrix}}\begin{pmatrix}x_b\\y_b\\z_b\end{pmatrix}$

  - in graphics
    - determine left / right
    - determine inside / outside
      - if a point is at the outside of shape, it will be locate at least at a vector's right

- Orthonormal bases and coordinate frames

  - important for representing points, positions, locations
  - often, many sets of coordinate systems
    - global,local,world,model,parts of model (head, hands, ...)
  - critical issue is transforming between these systems/bases
  - any set of 3 vectors (in 3D) that
    - $\left\|\vec{u}\right\|=\left\|\vec{v}\right\|=\left\|\vec{w}\right\|=1$
    - $\vec{u}\cdot\vec{v}=\vec{v}\cdot\vec{w}=\vec{u}\cdot\vec{w}=0$
    - $\vec{w}=\vec{u}\times\vec{v}$ (right-handed)
    - $\vec{p}=\underset{projection}{(\vec{p}\cdot\vec{u})}\vec{u}+(\vec{p}\cdot\vec{v})\vec{v}+(\vec{p}\cdot\vec{w})\vec{w}$

- Matrices

  - in graphics, pervasively used to represent transformations
    - translation, rotation, shear, scale
  - properties
    - transpose
      - $(AB)^T=B^TA^T$
    - non-commutative
      - AB and BA are different in general
    - associative and distributive
      - $(AB)C=A(BC)$
      - $A(B+C)=AB+AC$
      - $(A+B)C=AC+BC$

## Transformation

### scale matrix

$$\begin{bmatrix}x'\\y'\end{bmatrix}=\begin{bmatrix}s_x&0\\0&s_y\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}$$

### reflection matrix

horizontal reflection

$$\begin{bmatrix}x'\\y'\end{bmatrix}=\begin{bmatrix}-1&0\\0&1\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}$$

### shear matrix

$$\begin{bmatrix}x'\\y'\end{bmatrix}=\begin{bmatrix}1&a\\0&1\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}$$

### rotate (about the origin (0,0), CCW by default)

$$R_\theta=\begin{bmatrix}cos\theta&-sin\theta\\sin\theta&cos\theta\end{bmatrix}$$

$$R_{-\theta}=\begin{bmatrix}cos\theta&sin\theta\\-sin\theta&cos\theta\end{bmatrix}=R^T_\theta=R^{-1}_\theta$$

### linear transforms = matrix (of the same dimension)

$$x'=Mx$$

### homogeneous coordinates

#### why choose homogeneous coordinates?

- translation cannot be represented in matrix form

- $x'=ax+by+t_x, y'=cx+dy+t_y$

- $\begin{bmatrix}x'\\y'\end{bmatrix}=\begin{bmatrix}a&b\\c&d\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}+\begin{bmatrix}t_x\\t_y\end{bmatrix}$

- so, translation is not linear transform! 

  But we don't want translation to be a special case.

  Is there a unified way to represent all transformations? (and what's the cost?)

- add a third coordinate (w-coordinate)

  - 2D point = $(x,y,1)^T$
  - 2D vector = $(x,y,0)^T$
  - $\begin{pmatrix}x\\y\\w\end{pmatrix}$ is the 2D point $\begin{pmatrix}x/w\\y/w\\1\end{pmatrix}$, $w\ne 0$

- matrix representation of translations

  $$\begin{pmatrix}x'\\y'\\w'\end{pmatrix}=\begin{pmatrix}1&0&t_x\\0&1&t_y\\0&0&1\end{pmatrix}\cdot\begin{pmatrix}x\\y\\1\end{pmatrix}=\begin{pmatrix}x+t_x\\y+t_y\\1\end{pmatrix}$$

- valid operation if w-coordinate of result is 1 or 0

  - vector + vector =vector
  - point - point = vector
  - point + vector = point
  - point + point = the center between two points

#### affine transformations

- affine map = linear map + translation

  $\begin{pmatrix}x'\\y'\end{pmatrix}=\begin{pmatrix}a&b\\c&d\end{pmatrix}\cdot\begin{pmatrix}x\\y\end{pmatrix}+\begin{pmatrix}t_x\\t_y\end{pmatrix}$

- using homogeneous coordinates

  $\begin{pmatrix}x'\\y'\\1\end{pmatrix}=\begin{pmatrix}a&b&t_x\\c&d&t_y\\0&0&1\end{pmatrix}\cdot\begin{pmatrix}x\\y\\1\end{pmatrix}$

### 2D transformations

#### scale

$$S(s_x,s_y)=\begin{pmatrix}s_x&0&0\\0&s_y&0\\0&0&1\end{pmatrix}$$

#### rotation

![/Screenshot from 2022-01-10 14-25-25](/Screenshot from 2022-01-10 14-25-25.png)

$$R(\alpha)=\begin{pmatrix}cos\alpha& -sin\alpha &0\\sin\alpha & cos\alpha&0\\0&0&1\end{pmatrix}$$

#### translation

$$T(t_x,t_y)=\begin{pmatrix}1&0&t_x\\0&1&t_y\\0&0&1\end{pmatrix}$$

### composite transformation

#### transform ordering matters!!!

- if translate then rotate? or rotate then translate? --> rotate then translate is  correct

  $$T_{(1,0)}\cdot R_{45}\begin{bmatrix}x\\y\\1\end{bmatrix}=\begin{bmatrix}1&0&1\\0&1&0\\0&0&1\end{bmatrix}\begin{bmatrix}cos45^{\circ} &-sin45^{\circ} &0\\sin45^{\circ} &cos45^{\circ} &0\\0&0&1\end{bmatrix}\begin{bmatrix}x\\y\\1\end{bmatrix} $$

  **Note that matrices are applied right to left**

#### sequence of affine transforms $A_1,A_2,A_3...$

- compose by matrix multiplication
- $$A_n(...A_2(A_1(x)))=A_n...A_2\cdot A_1\cdot \begin{pmatrix}x\\y\\1\end{pmatrix}$$

### Decomposing complex transforms

#### Example : How to rotate around a given point c?

- translate center to origin
- rotate
- translate back

#### matrix representation?

$$T(c)\cdot R(\alpha)\cdot T(-c)$$

### 3D transformations

#### using homogeneous coordinates again

- 3D point = $(x,y,z,1)^T$
- 3D vector = $(x,y,z,0)^T$

in general, $(x,y,z,w)$ $(w\ne 0)$ is the 3D pont : $(x/w,y/w,z/w)$

use $4\times4$ matrices for affine transformations

$\begin{pmatrix}x'\\y'\\z'\\1\end{pmatrix}=\begin{pmatrix}a&b&c&t_x\\d&e&f&t_y\\g&h&i&t_z\\0&0&0&1\end{pmatrix}\cdot\begin{pmatrix}x\\y\\z\\1\end{pmatrix}$

#### what's the order?

linear first, then translation

## Transformation Cont.

### 3D transformations

#### scale

$$S(s_x,s_y,s_z)=\begin{pmatrix}s_x&0&0&0\\0&s_y&0&0\\0&0&s_z&0\\0&0&0&1\end{pmatrix}$$

#### translation

$$T(t_x,t_y,t_z)=\begin{pmatrix}1&0&0&t_x\\0&1&0&t_y\\0&0&1&t_z\\0&0&0&1\end{pmatrix}$$

#### rotation around x-, y-, or z-axis

![/Screenshot from 2022-01-10 14-23-11](/Screenshot from 2022-01-10 14-23-11.png)

$$R_x(\alpha)=\begin{pmatrix}1&0&0&0\\0&cos\alpha&-sin\alpha&0\\0&sin\alpha&cos\alpha&0\\0&0&0&1\end{pmatrix}$$

$$R_x(\alpha)=\begin{pmatrix}cos\alpha&0&sin\alpha&0\\0&1&0&0\\-sin\alpha&0&cos\alpha&0\\0&0&0&1\end{pmatrix}$$

$$R_x(\alpha)=\begin{pmatrix}cos\alpha&-sin\alpha&0&0\\sin\alpha&cos\alpha&0&0\\0&0&1&0\\0&0&0&1\end{pmatrix}$$

#### 3D rotations

compose any 3D rotation from $R_x,R_y,R_z$

$$R_{xyz}(\alpha,\beta,\gamma)=R_x(\alpha)R_y(\beta)R_z(\gamma)$$

- so-called Euler angles
- often used in flight simulator : roll,pitch, yaw

![/Screenshot from 2022-01-10 14-23-55](/Screenshot from 2022-01-10 14-23-55.png)

##### Rodrigues' rotation formula

- rotation by angle $\alpha$ around axis $n$

  $$R(n,\alpha)=cos(\alpha)I+(1-cos(\alpha))nn^T+sin(\alpha)\begin{pmatrix}0&-n_z&n_y\\n_z&0&-n_x\\-n_y&n_x&0\end{pmatrix}$$

**prove**

![/Screenshot from 2022-01-10 13-40-33](/Screenshot from 2022-01-10 13-40-33.png)

![/Screenshot from 2022-01-10 13-40-40](/Screenshot from 2022-01-10 13-40-40.png)

### viewing transformation

#### what's view transformation?

- think about how to take a photo?

  - find a good place and arrange people (model transformation)

  - find a good "angle" to put the camera (view transformation)

  - cheese! (projection transformation)

#### view / camera transformation

- how to perform view transformation?
- define the camera first
  - position $\vec{e}$
  - look-at / gaze direction $\hat{g}$
  - up direction $\hat{t}$ (assuming perp. to look-at)

![/Screenshot from 2022-01-10 14-24-26](/Screenshot from 2022-01-10 14-24-26.png)

- key observation
  - if the camera and all objects move together, the "photo" will be the same
- how about that we always transform the camera to 
  - the origin, up at Y, look at -Z
  - and transform the objects along with the camera
- transform the camera by $M_{view}$
- $M_{view}$ in math?
  - $M_{view}=R_{view}T_{view}$
    - $T_{view}=\begin{bmatrix}1&0&0&-x_e\\0&1&0&-y_e\\0&0&1&-z_e\\0&0&0&1\end{bmatrix}$
  - translate $e$ to origin
  - rotate $g$ to -Z, $t$ to Y, ($g\times t$) to X
    - but it's difficult to present
  - so consider its inverse rotation : X to ($g\times t$), Y to $t$, Z to $-g$ 
  - $$R^{-1}_{view}=\begin{bmatrix}x_{\hat{g}\times\hat{t}}&x_t&x_{-g}&0\\y_{\hat{g}\times\hat{t}}&y_t&y_{-g}&0\\z_{\hat{g}\times\hat{t}}&z_t&z_{-g}&0\\0&0&0&1\end{bmatrix}$$
  - because rotation matrix is orthogonal, so its inverse is its transposition matrix
  - $$R_{view}=\begin{bmatrix}x_{\hat{g}\times\hat{t}}&y_{\hat{g}\times\hat{t}}&z_{\hat{g}\times\hat{t}}&0\\x_t&y_t&z_t&0\\x_{-g}&y_{-g}&z_{-g}&0\\0&0&0&1\end{bmatrix}$$

### projection transformation

![/Screenshot from 2022-01-10 14-54-06](/Screenshot from 2022-01-10 14-54-06.png)

#### orthographic projection

- camera located at origin, looking at -Z, up at Y
- drop Z coordinate
- translate and scale the resulting rectangle to $[-1,1]^2$

![/Screenshot from 2022-01-10 14-58-31](/Screenshot from 2022-01-10 14-58-31.png)

- in general, we want to map a cuboid $[l,r]\times [b,t]\times[f,n]$ to the "canonical" cube $[-1,1]^3$
  - if something nearer to us , z's value will be samller

![/Screenshot from 2022-01-10 15-02-09](/Screenshot from 2022-01-10 15-02-09.png)

- transformation matrix

  - translate (center to origin) first, then scale (length/width/height to 2)

    $$M_{ortho}=\begin{bmatrix}\frac{2}{r-l}&0&0&0\\0&\frac{2}{t-b}&0&0\\0&0&\frac{2}{n-f}&0\\0&0&0&1\end{bmatrix}\begin{bmatrix}1&0&0&-\frac{r+l}{2}\\0&1&0&-\frac{t+b}{2}\\0&0&1&-\frac{n+f}{2}\\0&0&0&1\end{bmatrix}$$

- caveat

  - looking at / along -Z is making near and far not intuitive (n>f)
  - FYI : that's why OpenGL uses left hand coords

#### perspective projection

![/Screenshot from 2022-01-10 15-10-45](/Screenshot from 2022-01-10 15-10-45.png)

- most common projection
- further objects are smaller
- parallel lines not parallel
- coverage to single points

##### how to do perspective projection?

- first "squish" the frustum into a cuboid (n->n, f->f) ($M_{persp->ortho}$)
- do orthographic projection ($M_{ortho}$, already known!)

![/Screenshot from 2022-01-10 15-15-29](/Screenshot from 2022-01-10 15-15-29.png)

##### how to get transformation?

- recall the key idea : find the relationship between transformed points $(x',y',z')$ and the original points $(x,y,z)$
- $$y'=\frac{n}{z}y$$
- $$x'=\frac{n}{z}x$$

![/Screenshot from 2022-01-10 15-20-08](/Screenshot from 2022-01-10 15-20-08.png)

- in homogeneous coordinates

  $$\begin{pmatrix}x\\y\\z\\1\end{pmatrix}\rightarrow \begin{pmatrix}\frac{nx}{z}\\\frac{ny}{z}\\\text{unknown}\\1\end{pmatrix}=\begin{pmatrix}nx\\ny\\\text{still unknown}\\1\end{pmatrix}$$

- so the "squish" (persp to ortho) projection does this

  $$M^{(4\times4)}_{persp\rightarrow ortho}\begin{pmatrix}x\\y\\z\\1\end{pmatrix}=\begin{pmatrix}nx\\ny\\\text{unknown}\\1\end{pmatrix}$$

- already goog enough to figure out part of $M_{persp\rightarrow ortho}$

  $$M_{persp\rightarrow ortho}=\begin{pmatrix}n&0&0&0\\0&n&0&0\\?&?&?&?\\0&0&1&0\end{pmatrix}$$

- observation : the third row is responsible for z

  - any point on the near plane will not change

    $$M^{(4\times4)}_{persp\rightarrow ortho}\begin{pmatrix}x\\y\\z\\1\end{pmatrix}=\begin{pmatrix}nx\\ny\\\text{unknown}\\1\end{pmatrix}\underset{\text{replace z with n}}{\longrightarrow}\begin{pmatrix}x\\y\\n\\1\end{pmatrix}==\begin{pmatrix}nx\\ny\\n^2\\n\end{pmatrix}$$

    so the third row must be of the form $\begin{pmatrix}0&0&A&B\end{pmatrix}$

    $$\begin{pmatrix}0&0&A&B\end{pmatrix}\begin{pmatrix}x\\y\\n\\1\end{pmatrix}=n^2 (n^2\text{ has noting to do with x and y})$$

    $$An+b=n^2$$

  - any point's z on the far plane will not change

    $$\begin{pmatrix}0\\0\\f\\1\end{pmatrix}==\begin{pmatrix}0\\0\\f^2\\f\end{pmatrix}\rightarrow Af+B=f^2$$

- now we have two formulates and solve A and B

  $$\begin{matrix}An+B=n^2\\Af+B=f^2\end{matrix}\rightarrow\begin{matrix}A=n+f\\B=-nf\end{matrix}$$

- finally, every entry in $M_{persp\rightarrow ortho}$ is known

- what's next?

  - do orthographic projection ($M_{ortho}$) to finish
  - $M_{persp} = M_{ortho}M_{persp\rightarrow ortho}$

## Rasterization

### triangles

- sometimes people prefer : 

  vertical field-of-view (fovY) and aspect ratio (assume symmetry i.e. l = -r, b = -t)

![/Screenshot from 2022-01-10 15-48-57](/Screenshot from 2022-01-10 15-48-57.png)

- how to convert from fovY and aspect to l,r,b,t?
  - trivial

![/Screenshot from 2022-01-12 22-53-32](/Screenshot from 2022-01-12 22-53-32.png)

#### what's after MVP?

- Model transformation (placing objects)
- View transformation (placing camera)
- Projection transformation
  - orthographic projection (cuboid to "canonical" cube $[-1,1]^3$)
  - perspective projection (frustum to "canonical" cube)
- Canonical cube to screen
  - what's a screen?
    - an array of pixels
    - size of the array : resolution
    - a typical kind of raster display
  - raster == screen in german
    - rasterize == drawing onto the screen
  - pixel (FYI, short for "picture element")
    - color is a mixture of RGB
  - define the screen space
    - pixels' indices are in the form of (x,y), where both x and y are integers
    - pixels' indeices are form (0,0) to (width - 1, height - 1)
    - pixel (x,y) is centered at (x + 0.5, y + 0.5)
    - the screen covers range (0,0) to (width,height)

![/Screenshot from 2022-01-12 23-10-31](/微信截图_20220114151354.png)

- transform in xy plane : $[-1,1]^2$ to [0, width] *[0,height]
- irrelevant to z
- viewport transform matrix

$$M_{viewport}=\begin{pmatrix}\frac{width}{2}&0&0&\frac{width}{2}\\0&\frac{height}{2}&0&\frac{height}{2}\\0&0&1&0\\0&0&0&1\end{pmatrix}$$

#### drawing to raster displays

- polygon meshes
- triangle meshes
  - why triangles?
    - most basic polygon
      - break up other polygons
    - uniques properties
      - guaranteed to be planar
      - well-defined interior
      - well-defined method for interpolating values at vertices over triangle (barycentric interpolation)

##### what pixel values approximate a triangle?

![/微信截图_20220114150218](/微信截图_20220114150218.png)

- input : position of triangle vertices projected on screen
- output : set of pixel values approximating triangle

###### a simple approach : sampling

evaluating a function at a point is sampling, we can discretize a function by sampling

```c++
for (int x = 0; x < xmax; ++x)
	output[x] = f(x);
```

sampling is a core idea in graphics, we sample time (1D), area (2D), direction (2D), volume (3D)

we can sample if each pixel center is inside triangle

![/微信截图_20220114151354](/微信截图_20220114151354.png)

- rasterization = sampling a 2D indicator function

  ```c++
  for (int x = 0; x < xmax; ++x)
      for (int y = 0; y < ymax; ++y)
          image[x][y] = inside(tri, x + 0.5, y + 0.5); // pixel center
  ```

- evaluating **inside (tri, x, y)**

  - how to judge a point inside?
    - three cross products
      - AB->BC->CA
    - edge cases
      - is this sample point covered by triangle 1, triangle 2, or both?
        - some software have different rules
  - how to make compute faster?
    - use a bounding box

- in this class, we assume display pixels emit square of light

but rasterization will be lead to **aliasing (jaggies)**

![/微信截图_20220114154310](/微信截图_20220114154310.png)

### antialiasing and Z-Buffering

sampling in graphics

- photograph = sample image sensor plane
- video = sample time

sampling **artifacts** -> (errors / mistakes / inaccuracies) in computer graphics

- aliasing - sampling in space
- moiré patterns in imaging - undersampling image
- wagon wheel illusion (false motion) - sampling in time

behind the aliasing artifacts

- signals are changing too fast (high frequency), but sampled too slowly

antialiasing idea

- blurring (pre-filtering) before sampling
- but sample then filter is WRONG!

![/微信截图_20220114160649](/微信截图_20220114160649.png)

but why?

- why undersampling introduces aliasing?
- why pre-filtering then sampling can do antialiasing?

#### frequency domain

cosine’s frequencies $cos2\pi f x$ where $f=\frac{1}{T}$

![/微信截图_20220114182508](/微信截图_20220114182508.png)

##### fourier transform

- represent a function as a weighted sum of sines and cosines

```mermaid
graph LR
	1["spatial domain<br>f(x)"]--"fourier transform"-->2["frequency domian<br>F(ω)"]
	2--"inverse transform"-->1
```

$$\text{fourier transform}F(\omega)=\int^{\infty}_{-\infty} f(x)e^{-2\pi i\omega x}dx$$

$\text{inverse transform}f(x)=\int^{\infty}_{-\infty}F(\omega)e^{2\pi i \omega x}d\omega,\text{where }e^{ix}=\cos x+i \sin x$

- higher frequencies need faster sampling, undersampling creates frequency aliases
- two frequencies that are indistinguishable at a given sampling rate are called “aliases”

##### **filtering = getting rid of certain frequency contents**

![/微信截图_20220114182913](/微信截图_20220114182913.png)

![/微信截图_20220114182935](/微信截图_20220114182935.png)

![/微信截图_20220114182955](/微信截图_20220114182955.png)

![/微信截图_20220114183236](/微信截图_20220114183236.png)

![/微信截图_20220114183303](/微信截图_20220114183303.png)

##### **filtering = convolution (= averaging)**

#### convolution

![/微信截图_20220114183618](/微信截图_20220114183618.png)

- convolution in the spatial domain is equal to multiplication in the frequency, and vice versa
- filter by convolution in the spatial domain
- transform to frequency domain (Fourier transform)
- multiply by Fourier transform of convolution kernel
- transform back to spatial domain (inverse Fourier)

![/微信截图_20220114183930](/微信截图_20220114183930.png)

![/微信截图_20220114184105](/微信截图_20220114184105.png)

![/微信截图_20220114184146](/微信截图_20220114184146.png)

![/微信截图_20220114184205](/微信截图_20220114184205.png)

##### **Sampling = Repeating frequency contents**

![/微信截图_20220114184342](/微信截图_20220114184342.png)

##### Aliasing = Mixed Frequency Contents

![/微信截图_20220114184544](/微信截图_20220114184544.png)

#### Antialiasing

##### how can we reduce aliasing error?

- option 1 : increase sampling rate
  - essentially increasing the distance between replicas in the Fourier domain
  - higher resolution displays, sensors, framebuffers ...
  - but : costly & may need very high resolution

- option 2 : antialiasing
  - making fourier contents “narrower” before repeating
  - i.e. filtering out high frequencies before sampling

##### antialiasing = Limiting, the repeating

![/微信截图_20220114185110](/微信截图_20220114185110.png)

##### antialiasing by averaging values in pixel area

solution : 

- convolve f(x,y) by a 1-pixel box-blur
  - recall : convolving = filtering = averaging
- then sample at every pixel’s center

![/微信截图_20220114185412](/微信截图_20220114185412.png)

##### Antialiasing By Supersampling (MSAA)

![/微信截图_20220114185550](/微信截图_20220114185550.png)

![/微信截图_20220114185643](/微信截图_20220114185643.png)

![/微信截图_20220114185706](/微信截图_20220114185706.png)

![/微信截图_20220114185726](/微信截图_20220114185726.png)

![/微信截图_20220114185804](/微信截图_20220114185804.png)

![/微信截图_20220114185804](/微信截图_20220114185804.png)

![/微信截图_20220114185839](/微信截图_20220114185839.png)

No free lunch!

- what’s the cost of MSAA?

Development Milestone

-  FXAA (fast approximate AA)
- TAA (temporal AA)

Super resolution / super sampling

- From low resolution to high resolution
- Essentially still “not enough samples” problem
- DLSS  (Deep Learning Super Sampling)

#### Visibility / occlusion

##### painter’s algorithm

inspired by how painters paint, paint from back to front, overwrite in the framebuffer

requires sorting in depth (O(n log n)) for n triangles

can have unresolvable depth order

![/微信截图_20220114191440](/微信截图_20220114191440.png)

#### Z-buffer

- store current min z-value for each sample (pixel)
- needs an additional buffer for depth values
  - frame buffer stores color values
  - depth buffer (z-buffer) stores depth
- for simplicity we suppose z is always positive (smaller z -> closer, larger z -> further)

##### Z-buffer algorithm

initialize depth buffer to $\infty$

during rasterization : 

```c++
for (each triangle T)
	for (each sample (x,y,z) in T)
		if (z < zbuffer[x,y])		//closest sample so far
			framebuffer[x,y] rgb;	//update color
			zbuffer[x,y] = z;		//update depth
         else
         	;						//do nothing, this sample is occluded
```

![/微信截图_20220114201126](/微信截图_20220114201126.png)

##### complexity

- O(n) for n triangles (assuming constant coverage)
  - why assume this state?
    - judge whether float values are equal is difficult
- how is it possible to sort n triangles in linear time
  - it’s wrong, there is not sorting operation

Drawing triangles in different orders?

most important visibility algorithm

- implemented in hardware for all GPUs

## Shading

### illumination, shading 

#### Definition

In this course shading is the process of applying a material to an object

#### A Simple Shading Model (Blinn-Phong Reflectance Model)

##### Perceptual observations

![/微信截图_20220131204840](/微信截图_20220131204840.png)

##### Shading is Local (at a specific shading point)

![/微信截图_20220131204904](/微信截图_20220131204904.png)

No shadows will be generated! (**shading ≠ shadow**)

![/微信截图_20220131205527](/微信截图_20220131205527.png)

##### Diffuse Reflection

- Light is scattered uniformly in all directions

  - surface color is the same for all viewing directions

    ![/微信截图_20220131205630](/微信截图_20220131205630.png)

- But how much light (energy) is received?

  - Lambert's cosine law

    ![/微信截图_20220131205738](/微信截图_20220131205738.png)

![/微信截图_20220131214632](/微信截图_20220131214632.png)

###### Lambertian (Diffuse) Shading

![/微信截图_20220131214915](/微信截图_20220131214915.png)

Produces diffuse appearance :

![/微信截图_20220131215656](/微信截图_20220131215656.png)

##### Specular Term

Intensity depends on view direction

- Bright near mirror reflection direction

![/微信截图_20220131222835](/微信截图_20220131222835.png)

$V$ close to mirror direction $\longleftrightarrow$  half vector near normal

- Measure "near" by dot product of unit vectors

![/微信截图_20220131222855](/微信截图_20220131222855.png)

###### why there is a power "p"?

![/微信截图_20220131224430](/微信截图_20220131224430.png)

![/微信截图_20220131224457](/微信截图_20220131224457.png)

##### Ambient Term

shading that does not depend on anything

- add constant color to account for disregarded illumination and fill in black shadows
- this is approximate / fake!

![/微信截图_20220131225323](/微信截图_20220131225323.png)

##### Blinn-Phong Reflection Model

![/微信截图_20220131225703](/微信截图_20220131225703.png)

#### shading frequencies

![/微信截图_20220131230731](/微信截图_20220131230731.png)

##### shade each triangle (Flat shading)

- triangle face is flat - one normal vector
- not good for smooth surfaces

![/微信截图_20220131231952](/微信截图_20220131231952.png)

##### shade each vertex (Gouraud shading)

- interpolate colors from vertices across triangle
- each vertex has a normal vector (how?)

![/微信截图_20220131232018](/微信截图_20220131232018.png)

##### shade each pixel (Phong shading)

- interpolate normal vectors across each triangle
- computer full shading model at each pixel
- not the Blinn-Phong Reflectance Model

![/微信截图_20220131232417](/微信截图_20220131232417.png)

![/微信截图_20220131232438](/微信截图_20220131232438.png)

##### Defining Per-Vertex Normal Vectors

![/微信截图_20220131233756](/微信截图_20220131233756.png)

![/微信截图_20220131233819](/微信截图_20220131233819.png)

### graphics (Real-time Rendering) Pipeline

![/微信截图_20220131234015](/微信截图_20220131234015.png)

![/微信截图_20220131234316](/微信截图_20220131234316.png)

![/微信截图_20220131234348](/微信截图_20220131234348.png)

![/微信截图_20220131234411](/微信截图_20220131234411.png)

![/微信截图_20220131234428](/微信截图_20220131234428.png)

![/微信截图_20220131234450](/微信截图_20220131234450.png)

##### shader programs

- program vertex and fragment processing stages
- describe operation on a single vertex (or fragment)

![/微信截图_20220201003333](/微信截图_20220201003333.png)

![/微信截图_20220201003356](/微信截图_20220201003356.png)

##### graphics pipeline implementation : GPUs - heterogeneous, multi-core processor

specialized processors for executing graphics pipeline computations

![/微信截图_20220201010044](/微信截图_20220201010044.png)

![/微信截图_20220201010304](/微信截图_20220201010304.png)

### Texture Mapping

![/微信截图_20220201010357](/微信截图_20220201010357.png)

#### Surfaces are 2D

- surface lives in 3D world space
- every 3D surface point also has a place where it goes in the 2D image (texture)

![/微信截图_20220201010603](/微信截图_20220201010603.png)

![/微信截图_20220201010630](/微信截图_20220201010630.png)

![/微信截图_20220201010749](/微信截图_20220201010749.png)

![/微信截图_20220201012713](/微信截图_20220201012713.png)

![/微信截图_20220201014313](/微信截图_20220201014313.png)

![/微信截图_20220201014321](/微信截图_20220201014321.png)

![/微信截图_20220201014345](/微信截图_20220201014345.png)



#### Interpolation Across Triangles: Barycentric Coordinates

Why do we want to interpolate?

- specify values at vertices
- obtain smoothly varying values across triangles

What do we want to interpolate?

- Texture coordinates, colors, normal vectors, ...

How do we interpolate?

- Barycentric coordinates

![/微信截图_20220201015032](/微信截图_20220201015032.png)

![/微信截图_20220201015212](/微信截图_20220201015212.png)

![/微信截图_20220201015221](/微信截图_20220201015221.png)

![/微信截图_20220201015229](/微信截图_20220201015229.png)

![/微信截图_20220201015330](/微信截图_20220201015330.png)

![/微信截图_20220201015339](/微信截图_20220201015339.png)

**Barycentric coordinates are not invariant under projection**

#### Applying Textures

![/微信截图_20220201015732](/微信截图_20220201015732.png)

#### Texture Magnification - Easy case  (What if the texture is too small?)

![/微信截图_20220201015915](/微信截图_20220201015915.png)

##### Bilinear Interpolation

![/微信截图_20220201020015](/微信截图_20220201020015.png)

![/微信截图_20220201020023](/微信截图_20220201020023.png)

![/微信截图_20220201020036](/微信截图_20220201020036.png)

![/微信截图_20220201020046](/微信截图_20220201020046.png)

![/微信截图_20220201020058](/微信截图_20220201020058.png)

![/微信截图_20220201020112](/微信截图_20220201020112.png)

Bilinear interpolation usually gives pretty good results
at reasonable costs

![/微信截图_20220201020253](/微信截图_20220201020253.png)

#### Texture Magnification - hard case (What if the texture is too large?)

![/微信截图_20220201020535](/微信截图_20220201020535.png)

![/微信截图_20220201020643](/微信截图_20220201020643.png)

![/微信截图_20220201020725](/微信截图_20220201020725.png)

#### Antialiasing - supersampling?

Will supersampling work?

- yes, high quality, but costly
- when highly minified, many texels in pixel footprint
- signal frequency too large in a pixel
- need even higher sampling frequency

Let's understand this problem in another way

- what if we don't sample?
- just need to get the average value within a range!

![/微信截图_20220201021119](/微信截图_20220201021119.png)

![/微信截图_20220201021150](/微信截图_20220201021150.png)

#### Mipmap - Allowing (fast, approx., square) range queries

![/微信截图_20220201021343](/微信截图_20220201021343.png)

![/微信截图_20220201021407](/微信截图_20220201021407.png)

![/微信截图_20220201021423](/微信截图_20220201021423.png)

![/微信截图_20220201021443](/微信截图_20220201021443.png)

![/微信截图_20220201021503](/微信截图_20220201021503.png)

![/微信截图_20220201021521](/微信截图_20220201021521.png)

![/微信截图_20220201022030](/微信截图_20220201022030.png)

![/微信截图_20220201022046](/微信截图_20220201022046.png)

##### limitations

![/微信截图_20220201022143](/微信截图_20220201022143.png)

![/微信截图_20220201022152](/微信截图_20220201022152.png)

![/微信截图_20220201022201](/微信截图_20220201022201.png)

#### Anisotropic Filtering - fix mipmap's overblur

![/微信截图_20220201022247](/微信截图_20220201022247.png)

![/微信截图_20220201022406](/微信截图_20220201022406.png)

![/微信截图_20220201022426](/微信截图_20220201022426.png)

#### Applications of textures

In modern GPUs, texture = memory + range query (filtering)

- General method to bring data to fragment calculations

Many applications

- Environment lighting
- Store microgeometry
- Procedural textures
- Solid modeling
- Volume rendering

![/微信截图_20220201130907](/微信截图_20220201130907.png)

![/微信截图_20220201130930](/微信截图_20220201130930.png)

![/微信截图_20220201131008](/微信截图_20220201131008.png)

![/微信截图_20220201131022](/微信截图_20220201131022.png)

![/微信截图_20220201131033](/微信截图_20220201131033.png)

![/微信截图_20220201131116](/微信截图_20220201131116.png)

##### Bump Mapping

![/微信截图_20220201131127](/微信截图_20220201131127.png)

![/微信截图_20220201131138](/微信截图_20220201131138.png)

###### How to perturb the normal (in flatland)

![/微信截图_20220201132050](/微信截图_20220201132050.png)

###### How to perturb the normal (in 3D)

![/微信截图_20220201132138](/微信截图_20220201132138.png)

##### Displacement Mapping

![/微信截图_20220201132458](/微信截图_20220201132458.png)

![/微信截图_20220201132752](/微信截图_20220201132752.png)

![/微信截图_20220201132814](/微信截图_20220201132814.png)

![/微信截图_20220201132832](/微信截图_20220201132832.png)

## Geometry

### Introduction

![/微信截图_20220201133009](/微信截图_20220201133009.png)

![/微信截图_20220201133022](/微信截图_20220201133022.png)

![/微信截图_20220201133034](/微信截图_20220201133034.png)

![/微信截图_20220201133124](/微信截图_20220201133124.png)

![/微信截图_20220201133133](/微信截图_20220201133133.png)

![/微信截图_20220201133143](/微信截图_20220201133143.png)

![/微信截图_20220201133152](/微信截图_20220201133152.png)

![/微信截图_20220201133200](/微信截图_20220201133200.png)

![/微信截图_20220201133209](/微信截图_20220201133209.png)

![/微信截图_20220201133218](/微信截图_20220201133218.png)

![/微信截图_20220201133927](/微信截图_20220201133927.png)

### Implicit representations

![/微信截图_20220201133938](/微信截图_20220201133938.png)

![/微信截图_20220201133948](/微信截图_20220201133948.png)

#### Algebraic Surfaces

![/微信截图_20220201133957](/微信截图_20220201133957.png)

#### Constructive Solid Geometry

![/微信截图_20220201134007](/微信截图_20220201134007.png)

#### Distance Functions

![/微信截图_20220201134015](/微信截图_20220201134015.png)

![/微信截图_20220201134024](/微信截图_20220201134024.png)

#### Blending Distance Functions

![/微信截图_20220201134034](/微信截图_20220201134034.png)

![/微信截图_20220201134044](/微信截图_20220201134044.png)

#### Level Set Methods

![/微信截图_20220201134052](/微信截图_20220201134052.png)

![/微信截图_20220201134101](/微信截图_20220201134101.png)

![/微信截图_20220201134109](/微信截图_20220201134109.png)

#### Fractals

![/微信截图_20220201134119](/微信截图_20220201134119.png)

#### Pros & Cons

- Pros
  - compact description (e.g., a function)
  - certain queries easy (inside object, distance of surface)
  - good for ray-to-surface intersection (more later)
  - for simple shapes, exact description / no sampling error
  - easy to handle changes in topology (e.g., fluid)
- Cons
  - difficult to model complex shapes

### Explicit representations

![/微信截图_20220201142009](/微信截图_20220201142009.png)

#### Point Cloud

![/微信截图_20220201142018](/微信截图_20220201142018.png)

#### Polygon Mesh

![/微信截图_20220201142029](/微信截图_20220201142029.png)

![/微信截图_20220201142038](/微信截图_20220201142038.png)

#### Curves

applications:

- camera paths
- animation curves
- vector fonts

##### Bezier Curves

![/微信截图_20220201153129](/微信截图_20220201153129.png)

###### Evaluating Bezier Curves (de Casteljau Algorithm)

![/微信截图_20220201153140](/微信截图_20220201153140.png)

![/微信截图_20220201153148](/微信截图_20220201153148.png)

![/微信截图_20220201153154](/微信截图_20220201153154.png)

![/微信截图_20220201153200](/微信截图_20220201153200.png)

![/微信截图_20220201153206](/微信截图_20220201153206.png)

![/微信截图_20220201163903](/微信截图_20220201163903.png)

![/微信截图_20220201163911](/微信截图_20220201163911.png)

###### Evaluation Bezier Curves Algebraic Formula

![/微信截图_20220201164101](/微信截图_20220201164101.png)

![/微信截图_20220201164108](/微信截图_20220201164108.png)

![/微信截图_20220201164118](/微信截图_20220201164118.png)

![/微信截图_20220201164128](/微信截图_20220201164128.png)

![/微信截图_20220201164136](/微信截图_20220201164136.png)

![/微信截图_20220201164143](/微信截图_20220201164143.png)

![/微信截图_20220201164150](/微信截图_20220201164150.png)

#### Piecewise Bezier Curves

![/微信截图_20220201164907](/微信截图_20220201164907.png)

![/微信截图_20220201164913](/微信截图_20220201164913.png)

example: http://math.hws.edu/eck/cs424/notes2013/canvas/bezier.html

##### Continuity

![/微信截图_20220201164933](/微信截图_20220201164933.png)

![/微信截图_20220201164942](/微信截图_20220201164942.png)

![/微信截图_20220201164949](/微信截图_20220201164949.png)

![/微信截图_20220201164959](/微信截图_20220201164959.png)

![/微信截图_20220201165008](/微信截图_20220201165008.png)

In this course 

- We do not cover B-splines and NURBS

- We also do not cover operations on curves
  (e.g. increasing/decreasing orders, etc.)
- To learn more deeper, you are welcome to refer to Prof. Shi-Min Hu’s course: https://www.bilibili.com/video/av66548502?from=search&seid=65256805876131485

#### Surfaces

![/微信截图_20220201165738](/微信截图_20220201165738.png)

![/微信截图_20220201165746](/微信截图_20220201165746.png)

##### Evaluating Bezier Surfaces

![/微信截图_20220201165916](/微信截图_20220201165916.png)

![/微信截图_20220201165923](/微信截图_20220201165923.png)

#### mesh operations

![/微信截图_20220201165934](/微信截图_20220201165934.png)

##### mesh subdivision (upsampling)

![/微信截图_20220209134700](/微信截图_20220209134700.png)

###### loop subdivision

![/微信截图_20220209134909](/微信截图_20220209134909.png)

![/微信截图_20220209134918](/微信截图_20220209134918.png)

![/微信截图_20220209134926](/微信截图_20220209134926.png)

![/微信截图_20220209134935](/微信截图_20220209134935.png)

![/微信截图_20220209134942](/微信截图_20220209134942.png)

###### catmull-clark subdivision

![/微信截图_20220209134952](/微信截图_20220209134952.png)

![/微信截图_20220209135001](/微信截图_20220209135001.png)

![/微信截图_20220209135011](/微信截图_20220209135011.png)

![/微信截图_20220209135020](/微信截图_20220209135020.png)

![/微信截图_20220209135032](/微信截图_20220209135032.png)

![/微信截图_20220209135042](/微信截图_20220209135042.png)

##### mesh simplification (downsampling)

![/微信截图_20220209134718](/微信截图_20220209134718.png)

![/微信截图_20220209140248](/微信截图_20220209140248.png)

![/微信截图_20220209140254](/微信截图_20220209140254.png)

![/微信截图_20220209140304](/微信截图_20220209140304.png)

http://graphics.stanford.edu/courses/cs468-10-fall/LectureSlides/08_Simplification.pdf

![/微信截图_20220209140312](/微信截图_20220209140312.png)

![/微信截图_20220209140320](/微信截图_20220209140320.png)

![/微信截图_20220209140327](/微信截图_20220209140327.png)

##### mesh regularization (same # of triangles)

![/微信截图_20220209134728](/微信截图_20220209134728.png)

### Shadow Mapping

![/微信截图_20220209140931](/微信截图_20220209140931.png)

![/微信截图_20220209140939](/微信截图_20220209140939.png)

![/微信截图_20220209140947](/微信截图_20220209140947.png)

![/微信截图_20220209140956](/微信截图_20220209140956.png)

![/微信截图_20220209141003](/微信截图_20220209141003.png)

![/微信截图_20220209141015](/微信截图_20220209141015.png)

![/微信截图_20220209141023](/微信截图_20220209141023.png)

![/微信截图_20220209141031](/微信截图_20220209141031.png)

![/微信截图_20220209141040](/微信截图_20220209141040.png)

![/微信截图_20220209141050](/微信截图_20220209141050.png)

![/微信截图_20220209141103](/微信截图_20220209141103.png)

floating numbers' comparison is difficulty!

![/微信截图_20220209141112](/微信截图_20220209141112.png)

![/微信截图_20220209141930](/微信截图_20220209141930.png)

![/微信截图_20220209141938](/微信截图_20220209141938.png)

![/微信截图_20220209141948](/微信截图_20220209141948.png)

https://www.timeanddate.com/eclipse/umbra-shadow.html

## Ray tracing

![/微信截图_20220209142534](/微信截图_20220209142534.png)

![/微信截图_20220209142544](/微信截图_20220209142544.png)

![/微信截图_20220209142554](/微信截图_20220209142554.png)

### Basic Ray-Tracing Algorithm

![/微信截图_20220209142922](/微信截图_20220209142922.png)

![/微信截图_20220209142933](/微信截图_20220209142933.png)

![/微信截图_20220209142958](/微信截图_20220209142958.png)

![/微信截图_20220209143006](/微信截图_20220209143006.png)

![/微信截图_20220209143013](/微信截图_20220209143013.png)

![/微信截图_20220209143023](/微信截图_20220209143023.png)

![/微信截图_20220209143030](/微信截图_20220209143030.png)

![/微信截图_20220209143037](/微信截图_20220209143037.png)

![/微信截图_20220209143044](/微信截图_20220209143044.png)

![/微信截图_20220209143051](/微信截图_20220209143051.png)

![/微信截图_20220209143058](/微信截图_20220209143058.png)

![/微信截图_20220209143105](/微信截图_20220209143105.png)

#### Ray-Surface Intersection

![/微信截图_20220209144455](/微信截图_20220209144455.png)

![/微信截图_20220209144503](/微信截图_20220209144503.png)

![/微信截图_20220209144510](/微信截图_20220209144510.png)

![/微信截图_20220209144518](/微信截图_20220209144518.png)

![/微信截图_20220209144526](/微信截图_20220209144526.png)

![/微信截图_20220209144533](/微信截图_20220209144533.png)

![/微信截图_20220209144541](/微信截图_20220209144541.png)

![/微信截图_20220209144550](/微信截图_20220209144550.png)

![/微信截图_20220209144557](/微信截图_20220209144557.png)

### Accelerating Ray-Surface Intersection

![/微信截图_20220209144110](/微信截图_20220209144110.png)

![/微信截图_20220209144118](/微信截图_20220209144118.png)

![/微信截图_20220209144129](/微信截图_20220209144129.png)

#### Bounding Volumes

![/微信截图_20220209144150](/微信截图_20220209144150.png)

![/微信截图_20220209144206](/微信截图_20220209144206.png)

![/微信截图_20220209144222](/微信截图_20220209144222.png)

![/微信截图_20220209144230](/微信截图_20220209144230.png)

![/微信截图_20220209144238](/微信截图_20220209144238.png)

![/微信截图_20220209144247](/微信截图_20220209144247.png)

#### Uniform Spatial Partitions (Grids)

![/微信截图_20220209151115](/微信截图_20220209151115.png)

![/微信截图_20220209151124](/微信截图_20220209151124.png)

![/微信截图_20220209151131](/微信截图_20220209151131.png)

![/微信截图_20220209151139](/微信截图_20220209151139.png)

![/微信截图_20220209151146](/微信截图_20220209151146.png)

![/微信截图_20220209151154](/微信截图_20220209151154.png)

![/微信截图_20220209151201](/微信截图_20220209151201.png)

![/微信截图_20220209151210](/微信截图_20220209151210.png)

![/微信截图_20220209151219](/微信截图_20220209151219.png)

#### Spatial Partitions

![/微信截图_20220209151243](/微信截图_20220209151243.png)

![/微信截图_20220209151251](/微信截图_20220209151251.png)

![/微信截图_20220209151258](/微信截图_20220209151258.png)

![/微信截图_20220209151305](/微信截图_20220209151305.png)

![/微信截图_20220209151313](/微信截图_20220209151313.png)

![/微信截图_20220209151320](/微信截图_20220209151320.png)

![/微信截图_20220209151330](/微信截图_20220209151330.png)

![/微信截图_20220209151338](/微信截图_20220209151338.png)

![/微信截图_20220209151344](/微信截图_20220209151344.png)

![/微信截图_20220209151351](/微信截图_20220209151351.png)

![/微信截图_20220209151400](/微信截图_20220209151400.png)

![/微信截图_20220209151410](/微信截图_20220209151410.png)

![/微信截图_20220209151419](/微信截图_20220209151419.png)

#### Object Partitions & Bounding Volume Hierarchy (BVH)

![/微信截图_20220209151559](/微信截图_20220209151559.png)

![/微信截图_20220209151605](/微信截图_20220209151605.png)

![/微信截图_20220209151612](/微信截图_20220209151612.png)

![/微信截图_20220209151622](/微信截图_20220209151622.png)

![/微信截图_20220209151629](/微信截图_20220209151629.png)

![/微信截图_20220209151636](/微信截图_20220209151636.png)

![/微信截图_20220209151643](/微信截图_20220209151643.png)

![/微信截图_20220209151651](/微信截图_20220209151651.png)

![/微信截图_20220209151658](/微信截图_20220209151658.png)

### Basic radiometry

![/微信截图_20220209151757](/微信截图_20220209151757.png)

![/微信截图_20220209151804](/微信截图_20220209151804.png)

#### Radiant Energy and Flux (Power)

![/微信截图_20220209151843](/微信截图_20220209151843.png)

![/微信截图_20220209151851](/微信截图_20220209151851.png)

![/微信截图_20220209151858](/微信截图_20220209151858.png)

#### Radiant Intensity

![/微信截图_20220209151932](/微信截图_20220209151932.png)

![/微信截图_20220209151940](/微信截图_20220209151940.png)

![/微信截图_20220209151949](/微信截图_20220209151949.png)

![/微信截图_20220209151957](/微信截图_20220209151957.png)

![/微信截图_20220209152003](/微信截图_20220209152003.png)

![/微信截图_20220209152010](/微信截图_20220209152010.png)

![/微信截图_20220209152017](/微信截图_20220209152017.png)

![/微信截图_20220209152258](/微信截图_20220209152258.png)

![/微信截图_20220209152305](/微信截图_20220209152305.png)

#### Irradiance

![/微信截图_20220209152339](/微信截图_20220209152339.png)

![/微信截图_20220209152347](/微信截图_20220209152347.png)

![/微信截图_20220209152354](/微信截图_20220209152354.png)

![/微信截图_20220209152405](/微信截图_20220209152405.png)

The differential solid angle is not change, so what really falloff is irradiance neither intensity.

#### Radiance

![/微信截图_20220209152435](/微信截图_20220209152435.png)

![/微信截图_20220209152443](/微信截图_20220209152443.png)

![/微信截图_20220209152451](/微信截图_20220209152451.png)

![/微信截图_20220209152458](/微信截图_20220209152458.png)

![/微信截图_20220209152504](/微信截图_20220209152504.png)

##### Irradiance VS Radiance

![/微信截图_20220209152511](/微信截图_20220209152511.png)

#### Bidirectional Reflectance Distribution Function (BRDF)

![/微信截图_20220209152644](/微信截图_20220209152644.png)

![/微信截图_20220209152651](/微信截图_20220209152651.png)

![/微信截图_20220209152659](/微信截图_20220209152659.png)

![/微信截图_20220209152705](/微信截图_20220209152705.png)

![/微信截图_20220209152713](/微信截图_20220209152713.png)

### Light transport

#### Understanding the rendering equation

![/微信截图_20220209152755](/微信截图_20220209152755.png)

![/微信截图_20220209152803](/微信截图_20220209152803.png)

![/微信截图_20220209152816](/微信截图_20220209152816.png)

![/微信截图_20220209152825](/微信截图_20220209152825.png)

![/微信截图_20220209152833](/微信截图_20220209152833.png)

![/微信截图_20220209152842](/微信截图_20220209152842.png)

![/微信截图_20220209152849](/微信截图_20220209152849.png)

![/微信截图_20220209152859](/微信截图_20220209152859.png)

![/微信截图_20220209152905](/微信截图_20220209152905.png)

### Global illumination

![/微信截图_20220209152913](/微信截图_20220209152913.png)

![/微信截图_20220209152932](/微信截图_20220209152932.png)

![/微信截图_20220209152944](/微信截图_20220209152944.png)

![/微信截图_20220209152954](/微信截图_20220209152954.png)

![/微信截图_20220209153005](/微信截图_20220209153005.png)

![/微信截图_20220209153016](/微信截图_20220209153016.png)

![/微信截图_20220209153026](/微信截图_20220209153026.png)

### Probability Review

![/微信截图_20220209153130](/微信截图_20220209153130.png)

![/微信截图_20220209153137](/微信截图_20220209153137.png)

![/微信截图_20220209153144](/微信截图_20220209153144.png)

![/微信截图_20220209153150](/微信截图_20220209153150.png)

![/微信截图_20220209153156](/微信截图_20220209153156.png)

### Monte Carlo Integration

![/微信截图_20220210190813](/微信截图_20220210190813.png)

![/微信截图_20220210190823](/微信截图_20220210190823.png)

![/微信截图_20220210190835](/微信截图_20220210190835.png)

![/微信截图_20220210190846](/微信截图_20220210190846.png)

![/微信截图_20220210190853](/微信截图_20220210190853.png)

![/微信截图_20220210190902](/微信截图_20220210190902.png)

![/微信截图_20220210190910](/微信截图_20220210190910.png)

![/微信截图_20220210190920](/微信截图_20220210190920.png)

### Path Tracing

![/微信截图_20220210191004](/微信截图_20220210191004.png)

![/微信截图_20220210191014](/微信截图_20220210191014.png)

![/微信截图_20220210201019](/微信截图_20220210201019.png)

![/微信截图_20220210201027](/微信截图_20220210201027.png)

![/微信截图_20220210201037](/微信截图_20220210201037.png)

![/微信截图_20220210201046](/微信截图_20220210201046.png)

![/微信截图_20220210201055](/微信截图_20220210201055.png)

![/微信截图_20220210201110](/微信截图_20220210201110.png)

![/微信截图_20220210201118](/微信截图_20220210201118.png)

![/微信截图_20220210201125](/微信截图_20220210201125.png)

![/微信截图_20220210201134](/微信截图_20220210201134.png)

![/微信截图_20220210201150](/微信截图_20220210201150.png)

![/微信截图_20220210201158](/微信截图_20220210201158.png)

![/微信截图_20220210201206](/微信截图_20220210201206.png)

![/微信截图_20220210201214](/微信截图_20220210201214.png)

![/微信截图_20220210201222](/微信截图_20220210201222.png)

![/微信截图_20220210201232](/微信截图_20220210201232.png)

![/微信截图_20220210201241](/微信截图_20220210201241.png)

![/微信截图_20220210201249](/微信截图_20220210201249.png)

![/微信截图_20220210201302](/微信截图_20220210201302.png)

![/微信截图_20220210201311](/微信截图_20220210201311.png)

![/微信截图_20220210201320](/微信截图_20220210201320.png)

![/微信截图_20220210201330](/微信截图_20220210201330.png)

![/微信截图_20220210201338](/微信截图_20220210201338.png)

![/微信截图_20220210201356](/微信截图_20220210201356.png)

![/微信截图_20220210201407](/微信截图_20220210201407.png)

![/微信截图_20220210201415](/微信截图_20220210201415.png)

![/微信截图_20220210201423](/微信截图_20220210201423.png)

![/微信截图_20220210201433](/微信截图_20220210201433.png)

![/微信截图_20220210201444](/微信截图_20220210201444.png)

![/微信截图_20220210201457](/微信截图_20220210201457.png)

http://www.graphics.cornell.edu/online/box/compare.html

![/微信截图_20220210201620](/微信截图_20220210201620.png)

![/微信截图_20220210201629](/微信截图_20220210201629.png)

![/微信截图_20220210201637](/微信截图_20220210201637.png)

## Materials and Appearances

![/微信截图_20220210210841](/微信截图_20220210210841.png)

![/微信截图_20220210210851](/微信截图_20220210210851.png)

### Material == BRDF

![/微信截图_20220210210859](/微信截图_20220210210859.png)

![/微信截图_20220210210950](/微信截图_20220210210950.png)

![/微信截图_20220210210957](/微信截图_20220210210957.png)

![/微信截图_20220210211003](/微信截图_20220210211003.png)

![/微信截图_20220210211014](/微信截图_20220210211014.png)

![/微信截图_20220210211022](/微信截图_20220210211022.png)

![/微信截图_20220210211033](/微信截图_20220210211033.png)

![/微信截图_20220210211047](/微信截图_20220210211047.png)

![/微信截图_20220210211056](/微信截图_20220210211056.png)

![/微信截图_20220210211106](/微信截图_20220210211106.png)

![/微信截图_20220210211114](/微信截图_20220210211114.png)

![/微信截图_20220210211123](/微信截图_20220210211123.png)

![/微信截图_20220210211132](/微信截图_20220210211132.png)

![/微信截图_20220210211140](/微信截图_20220210211140.png)

![/微信截图_20220210211149](/微信截图_20220210211149.png)

![/微信截图_20220210211157](/微信截图_20220210211157.png)

![/微信截图_20220210211205](/微信截图_20220210211205.png)

![/微信截图_20220210211213](/微信截图_20220210211213.png)

### Microfacet Material

![/微信截图_20220210212942](/微信截图_20220210212942.png)

![/微信截图_20220210212952](/微信截图_20220210212952.png)

![/微信截图_20220210213001](/微信截图_20220210213001.png)

![/微信截图_20220210213009](/微信截图_20220210213009.png)

![/微信截图_20220210213018](/微信截图_20220210213018.png)

![/微信截图_20220210213029](/微信截图_20220210213029.png)

![/微信截图_20220210213037](/微信截图_20220210213037.png)

![/微信截图_20220210213045](/微信截图_20220210213045.png)

![/微信截图_20220210213128](/微信截图_20220210213128.png)

![/微信截图_20220210213139](/微信截图_20220210213139.png)

![/微信截图_20220210213154](/微信截图_20220210213154.png)

![/微信截图_20220210213204](/微信截图_20220210213204.png)

![/微信截图_20220210213213](/微信截图_20220210213213.png)

![/微信截图_20220210213221](/微信截图_20220210213221.png)

![/微信截图_20220210213231](/微信截图_20220210213231.png)

### Measuring BRDFs

![/微信截图_20220210213336](/微信截图_20220210213336.png)

![/微信截图_20220210213432](/微信截图_20220210213432.png)

![/微信截图_20220210213445](/微信截图_20220210213445.png)

![/微信截图_20220210213513](/微信截图_20220210213513.png)

![/微信截图_20220210213522](/微信截图_20220210213522.png)

![/微信截图_20220210213530](/微信截图_20220210213530.png)

![/微信截图_20220210213540](/微信截图_20220210213540.png)

## Advanced Light Transport

![/微信截图_20220210215306](/微信截图_20220210215306.png)

### Unbiased light transport methods

![/微信截图_20220210215314](/微信截图_20220210215314.png)

![/微信截图_20220210215321](/微信截图_20220210215321.png)

![/微信截图_20220210215330](/微信截图_20220210215330.png)

![/微信截图_20220210215338](/微信截图_20220210215338.png)

![/微信截图_20220210215346](/微信截图_20220210215346.png)

![/微信截图_20220210215355](/微信截图_20220210215355.png)

### Biased light transport methods

![/微信截图_20220210215403](/微信截图_20220210215403.png)

![/微信截图_20220210215411](/微信截图_20220210215411.png)

![/微信截图_20220210215423](/微信截图_20220210215423.png)

![/微信截图_20220210215438](/微信截图_20220210215438.png)

![/微信截图_20220210215445](/微信截图_20220210215445.png)

![/微信截图_20220210215455](/微信截图_20220210215455.png)

### Instant radiosity (VPL / many light methods)

![/微信截图_20220210215505](/微信截图_20220210215505.png)

![/微信截图_20220210215518](/微信截图_20220210215518.png)

## Advanced Appearance Modeling

![/微信截图_20220210215613](/微信截图_20220210215613.png)

### Non-surface models

![/微信截图_20220210215705](/微信截图_20220210215705.png)

![/微信截图_20220210215716](/微信截图_20220210215716.png)

![/微信截图_20220210215726](/微信截图_20220210215726.png)

![/微信截图_20220210215736](/微信截图_20220210215736.png)

![/微信截图_20220210215746](/微信截图_20220210215746.png)

![/微信截图_20220210215757](/微信截图_20220210215757.png)

![/微信截图_20220210215811](/微信截图_20220210215811.png)

![/微信截图_20220210215823](/微信截图_20220210215823.png)

![/微信截图_20220210215832](/微信截图_20220210215832.png)

![/微信截图_20220210215839](/微信截图_20220210215839.png)

![/微信截图_20220210215848](/微信截图_20220210215848.png)

![/微信截图_20220210215856](/微信截图_20220210215856.png)

![/微信截图_20220210215906](/微信截图_20220210215906.png)

![/微信截图_20220210215924](/微信截图_20220210215924.png)

![/微信截图_20220210215934](/微信截图_20220210215934.png)

![/微信截图_20220210215944](/微信截图_20220210215944.png)

![/微信截图_20220210220004](/微信截图_20220210220004.png)

![/微信截图_20220210220013](/微信截图_20220210220013.png)

![/微信截图_20220210220027](/微信截图_20220210220027.png)

![/微信截图_20220210220036](/微信截图_20220210220036.png)

![/微信截图_20220210220048](/微信截图_20220210220048.png)

![/微信截图_20220210220054](/微信截图_20220210220054.png)

![/微信截图_20220210220102](/微信截图_20220210220102.png)

![/微信截图_20220210220112](/微信截图_20220210220112.png)

![/微信截图_20220210220123](/微信截图_20220210220123.png)

![/微信截图_20220210220132](/微信截图_20220210220132.png)

![/微信截图_20220210220140](/微信截图_20220210220140.png)

![/微信截图_20220210220150](/微信截图_20220210220150.png)

![/微信截图_20220210220159](/微信截图_20220210220159.png)

![/微信截图_20220210220209](/微信截图_20220210220209.png)

### Surface models

![/微信截图_20220210220415](/微信截图_20220210220415.png)

![/微信截图_20220210220423](/微信截图_20220210220423.png)

![/微信截图_20220210220431](/微信截图_20220210220431.png)

![/微信截图_20220210220439](/微信截图_20220210220439.png)

![/微信截图_20220210220446](/微信截图_20220210220446.png)

![/微信截图_20220210220454](/微信截图_20220210220454.png)

![/微信截图_20220210220502](/微信截图_20220210220502.png)

![/微信截图_20220210220513](/微信截图_20220210220513.png)

![/微信截图_20220210220521](/微信截图_20220210220521.png)

![/微信截图_20220210220536](/微信截图_20220210220536.png)

![/微信截图_20220210220601](/微信截图_20220210220601.png)

![/微信截图_20220210220614](/微信截图_20220210220614.png)

![/微信截图_20220210220623](/微信截图_20220210220623.png)

![/微信截图_20220210220631](/微信截图_20220210220631.png)

![/微信截图_20220210220643](/微信截图_20220210220643.png)

![/微信截图_20220210220653](/微信截图_20220210220653.png)

![/微信截图_20220210220704](/微信截图_20220210220704.png)

![/微信截图_20220210220717](/微信截图_20220210220717.png)

![/微信截图_20220210220727](/微信截图_20220210220727.png)

![/微信截图_20220210220739](/微信截图_20220210220739.png)

![/微信截图_20220210220748](/微信截图_20220210220748.png)

![/微信截图_20220210220756](/微信截图_20220210220756.png)

![/微信截图_20220210220803](/微信截图_20220210220803.png)

![/微信截图_20220210220810](/微信截图_20220210220810.png)

![/微信截图_20220210220819](/微信截图_20220210220819.png)

![/微信截图_20220210220834](/微信截图_20220210220834.png)

![/微信截图_20220210220844](/微信截图_20220210220844.png)

![/微信截图_20220210220855](/微信截图_20220210220855.png)

![/微信截图_20220210220903](/微信截图_20220210220903.png)

![/微信截图_20220210220911](/微信截图_20220210220911.png)

![/微信截图_20220210220920](/微信截图_20220210220920.png)

![/微信截图_20220210220929](/微信截图_20220210220929.png)

![/微信截图_20220210220948](/微信截图_20220210220948.png)

![/微信截图_20220210220959](/微信截图_20220210220959.png)

![/微信截图_20220210221009](/微信截图_20220210221009.png)

![/微信截图_20220210221019](/微信截图_20220210221019.png)

![/微信截图_20220210221028](/微信截图_20220210221028.png)

![/微信截图_20220210221036](/微信截图_20220210221036.png)

![/微信截图_20220210221045](/微信截图_20220210221045.png)

![/微信截图_20220210221059](/微信截图_20220210221059.png)

![/微信截图_20220210221115](/微信截图_20220210221115.png)

![/微信截图_20220210221123](/微信截图_20220210221123.png)

### Procedural appearance

![/微信截图_20220210221758](/微信截图_20220210221758.png)

![/微信截图_20220210221807](/微信截图_20220210221807.png)

![/微信截图_20220210221817](/微信截图_20220210221817.png)

![/微信截图_20220210221824](/微信截图_20220210221824.png)

![/微信截图_20220210221833](/微信截图_20220210221833.png)

![/微信截图_20220210221842](/微信截图_20220210221842.png)

![/微信截图_20220210221851](/微信截图_20220210221851.png)

## Cameras, Lenses and Light Fields

### Imaging = Synthesis + Capture

![/微信截图_20220211001147](/微信截图_20220211001147.png)

![/微信截图_20220211001157](/微信截图_20220211001157.png)

![/微信截图_20220211001212](/微信截图_20220211001212.png)

![/微信截图_20220211001222](/微信截图_20220211001222.png)

![/微信截图_20220211001230](/微信截图_20220211001230.png)

![/微信截图_20220211001242](/微信截图_20220211001242.png)

### Pinhole Image Formation

![/微信截图_20220211001329](/微信截图_20220211001329.png)

![/微信截图_20220211001355](/微信截图_20220211001355.png)

![/微信截图_20220211001409](/微信截图_20220211001409.png)

### Filed of View (FOV)

![/微信截图_20220211001450](/微信截图_20220211001450.png)

![/微信截图_20220211001458](/微信截图_20220211001458.png)

![/微信截图_20220211001505](/微信截图_20220211001505.png)

![/微信截图_20220211001523](/微信截图_20220211001523.png)

![/微信截图_20220211001531](/微信截图_20220211001531.png)

![/微信截图_20220211001540](/微信截图_20220211001540.png)

### Exposure

![/微信截图_20220211001608](/微信截图_20220211001608.png)

![/微信截图_20220211001621](/微信截图_20220211001621.png)

![/微信截图_20220211001629](/微信截图_20220211001629.png)

![/微信截图_20220211001635](/微信截图_20220211001635.png)

![/微信截图_20220211001642](/微信截图_20220211001642.png)

![/微信截图_20220211001650](/微信截图_20220211001650.png)

![/微信截图_20220211001658](/微信截图_20220211001658.png)

![/微信截图_20220211001706](/微信截图_20220211001706.png)

![/微信截图_20220211001714](/微信截图_20220211001714.png)

![/微信截图_20220211001722](/微信截图_20220211001722.png)

![/微信截图_20220211001732](/微信截图_20220211001732.png)

### Fast and Slow Photography

![/微信截图_20220211001817](/微信截图_20220211001817.png)

![/微信截图_20220211001828](/微信截图_20220211001828.png)

![/微信截图_20220211001840](/微信截图_20220211001840.png)

![/微信截图_20220211001849](/微信截图_20220211001849.png)

![/微信截图_20220211001858](/微信截图_20220211001858.png)

### Thin Lens Approximation

![/微信截图_20220211001937](/微信截图_20220211001937.png)

![/微信截图_20220211001944](/微信截图_20220211001944.png)

![/微信截图_20220211001954](/微信截图_20220211001954.png)

![/微信截图_20220211002002](/微信截图_20220211002002.png)

![/微信截图_20220211002010](/微信截图_20220211002010.png)

![/微信截图_20220211002016](/微信截图_20220211002016.png)

![/微信截图_20220211002023](/微信截图_20220211002023.png)

![/微信截图_20220211002030](/微信截图_20220211002030.png)

![/微信截图_20220211002038](/微信截图_20220211002038.png)

http://graphics.stanford.edu/courses/cs178-10/applets/gaussian.html

### Defocus Blur

![/微信截图_20220211002116](/微信截图_20220211002116.png)

![/微信截图_20220211002125](/微信截图_20220211002125.png)

![/微信截图_20220211002133](/微信截图_20220211002133.png)

![/微信截图_20220211002139](/微信截图_20220211002139.png)

![/微信截图_20220211002146](/微信截图_20220211002146.png)

### Ray Tracing Ideal Thin Lenses

![/微信截图_20220211002221](/微信截图_20220211002221.png)

![/微信截图_20220211002228](/微信截图_20220211002228.png)

![/微信截图_20220211002235](/微信截图_20220211002235.png)

### Depth of Field

![/微信截图_20220211002309](/微信截图_20220211002309.png)

![/微信截图_20220211002317](/微信截图_20220211002317.png)

![/微信截图_20220211002325](/微信截图_20220211002325.png)

![/微信截图_20220211002335](/微信截图_20220211002335.png)

http://graphics.stanford.edu/courses/cs178/applets/dof.html

### Light Field / Lumigraph

![/微信截图_20220211024610](/微信截图_20220211024610.png)

![/微信截图_20220211024620](/微信截图_20220211024620.png)

![/微信截图_20220211024628](/微信截图_20220211024628.png)

![/微信截图_20220211024634](/微信截图_20220211024634.png)

![/微信截图_20220211024642](/微信截图_20220211024642.png)

![/微信截图_20220211024650](/微信截图_20220211024650.png)

![/微信截图_20220211024657](/微信截图_20220211024657.png)

![/微信截图_20220211024705](/微信截图_20220211024705.png)

![/微信截图_20220211024724](/微信截图_20220211024724.png)

![/微信截图_20220211024732](/微信截图_20220211024732.png)

![/微信截图_20220211024738](/微信截图_20220211024738.png)

![/微信截图_20220211024745](/微信截图_20220211024745.png)

![/微信截图_20220211024755](/微信截图_20220211024755.png)

![/微信截图_20220211024803](/微信截图_20220211024803.png)

![/微信截图_20220211024810](/微信截图_20220211024810.png)

![/微信截图_20220211024817](/微信截图_20220211024817.png)

![/微信截图_20220211024824](/微信截图_20220211024824.png)

![/微信截图_20220211024832](/微信截图_20220211024832.png)

![/微信截图_20220211024843](/微信截图_20220211024843.png)

![/微信截图_20220211024851](/微信截图_20220211024851.png)

![/微信截图_20220211024902](/微信截图_20220211024902.png)

#### Light Field Camera

![/微信截图_20220211025056](/微信截图_20220211025056.png)

![/微信截图_20220211025104](/微信截图_20220211025104.png)

![/微信截图_20220211025114](/微信截图_20220211025114.png)

![/微信截图_20220211025124](/微信截图_20220211025124.png)

![/微信截图_20220211025133](/微信截图_20220211025133.png)

![/微信截图_20220211025141](/微信截图_20220211025141.png)

![/微信截图_20220211025149](/微信截图_20220211025149.png)

![/微信截图_20220211025156](/微信截图_20220211025156.png)

![/微信截图_20220211025204](/微信截图_20220211025204.png)

![/微信截图_20220211025211](/微信截图_20220211025211.png)

### Color

#### What is color ?

##### Physical Basis of Color

![/微信截图_20220211025248](/微信截图_20220211025248.png)

![/微信截图_20220211025254](/微信截图_20220211025254.png)

![/微信截图_20220211025301](/微信截图_20220211025301.png)

![/微信截图_20220211025307](/微信截图_20220211025307.png)

![/微信截图_20220211025314](/微信截图_20220211025314.png)

![/微信截图_20220211025321](/微信截图_20220211025321.png)

![/微信截图_20220211025414](/微信截图_20220211025414.png)

##### Biological Basis of Color

![/微信截图_20220211025456](/微信截图_20220211025456.png)

![/微信截图_20220211025504](/微信截图_20220211025504.png)

![/微信截图_20220211025511](/微信截图_20220211025511.png)

![/微信截图_20220211025518](/微信截图_20220211025518.png)

#### Color perception

##### Tristimulus Theory of Color

![/微信截图_20220211025559](/微信截图_20220211025559.png)

![/微信截图_20220211025606](/微信截图_20220211025606.png)

##### Metamerism (同色异谱)

![/微信截图_20220211025653](/微信截图_20220211025653.png)

![/微信截图_20220211025659](/微信截图_20220211025659.png)

![/微信截图_20220211025708](/微信截图_20220211025708.png)

#### Color reproduction / matching

![/微信截图_20220211025821](/微信截图_20220211025821.png)

![/微信截图_20220211025828](/微信截图_20220211025828.png)

![/微信截图_20220211025833](/微信截图_20220211025833.png)

![/微信截图_20220211025840](/微信截图_20220211025840.png)

![/微信截图_20220211025847](/微信截图_20220211025847.png)

![/微信截图_20220211025853](/微信截图_20220211025853.png)

![/微信截图_20220211025859](/微信截图_20220211025859.png)

![/微信截图_20220211025905](/微信截图_20220211025905.png)

![/微信截图_20220211025912](/微信截图_20220211025912.png)

![/微信截图_20220211025919](/微信截图_20220211025919.png)

![/微信截图_20220211025927](/微信截图_20220211025927.png)

![/微信截图_20220211025934](/微信截图_20220211025934.png)

![/微信截图_20220211025941](/微信截图_20220211025941.png)

#### Color space

![/微信截图_20220211030007](/微信截图_20220211030007.png)

![/微信截图_20220211030013](/微信截图_20220211030013.png)

![/微信截图_20220211030020](/微信截图_20220211030020.png)

![/微信截图_20220211030031](/微信截图_20220211030031.png)

![/微信截图_20220211030037](/微信截图_20220211030037.png)

![/微信截图_20220211030043](/微信截图_20220211030043.png)

#### Perceptually Organized Color Spaces

![/微信截图_20220211030122](/微信截图_20220211030122.png)

![/微信截图_20220211030129](/微信截图_20220211030129.png)

![/微信截图_20220211030136](/微信截图_20220211030136.png)

![/微信截图_20220211030142](/微信截图_20220211030142.png)

![/微信截图_20220211030149](/微信截图_20220211030149.png)

![/微信截图_20220211030204](/微信截图_20220211030204.png)

![/微信截图_20220211030213](/微信截图_20220211030213.png)

![/微信截图_20220211030221](/微信截图_20220211030221.png)

![/微信截图_20220211030230](/微信截图_20220211030230.png)

![/微信截图_20220211030236](/微信截图_20220211030236.png)

![/微信截图_20220211030243](/微信截图_20220211030243.png)

![/微信截图_20220211030251](/微信截图_20220211030251.png)

![/微信截图_20220211030259](/微信截图_20220211030259.png)

![/微信截图_20220211030305](/微信截图_20220211030305.png)

![/微信截图_20220211030312](/微信截图_20220211030312.png)

![/微信截图_20220211030319](/微信截图_20220211030319.png)

![/微信截图_20220211030326](/微信截图_20220211030326.png)

![/微信截图_20220211030332](/微信截图_20220211030332.png)

## Animation

![/微信截图_20220211184525](/微信截图_20220211184525.png)

### Historical Points in Animation

![/微信截图_20220211184551](/微信截图_20220211184551.png)

![/微信截图_20220211184600](/微信截图_20220211184600.png)

![/微信截图_20220211184608](/微信截图_20220211184608.png)

![/微信截图_20220211184616](/微信截图_20220211184616.png)

![/微信截图_20220211184624](/微信截图_20220211184624.png)

![/微信截图_20220211184631](/微信截图_20220211184631.png)

![/微信截图_20220211184639](/微信截图_20220211184639.png)

![/微信截图_20220211184646](/微信截图_20220211184646.png)

![/微信截图_20220211184657](/微信截图_20220211184657.png)

![/微信截图_20220211184709](/微信截图_20220211184709.png)

### Keyframe Animation

![/微信截图_20220211184759](/微信截图_20220211184759.png)

![/微信截图_20220211184805](/微信截图_20220211184805.png)

![/微信截图_20220211184812](/微信截图_20220211184812.png)

### Physical Simulation

![/微信截图_20220211184838](/微信截图_20220211184838.png)

![/微信截图_20220211184844](/微信截图_20220211184844.png)

![/微信截图_20220211184852](/微信截图_20220211184852.png)

![/微信截图_20220211184859](/微信截图_20220211184859.png)

#### Mass Spring System: Example of Modeling a Dynamic System

![/微信截图_20220211185011](picture/微信截图_20220211185011.png)

![/微信截图_20220212000421](picture/微信截图_20220212000421.png)

![/微信截图_20220211185029](picture/微信截图_20220211185029.png)

![/微信截图_20220211185034](picture/微信截图_20220211185034.png)

![/微信截图_20220211185040](picture/微信截图_20220211185040.png)

![/微信截图_20220211185046](picture/微信截图_20220211185046.png)

![/微信截图_20220211185052](picture/微信截图_20220211185052.png)

![/微信截图_20220211185059](picture/微信截图_20220211185059.png)

![/微信截图_20220211185105](picture/微信截图_20220211185105.png)

![/微信截图_20220211185111](picture/微信截图_20220211185111.png)

![/微信截图_20220211185118](picture/微信截图_20220211185118.png)

![/微信截图_20220211185126](picture/微信截图_20220211185126.png)

![/微信截图_20220211185136](picture/微信截图_20220211185136.png)

![/微信截图_20220211185143](picture/微信截图_20220211185143.png)

### Particle Systems

![/微信截图_20220211200403](picture/微信截图_20220211200403.png)

![/微信截图_20220211200410](picture/微信截图_20220211200410.png)

![/微信截图_20220211200416](picture/微信截图_20220211200416.png)

![/微信截图_20220211200424](picture/微信截图_20220211200424.png)

![/微信截图_20220211200432](picture/微信截图_20220211200432.png)

![/微信截图_20220211200440](picture/微信截图_20220211200440.png)

![/微信截图_20220211200450](picture/微信截图_20220211200450.png)

![/微信截图_20220211200457](picture/微信截图_20220211200457.png)

![/微信截图_20220211200510](picture/微信截图_20220211200510.png)

### Kinematics

#### Forward Kinematics

![/微信截图_20220211200546](picture/微信截图_20220211200546.png)

![/微信截图_20220211200558](picture/微信截图_20220211200558.png)

![/微信截图_20220211200606](picture/微信截图_20220211200606.png)

![/微信截图_20220211200613](picture/微信截图_20220211200613.png)

![/微信截图_20220211200623](picture/微信截图_20220211200623.png)

![/微信截图_20220211200630](picture/微信截图_20220211200630.png)

#### Inverse Kinematics

![/微信截图_20220211200705](picture/微信截图_20220211200705.png)

![/微信截图_20220211200711](picture/微信截图_20220211200711.png)

![/微信截图_20220211200720](picture/微信截图_20220211200720.png)

![/微信截图_20220211200726](picture/微信截图_20220211200726.png)

![/微信截图_20220211200732](picture/微信截图_20220211200732.png)

![/微信截图_20220211200739](picture/微信截图_20220211200739.png)

![/微信截图_20220211200745](picture/微信截图_20220211200745.png)

![/微信截图_20220211200753](picture/微信截图_20220211200753.png)

### Rigging

![/微信截图_20220211200823](picture/微信截图_20220211200823.png)

![/微信截图_20220211200830](picture/微信截图_20220211200830.png)

![/微信截图_20220211200843](picture/微信截图_20220211200843.png)

![/微信截图_20220211200853](picture/微信截图_20220211200853.png)

### Motion Capture

![/微信截图_20220211200925](picture/微信截图_20220211200925.png)

![/微信截图_20220211200932](picture/微信截图_20220211200932.png)

![/微信截图_20220211200944](picture/微信截图_20220211200944.png)

![/微信截图_20220211200954](picture/微信截图_20220211200954.png)

![/微信截图_20220211201002](picture/微信截图_20220211201002.png)

![/微信截图_20220211201009](picture/微信截图_20220211201009.png)

![/微信截图_20220211201021](picture/微信截图_20220211201021.png)

![/微信截图_20220211201029](picture/微信截图_20220211201029.png)

![/微信截图_20220211201043](picture/微信截图_20220211201043.png)

![/微信截图_20220211201103](picture/微信截图_20220211201103.png)

## Simulation

### Single particle simulation

![/微信截图_20220211201354](picture/微信截图_20220211201354.png)

![/微信截图_20220211202432](picture/微信截图_20220211202432.png)

![/微信截图_20220211202439](picture/微信截图_20220211202439.png)

![/微信截图_20220211202444](picture/微信截图_20220211202444.png)

![/微信截图_20220211202452](picture/微信截图_20220211202452.png)

![/微信截图_20220211202500](picture/微信截图_20220211202500.png)

![/微信截图_20220211202507](picture/微信截图_20220211202507.png)

![/微信截图_20220211202516](picture/微信截图_20220211202516.png)

#### Combating Instability

![/微信截图_20220211202651](picture/微信截图_20220211202651.png)

![/微信截图_20220211202658](picture/微信截图_20220211202658.png)

![/微信截图_20220211202706](picture/微信截图_20220211202706.png)

![/微信截图_20220211202713](picture/微信截图_20220211202713.png)

![/微信截图_20220211202721](picture/微信截图_20220211202721.png)

![/微信截图_20220211202727](picture/微信截图_20220211202727.png)

![/微信截图_20220211202734](picture/微信截图_20220211202734.png)

![/微信截图_20220211202740](picture/微信截图_20220211202740.png)

![/微信截图_20220211202747](picture/微信截图_20220211202747.png)

### Rigid body simulation

![/微信截图_20220211202753](picture/微信截图_20220211202753.png)

### Fluid simulation

![/微信截图_20220211202914](picture/微信截图_20220211202914.png)

![/微信截图_20220211202920](picture/微信截图_20220211202920.png)

![/微信截图_20220211202928](picture/微信截图_20220211202928.png)

