# Modern Computer Graphics

It's a notebook of games-101 which is instructed by Lingqi Yan

![微信截图_20220212021425](picture/微信截图_20220212021425.png)

Class link： https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html



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

### rotate 

**about the origin (0,0), CCW by default**

$$R_\theta=\begin{bmatrix}cos\theta&-sin\theta\\sin\theta&cos\theta\end{bmatrix}$$

$$R_{-\theta}=\begin{bmatrix}cos\theta&sin\theta\\-sin\theta&cos\theta\end{bmatrix}=R^T_\theta=R^{-1}_\theta$$

### linear transforms

**linear transforms = matrix (of the same dimension)**

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

![picture/Screenshot from 2022-01-10 14-25-25](picture/Screenshot-2022-01-10-14-25-25.png)

$$R(\alpha)=\begin{pmatrix}cos\alpha& -sin\alpha &0\\sin\alpha & cos\alpha&0\\0&0&1\end{pmatrix}$$

#### translation

$$T(t_x,t_y)=\begin{pmatrix}1&0&t_x\\0&1&t_y\\0&0&1\end{pmatrix}$$

### composite transformation

#### transform ordering matters!!!

- if translate then rotate? or rotate then translate? --> rotate then translate is  correct

  $$T_{(1,0)}\cdot R_{45}\begin{bmatrix}x\\y\\1\end{bmatrix}=\begin{bmatrix}1&0&1\\0&1&0\\0&0&1\end{bmatrix}\begin{bmatrix}cos45^{\circ} &-sin45^{\circ} &0\\sin45^{\circ} &cos45^{\circ} &0\\0&0&1\end{bmatrix}\begin{bmatrix}x\\y\\1\end{bmatrix} $$

  **Note that matrices are applied right to left**

#### sequence of affine transforms

- compose by matrix multiplication
- $$A_n(...A_2(A_1(x)))=A_n...A_2\cdot A_1\cdot \begin{pmatrix}x\\y\\1\end{pmatrix}$$

### Decomposing complex transforms

#### Example: How to rotate around a given point?

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

#### 3D scale

$$S(s_x,s_y,s_z)=\begin{pmatrix}s_x&0&0&0\\0&s_y&0&0\\0&0&s_z&0\\0&0&0&1\end{pmatrix}$$

#### 3D translation

$$T(t_x,t_y,t_z)=\begin{pmatrix}1&0&0&t_x\\0&1&0&t_y\\0&0&1&t_z\\0&0&0&1\end{pmatrix}$$

#### rotation around x, y, or z-axis

![picture/Screenshot from 2022-01-10 14-23-11](picture/Screenshot-2022-01-10-14-23-11.png)

$$R_x(\alpha)=\begin{pmatrix}1&0&0&0\\0&cos\alpha&-sin\alpha&0\\0&sin\alpha&cos\alpha&0\\0&0&0&1\end{pmatrix}$$

$$R_x(\alpha)=\begin{pmatrix}cos\alpha&0&sin\alpha&0\\0&1&0&0\\-sin\alpha&0&cos\alpha&0\\0&0&0&1\end{pmatrix}$$

$$R_x(\alpha)=\begin{pmatrix}cos\alpha&-sin\alpha&0&0\\sin\alpha&cos\alpha&0&0\\0&0&1&0\\0&0&0&1\end{pmatrix}$$

#### 3D rotations

compose any 3D rotation from $R_x,R_y,R_z$

$$R_{xyz}(\alpha,\beta,\gamma)=R_x(\alpha)R_y(\beta)R_z(\gamma)$$

- so-called Euler angles
- often used in flight simulator : roll,pitch, yaw

![picture/Screenshot from 2022-01-10 14-23-55](picture/Screenshot-2022-01-10-14-23-55.png)

##### Rodrigues' rotation formula

- rotation by angle $\alpha$ around axis $n$

  $$R(n,\alpha)=cos(\alpha)I+(1-cos(\alpha))nn^T+sin(\alpha)\begin{pmatrix}0&-n_z&n_y\\n_z&0&-n_x\\-n_y&n_x&0\end{pmatrix}$$

**prove**

![picture/Screenshot from 2022-01-10 13-40-33](picture/Screenshot-2022-01-10-13-40-33.png)

![picture/Screenshot from 2022-01-10 13-40-40](picture/Screenshot-2022-01-10-13-40-40.png)

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

![picture/Screenshot from 2022-01-10 14-24-26](picture/Screenshot-2022-01-10-14-24-26.png)

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

![picture/Screenshot from 2022-01-10 14-54-06](picture/Screenshot-2022-01-10-14-54-06.png)

#### orthographic projection

- camera located at origin, looking at -Z, up at Y
- drop Z coordinate
- translate and scale the resulting rectangle to $[-1,1]^2$

![picture/Screenshot from 2022-01-10 14-58-31](picture/Screenshot-2022-01-10-14-58-31.png)

- in general, we want to map a cuboid $[l,r]\times [b,t]\times[f,n]$ to the "canonical" cube $[-1,1]^3$
  - if something nearer to us , z's value will be samller

![picture/Screenshot from 2022-01-10 15-02-09](picture/Screenshot-2022-01-10-15-02-09.png)

- transformation matrix

  - translate (center to origin) first, then scale (length/width/height to 2)

    $$M_{ortho}=\begin{bmatrix}\frac{2}{r-l}&0&0&0\\0&\frac{2}{t-b}&0&0\\0&0&\frac{2}{n-f}&0\\0&0&0&1\end{bmatrix}\begin{bmatrix}1&0&0&-\frac{r+l}{2}\\0&1&0&-\frac{t+b}{2}\\0&0&1&-\frac{n+f}{2}\\0&0&0&1\end{bmatrix}$$

- caveat

  - looking at / along -Z is making near and far not intuitive (n>f)
  - FYI : that's why OpenGL uses left hand coords

#### perspective projection

![picture/Screenshot from 2022-01-10 15-10-45](picture/Screenshot-2022-01-10-15-10-45.png)

- most common projection
- further objects are smaller
- parallel lines not parallel
- coverage to single points

##### how to do perspective projection?

- first "squish" the frustum into a cuboid (n->n, f->f) ($M_{persp->ortho}$)
- do orthographic projection ($M_{ortho}$, already known!)

![picture/Screenshot from 2022-01-10 15-15-29](picture/Screenshot-2022-01-10-15-15-29.png)

##### how to get transformation?

- recall the key idea : find the relationship between transformed points $(x',y',z')$ and the original points $(x,y,z)$
- $$y'=\frac{n}{z}y$$
- $$x'=\frac{n}{z}x$$

![picture/Screenshot from 2022-01-10 15-20-08](picture/Screenshot-2022-01-10-15-20-08.png)

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

![picture/Screenshot from 2022-01-10 15-48-57](picture/Screenshot-2022-01-10-15-48-57.png)

- how to convert from fovY and aspect to l,r,b,t?
  - trivial

![picture/Screenshot from 2022-01-12 22-53-32](picture/Screenshot-2022-01-12-22-53-32.png)

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

![picture/Screenshot from 2022-01-12 23-10-31](picture/微信截图_20220114151354.png)

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

![picture/微信截图_20220114150218](picture/微信截图_20220114150218.png)

- input : position of triangle vertices projected on screen
- output : set of pixel values approximating triangle

###### a simple approach: sampling

evaluating a function at a point is sampling, we can discretize a function by sampling

```c++
for (int x = 0; x < xmax; ++x)
	output[x] = f(x);
```

sampling is a core idea in graphics, we sample time (1D), area (2D), direction (2D), volume (3D)

we can sample if each pixel center is inside triangle

![picture/微信截图_20220114151354](picture/微信截图_20220114151354.png)

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

![picture/微信截图_20220114154310](picture/微信截图_20220114154310.png)

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

![picture/微信截图_20220114160649](picture/微信截图_20220114160649.png)

but why?

- why undersampling introduces aliasing?
- why pre-filtering then sampling can do antialiasing?

#### frequency domain

cosine’s frequencies $cos2\pi f x$ where $f=\frac{1}{T}$

![picture/微信截图_20220114182508](picture/微信截图_20220114182508.png)

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

![picture/微信截图_20220114182913](picture/微信截图_20220114182913.png)

![picture/微信截图_20220114182935](picture/微信截图_20220114182935.png)

![picture/微信截图_20220114182955](picture/微信截图_20220114182955.png)

![picture/微信截图_20220114183236](picture/微信截图_20220114183236.png)

![picture/微信截图_20220114183303](picture/微信截图_20220114183303.png)

##### **filtering = convolution (= averaging)**

#### convolution

![picture/微信截图_20220114183618](picture/微信截图_20220114183618.png)

- convolution in the spatial domain is equal to multiplication in the frequency, and vice versa
- filter by convolution in the spatial domain
- transform to frequency domain (Fourier transform)
- multiply by Fourier transform of convolution kernel
- transform back to spatial domain (inverse Fourier)

![picture/微信截图_20220114183930](picture/微信截图_20220114183930.png)

![picture/微信截图_20220114184105](picture/微信截图_20220114184105.png)

![picture/微信截图_20220114184146](picture/微信截图_20220114184146.png)

![picture/微信截图_20220114184205](picture/微信截图_20220114184205.png)

##### **Sampling = Repeating frequency contents**

![picture/微信截图_20220114184342](picture/微信截图_20220114184342.png)

##### Aliasing = Mixed Frequency Contents

![picture/微信截图_20220114184544](picture/微信截图_20220114184544.png)

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

![picture/微信截图_20220114185110](picture/微信截图_20220114185110.png)

##### antialiasing by averaging values in pixel area

solution : 

- convolve f(x,y) by a 1-pixel box-blur
  - recall : convolving = filtering = averaging
- then sample at every pixel’s center

![picture/微信截图_20220114185412](picture/微信截图_20220114185412.png)

##### Antialiasing By Supersampling (MSAA)

![picture/微信截图_20220114185550](picture/微信截图_20220114185550.png)

![picture/微信截图_20220114185643](picture/微信截图_20220114185643.png)

![picture/微信截图_20220114185706](picture/微信截图_20220114185706.png)

![picture/微信截图_20220114185726](picture/微信截图_20220114185726.png)

![picture/微信截图_20220114185804](picture/微信截图_20220114185804.png)

![picture/微信截图_20220114185804](picture/微信截图_20220114185804.png)

![picture/微信截图_20220114185839](picture/微信截图_20220114185839.png)

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

![picture/微信截图_20220114191440](picture/微信截图_20220114191440.png)

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

![picture/微信截图_20220114201126](picture/微信截图_20220114201126.png)

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

![picture/微信截图_20220131204840](picture/微信截图_20220131204840.png)

##### Shading is Local (at a specific shading point)

![picture/微信截图_20220131204904](picture/微信截图_20220131204904.png)

No shadows will be generated! (**shading ≠ shadow**)

![picture/微信截图_20220131205527](picture/微信截图_20220131205527.png)

##### Diffuse Reflection

- Light is scattered uniformly in all directions

  - surface color is the same for all viewing directions

    ![picture/微信截图_20220131205630](picture/微信截图_20220131205630.png)

- But how much light (energy) is received?

  - Lambert's cosine law

    ![picture/微信截图_20220131205738](picture/微信截图_20220131205738.png)

![picture/微信截图_20220131214632](picture/微信截图_20220131214632.png)

###### Lambertian (Diffuse) Shading

![picture/微信截图_20220131214915](picture/微信截图_20220131214915.png)

Produces diffuse appearance :

![picture/微信截图_20220131215656](picture/微信截图_20220131215656.png)

##### Specular Term

Intensity depends on view direction

- Bright near mirror reflection direction

![picture/微信截图_20220131222835](picture/微信截图_20220131222835.png)

$V$ close to mirror direction $\longleftrightarrow$  half vector near normal

- Measure "near" by dot product of unit vectors

![picture/微信截图_20220131222855](picture/微信截图_20220131222855.png)

###### why there is a power "p"?

![picture/微信截图_20220131224430](picture/微信截图_20220131224430.png)

![picture/微信截图_20220131224457](picture/微信截图_20220131224457.png)

##### Ambient Term

shading that does not depend on anything

- add constant color to account for disregarded illumination and fill in black shadows
- this is approximate / fake!

![picture/微信截图_20220131225323](picture/微信截图_20220131225323.png)

##### Blinn-Phong Reflection Model

![picture/微信截图_20220131225703](picture/微信截图_20220131225703.png)

#### shading frequencies

![picture/微信截图_20220131230731](picture/微信截图_20220131230731.png)

##### shade each triangle (Flat shading)

- triangle face is flat - one normal vector
- not good for smooth surfaces

![picture/微信截图_20220131231952](picture/微信截图_20220131231952.png)

##### shade each vertex (Gouraud shading)

- interpolate colors from vertices across triangle
- each vertex has a normal vector (how?)

![picture/微信截图_20220131232018](picture/微信截图_20220131232018.png)

##### shade each pixel (Phong shading)

- interpolate normal vectors across each triangle
- computer full shading model at each pixel
- not the Blinn-Phong Reflectance Model

![picture/微信截图_20220131232417](picture/微信截图_20220131232417.png)

![picture/微信截图_20220131232438](picture/微信截图_20220131232438.png)

##### Defining Per-Vertex Normal Vectors

![picture/微信截图_20220131233756](picture/微信截图_20220131233756.png)

![picture/微信截图_20220131233819](picture/微信截图_20220131233819.png)

### graphics (Real-time Rendering) Pipeline

![picture/微信截图_20220131234015](picture/微信截图_20220131234015.png)

![picture/微信截图_20220131234316](picture/微信截图_20220131234316.png)

![picture/微信截图_20220131234348](picture/微信截图_20220131234348.png)

![picture/微信截图_20220131234411](picture/微信截图_20220131234411.png)

![picture/微信截图_20220131234428](picture/微信截图_20220131234428.png)

![picture/微信截图_20220131234450](picture/微信截图_20220131234450.png)

##### shader programs

- program vertex and fragment processing stages
- describe operation on a single vertex (or fragment)

![picture/微信截图_20220201003333](picture/微信截图_20220201003333.png)

![picture/微信截图_20220201003356](picture/微信截图_20220201003356.png)

##### graphics pipeline implementation: GPUs

GPUs: heterogeneous, multi-core processor

specialized processors for executing graphics pipeline computations

![picture/微信截图_20220201010044](picture/微信截图_20220201010044.png)

![picture/微信截图_20220201010304](picture/微信截图_20220201010304.png)

### Texture Mapping

![picture/微信截图_20220201010357](picture/微信截图_20220201010357.png)

#### Surfaces are 2D

- surface lives in 3D world space
- every 3D surface point also has a place where it goes in the 2D image (texture)

![picture/微信截图_20220201010603](picture/微信截图_20220201010603.png)

![picture/微信截图_20220201010630](picture/微信截图_20220201010630.png)

![picture/微信截图_20220201010749](picture/微信截图_20220201010749.png)

![picture/微信截图_20220201012713](picture/微信截图_20220201012713.png)

![picture/微信截图_20220201014313](picture/微信截图_20220201014313.png)

![picture/微信截图_20220201014321](picture/微信截图_20220201014321.png)

![picture/微信截图_20220201014345](picture/微信截图_20220201014345.png)



#### Interpolation Across Triangles: Barycentric Coordinates

Why do we want to interpolate?

- specify values at vertices
- obtain smoothly varying values across triangles

What do we want to interpolate?

- Texture coordinates, colors, normal vectors, ...

How do we interpolate?

- Barycentric coordinates

![picture/微信截图_20220201015032](picture/微信截图_20220201015032.png)

![picture/微信截图_20220201015212](picture/微信截图_20220201015212.png)

![picture/微信截图_20220201015221](picture/微信截图_20220201015221.png)

![picture/微信截图_20220201015229](picture/微信截图_20220201015229.png)

![picture/微信截图_20220201015330](picture/微信截图_20220201015330.png)

![picture/微信截图_20220201015339](picture/微信截图_20220201015339.png)

**Barycentric coordinates are not invariant under projection**

#### Applying Textures

![picture/微信截图_20220201015732](picture/微信截图_20220201015732.png)

#### Texture Magnification

##### Easy case: What if the texture is too small?

![picture/微信截图_20220201015915](picture/微信截图_20220201015915.png)

###### Bilinear Interpolation

![picture/微信截图_20220201020015](picture/微信截图_20220201020015.png)

![picture/微信截图_20220201020023](picture/微信截图_20220201020023.png)

![picture/微信截图_20220201020036](picture/微信截图_20220201020036.png)

![picture/微信截图_20220201020046](picture/微信截图_20220201020046.png)

![picture/微信截图_20220201020058](picture/微信截图_20220201020058.png)

![picture/微信截图_20220201020112](picture/微信截图_20220201020112.png)

Bilinear interpolation usually gives pretty good results
at reasonable costs

![picture/微信截图_20220201020253](picture/微信截图_20220201020253.png)

##### Hard case: What if the texture is too large?

![picture/微信截图_20220201020535](picture/微信截图_20220201020535.png)

![picture/微信截图_20220201020643](picture/微信截图_20220201020643.png)

![picture/微信截图_20220201020725](picture/微信截图_20220201020725.png)

#### Antialiasing: supersampling?

Will supersampling work?

- yes, high quality, but costly
- when highly minified, many texels in pixel footprint
- signal frequency too large in a pixel
- need even higher sampling frequency

Let's understand this problem in another way

- what if we don't sample?
- just need to get the average value within a range!

![picture/微信截图_20220201021119](picture/微信截图_20220201021119.png)

![picture/微信截图_20220201021150](picture/微信截图_20220201021150.png)

#### Mipmap: Allowing (fast, approx., square) range queries

![picture/微信截图_20220201021343](picture/微信截图_20220201021343.png)

![picture/微信截图_20220201021407](picture/微信截图_20220201021407.png)

![picture/微信截图_20220201021423](picture/微信截图_20220201021423.png)

![picture/微信截图_20220201021443](picture/微信截图_20220201021443.png)

![picture/微信截图_20220201021503](picture/微信截图_20220201021503.png)

![picture/微信截图_20220201021521](picture/微信截图_20220201021521.png)

![picture/微信截图_20220201022030](picture/微信截图_20220201022030.png)

![picture/微信截图_20220201022046](picture/微信截图_20220201022046.png)

##### limitations

![picture/微信截图_20220201022143](picture/微信截图_20220201022143.png)

![picture/微信截图_20220201022152](picture/微信截图_20220201022152.png)

![picture/微信截图_20220201022201](picture/微信截图_20220201022201.png)

#### Anisotropic Filtering: fix mipmap's overblur

![picture/微信截图_20220201022247](picture/微信截图_20220201022247.png)

![picture/微信截图_20220201022406](picture/微信截图_20220201022406.png)

![picture/微信截图_20220201022426](picture/微信截图_20220201022426.png)

#### Applications of textures

In modern GPUs, texture = memory + range query (filtering)

- General method to bring data to fragment calculations

Many applications

- Environment lighting
- Store microgeometry
- Procedural textures
- Solid modeling
- Volume rendering

![picture/微信截图_20220201130907](picture/微信截图_20220201130907.png)

![picture/微信截图_20220201130930](picture/微信截图_20220201130930.png)

![picture/微信截图_20220201131008](picture/微信截图_20220201131008.png)

![picture/微信截图_20220201131022](picture/微信截图_20220201131022.png)

![picture/微信截图_20220201131033](picture/微信截图_20220201131033.png)

![picture/微信截图_20220201131116](picture/微信截图_20220201131116.png)

##### Bump Mapping

![picture/微信截图_20220201131127](picture/微信截图_20220201131127.png)

![picture/微信截图_20220201131138](picture/微信截图_20220201131138.png)

###### How to perturb the normal (in flatland)

![picture/微信截图_20220201132050](picture/微信截图_20220201132050.png)

###### How to perturb the normal (in 3D)

![picture/微信截图_20220201132138](picture/微信截图_20220201132138.png)

##### Displacement Mapping

![picture/微信截图_20220201132458](picture/微信截图_20220201132458.png)

![picture/微信截图_20220201132752](picture/微信截图_20220201132752.png)

![picture/微信截图_20220201132814](picture/微信截图_20220201132814.png)

![picture/微信截图_20220201132832](picture/微信截图_20220201132832.png)

## Geometry

### Introduction

![picture/微信截图_20220201133009](picture/微信截图_20220201133009.png)

![picture/微信截图_20220201133022](picture/微信截图_20220201133022.png)

![picture/微信截图_20220201133034](picture/微信截图_20220201133034.png)

![picture/微信截图_20220201133124](picture/微信截图_20220201133124.png)

![picture/微信截图_20220201133133](picture/微信截图_20220201133133.png)

![picture/微信截图_20220201133143](picture/微信截图_20220201133143.png)

![picture/微信截图_20220201133152](picture/微信截图_20220201133152.png)

![picture/微信截图_20220201133200](picture/微信截图_20220201133200.png)

![picture/微信截图_20220201133209](picture/微信截图_20220201133209.png)

![picture/微信截图_20220201133218](picture/微信截图_20220201133218.png)

![picture/微信截图_20220201133927](picture/微信截图_20220201133927.png)

### Implicit representations

![picture/微信截图_20220201133938](picture/微信截图_20220201133938.png)

![picture/微信截图_20220201133948](picture/微信截图_20220201133948.png)

#### Algebraic Surfaces

![picture/微信截图_20220201133957](picture/微信截图_20220201133957.png)

#### Constructive Solid Geometry

![picture/微信截图_20220201134007](picture/微信截图_20220201134007.png)

#### Distance Functions

![picture/微信截图_20220201134015](picture/微信截图_20220201134015.png)

![picture/微信截图_20220201134024](picture/微信截图_20220201134024.png)

#### Blending Distance Functions

![picture/微信截图_20220201134034](picture/微信截图_20220201134034.png)

![picture/微信截图_20220201134044](picture/微信截图_20220201134044.png)

#### Level Set Methods

![picture/微信截图_20220201134052](picture/微信截图_20220201134052.png)

![picture/微信截图_20220201134101](picture/微信截图_20220201134101.png)

![picture/微信截图_20220201134109](picture/微信截图_20220201134109.png)

#### Fractals

![picture/微信截图_20220201134119](picture/微信截图_20220201134119.png)

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

![picture/微信截图_20220201142009](picture/微信截图_20220201142009.png)

#### Point Cloud

![picture/微信截图_20220201142018](picture/微信截图_20220201142018.png)

#### Polygon Mesh

![picture/微信截图_20220201142029](picture/微信截图_20220201142029.png)

![picture/微信截图_20220201142038](picture/微信截图_20220201142038.png)

#### Curves

applications:

- camera paths
- animation curves
- vector fonts

##### Bezier Curves

![picture/微信截图_20220201153129](picture/微信截图_20220201153129.png)

###### Evaluating Bezier Curves (de Casteljau Algorithm)

![picture/微信截图_20220201153140](picture/微信截图_20220201153140.png)

![picture/微信截图_20220201153148](picture/微信截图_20220201153148.png)

![picture/微信截图_20220201153154](picture/微信截图_20220201153154.png)

![picture/微信截图_20220201153200](picture/微信截图_20220201153200.png)

![picture/微信截图_20220201153206](picture/微信截图_20220201153206.png)

![picture/微信截图_20220201163903](picture/微信截图_20220201163903.png)

![picture/微信截图_20220201163911](picture/微信截图_20220201163911.png)

###### Evaluation Bezier Curves Algebraic Formula

![picture/微信截图_20220201164101](picture/微信截图_20220201164101.png)

![picture/微信截图_20220201164108](picture/微信截图_20220201164108.png)

![picture/微信截图_20220201164118](picture/微信截图_20220201164118.png)

![picture/微信截图_20220201164128](picture/微信截图_20220201164128.png)

![picture/微信截图_20220201164136](picture/微信截图_20220201164136.png)

![picture/微信截图_20220201164143](picture/微信截图_20220201164143.png)

![picture/微信截图_20220201164150](picture/微信截图_20220201164150.png)

##### Piecewise Bezier Curves

![picture/微信截图_20220201164907](picture/微信截图_20220201164907.png)

![picture/微信截图_20220201164913](picture/微信截图_20220201164913.png)

example: http://math.hws.edu/eck/cs424/notes2013/canvas/bezier.html

###### Continuity

![picture/微信截图_20220201164933](picture/微信截图_20220201164933.png)

![picture/微信截图_20220201164942](picture/微信截图_20220201164942.png)

![picture/微信截图_20220201164949](picture/微信截图_20220201164949.png)

![picture/微信截图_20220201164959](picture/微信截图_20220201164959.png)

![picture/微信截图_20220201165008](picture/微信截图_20220201165008.png)

In this course 

- We do not cover B-splines and NURBS

- We also do not cover operations on curves
  (e.g. increasing/decreasing orders, etc.)
- To learn more deeper, you are welcome to refer to Prof. Shi-Min Hu’s course: https://www.bilibili.com/video/av66548502?from=search&seid=65256805876131485

#### Surfaces

![picture/微信截图_20220201165738](picture/微信截图_20220201165738.png)

![picture/微信截图_20220201165746](picture/微信截图_20220201165746.png)

##### Evaluating Bezier Surfaces

![picture/微信截图_20220201165916](picture/微信截图_20220201165916.png)

![picture/微信截图_20220201165923](picture/微信截图_20220201165923.png)

#### mesh operations

![picture/微信截图_20220201165934](picture/微信截图_20220201165934.png)

##### mesh subdivision (upsampling)

![picture/微信截图_20220209134700](picture/微信截图_20220209134700.png)

###### loop subdivision

![picture/微信截图_20220209134909](picture/微信截图_20220209134909.png)

![picture/微信截图_20220209134918](picture/微信截图_20220209134918.png)

![picture/微信截图_20220209134926](picture/微信截图_20220209134926.png)

![picture/微信截图_20220209134935](picture/微信截图_20220209134935.png)

![picture/微信截图_20220209134942](picture/微信截图_20220209134942.png)

###### catmull-clark subdivision

![picture/微信截图_20220209134952](picture/微信截图_20220209134952.png)

![picture/微信截图_20220209135001](picture/微信截图_20220209135001.png)

![picture/微信截图_20220209135011](picture/微信截图_20220209135011.png)

![picture/微信截图_20220209135020](picture/微信截图_20220209135020.png)

![picture/微信截图_20220209135032](picture/微信截图_20220209135032.png)

![picture/微信截图_20220209135042](picture/微信截图_20220209135042.png)

##### mesh simplification (downsampling)

![picture/微信截图_20220209134718](picture/微信截图_20220209134718.png)

![picture/微信截图_20220209140248](picture/微信截图_20220209140248.png)

![picture/微信截图_20220209140254](picture/微信截图_20220209140254.png)

![picture/微信截图_20220209140304](picture/微信截图_20220209140304.png)

http://graphics.stanford.edu/courses/cs468-10-fall/LectureSlides/08_Simplification.pdf

![picture/微信截图_20220209140312](picture/微信截图_20220209140312.png)

![picture/微信截图_20220209140320](picture/微信截图_20220209140320.png)

![picture/微信截图_20220209140327](picture/微信截图_20220209140327.png)

##### mesh regularization (same # of triangles)

![picture/微信截图_20220209134728](picture/微信截图_20220209134728.png)

### Shadow Mapping

![picture/微信截图_20220209140931](picture/微信截图_20220209140931.png)

![picture/微信截图_20220209140939](picture/微信截图_20220209140939.png)

![picture/微信截图_20220209140947](picture/微信截图_20220209140947.png)

![picture/微信截图_20220209140956](picture/微信截图_20220209140956.png)

![picture/微信截图_20220209141003](picture/微信截图_20220209141003.png)

![picture/微信截图_20220209141015](picture/微信截图_20220209141015.png)

![picture/微信截图_20220209141023](picture/微信截图_20220209141023.png)

![picture/微信截图_20220209141031](picture/微信截图_20220209141031.png)

![picture/微信截图_20220209141040](picture/微信截图_20220209141040.png)

![picture/微信截图_20220209141050](picture/微信截图_20220209141050.png)

![picture/微信截图_20220209141103](picture/微信截图_20220209141103.png)

floating numbers' comparison is difficulty!

![picture/微信截图_20220209141112](picture/微信截图_20220209141112.png)

![picture/微信截图_20220209141930](picture/微信截图_20220209141930.png)

![picture/微信截图_20220209141938](picture/微信截图_20220209141938.png)

![picture/微信截图_20220209141948](picture/微信截图_20220209141948.png)

https://www.timeanddate.com/eclipse/umbra-shadow.html

## Ray tracing

![picture/微信截图_20220209142534](picture/微信截图_20220209142534.png)

![picture/微信截图_20220209142544](picture/微信截图_20220209142544.png)

![picture/微信截图_20220209142554](picture/微信截图_20220209142554.png)

### Basic Ray-Tracing Algorithm

![picture/微信截图_20220209142922](picture/微信截图_20220209142922.png)

![picture/微信截图_20220209142933](picture/微信截图_20220209142933.png)

![picture/微信截图_20220209142958](picture/微信截图_20220209142958.png)

![picture/微信截图_20220209143006](picture/微信截图_20220209143006.png)

![picture/微信截图_20220209143013](picture/微信截图_20220209143013.png)

![picture/微信截图_20220209143023](picture/微信截图_20220209143023.png)

![picture/微信截图_20220209143030](picture/微信截图_20220209143030.png)

![picture/微信截图_20220209143037](picture/微信截图_20220209143037.png)

![picture/微信截图_20220209143044](picture/微信截图_20220209143044.png)

![picture/微信截图_20220209143051](picture/微信截图_20220209143051.png)

![picture/微信截图_20220209143058](picture/微信截图_20220209143058.png)

![picture/微信截图_20220209143105](picture/微信截图_20220209143105.png)

#### Ray-Surface Intersection

![picture/微信截图_20220209144455](picture/微信截图_20220209144455.png)

![picture/微信截图_20220209144503](picture/微信截图_20220209144503.png)

![picture/微信截图_20220209144510](picture/微信截图_20220209144510.png)

![picture/微信截图_20220209144518](picture/微信截图_20220209144518.png)

![picture/微信截图_20220209144526](picture/微信截图_20220209144526.png)

![picture/微信截图_20220209144533](picture/微信截图_20220209144533.png)

![picture/微信截图_20220209144541](picture/微信截图_20220209144541.png)

![picture/微信截图_20220209144550](picture/微信截图_20220209144550.png)

![picture/微信截图_20220209144557](picture/微信截图_20220209144557.png)

### Accelerating Ray-Surface Intersection

![picture/微信截图_20220209144110](picture/微信截图_20220209144110.png)

![picture/微信截图_20220209144118](picture/微信截图_20220209144118.png)

![picture/微信截图_20220209144129](picture/微信截图_20220209144129.png)

#### Bounding Volumes

![picture/微信截图_20220209144150](picture/微信截图_20220209144150.png)

![picture/微信截图_20220209144206](picture/微信截图_20220209144206.png)

![picture/微信截图_20220209144222](picture/微信截图_20220209144222.png)

![picture/微信截图_20220209144230](picture/微信截图_20220209144230.png)

![picture/微信截图_20220209144238](picture/微信截图_20220209144238.png)

![picture/微信截图_20220209144247](picture/微信截图_20220209144247.png)

#### Uniform Spatial Partitions (Grids)

![picture/微信截图_20220209151115](picture/微信截图_20220209151115.png)

![picture/微信截图_20220209151124](picture/微信截图_20220209151124.png)

![picture/微信截图_20220209151131](picture/微信截图_20220209151131.png)

![picture/微信截图_20220209151139](picture/微信截图_20220209151139.png)

![picture/微信截图_20220209151146](picture/微信截图_20220209151146.png)

![picture/微信截图_20220209151154](picture/微信截图_20220209151154.png)

![picture/微信截图_20220209151201](picture/微信截图_20220209151201.png)

![picture/微信截图_20220209151210](picture/微信截图_20220209151210.png)

![picture/微信截图_20220209151219](picture/微信截图_20220209151219.png)

#### Spatial Partitions

![picture/微信截图_20220209151243](picture/微信截图_20220209151243.png)

![picture/微信截图_20220209151251](picture/微信截图_20220209151251.png)

![picture/微信截图_20220209151258](picture/微信截图_20220209151258.png)

![picture/微信截图_20220209151305](picture/微信截图_20220209151305.png)

![picture/微信截图_20220209151313](picture/微信截图_20220209151313.png)

![picture/微信截图_20220209151320](picture/微信截图_20220209151320.png)

![picture/微信截图_20220209151330](picture/微信截图_20220209151330.png)

![picture/微信截图_20220209151338](picture/微信截图_20220209151338.png)

![picture/微信截图_20220209151344](picture/微信截图_20220209151344.png)

![picture/微信截图_20220209151351](picture/微信截图_20220209151351.png)

![picture/微信截图_20220209151400](picture/微信截图_20220209151400.png)

![picture/微信截图_20220209151410](picture/微信截图_20220209151410.png)

![picture/微信截图_20220209151419](picture/微信截图_20220209151419.png)

#### Object Partitions & Bounding Volume Hierarchy (BVH)

![picture/微信截图_20220209151559](picture/微信截图_20220209151559.png)

![picture/微信截图_20220209151605](picture/微信截图_20220209151605.png)

![picture/微信截图_20220209151612](picture/微信截图_20220209151612.png)

![picture/微信截图_20220209151622](picture/微信截图_20220209151622.png)

![picture/微信截图_20220209151629](picture/微信截图_20220209151629.png)

![picture/微信截图_20220209151636](picture/微信截图_20220209151636.png)

![picture/微信截图_20220209151643](picture/微信截图_20220209151643.png)

![picture/微信截图_20220209151651](picture/微信截图_20220209151651.png)

![picture/微信截图_20220209151658](picture/微信截图_20220209151658.png)

### Basic radiometry

![picture/微信截图_20220209151757](picture/微信截图_20220209151757.png)

![picture/微信截图_20220209151804](picture/微信截图_20220209151804.png)

#### Radiant Energy and Flux (Power)

![picture/微信截图_20220209151843](picture/微信截图_20220209151843.png)

![picture/微信截图_20220209151851](picture/微信截图_20220209151851.png)

![picture/微信截图_20220209151858](picture/微信截图_20220209151858.png)

#### Radiant Intensity

![picture/微信截图_20220209151932](picture/微信截图_20220209151932.png)

![picture/微信截图_20220209151940](picture/微信截图_20220209151940.png)

![picture/微信截图_20220209151949](picture/微信截图_20220209151949.png)

![picture/微信截图_20220209151957](picture/微信截图_20220209151957.png)

![picture/微信截图_20220209152003](picture/微信截图_20220209152003.png)

![picture/微信截图_20220209152010](picture/微信截图_20220209152010.png)

![picture/微信截图_20220209152017](picture/微信截图_20220209152017.png)

![picture/微信截图_20220209152258](picture/微信截图_20220209152258.png)

![picture/微信截图_20220209152305](picture/微信截图_20220209152305.png)

#### Irradiance

![picture/微信截图_20220209152339](picture/微信截图_20220209152339.png)

![picture/微信截图_20220209152347](picture/微信截图_20220209152347.png)

![picture/微信截图_20220209152354](picture/微信截图_20220209152354.png)

![picture/微信截图_20220209152405](picture/微信截图_20220209152405.png)

The differential solid angle is not change, so what really falloff is irradiance neither intensity.

#### Radiance

![picture/微信截图_20220209152435](picture/微信截图_20220209152435.png)

![picture/微信截图_20220209152443](picture/微信截图_20220209152443.png)

![picture/微信截图_20220209152451](picture/微信截图_20220209152451.png)

![picture/微信截图_20220209152458](picture/微信截图_20220209152458.png)

![picture/微信截图_20220209152504](picture/微信截图_20220209152504.png)

##### Irradiance VS Radiance

![picture/微信截图_20220209152511](picture/微信截图_20220209152511.png)

#### Bidirectional Reflectance Distribution Function (BRDF)

![picture/微信截图_20220209152644](picture/微信截图_20220209152644.png)

![picture/微信截图_20220209152651](picture/微信截图_20220209152651.png)

![picture/微信截图_20220209152659](picture/微信截图_20220209152659.png)

![picture/微信截图_20220209152705](picture/微信截图_20220209152705.png)

![picture/微信截图_20220209152713](picture/微信截图_20220209152713.png)

### Light transport

#### Understanding the rendering equation

![picture/微信截图_20220209152755](picture/微信截图_20220209152755.png)

![picture/微信截图_20220209152803](picture/微信截图_20220209152803.png)

![picture/微信截图_20220209152816](picture/微信截图_20220209152816.png)

![picture/微信截图_20220209152825](picture/微信截图_20220209152825.png)

![picture/微信截图_20220209152833](picture/微信截图_20220209152833.png)

![picture/微信截图_20220209152842](picture/微信截图_20220209152842.png)

![picture/微信截图_20220209152849](picture/微信截图_20220209152849.png)

![picture/微信截图_20220209152859](picture/微信截图_20220209152859.png)

![picture/微信截图_20220209152905](picture/微信截图_20220209152905.png)

### Global illumination

![picture/微信截图_20220209152913](picture/微信截图_20220209152913.png)

![picture/微信截图_20220209152932](picture/微信截图_20220209152932.png)

![picture/微信截图_20220209152944](picture/微信截图_20220209152944.png)

![picture/微信截图_20220209152954](picture/微信截图_20220209152954.png)

![picture/微信截图_20220209153005](picture/微信截图_20220209153005.png)

![picture/微信截图_20220209153016](picture/微信截图_20220209153016.png)

![picture/微信截图_20220209153026](picture/微信截图_20220209153026.png)

### Probability Review

![picture/微信截图_20220209153130](picture/微信截图_20220209153130.png)

![picture/微信截图_20220209153137](picture/微信截图_20220209153137.png)

![picture/微信截图_20220209153144](picture/微信截图_20220209153144.png)

![picture/微信截图_20220209153150](picture/微信截图_20220209153150.png)

![picture/微信截图_20220209153156](picture/微信截图_20220209153156.png)

### Monte Carlo Integration

![picture/微信截图_20220210190813](picture/微信截图_20220210190813.png)

![picture/微信截图_20220210190823](picture/微信截图_20220210190823.png)

![picture/微信截图_20220210190835](picture/微信截图_20220210190835.png)

![picture/微信截图_20220210190846](picture/微信截图_20220210190846.png)

![picture/微信截图_20220210190853](picture/微信截图_20220210190853.png)

![picture/微信截图_20220210190902](picture/微信截图_20220210190902.png)

![picture/微信截图_20220210190910](picture/微信截图_20220210190910.png)

![picture/微信截图_20220210190920](picture/微信截图_20220210190920.png)

### Path Tracing

![picture/微信截图_20220210191004](picture/微信截图_20220210191004.png)

![picture/微信截图_20220210191014](picture/微信截图_20220210191014.png)

![picture/微信截图_20220210201019](picture/微信截图_20220210201019.png)

![picture/微信截图_20220210201027](picture/微信截图_20220210201027.png)

![picture/微信截图_20220210201037](picture/微信截图_20220210201037.png)

![picture/微信截图_20220210201046](picture/微信截图_20220210201046.png)

![picture/微信截图_20220210201055](picture/微信截图_20220210201055.png)

![picture/微信截图_20220210201110](picture/微信截图_20220210201110.png)

![picture/微信截图_20220210201118](picture/微信截图_20220210201118.png)

![picture/微信截图_20220210201125](picture/微信截图_20220210201125.png)

![picture/微信截图_20220210201134](picture/微信截图_20220210201134.png)

![picture/微信截图_20220210201150](picture/微信截图_20220210201150.png)

![picture/微信截图_20220210201158](picture/微信截图_20220210201158.png)

![picture/微信截图_20220210201206](picture/微信截图_20220210201206.png)

![picture/微信截图_20220210201214](picture/微信截图_20220210201214.png)

![picture/微信截图_20220210201222](picture/微信截图_20220210201222.png)

![picture/微信截图_20220210201232](picture/微信截图_20220210201232.png)

![picture/微信截图_20220210201241](picture/微信截图_20220210201241.png)

![picture/微信截图_20220210201249](picture/微信截图_20220210201249.png)

![picture/微信截图_20220210201302](picture/微信截图_20220210201302.png)

![picture/微信截图_20220210201311](picture/微信截图_20220210201311.png)

![picture/微信截图_20220210201320](picture/微信截图_20220210201320.png)

![picture/微信截图_20220210201330](picture/微信截图_20220210201330.png)

![picture/微信截图_20220210201338](picture/微信截图_20220210201338.png)

![picture/微信截图_20220210201356](picture/微信截图_20220210201356.png)

![picture/微信截图_20220210201407](picture/微信截图_20220210201407.png)

![picture/微信截图_20220210201415](picture/微信截图_20220210201415.png)

![picture/微信截图_20220210201423](picture/微信截图_20220210201423.png)

![picture/微信截图_20220210201433](picture/微信截图_20220210201433.png)

![picture/微信截图_20220210201444](picture/微信截图_20220210201444.png)

![picture/微信截图_20220210201457](picture/微信截图_20220210201457.png)

http://www.graphics.cornell.edu/online/box/compare.html

![picture/微信截图_20220210201620](picture/微信截图_20220210201620.png)

![picture/微信截图_20220210201629](picture/微信截图_20220210201629.png)

![picture/微信截图_20220210201637](picture/微信截图_20220210201637.png)

## Materials and Appearances

![picture/微信截图_20220210210841](picture/微信截图_20220210210841.png)

![picture/微信截图_20220210210851](picture/微信截图_20220210210851.png)

### Material == BRDF

![picture/微信截图_20220210210859](picture/微信截图_20220210210859.png)

![picture/微信截图_20220210210950](picture/微信截图_20220210210950.png)

![picture/微信截图_20220210210957](picture/微信截图_20220210210957.png)

![picture/微信截图_20220210211003](picture/微信截图_20220210211003.png)

![picture/微信截图_20220210211014](picture/微信截图_20220210211014.png)

![picture/微信截图_20220210211022](picture/微信截图_20220210211022.png)

![picture/微信截图_20220210211033](picture/微信截图_20220210211033.png)

![picture/微信截图_20220210211047](picture/微信截图_20220210211047.png)

![picture/微信截图_20220210211056](picture/微信截图_20220210211056.png)

![picture/微信截图_20220210211106](picture/微信截图_20220210211106.png)

![picture/微信截图_20220210211114](picture/微信截图_20220210211114.png)

![picture/微信截图_20220210211123](picture/微信截图_20220210211123.png)

![picture/微信截图_20220210211132](picture/微信截图_20220210211132.png)

![picture/微信截图_20220210211140](picture/微信截图_20220210211140.png)

![picture/微信截图_20220210211149](picture/微信截图_20220210211149.png)

![picture/微信截图_20220210211157](picture/微信截图_20220210211157.png)

![picture/微信截图_20220210211205](picture/微信截图_20220210211205.png)

![picture/微信截图_20220210211213](picture/微信截图_20220210211213.png)

### Microfacet Material

![picture/微信截图_20220210212942](picture/微信截图_20220210212942.png)

![picture/微信截图_20220210212952](picture/微信截图_20220210212952.png)

![picture/微信截图_20220210213001](picture/微信截图_20220210213001.png)

![picture/微信截图_20220210213009](picture/微信截图_20220210213009.png)

![picture/微信截图_20220210213018](picture/微信截图_20220210213018.png)

![picture/微信截图_20220210213029](picture/微信截图_20220210213029.png)

![picture/微信截图_20220210213037](picture/微信截图_20220210213037.png)

![picture/微信截图_20220210213045](picture/微信截图_20220210213045.png)

![picture/微信截图_20220210213128](picture/微信截图_20220210213128.png)

![picture/微信截图_20220210213139](picture/微信截图_20220210213139.png)

![picture/微信截图_20220210213154](picture/微信截图_20220210213154.png)

![picture/微信截图_20220210213204](picture/微信截图_20220210213204.png)

![picture/微信截图_20220210213213](picture/微信截图_20220210213213.png)

![picture/微信截图_20220210213221](picture/微信截图_20220210213221.png)

![picture/微信截图_20220210213231](picture/微信截图_20220210213231.png)

### Measuring BRDFs

![picture/微信截图_20220210213336](picture/微信截图_20220210213336.png)

![picture/微信截图_20220210213432](picture/微信截图_20220210213432.png)

![picture/微信截图_20220210213445](picture/微信截图_20220210213445.png)

![picture/微信截图_20220210213513](picture/微信截图_20220210213513.png)

![picture/微信截图_20220210213522](picture/微信截图_20220210213522.png)

![picture/微信截图_20220210213530](picture/微信截图_20220210213530.png)

![picture/微信截图_20220210213540](picture/微信截图_20220210213540.png)

## Advanced Light Transport

![picture/微信截图_20220210215306](picture/微信截图_20220210215306.png)

### Unbiased light transport methods

![picture/微信截图_20220210215314](picture/微信截图_20220210215314.png)

![picture/微信截图_20220210215321](picture/微信截图_20220210215321.png)

![picture/微信截图_20220210215330](picture/微信截图_20220210215330.png)

![picture/微信截图_20220210215338](picture/微信截图_20220210215338.png)

![picture/微信截图_20220210215346](picture/微信截图_20220210215346.png)

![picture/微信截图_20220210215355](picture/微信截图_20220210215355.png)

### Biased light transport methods

![picture/微信截图_20220210215403](picture/微信截图_20220210215403.png)

![picture/微信截图_20220210215411](picture/微信截图_20220210215411.png)

![picture/微信截图_20220210215423](picture/微信截图_20220210215423.png)

![picture/微信截图_20220210215438](picture/微信截图_20220210215438.png)

![picture/微信截图_20220210215445](picture/微信截图_20220210215445.png)

![picture/微信截图_20220210215455](picture/微信截图_20220210215455.png)

### Instant radiosity (VPL / many light methods)

![picture/微信截图_20220210215505](picture/微信截图_20220210215505.png)

![picture/微信截图_20220210215518](picture/微信截图_20220210215518.png)

## Advanced Appearance Modeling

![picture/微信截图_20220210215613](picture/微信截图_20220210215613.png)

### Non-surface models

![picture/微信截图_20220210215705](picture/微信截图_20220210215705.png)

![picture/微信截图_20220210215716](picture/微信截图_20220210215716.png)

![picture/微信截图_20220210215726](picture/微信截图_20220210215726.png)

![picture/微信截图_20220210215736](picture/微信截图_20220210215736.png)

![picture/微信截图_20220210215746](picture/微信截图_20220210215746.png)

![picture/微信截图_20220210215757](picture/微信截图_20220210215757.png)

![picture/微信截图_20220210215811](picture/微信截图_20220210215811.png)

![picture/微信截图_20220210215823](picture/微信截图_20220210215823.png)

![picture/微信截图_20220210215832](picture/微信截图_20220210215832.png)

![picture/微信截图_20220210215839](picture/微信截图_20220210215839.png)

![picture/微信截图_20220210215848](picture/微信截图_20220210215848.png)

![picture/微信截图_20220210215856](picture/微信截图_20220210215856.png)

![picture/微信截图_20220210215906](picture/微信截图_20220210215906.png)

![picture/微信截图_20220210215924](picture/微信截图_20220210215924.png)

![picture/微信截图_20220210215934](picture/微信截图_20220210215934.png)

![picture/微信截图_20220210215944](picture/微信截图_20220210215944.png)

![picture/微信截图_20220210220004](picture/微信截图_20220210220004.png)

![picture/微信截图_20220210220013](picture/微信截图_20220210220013.png)

![picture/微信截图_20220210220027](picture/微信截图_20220210220027.png)

![picture/微信截图_20220210220036](picture/微信截图_20220210220036.png)

![picture/微信截图_20220210220048](picture/微信截图_20220210220048.png)

![picture/微信截图_20220210220054](picture/微信截图_20220210220054.png)

![picture/微信截图_20220210220102](picture/微信截图_20220210220102.png)

![picture/微信截图_20220210220112](picture/微信截图_20220210220112.png)

![picture/微信截图_20220210220123](picture/微信截图_20220210220123.png)

![picture/微信截图_20220210220132](picture/微信截图_20220210220132.png)

![picture/微信截图_20220210220140](picture/微信截图_20220210220140.png)

![picture/微信截图_20220210220150](picture/微信截图_20220210220150.png)

![picture/微信截图_20220210220159](picture/微信截图_20220210220159.png)

![picture/微信截图_20220210220209](picture/微信截图_20220210220209.png)

### Surface models

![picture/微信截图_20220210220415](picture/微信截图_20220210220415.png)

![picture/微信截图_20220210220423](picture/微信截图_20220210220423.png)

![picture/微信截图_20220210220431](picture/微信截图_20220210220431.png)

![picture/微信截图_20220210220439](picture/微信截图_20220210220439.png)

![picture/微信截图_20220210220446](picture/微信截图_20220210220446.png)

![picture/微信截图_20220210220454](picture/微信截图_20220210220454.png)

![picture/微信截图_20220210220502](picture/微信截图_20220210220502.png)

![picture/微信截图_20220210220513](picture/微信截图_20220210220513.png)

![picture/微信截图_20220210220521](picture/微信截图_20220210220521.png)

![picture/微信截图_20220210220536](picture/微信截图_20220210220536.png)

![picture/微信截图_20220210220601](picture/微信截图_20220210220601.png)

![picture/微信截图_20220210220614](picture/微信截图_20220210220614.png)

![picture/微信截图_20220210220623](picture/微信截图_20220210220623.png)

![picture/微信截图_20220210220631](picture/微信截图_20220210220631.png)

![picture/微信截图_20220210220643](picture/微信截图_20220210220643.png)

![picture/微信截图_20220210220653](picture/微信截图_20220210220653.png)

![picture/微信截图_20220210220704](picture/微信截图_20220210220704.png)

![picture/微信截图_20220210220717](picture/微信截图_20220210220717.png)

![picture/微信截图_20220210220727](picture/微信截图_20220210220727.png)

![picture/微信截图_20220210220739](picture/微信截图_20220210220739.png)

![picture/微信截图_20220210220748](picture/微信截图_20220210220748.png)

![picture/微信截图_20220210220756](picture/微信截图_20220210220756.png)

![picture/微信截图_20220210220803](picture/微信截图_20220210220803.png)

![picture/微信截图_20220210220810](picture/微信截图_20220210220810.png)

![picture/微信截图_20220210220819](picture/微信截图_20220210220819.png)

![picture/微信截图_20220210220834](picture/微信截图_20220210220834.png)

![picture/微信截图_20220210220844](picture/微信截图_20220210220844.png)

![picture/微信截图_20220210220855](picture/微信截图_20220210220855.png)

![picture/微信截图_20220210220903](picture/微信截图_20220210220903.png)

![picture/微信截图_20220210220911](picture/微信截图_20220210220911.png)

![picture/微信截图_20220210220920](picture/微信截图_20220210220920.png)

![picture/微信截图_20220210220929](picture/微信截图_20220210220929.png)

![picture/微信截图_20220210220948](picture/微信截图_20220210220948.png)

![picture/微信截图_20220210220959](picture/微信截图_20220210220959.png)

![picture/微信截图_20220210221009](picture/微信截图_20220210221009.png)

![picture/微信截图_20220210221019](picture/微信截图_20220210221019.png)

![picture/微信截图_20220210221028](picture/微信截图_20220210221028.png)

![picture/微信截图_20220210221036](picture/微信截图_20220210221036.png)

![picture/微信截图_20220210221045](picture/微信截图_20220210221045.png)

![picture/微信截图_20220210221059](picture/微信截图_20220210221059.png)

![picture/微信截图_20220210221115](picture/微信截图_20220210221115.png)

![picture/微信截图_20220210221123](picture/微信截图_20220210221123.png)

### Procedural appearance

![picture/微信截图_20220210221758](picture/微信截图_20220210221758.png)

![picture/微信截图_20220210221807](picture/微信截图_20220210221807.png)

![picture/微信截图_20220210221817](picture/微信截图_20220210221817.png)

![picture/微信截图_20220210221824](picture/微信截图_20220210221824.png)

![picture/微信截图_20220210221833](picture/微信截图_20220210221833.png)

![picture/微信截图_20220210221842](picture/微信截图_20220210221842.png)

![picture/微信截图_20220210221851](picture/微信截图_20220210221851.png)

## Cameras, Lenses and Light Fields

### Imaging = Synthesis + Capture

![picture/微信截图_20220211001147](picture/微信截图_20220211001147.png)

![picture/微信截图_20220211001157](picture/微信截图_20220211001157.png)

![picture/微信截图_20220211001212](picture/微信截图_20220211001212.png)

![picture/微信截图_20220211001222](picture/微信截图_20220211001222.png)

![picture/微信截图_20220211001230](picture/微信截图_20220211001230.png)

![picture/微信截图_20220211001242](picture/微信截图_20220211001242.png)

### Pinhole Image Formation

![picture/微信截图_20220211001329](picture/微信截图_20220211001329.png)

![picture/微信截图_20220211001355](picture/微信截图_20220211001355.png)

![picture/微信截图_20220211001409](picture/微信截图_20220211001409.png)

### Filed of View (FOV)

![picture/微信截图_20220211001450](picture/微信截图_20220211001450.png)

![picture/微信截图_20220211001458](picture/微信截图_20220211001458.png)

![picture/微信截图_20220211001505](picture/微信截图_20220211001505.png)

![picture/微信截图_20220211001523](picture/微信截图_20220211001523.png)

![picture/微信截图_20220211001531](picture/微信截图_20220211001531.png)

![picture/微信截图_20220211001540](picture/微信截图_20220211001540.png)

### Exposure

![picture/微信截图_20220211001608](picture/微信截图_20220211001608.png)

![picture/微信截图_20220211001621](picture/微信截图_20220211001621.png)

![picture/微信截图_20220211001629](picture/微信截图_20220211001629.png)

![picture/微信截图_20220211001635](picture/微信截图_20220211001635.png)

![picture/微信截图_20220211001642](picture/微信截图_20220211001642.png)

![picture/微信截图_20220211001650](picture/微信截图_20220211001650.png)

![picture/微信截图_20220211001658](picture/微信截图_20220211001658.png)

![picture/微信截图_20220211001706](picture/微信截图_20220211001706.png)

![picture/微信截图_20220211001714](picture/微信截图_20220211001714.png)

![picture/微信截图_20220211001722](picture/微信截图_20220211001722.png)

![picture/微信截图_20220211001732](picture/微信截图_20220211001732.png)

### Fast and Slow Photography

![picture/微信截图_20220211001817](picture/微信截图_20220211001817.png)

![picture/微信截图_20220211001828](picture/微信截图_20220211001828.png)

![picture/微信截图_20220211001840](picture/微信截图_20220211001840.png)

![picture/微信截图_20220211001849](picture/微信截图_20220211001849.png)

![picture/微信截图_20220211001858](picture/微信截图_20220211001858.png)

### Thin Lens Approximation

![picture/微信截图_20220211001937](picture/微信截图_20220211001937.png)

![picture/微信截图_20220211001944](picture/微信截图_20220211001944.png)

![picture/微信截图_20220211001954](picture/微信截图_20220211001954.png)

![picture/微信截图_20220211002002](picture/微信截图_20220211002002.png)

![picture/微信截图_20220211002010](picture/微信截图_20220211002010.png)

![picture/微信截图_20220211002016](picture/微信截图_20220211002016.png)

![picture/微信截图_20220211002023](picture/微信截图_20220211002023.png)

![picture/微信截图_20220211002030](picture/微信截图_20220211002030.png)

![picture/微信截图_20220211002038](picture/微信截图_20220211002038.png)

http://graphics.stanford.edu/courses/cs178-10/applets/gaussian.html

### Defocus Blur

![picture/微信截图_20220211002116](picture/微信截图_20220211002116.png)

![picture/微信截图_20220211002125](picture/微信截图_20220211002125.png)

![picture/微信截图_20220211002133](picture/微信截图_20220211002133.png)

![picture/微信截图_20220211002139](picture/微信截图_20220211002139.png)

![picture/微信截图_20220211002146](picture/微信截图_20220211002146.png)

### Ray Tracing Ideal Thin Lenses

![picture/微信截图_20220211002221](picture/微信截图_20220211002221.png)

![picture/微信截图_20220211002228](picture/微信截图_20220211002228.png)

![picture/微信截图_20220211002235](picture/微信截图_20220211002235.png)

### Depth of Field

![picture/微信截图_20220211002309](picture/微信截图_20220211002309.png)

![picture/微信截图_20220211002317](picture/微信截图_20220211002317.png)

![picture/微信截图_20220211002325](picture/微信截图_20220211002325.png)

![picture/微信截图_20220211002335](picture/微信截图_20220211002335.png)

http://graphics.stanford.edu/courses/cs178/applets/dof.html

### Light Field / Lumigraph

![picture/微信截图_20220211024610](picture/微信截图_20220211024610.png)

![picture/微信截图_20220211024620](picture/微信截图_20220211024620.png)

![picture/微信截图_20220211024628](picture/微信截图_20220211024628.png)

![picture/微信截图_20220211024634](picture/微信截图_20220211024634.png)

![picture/微信截图_20220211024642](picture/微信截图_20220211024642.png)

![picture/微信截图_20220211024650](picture/微信截图_20220211024650.png)

![picture/微信截图_20220211024657](picture/微信截图_20220211024657.png)

![picture/微信截图_20220211024705](picture/微信截图_20220211024705.png)

![picture/微信截图_20220211024724](picture/微信截图_20220211024724.png)

![picture/微信截图_20220211024732](picture/微信截图_20220211024732.png)

![picture/微信截图_20220211024738](picture/微信截图_20220211024738.png)

![picture/微信截图_20220211024745](picture/微信截图_20220211024745.png)

![picture/微信截图_20220211024755](picture/微信截图_20220211024755.png)

![picture/微信截图_20220211024803](picture/微信截图_20220211024803.png)

![picture/微信截图_20220211024810](picture/微信截图_20220211024810.png)

![picture/微信截图_20220211024817](picture/微信截图_20220211024817.png)

![picture/微信截图_20220211024824](picture/微信截图_20220211024824.png)

![picture/微信截图_20220211024832](picture/微信截图_20220211024832.png)

![picture/微信截图_20220211024843](picture/微信截图_20220211024843.png)

![picture/微信截图_20220211024851](picture/微信截图_20220211024851.png)

![picture/微信截图_20220211024902](picture/微信截图_20220211024902.png)

#### Light Field Camera

![picture/微信截图_20220211025056](picture/微信截图_20220211025056.png)

![picture/微信截图_20220211025104](picture/微信截图_20220211025104.png)

![picture/微信截图_20220211025114](picture/微信截图_20220211025114.png)

![picture/微信截图_20220211025124](picture/微信截图_20220211025124.png)

![picture/微信截图_20220211025133](picture/微信截图_20220211025133.png)

![picture/微信截图_20220211025141](picture/微信截图_20220211025141.png)

![picture/微信截图_20220211025149](picture/微信截图_20220211025149.png)

![picture/微信截图_20220211025156](picture/微信截图_20220211025156.png)

![picture/微信截图_20220211025204](picture/微信截图_20220211025204.png)

![picture/微信截图_20220211025211](picture/微信截图_20220211025211.png)

### Color

#### What is color ?

##### Physical Basis of Color

![picture/微信截图_20220211025248](picture/微信截图_20220211025248.png)

![picture/微信截图_20220211025254](picture/微信截图_20220211025254.png)

![picture/微信截图_20220211025301](picture/微信截图_20220211025301.png)

![picture/微信截图_20220211025307](picture/微信截图_20220211025307.png)

![picture/微信截图_20220211025314](picture/微信截图_20220211025314.png)

![picture/微信截图_20220211025321](picture/微信截图_20220211025321.png)

![picture/微信截图_20220211025414](picture/微信截图_20220211025414.png)

##### Biological Basis of Color

![picture/微信截图_20220211025456](picture/微信截图_20220211025456.png)

![picture/微信截图_20220211025504](picture/微信截图_20220211025504.png)

![picture/微信截图_20220211025511](picture/微信截图_20220211025511.png)

![picture/微信截图_20220211025518](picture/微信截图_20220211025518.png)

#### Color perception

##### Tristimulus Theory of Color

![picture/微信截图_20220211025559](picture/微信截图_20220211025559.png)

![picture/微信截图_20220211025606](picture/微信截图_20220211025606.png)

##### Metamerism (同色异谱)

![picture/微信截图_20220211025653](picture/微信截图_20220211025653.png)

![picture/微信截图_20220211025659](picture/微信截图_20220211025659.png)

![picture/微信截图_20220211025708](picture/微信截图_20220211025708.png)

#### Color reproduction / matching

![picture/微信截图_20220211025821](picture/微信截图_20220211025821.png)

![picture/微信截图_20220211025828](picture/微信截图_20220211025828.png)

![picture/微信截图_20220211025833](picture/微信截图_20220211025833.png)

![picture/微信截图_20220211025840](picture/微信截图_20220211025840.png)

![picture/微信截图_20220211025847](picture/微信截图_20220211025847.png)

![picture/微信截图_20220211025853](picture/微信截图_20220211025853.png)

![picture/微信截图_20220211025859](picture/微信截图_20220211025859.png)

![picture/微信截图_20220211025905](picture/微信截图_20220211025905.png)

![picture/微信截图_20220211025912](picture/微信截图_20220211025912.png)

![picture/微信截图_20220211025919](picture/微信截图_20220211025919.png)

![picture/微信截图_20220211025927](picture/微信截图_20220211025927.png)

![picture/微信截图_20220211025934](picture/微信截图_20220211025934.png)

![picture/微信截图_20220211025941](picture/微信截图_20220211025941.png)

#### Color space

![picture/微信截图_20220211030007](picture/微信截图_20220211030007.png)

![picture/微信截图_20220211030013](picture/微信截图_20220211030013.png)

![picture/微信截图_20220211030020](picture/微信截图_20220211030020.png)

![picture/微信截图_20220211030031](picture/微信截图_20220211030031.png)

![picture/微信截图_20220211030037](picture/微信截图_20220211030037.png)

![picture/微信截图_20220211030043](picture/微信截图_20220211030043.png)

#### Perceptually Organized Color Spaces

![picture/微信截图_20220211030122](picture/微信截图_20220211030122.png)

![picture/微信截图_20220211030129](picture/微信截图_20220211030129.png)

![picture/微信截图_20220211030136](picture/微信截图_20220211030136.png)

![picture/微信截图_20220211030142](picture/微信截图_20220211030142.png)

![picture/微信截图_20220211030149](picture/微信截图_20220211030149.png)

![picture/微信截图_20220211030204](picture/微信截图_20220211030204.png)

![picture/微信截图_20220211030213](picture/微信截图_20220211030213.png)

![picture/微信截图_20220211030221](picture/微信截图_20220211030221.png)

![picture/微信截图_20220211030230](picture/微信截图_20220211030230.png)

![picture/微信截图_20220211030236](picture/微信截图_20220211030236.png)

![picture/微信截图_20220211030243](picture/微信截图_20220211030243.png)

![picture/微信截图_20220211030251](picture/微信截图_20220211030251.png)

![picture/微信截图_20220211030259](picture/微信截图_20220211030259.png)

![picture/微信截图_20220211030305](picture/微信截图_20220211030305.png)

![picture/微信截图_20220211030312](picture/微信截图_20220211030312.png)

![picture/微信截图_20220211030319](picture/微信截图_20220211030319.png)

![picture/微信截图_20220211030326](picture/微信截图_20220211030326.png)

![picture/微信截图_20220211030332](picture/微信截图_20220211030332.png)

## Animation

![picture/微信截图_20220211184525](picture/微信截图_20220211184525.png)

### Historical Points in Animation

![picture/微信截图_20220211184551](picture/微信截图_20220211184551.png)

![picture/微信截图_20220211184600](picture/微信截图_20220211184600.png)

![picture/微信截图_20220211184608](picture/微信截图_20220211184608.png)

![picture/微信截图_20220211184616](picture/微信截图_20220211184616.png)

![picture/微信截图_20220211184624](picture/微信截图_20220211184624.png)

![picture/微信截图_20220211184631](picture/微信截图_20220211184631.png)

![picture/微信截图_20220211184639](picture/微信截图_20220211184639.png)

![picture/微信截图_20220211184646](picture/微信截图_20220211184646.png)

![picture/微信截图_20220211184657](picture/微信截图_20220211184657.png)

![picture/微信截图_20220211184709](picture/微信截图_20220211184709.png)

### Keyframe Animation

![picture/微信截图_20220211184759](picture/微信截图_20220211184759.png)

![picture/微信截图_20220211184805](picture/微信截图_20220211184805.png)

![picture/微信截图_20220211184812](picture/微信截图_20220211184812.png)

### Physical Simulation

![picture/微信截图_20220211184838](picture/微信截图_20220211184838.png)

![picture/微信截图_20220211184844](picture/微信截图_20220211184844.png)

![picture/微信截图_20220211184852](picture/微信截图_20220211184852.png)

![picture/微信截图_20220211184859](picture/微信截图_20220211184859.png)

#### Mass Spring System: Example of Modeling a Dynamic System

![picture/微信截图_20220211185011](picture/微信截图_20220211185011.png)

![picture/微信截图_20220212000421](picture/微信截图_20220212000421.png)

![picture/微信截图_20220211185029](picture/微信截图_20220211185029.png)

![picture/微信截图_20220211185034](picture/微信截图_20220211185034.png)

![picture/微信截图_20220211185040](picture/微信截图_20220211185040.png)

![picture/微信截图_20220211185046](picture/微信截图_20220211185046.png)

![picture/微信截图_20220211185052](picture/微信截图_20220211185052.png)

![picture/微信截图_20220211185059](picture/微信截图_20220211185059.png)

![picture/微信截图_20220211185105](picture/微信截图_20220211185105.png)

![picture/微信截图_20220211185111](picture/微信截图_20220211185111.png)

![picture/微信截图_20220211185118](picture/微信截图_20220211185118.png)

![picture/微信截图_20220211185126](picture/微信截图_20220211185126.png)

![picture/微信截图_20220211185136](picture/微信截图_20220211185136.png)

![picture/微信截图_20220211185143](picture/微信截图_20220211185143.png)

### Particle Systems

![picture/微信截图_20220211200403](picture/微信截图_20220211200403.png)

![picture/微信截图_20220211200410](picture/微信截图_20220211200410.png)

![picture/微信截图_20220211200416](picture/微信截图_20220211200416.png)

![picture/微信截图_20220211200424](picture/微信截图_20220211200424.png)

![picture/微信截图_20220211200432](picture/微信截图_20220211200432.png)

![picture/微信截图_20220211200440](picture/微信截图_20220211200440.png)

![picture/微信截图_20220211200450](picture/微信截图_20220211200450.png)

![picture/微信截图_20220211200457](picture/微信截图_20220211200457.png)

![picture/微信截图_20220211200510](picture/微信截图_20220211200510.png)

### Kinematics

#### Forward Kinematics

![picture/微信截图_20220211200546](picture/微信截图_20220211200546.png)

![picture/微信截图_20220211200558](picture/微信截图_20220211200558.png)

![picture/微信截图_20220211200606](picture/微信截图_20220211200606.png)

![picture/微信截图_20220211200613](picture/微信截图_20220211200613.png)

![picture/微信截图_20220211200623](picture/微信截图_20220211200623.png)

![picture/微信截图_20220211200630](picture/微信截图_20220211200630.png)

#### Inverse Kinematics

![picture/微信截图_20220211200705](picture/微信截图_20220211200705.png)

![picture/微信截图_20220211200711](picture/微信截图_20220211200711.png)

![picture/微信截图_20220211200720](picture/微信截图_20220211200720.png)

![picture/微信截图_20220211200726](picture/微信截图_20220211200726.png)

![picture/微信截图_20220211200732](picture/微信截图_20220211200732.png)

![picture/微信截图_20220211200739](picture/微信截图_20220211200739.png)

![picture/微信截图_20220211200745](picture/微信截图_20220211200745.png)

![picture/微信截图_20220211200753](picture/微信截图_20220211200753.png)

### Rigging

![picture/微信截图_20220211200823](picture/微信截图_20220211200823.png)

![picture/微信截图_20220211200830](picture/微信截图_20220211200830.png)

![picture/微信截图_20220211200843](picture/微信截图_20220211200843.png)

![picture/微信截图_20220211200853](picture/微信截图_20220211200853.png)

### Motion Capture

![picture/微信截图_20220211200925](picture/微信截图_20220211200925.png)

![picture/微信截图_20220211200932](picture/微信截图_20220211200932.png)

![picture/微信截图_20220211200944](picture/微信截图_20220211200944.png)

![picture/微信截图_20220211200954](picture/微信截图_20220211200954.png)

![picture/微信截图_20220211201002](picture/微信截图_20220211201002.png)

![picture/微信截图_20220211201009](picture/微信截图_20220211201009.png)

![picture/微信截图_20220211201021](picture/微信截图_20220211201021.png)

![picture/微信截图_20220211201029](picture/微信截图_20220211201029.png)

![picture/微信截图_20220211201043](picture/微信截图_20220211201043.png)

![picture/微信截图_20220211201103](picture/微信截图_20220211201103.png)

## Simulation

### Single particle simulation

![picture/微信截图_20220211201354](picture/微信截图_20220211201354.png)

![picture/微信截图_20220211202432](picture/微信截图_20220211202432.png)

![picture/微信截图_20220211202439](picture/微信截图_20220211202439.png)

![picture/微信截图_20220211202444](picture/微信截图_20220211202444.png)

![picture/微信截图_20220211202452](picture/微信截图_20220211202452.png)

![picture/微信截图_20220211202500](picture/微信截图_20220211202500.png)

![picture/微信截图_20220211202507](picture/微信截图_20220211202507.png)

![picture/微信截图_20220211202516](picture/微信截图_20220211202516.png)

#### Combating Instability

![picture/微信截图_20220211202651](picture/微信截图_20220211202651.png)

![picture/微信截图_20220211202658](picture/微信截图_20220211202658.png)

![picture/微信截图_20220211202706](picture/微信截图_20220211202706.png)

![picture/微信截图_20220211202713](picture/微信截图_20220211202713.png)

![picture/微信截图_20220211202721](picture/微信截图_20220211202721.png)

![picture/微信截图_20220211202727](picture/微信截图_20220211202727.png)

![picture/微信截图_20220211202734](picture/微信截图_20220211202734.png)

![picture/微信截图_20220211202740](picture/微信截图_20220211202740.png)

![picture/微信截图_20220211202747](picture/微信截图_20220211202747.png)

### Rigid body simulation

![picture/微信截图_20220211202753](picture/微信截图_20220211202753.png)

### Fluid simulation

![picture/微信截图_20220211202914](picture/微信截图_20220211202914.png)

![picture/微信截图_20220211202920](picture/微信截图_20220211202920.png)

![picture/微信截图_20220211202928](picture/微信截图_20220211202928.png)

