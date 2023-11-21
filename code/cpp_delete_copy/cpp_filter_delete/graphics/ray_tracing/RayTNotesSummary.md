# Ray tracing in one weekend

## Output an image

```c++
#include <iostream>

int main() {

    // Image

    const int image_width = 256;
    const int image_height = 256;

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        for (int i = 0; i < image_width; ++i) {
            auto r = double(i) / (image_width-1);
            auto g = double(j) / (image_height-1);
            auto b = 0.25;

            int ir = static_cast<int>(255.999 * r);
            int ig = static_cast<int>(255.999 * g);
            int ib = static_cast<int>(255.999 * b);

            std::cout << ir << ' ' << ig << ' ' << ib << '\n';
        }
    }
}
```

1. The pixels are written out in rows with pixels left to right.
2. The rows are written out from top to bottom.
3. By convention, each of the red/green/blue components range from 0.0 to 1.0. We will relax that later when we internally use high dynamic range, but before output we will tone map to the zero to one range, so this code won’t change.
4. Red goes from fully off (black) to fully on (bright red) from left to right, and green goes from black at the bottom to fully on at the top. Red and green together make yellow so we should expect the upper right corner to be yellow.

---

- Read into a **filestream** rather than **console output** 

```c++
#include <ofstream>
#include <cstdlib>

int main(){
    
    std::ofstream file;
    
    file.open("image.ppm");
    
    if (!file){
        std::cerr << "error opening" << "\n";
        exit(1)
     }
    
    file << "ppm input to this file.";
    file.close();
}
```

### Progress indicator

```c++
 for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            auto r = double(i) / (image_width-1);
            auto g = double(j) / (image_height-1);
            auto b = 0.25;

            int ir = static_cast<int>(255.999 * r);
            int ig = static_cast<int>(255.999 * g);
            int ib = static_cast<int>(255.999 * b);

            std::cout << ir << ' ' << ig << ' ' << ib << '\n';
        }
    }

    std::cerr << "\nDone.\n";
```

## Vec3 Class

```c++
#ifndef VEC3_H
#define VEC3_H

#include <cmath>
#include <iostream>

using std::sqrt;

class vec3 {
    public:
        vec3() : e{0,0,0} {}
        vec3(double e0, double e1, double e2) : e{e0, e1, e2} {}

        double x() const { return e[0]; }
        double y() const { return e[1]; }
        double z() const { return e[2]; }

        vec3 operator-() const { return vec3(-e[0], -e[1], -e[2]); }
        double operator[](int i) const { return e[i]; }
        double& operator[](int i) { return e[i]; }

        vec3& operator+=(const vec3 &v) {
            e[0] += v.e[0];
            e[1] += v.e[1];
            e[2] += v.e[2];
            return *this;
        }

        vec3& operator*=(const double t) {
            e[0] *= t;
            e[1] *= t;
            e[2] *= t;
            return *this;
        }

        vec3& operator/=(const double t) {
            return *this *= 1/t;
        }

        double length() const {
            return sqrt(length_squared());
        }

        double length_squared() const {
            return e[0]*e[0] + e[1]*e[1] + e[2]*e[2];
        }

    public:
        double e[3];
};

// Type aliases for vec3
using point3 = vec3;   // 3D point
using color = vec3;    // RGB color

#endif
```

### Vec3 utility functions

```c++
// vec3 Utility Functions

inline std::ostream& operator<<(std::ostream &out, const vec3 &v) {
    return out << v.e[0] << ' ' << v.e[1] << ' ' << v.e[2];
}

inline vec3 operator+(const vec3 &u, const vec3 &v) {
    return vec3(u.e[0] + v.e[0], u.e[1] + v.e[1], u.e[2] + v.e[2]);
}

inline vec3 operator-(const vec3 &u, const vec3 &v) {
    return vec3(u.e[0] - v.e[0], u.e[1] - v.e[1], u.e[2] - v.e[2]);
}

inline vec3 operator*(const vec3 &u, const vec3 &v) {
    return vec3(u.e[0] * v.e[0], u.e[1] * v.e[1], u.e[2] * v.e[2]);
}

inline vec3 operator*(double t, const vec3 &v) {
    return vec3(t*v.e[0], t*v.e[1], t*v.e[2]);
}

inline vec3 operator*(const vec3 &v, double t) {
    return t * v;
}

inline vec3 operator/(vec3 v, double t) {
    return (1/t) * v;
}

inline double dot(const vec3 &u, const vec3 &v) {
    return u.e[0] * v.e[0]
         + u.e[1] * v.e[1]
         + u.e[2] * v.e[2];
}

inline vec3 cross(const vec3 &u, const vec3 &v) {
    return vec3(u.e[1] * v.e[2] - u.e[2] * v.e[1],
                u.e[2] * v.e[0] - u.e[0] * v.e[2],
                u.e[0] * v.e[1] - u.e[1] * v.e[0]);
}

inline vec3 unit_vector(vec3 v) {
    return v / v.length();
}
```

### Color utility functions

```c++
#ifndef COLOR_H
#define COLOR_H

#include "vec3.h"

#include <iostream>

void write_color(std::ostream &out, color pixel_color) {
    // Write the translated [0,255] value of each color component.
    out << static_cast<int>(255.999 * pixel_color.x()) << ' '
        << static_cast<int>(255.999 * pixel_color.y()) << ' '
        << static_cast<int>(255.999 * pixel_color.z()) << '\n';
}

#endif
```

---

- Back to main code use this

```c++
#include "color.h"
#include "vec3.h"

#include <iostream>

int main() {

    // Image

    const int image_width = 256;
    const int image_height = 256;

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            color pixel_color(double(i)/(image_width-1), double(j)/(image_height-1), 0.25);
            write_color(std::cout, pixel_color);
        }
    }
    
    std::cerr << "\nDone.\n";
}
```

## Rays , simple camera, and background

```c++
#ifndef RAY_H
#define RAY_H

#include "vec3.h"

class ray {
    public:
        ray() {}
        ray(const point3& origin, const vec3& direction)
            : orig(origin), dir(direction)
        {}

        point3 origin() const  { return orig; }
        vec3 direction() const { return dir; }

        point3 at(double t) const {
            return orig + t*dir;
        }

    public:
        point3 orig;
        vec3 dir;
};

#endif
```

### Sending rays into the scene

$r$ goes approximately to pixel centers.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126182048618.png)

```c++
#include "color.h"
#include "ray.h"
#include "vec3.h"

#include <iostream>

/* linear interpolation , read above*/
color ray_color(const ray& r) {
    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}

int main() {

    // Image
    const auto aspect_ratio = 16.0 / 9.0;
    const int image_width = 400;
    const int image_height = static_cast<int>(image_width / aspect_ratio);

    // Camera

    auto viewport_height = 2.0;
    auto viewport_width = aspect_ratio * viewport_height;
    auto focal_length = 1.0;

    auto origin = point3(0, 0, 0);
    auto horizontal = vec3(viewport_width, 0, 0);
    auto vertical = vec3(0, viewport_height, 0);
    auto lower_left_corner = origin - horizontal/2 - vertical/2 - vec3(0, 0, focal_length);

    // Render

    std::cout << "P3\n" << image_width << " " << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            auto u = double(i) / (image_width-1);
            auto v = double(j) / (image_height-1);
            ray r(origin, lower_left_corner + u*horizontal + v*vertical - origin);
            color pixel_color = ray_color(r);
            write_color(std::cout, pixel_color);
        }
    }

    std::cerr << "\nDone.\n";
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126173956590.png)

1. Calculate ray from eye to pixel.
2. Determine which object ray intersects.
3. Compute a color for the intersection point.

---

- Make simple camera.

- Make simple `ray_color(ray)` to return color and make basic gradient.

- Use $16:9$ aspect ratio

- Virtual viewport $\to$ aspect ratio = rendered image

  - 2 Units high

  - Point to plane is 1 unit high (*focal length*)

- Eye (*camera center*) at $(0,0,0)$ 
- $y$ = up
- $x$ = right
- $-z$ = into the screen (RH coordinates) 

---

- Traversal 
  - Upper left using two offset vectors to move ray endpoint across screen

## Adding Sphere

- Lets **a single object** to **our ray tracer**. 
  - Use spheres **in ray tracers** because calculating a ray hits a sphere is **pretty simple**

### Ray sphere intersection

- Arbitrary center $(C_x, C_y, C_z)$: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126192552354.png)

- General Sphere (implicit equation $f(x,y,z)$ taking -minus R^2) :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126192615734.png)

- Where $R$ is the **radius**. 

---

- Require vectors in our formulae. 
- Vector from *center* to *point* $P = (x,y,z)$ is $(P-C)$: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126193705423.png)

- Thus our ray we plug in $P= P(t)$ to **test intersection**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126193854266.png)

- vectors / r $\to$ *constant known*
- $t$ $\to$ *unknown*
  - Solve for $t$ 
    - Either **positive** (*two real solutions*) 
    - **negative** (*no real solutions*)
    - **zero** (*one real solution*) 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126194056600.png)

### Create first Raytraced image

```c++
/* literally just calculates a discriminant */
bool hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;
    return (discriminant > 0);
}

color ray_color(const ray& r) {
    /* center based on the camera 0,0,-1 which essentially means its on the plane exactly */
    if (hit_sphere(point3(0,0,-1), 0.5, r))
        return color(1, 0, 0);
    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126213305500.png)

## Surface Normals and Multiple Objects

- Make surface normal with unit length for shading.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126214229060.png)

- Visualise normals with **colour maps**

- Direction of normal is the **point** minus the **center**
  - Thus from the **center** to **us** on the earth is **upwards pointing**.
- Trick to **visualise normals**
  - assume $n$ is a **unit vector** between $-1 $ and $1$. 
    - Map each component to interval of $0$ and $1$ then map $x,y,z \to r,g,b$ 
- For *normal* $\to$ require **hit point** 
  - Sphere *directly* in front of the **camera** thus *not concern* with **negative t values** at the moment.
    - Assume the **just closest hit point** (*smallest* $t$ ) 
      - Changes in code enable us to compute then **visualise** $n$. 

```c++
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;
    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-b - sqrt(discriminant) ) / (2.0*a);
    }
}

color ray_color(const ray& r) {
    auto t = hit_sphere(point3(0,0,-1), 0.5, r);
    /* if we hit, then use the hit point to calculate a normal, then map to between 0 and 1 as normal
    	passing this value into color.
    */
    if (t > 0.0) {
        vec3 N = unit_vector(r.at(t) - vec3(0,0,-1));
        return 0.5*color(N.x()+1, N.y()+1, N.z()+1);
    }
    vec3 unit_direction = unit_vector(r.direction());
    t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

### Simplifying ray sphere intersection code

```c++
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;

    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-b - sqrt(discriminant) ) / (2.0*a);
    }
}
```

- Vector **dotted** *with itself* is the **squared length** of that vector
- Equation for $b$ has a **factor** of *two in it*
  - Consider what occurs to the **quadratic equation** if $b = 2h$ (*2 half b's*):

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126222149447.png)

```c++
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = r.direction().length_squared();
    auto half_b = dot(oc, r.direction());
    auto c = oc.length_squared() - radius*radius;
    auto discriminant = half_b*half_b - a*c;

    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-half_b - sqrt(discriminant) ) / a;
    }
}
```

### Abstraction for hittable objects

- Design **abstract class** for a **hittable object** by a *ray*
  - `hittable` is a suitable name.
- Has a `hit` function takes in a **ray**
  - Add a **valid interval** for hits $t_{min} \to t_{max}$ which is the **range** of **acceptance** $t_{min} < t < t_{max}$ 
- May **compute normal** when **intersection** occurs
- May **hit something closer** doing our **search**
  - Require the **closest normal only**.
- Store hit **data** in a **structure**.

```c++
#ifndef HITTABLE_H
#define HITTABLE_H

#include "ray.h"

struct hit_record {
    point3 p;
    vec3 normal;
    double t;
};

class hittable {
    public:
        virtual bool hit(const ray& r, double t_min, double t_max, hit_record& rec) const = 0;
};

#endif
```

- Here is the sphere

```c++
#ifndef SPHERE_H
#define SPHERE_H

#include "hittable.h"
#include "vec3.h"

class sphere : public hittable {
    public:
        sphere() {}
        sphere(point3 cen, double r) : center(cen), radius(r) {};

        virtual bool hit(
            const ray& r, double t_min, double t_max, hit_record& rec) const override;

    public:
        point3 center;
        double radius;
};

bool sphere::hit(const ray& r, double t_min, double t_max, hit_record& rec) const {
    vec3 oc = r.origin() - center;
    auto a = r.direction().length_squared();
    auto half_b = dot(oc, r.direction());
    auto c = oc.length_squared() - radius*radius;

    auto discriminant = half_b*half_b - a*c;
    if (discriminant < 0) return false;
    auto sqrtd = sqrt(discriminant);

    // Find the nearest root that lies in the acceptable range.
    auto root = (-half_b - sqrtd) / a;
    if (root < t_min || t_max < root) {
        root = (-half_b + sqrtd) / a;
        if (root < t_min || t_max < root)
            return false;
    }

    rec.t = root;
    rec.p = r.at(rec.t);
    rec.normal = (rec.p - center) / radius;

    return true;
}

#endif
```

### Front Faces vs Back Faces

- Normal found always in **direction** of the **center** to **intersection point**
- If the **ray intersects** the *sphere* from the **outside**, this normal points **against ray**. 
- If the **ray intersects** the *sphere* from the **inside** , the normal points **with the ray**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126225308188.png)

- Must select **one possibility** 
  - Must determine the **side** of the **surface** the ray is **coming from**
    - Vital for when **rendering** is *different* on each sides.

---

- Normals **always pointing out**
  - Decide *which* side the **ray** is on when we **color it**.
    - Compare the **ray** with the **normal**. 
      - if ray/normal **same direction**, the ray is **inside**, otherwise **outside**.
- Determine via **dot product**.

```c++
/* positive dot means inside the sphere */
if (dot(ray_direction, outward_normal) > 0.0) {
    // ray is inside the sphere
    ...
} else {
    // ray is outside the sphere
    ...
}
```

- Using **normals** that go **against the ray**
  - We *cannot use dot* 
    - We instead **store information**

```c++
bool front_face;
if (dot(ray_direction, outward_normal) > 0.0) {
    // ray is inside the sphere
    normal = -outward_normal;
    front_face = false;
} else {
    // ray is outside the sphere
    normal = outward_normal;
    front_face = true;
}
```

- Decision is whether we want to **determine decision** on the *intersection* or time of **coloring**. 
  - Place determination at **geometry time**
    - This is *preference*.
- Adding `front_face` and `hit_record` struct.
  - Adding a function to solve this calculation for us.

```c++
struct hit_record {
    point3 p;
    vec3 normal;
    double t;
    bool front_face;

    inline void set_face_normal(const ray& r, const vec3& outward_normal) {
        front_face = dot(r.direction(), outward_normal) < 0;
        normal = front_face ? outward_normal :-outward_normal;
    }
};
```

### List of hittable objects

- Have some **generic object** called a `hittable` that the **ray intersects** with
  - Add class that **stores** a *list* of `hittables` 

```c++
#ifndef HITTABLE_LIST_H
#define HITTABLE_LIST_H

#include "hittable.h"

#include <memory>
#include <vector>

using std::shared_ptr;
using std::make_shared;

class hittable_list : public hittable {
    public:
        hittable_list() {}
        hittable_list(shared_ptr<hittable> object) { add(object); }

        void clear() { objects.clear(); }
        void add(shared_ptr<hittable> object) { objects.push_back(object); }

        virtual bool hit(
            const ray& r, double t_min, double t_max, hit_record& rec) const override;

    public:
        std::vector<shared_ptr<hittable>> objects;
};

bool hittable_list::hit(const ray& r, double t_min, double t_max, hit_record& rec) const {
    hit_record temp_rec;
    bool hit_anything = false;
    auto closest_so_far = t_max;

    for (const auto& object : objects) {
        if (object->hit(r, t_min, closest_so_far, temp_rec)) {
            hit_anything = true;
            closest_so_far = temp_rec.t;
            rec = temp_rec;
        }
    }

    return hit_anything;
}

#endif
```

#### New C++ Features

- The `hittable_list` class code uses `shared_ptr<T>` 
  - Have **reference** *counting* semantics.
    - Decreases on **out of scope**.

```c++
shared_ptr<double> double_ptr = make_shared<double>(0.37);
shared_ptr<vec3>   vec3_ptr   = make_shared<vec3>(1.414214, 2.718281, 1.618034);
shared_ptr<sphere> sphere_ptr = make_shared<sphere>(point3(0,0,0), 1.0);
```

- **Shared pointer** is *first initialized* with a **newly allocated object** 

---

- `make_shared<T>(constructor_params_T ...)` 
  - Allocates a **new instance** of type $T$ using **constructor parameters**.
    - Returns `shared_ptr<T>` 

- The **type** can be **automatically** deduced by the **return type** `make_shared<type>(...)` 
  - Above lines can be **simply expressed** using `C++` *auto* type specifier.

```c++
auto double_ptr = make_shared<double>(0.37);
auto vec3_ptr   = make_shared<vec3>(1.414214, 2.718281, 1.618034);
auto sphere_ptr = make_shared<sphere>(point3(0,0,0), 1.0);
```

- We use **shared pointers** in *our code*
  - Because it **allows geometries** to share some **common instance** (*sphere bunch* using **same texture map material**) 
    - Makes **memory management** automatic and **easier** to *reason about*.
- Included in `<memory>` header

#### Common constants and utility functions

```c++
#ifndef RTWEEKEND_H
#define RTWEEKEND_H

#include <cmath>
#include <limits>
#include <memory>


// Usings

using std::shared_ptr;
using std::make_shared;
using std::sqrt;

// Constants

const double infinity = std::numeric_limits<double>::infinity();
const double pi = 3.1415926535897932385;

// Utility Functions

inline double degrees_to_radians(double degrees) {
    return degrees * pi / 180.0;
}

// Common Headers

#include "ray.h"
#include "vec3.h"

#endif
```

- New main

```c++
#include "rtweekend.h"

#include "color.h"
#include "hittable_list.h"
#include "sphere.h"

#include <iostream>
color ray_color(const ray& r, const hittable& world) {
    hit_record rec;
    if (world.hit(r, 0, infinity, rec)) {
        return 0.5 * (rec.normal + color(1,1,1));
    }
    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}

int main() {

    // Image

    const auto aspect_ratio = 16.0 / 9.0;
    const int image_width = 400;
    const int image_height = static_cast<int>(image_width / aspect_ratio);

    // World
    hittable_list world;
    world.add(make_shared<sphere>(point3(0,0,-1), 0.5));
    world.add(make_shared<sphere>(point3(0,-100.5,-1), 100));

    // Camera

    auto viewport_height = 2.0;
    auto viewport_width = aspect_ratio * viewport_height;
    auto focal_length = 1.0;

    auto origin = point3(0, 0, 0);
    auto horizontal = vec3(viewport_width, 0, 0);
    auto vertical = vec3(0, viewport_height, 0);
    auto lower_left_corner = origin - horizontal/2 - vertical/2 - vec3(0, 0, focal_length);

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            auto u = double(i) / (image_width-1);
            auto v = double(j) / (image_height-1);
            ray r(origin, lower_left_corner + u*horizontal + v*vertical);
            color pixel_color = ray_color(r, world);
            write_color(std::cout, pixel_color);
        }
    }

    std::cerr << "\nDone.\n";
}
```

- Yields this picture:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220127194945934.png)

## Antialiasing

- Camera $\to$ *no jaggies* along **edges** because the **edge pixels** are *blend* some **foreground** some:
  - We can **get** the **same effect** by *averaging* many samples **inside each pixel**.
    - No *stratification*

### Random Number utilities

- Return **canonical random number** which by convention returns range $0 \leq r < 1$ 
  - The *less than* before the $1$ is vital as we **sometimes take care of this**.
- Use `rand()` in `<cstdlib>` 
  - This returns a **random integer** in range $0$ and `RAND_MAX` 
    - Thus obtain a **real random** number as **desired** with the **following code snippet**, added to `rtweekend.h` 

```c++
#include <cstdlib>
...

inline double random_double() {
    // Returns a random real in [0,1).
    return rand() / (RAND_MAX + 1.0);
}

inline double random_double(double min, double max) {
    // Returns a random real in [min,max).
    return min + (max-min)*random_double();
}
```

- C++ does **not standardly** have some **random number generator.**
  - Newer *versions* of $C++$ addressed with `<random>` header.
    - Obtain a **random number** with **the conditions**

```c++
#include <random>

inline double random_double() {
    static std::uniform_real_distribution<double> distribution(0.0, 1.0);
    static std::mt19937 generator;
    return distribution(generator);
}
```

### Generating pixels with multiple samples

- For some **given pixel** $\to$ have *several samples* within that **pixel** and **send rays** *through each* of the **samples**
  - Colors of these **rays** are **averaged**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220127201522765.png)

- Create some `camera` class to **manage** the **virtual camera** with related tasks of **scene sample**.
  - Following **class** implements a **simple camera** using **axis aligned camera** as *before*.

```c++
#ifndef CAMERA_H
#define CAMERA_H

#include "rtweekend.h"

class camera {
    public:
        camera() {
            auto aspect_ratio = 16.0 / 9.0;
            auto viewport_height = 2.0;
            auto viewport_width = aspect_ratio * viewport_height;
            auto focal_length = 1.0;

            origin = point3(0, 0, 0);
            horizontal = vec3(viewport_width, 0.0, 0.0);
            vertical = vec3(0.0, viewport_height, 0.0);
            lower_left_corner = origin - horizontal/2 - vertical/2 - vec3(0, 0, focal_length);
        }

        ray get_ray(double u, double v) const {
            return ray(origin, lower_left_corner + u*horizontal + v*vertical - origin);
        }

    private:
        point3 origin;
        point3 lower_left_corner;
        vec3 horizontal;
        vec3 vertical;
};
#endif
```

- To **handle** the *multi sampled color computation*
  - Update `write_color()` function.
- Rather than adding in **fractional contribution** each time
  - we **accumulate** *more light* to the *color*.
    - Just add the **full color each iteration.**
      - Then **perform** a *single divide* at the **end** (by the **number of samples**) when **writing the color**.
- Addition $\to$ add a **handy utility function** to the `rtweekend.h` utility header `clamp(x, min ,max)` 
  - Clamps the value $x$ to the **range** $[min, max]$ 

```c++
inline double clamp(double x, double min, double max) {
    if (x < min) return min;
    if (x > max) return max;
    return x;
}
```

```c++
void write_color(std::ostream &out, color pixel_color, int samples_per_pixel) {
    auto r = pixel_color.x();
    auto g = pixel_color.y();
    auto b = pixel_color.z();

    // Divide the color by the number of samples.
    auto scale = 1.0 / samples_per_pixel;
    r *= scale;
    g *= scale;
    b *= scale;

    // Write the translated [0,255] value of each color component.
    out << static_cast<int>(256 * clamp(r, 0.0, 0.999)) << ' '
        << static_cast<int>(256 * clamp(g, 0.0, 0.999)) << ' '
        << static_cast<int>(256 * clamp(b, 0.0, 0.999)) << '\n';
}
```

- Main changed also.

```c++
#include "camera.h"

...

int main() {

    // Image

    const auto aspect_ratio = 16.0 / 9.0;
    const int image_width = 400;
    const int image_height = static_cast<int>(image_width / aspect_ratio);
    const int samples_per_pixel = 100;

    // World

    hittable_list world;
    world.add(make_shared<sphere>(point3(0,0,-1), 0.5));
    world.add(make_shared<sphere>(point3(0,-100.5,-1), 100));

    // Camera
    camera cam;

    // Render

    std::cout << "P3\n" << image_width << " " << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            color pixel_color(0, 0, 0);
            for (int s = 0; s < samples_per_pixel; ++s) {
                auto u = (i + random_double()) / (image_width-1);
                auto v = (j + random_double()) / (image_height-1);
                ray r = cam.get_ray(u, v);
                pixel_color += ray_color(r, world);
            }
            write_color(std::cout, pixel_color, samples_per_pixel);
        }
    }

    std::cerr << "\nDone.\n";
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220127220217427.png)

## Diffuse Materials

- Objects 
- Multiple rays per pixel.
- Make some **realistic looking materials.** 

---

- Start with **diffuse** *matte* **materials**.
  - Question whether we *mix / match* geometry and **materials** (materials to multiple spheres).
  - Alternatively being **geometry** and **material** are *tightly bound*.
    -  Useful for **procedural objects** where geometry and **material** are *tightly linked* 
- Go **seperate** as its **common in most renders** 

### Simple Diffuse material.

- Take on color of **surroundings**
  - **Modulate** with our **own intrinsic color**. 
- Light that **reflects** off a **diffuse surface** has the direction **randomized**
  - Sending *three rays* into a **crack** between **two diffuse surface**
    - They have **different random behaviour**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220127223150853.png)

- Potential for **absorption** over *reflection*

  - The **darker the surface** $\to$ more *likely some absorption is* thus **darker**.

- **Randomized** direction produces surfaces that **look matte**
  - A simple way to do this turns out to be the **correct** method for **ideal diffuse surfaces**.

---

- There are **two unit radius spheres** that are *tangent* to some **hit point** $p$ of a surface.
  - Each having *center* $(P+n)$ and $(P-n)$, where $n$ is the **normal of the surface**.
    - Sphere *center* $(P-n)$ consider **inside** the *surface*
    - Sphere *center* $(P+n)$ consider **outside** the *surface*.
- Select the **tangent** unit radius sphere that is on the **same side** of the **surface** as the **ray origin**
  - Pick random point $S$ in the **unit radius sphere**.
  - Send a **ray** from the **hit point** to *random point* $S$
  - Giving us $(P-s)$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220127232638034.png)

- Select **random point** in the **unit radius sphere**
  - The **simplest algorithm** $\to$ *rejection method*.
- Select some **random point** in the *unit cube* where $x,y$ and $z$ range $-1 \to +1$ 
  - Reject this point and **try again** if the **point** is **outside the sphere**.

```c++
class vec3 {
  public:
    ...
    inline static vec3 random() {
        return vec3(random_double(), random_double(), random_double());
    }

    inline static vec3 random(double min, double max) {
        return vec3(random_double(min,max), random_double(min,max), random_double(min,max));
    }
```

```c++
vec3 random_in_unit_sphere() {
    while (true) {
        auto p = vec3::random(-1,1);
        if (p.length_squared() >= 1) continue;
        return p;
    }
}
```

- Update `ray_color()` to use the **new random direction generator**.

```c++
color ray_color(const ray& r, const hittable& world) {
    hit_record rec;

    if (world.hit(r, 0, infinity, rec)) {
        point3 target = rec.p + rec.normal + random_in_unit_sphere();
        return 0.5 * ray_color(ray(rec.p, target - rec.p), world);
    }

    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

### Limit number of child rays

- `ray_color` is **recursive**.
  - Decide on **base case** to *break this.*
    - This is when it **fails** to *hit*.
- This may be **long** enough to **blow the stack**
  - Guard against this $\to$ limit the **recursion depth**
    - Return **no light** contribution at the **maximum depth**.

```c++
color ray_color(const ray& r, const hittable& world, int depth) {
    hit_record rec;

    // If we've exceeded the ray bounce limit, no more light is gathered.
    if (depth <= 0)
        return color(0,0,0);

    if (world.hit(r, 0, infinity, rec)) {
        point3 target = rec.p + rec.normal + random_in_unit_sphere();
        return 0.5 * ray_color(ray(rec.p, target - rec.p), world, depth-1);
    }

    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}

...

int main() {

    // Image

    const auto aspect_ratio = 16.0 / 9.0;
    const int image_width = 400;
    const int image_height = static_cast<int>(image_width / aspect_ratio);
    const int samples_per_pixel = 100;
    const int max_depth = 50;
    ...

    // Render

    std::cout << "P3\n" << image_width << " " << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            color pixel_color(0, 0, 0);
            for (int s = 0; s < samples_per_pixel; ++s) {
                auto u = (i + random_double()) / (image_width-1);
                auto v = (j + random_double()) / (image_height-1);
                ray r = cam.get_ray(u, v);
                pixel_color += ray_color(r, world, max_depth);
            }
            write_color(std::cout, pixel_color, samples_per_pixel);
        }
    }

    std::cerr << "\nDone.\n";
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128031033034.png)

### Gamma correction for accurate color intensity

https://www.youtube.com/watch?v=wFx0d9c8WMs

- Shadowing under the sphere
- Dark picture, must fix now
- Sphere should be **light** as all image viewers assume **gamma correctness**.
  - $0 \to 1$ values have some **transform** being **stored** as a **byte**.
    - There are **many purposes** for this, but must be aware.
- First **approximation** $\to$ use $gamma \ 2$ 
  - Means to raise the **color** to $\frac{1}{gamma}$ power
    - In this case $\frac{1}{2}$ thus **square root**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128032653913.png)

### Fixing Shadow acne

- There is a **subtle bug**
  - Some **reflected rays** hit the **object** hit the **object** they are *reflecting* off not at **exactly** $t = 0$ 
    - But instead, $t = -0.0000001$ or $t = 0.00000001$ or whatever **floating** point **approximation** the **sphere** intersector gives us
      - So we **need** to **ignore** hits **very near zero**.

```c++
if (world.hit(r, 0.001, infinity, rec)) {
```

### True Lambertian Reflection

- The rejection method presented here **produces random points** in the **unit ball offset** along the **surface normal**. 
- This corresponds to **picking directions** on the **hemisphere** with **high probability close to the normal**, and a **lower probability** of **scattering** rays at **grazing angles**. 
- This distribution scales by the $cos^3(\phi)$ where $\phi$ is the angle from the normal. This is useful since **light arriving at shallow** angles **spreads** over a **larger area**, and thus has a **lower contribution** to the **final** **color**.

```c++
inline vec3 random_in_unit_sphere() {
    ...
}
vec3 random_unit_vector() {
    return unit_vector(random_in_unit_sphere());
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128214524129.png)

- This `random_unit_vector()` is a **drop in replacement** for the **existing** `random_in_unit_sphere()` function.

```c++
color ray_color(const ray& r, const hittable& world, int depth) {
    hit_record rec;

    // If we've exceeded the ray bounce limit, no more light is gathered.
    if (depth <= 0)
        return color(0,0,0);

    if (world.hit(r, 0.001, infinity, rec)) {
        point3 target = rec.p + rec.normal + random_unit_vector();
        return 0.5 * ray_color(ray(rec.p, target - rec.p), world, depth-1);
    }

    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

- After render, obtain a **similar image**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128214932958.png)

- Hard to differentiate the **two diffuse methods**

  - Given our scene of **two spheres** is basic

    - Should notice that **two important visual differences**

      1. The shadows are less pronounced after the change
      2. Both spheres are lighter in appearance after the change

- Both of these changes are due to the **more uniform scattering** of the **light rays**, **fewer rays** are **scattering toward the normal**.
-  This means that for **diffuse objects**, they will appear ***lighter*** because **more light bounces toward the camera**. 
- For the **shadows**, **less** **light** bounces straight-up, so the **parts of the larger sphere** directly **underneath the smaller sphere** **are brighter.**

### Alternative diffuse formulation

- Error for **incorrect approximation** of *ideal lambertian diffuse*

  - Reason for the **error persisting** for so *long* is that it **can be difficult** to:

  1. Mathematically prove that the **probability distribution** is **incorrect**
  2. Intuitively explain why a $cos(ϕ)$ **distribution** is **desirable** (and what it would look like)

- **Not a lot** of common, everyday objects are **perfectly diffuse**, so our visual intuition of how these objects behave under light can be poorly formed.

- For the two methods above we **had a random vector**, first of **random length** and then of **unit length**, **offset** from the **hit point by the normal**. It may **not be immediately obvious** why the **vectors should be displaced by the normal**.

- A more intuitive approach is to have a **uniform scatter direction** for **all angles** away **from** the **hit point**, with **no dependence** on the **angle from the normal**. Many of the **first raytracing papers** used this **diffuse method** (before **adopting Lambertian** **diffuse**).

```c++
vec3 random_in_hemisphere(const vec3& normal) {
    vec3 in_unit_sphere = random_in_unit_sphere();
    if (dot(in_unit_sphere, normal) > 0.0) // In the same hemisphere as the normal
        return in_unit_sphere;
    else
        return -in_unit_sphere;
}
```

- Plugging the new formulae into the `ray_color()` function.

```c++
color ray_color(const ray& r, const hittable& world, int depth) {
    hit_record rec;

    // If we've exceeded the ray bounce limit, no more light is gathered.
    if (depth <= 0)
        return color(0,0,0);

    if (world.hit(r, 0.001, infinity, rec)) {
        point3 target = rec.p + random_in_hemisphere(rec.normal);
        return 0.5 * ray_color(ray(rec.p, target - rec.p), world, depth-1);
    }

    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

- Obtain the following image.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128223615563.png)

## Metal

### Abstract class for materials

- If we want **different objects** to have **different materials**, we have a **design decision**. 
- We could have a **universal material** with **lots** of **parameters** and **different material** types **just zero** out some of those **parameters**. This is **not a bad approach**. 
- Or we could have an **abstract material class** that **encapsulates behavior**. I am a fan of the **latter approach**. For our **program** the **material** needs to **do two things**:

1. Produce a **scattered** **ray** (or say it absorbed the incident ray).
2. If **scattered**, say **how** much the **ray should be attenuated**.

- Leading to this abstract class.

```c++
#ifndef MATERIAL_H
#define MATERIAL_H

#include "rtweekend.h"

struct hit_record;

class material {
    public:
        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const = 0;
};

#endif
```

### Data structure to describe ray object intersections

- The `hit_record` avoids a **bung of arguments** to *stuff whatever* what we wish.

- You can use **arguments** instead; it’s a **matter of taste**. 
- **Hittables** and **materials** need to **know each other** so there is some **circularity of the references**. 
- In C++ you just need to **alert the compiler** that the **pointer is to a class,** which the “**class material**” in the **hittable class below** does:

```c++
#include "rtweekend.h"

class material;

struct hit_record {
    point3 p;
    vec3 normal;
    shared_ptr<material> mat_ptr;
    double t;
    bool front_face;

    inline void set_face_normal(const ray& r, const vec3& outward_normal) {
        front_face = dot(r.direction(), outward_normal) < 0;
        normal = front_face ? outward_normal :-outward_normal;
    }
};
```

- What we have set up here is that material will **tell us how rays interact with the surface**. 
- `hit_record` is just a way to stuff a bunch of arguments into a struct so we can send them as a group. 
- When a **ray hits a surface** (a particular sphere for example), the **material pointer** in the `hit_record` will **be set to point** at the **material pointer** the **sphere was given** when it was set up in `main()` when we start. 
- When the `ray_color()` routine gets the `hit_record` it can **call member functions** of the **material pointer** to find out w**hat ray**, if any, is **scattered**.

- To achieve this, we must have a **reference to the material** for our **sphere class** to **returned** within `hit_record`. See the highlighted lines below:

```c++
class sphere : public hittable {
    public:
        sphere() {}
        sphere(point3 cen, double r, shared_ptr<material> m)
            : center(cen), radius(r), mat_ptr(m) {};

        virtual bool hit(
            const ray& r, double t_min, double t_max, hit_record& rec) const override;

    public:
        point3 center;
        double radius;
        shared_ptr<material> mat_ptr;
};

bool sphere::hit(const ray& r, double t_min, double t_max, hit_record& rec) const {
    ...

    rec.t = root;
    rec.p = r.at(rec.t);
    vec3 outward_normal = (rec.p - center) / radius;
    rec.set_face_normal(r, outward_normal);
    rec.mat_ptr = mat_ptr;

    return true;
}
```

### Modelling Light Scatter and Reflectance.

- For the **Lambertian (diffuse)** case we already have, it can either **scatter** always and **attenuate** by its **reflectance** $R$, or it can scatter with no attenuation but absorb the fraction $1−R$ of the rays, or it could be a mixture of those strategies. For **Lambertian materials** we **get** this **simple class**:

```c++
class lambertian : public material {
    public:
        lambertian(const color& a) : albedo(a) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            auto scatter_direction = rec.normal + random_unit_vector();
            scattered = ray(rec.p, scatter_direction);
            attenuation = albedo;
            return true;
        }

    public:
        color albedo;
};
```

- Note we could just as **well only scatter** with some **probability** $p$ and have **attenuation** be $albedo/p$.
- If the **random unit vector** we generate is exactly **opposite the normal vector**, the **two will sum to zero**, which will result in a **zero scatter direction vector**. 
- This leads to **bad scenarios** later on (**infinities** and **NaNs**), so we need to **intercept the condition** before we pass it on.
- In service of this, we'll create a **new vector** method — `vec3::near_zero()` — that **returns true** if the **vector** is very **close** to **zero in all dimensions**.

```c++
class vec3 {
    ...
    bool near_zero() const {
        // Return true if the vector is close to zero in all dimensions.
        const auto s = 1e-8;
        return (fabs(e[0]) < s) && (fabs(e[1]) < s) && (fabs(e[2]) < s);
    }
    ...
};
```

```c++
class lambertian : public material {
    public:
        lambertian(const color& a) : albedo(a) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            auto scatter_direction = rec.normal + random_unit_vector();

            // Catch degenerate scatter direction
            if (scatter_direction.near_zero())
                scatter_direction = rec.normal;

            scattered = ray(rec.p, scatter_direction);
            attenuation = albedo;
            return true;
        }

    public:
        color albedo;
};
```

### Mirrored light reflection

- Smooth metals $\to$ **ray** does **not randomly scatter**.
  - Vector math:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129010128630.png)

- reflected ray direction in **red** is $v+ 2b$ 
- $n$ is a **unit vector** but $v$ may **not be**
- Length of $b$ should be $v \cdot n$.
- $v$ points inwards thus require a **minus sign**.

```c++
vec3 reflect(const vec3& v, const vec3& n) {
    return v - 2*dot(v,n)*n;
}
```

- Metal material just reflects **rays** using **that formulae**.

```c++
class metal : public material {
    public:
        metal(const color& a) : albedo(a) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            vec3 reflected = reflect(unit_vector(r_in.direction()), rec.normal);
            scattered = ray(rec.p, reflected);
            attenuation = albedo;
            return (dot(scattered.direction(), rec.normal) > 0);
        }

    public:
        color albedo;
};
```

- Must **modify** the `ray_color()` function to use this.

```c++
color ray_color(const ray& r, const hittable& world, int depth) {
    hit_record rec;

    // If we've exceeded the ray bounce limit, no more light is gathered.
    if (depth <= 0)
        return color(0,0,0);

    if (world.hit(r, 0.001, infinity, rec)) {
        ray scattered;
        color attenuation;
        if (rec.mat_ptr->scatter(r, rec, attenuation, scattered))
            return attenuation * ray_color(scattered, world, depth-1);
        return color(0,0,0);
    }

    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

### Scene with metal spheres

```c++
...

#include "material.h"

...

int main() {

    // Image

    const auto aspect_ratio = 16.0 / 9.0;
    const int image_width = 400;
    const int image_height = static_cast<int>(image_width / aspect_ratio);
    const int samples_per_pixel = 100;
    const int max_depth = 50;

    // World

    hittable_list world;

    auto material_ground = make_shared<lambertian>(color(0.8, 0.8, 0.0));
    auto material_center = make_shared<lambertian>(color(0.7, 0.3, 0.3));
    auto material_left   = make_shared<metal>(color(0.8, 0.8, 0.8));
    auto material_right  = make_shared<metal>(color(0.8, 0.6, 0.2));

    world.add(make_shared<sphere>(point3( 0.0, -100.5, -1.0), 100.0, material_ground));
    world.add(make_shared<sphere>(point3( 0.0,    0.0, -1.0),   0.5, material_center));
    world.add(make_shared<sphere>(point3(-1.0,    0.0, -1.0),   0.5, material_left));
    world.add(make_shared<sphere>(point3( 1.0,    0.0, -1.0),   0.5, material_right));

    // Camera

    camera cam;

    // Render

    std::cout << "P3\n" << image_width << " " << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            color pixel_color(0, 0, 0);
            for (int s = 0; s < samples_per_pixel; ++s) {
                auto u = (i + random_double()) / (image_width-1);
                auto v = (j + random_double()) / (image_height-1);
                ray r = cam.get_ray(u, v);
                pixel_color += ray_color(r, world, max_depth);
            }
            write_color(std::cout, pixel_color, samples_per_pixel);
        }
    }

    std::cerr << "\nDone.\n";
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129043845352.png)

### Fuzzy reflection

- Can **Randomize** the *reflected direction* using a **small sphere** and choosing a **new endpoint** for the **ray**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129043926874.png)

- The **bigger** the **sphere** $\to$ the **fuzzier the reflections**
  - This suggests adding some **fuzziness parameter** that is the **radius** of the *sphere* thus **zero** is *no perturbation*.
- Catch is that for **larger sphere** or **grazing rays**
  - May **scatter below the surface**
    - We can just have the **surface absorb these**

```c++
class metal : public material {
    public:
        metal(const color& a, double f) : albedo(a), fuzz(f < 1 ? f : 1) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            vec3 reflected = reflect(unit_vector(r_in.direction()), rec.normal);
            scattered = ray(rec.p, reflected + fuzz*random_in_unit_sphere());
            attenuation = albedo;
            return (dot(scattered.direction(), rec.normal) > 0);
        }

    public:
        color albedo;
        double fuzz;
};
```

We can try that out by adding fuzziness 0.3 and 1.0 to the metals:

```c++

int main() {
    ...
    // World

    auto material_ground = make_shared<lambertian>(color(0.8, 0.8, 0.0));
    auto material_center = make_shared<lambertian>(color(0.7, 0.3, 0.3));
    auto material_left   = make_shared<metal>(color(0.8, 0.8, 0.8), 0.3);
    auto material_right  = make_shared<metal>(color(0.8, 0.6, 0.2), 1.0);
    ...
}
```



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129045030478.png)

## Dielectrics

- Clear materials such as **water / glass** and *diamonds* are **dielectrics**.
  - Light hitting $\to$ splits the **reflected** ray and a **refracted** *transmitted* ray
    - Handle that by **randomly** choosing a **between**  **reflection or** **refraction**
      - Only **generating** one **scattered** *ray per interaction*.

### Refraction

- The hardest part to **debug** is the **refracted** ray.
-  I usually first just have all the **light refract if there is a refraction ray at all**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129045552482.png)

- Is **that right** $\to$ *glass balls* look **odd in the real life**
  - This is wrong, the world **should be flipped** *upside* down and **no weird black stuff**
    - Printed the **ray** through the **middle** of the **image**

### Snells law

https://www.youtube.com/watch?v=y55tzg_jW9I

- The **refraction** is *described* by **snells law**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129050551044.png)

- Where $\theta$ and $\theta^{'}$ are the **angles** from the **normal** 
- $\eta$ and $\eta^{'}$ are the **refractive indices**
  - Air  = 1.0
  - glass = 1.3-1.7
  - diamond = 2.4
- The **geometry** is:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129050731871.png)

- Determine the **direction** of the **refracted ray** 
  - Must solve for $\sin \theta^{'}$: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129051022941.png)

- On the **refracted side** of the *surface* there is **a refracted ray** $R^{'}$ and a **normal** $n'$ 
  - There exists some and **angle** $\theta^{'}$ between them
- Split $R^{'}$ into the **parts** of the **ray** that are **perpendicular** to $n'$ and *parallel*  to $n'$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129051303302.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129051323898.png)

- Must solve for $\cos \theta$, it is well *known* that the **dot product** of the **two vectors** that can be explained in terms of the **cosine angle between them**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129051513766.png)

```c++
vec3 refract(const vec3& uv, const vec3& n, double etai_over_etat) {
    auto cos_theta = fmin(dot(-uv, n), 1.0);
    vec3 r_out_perp =  etai_over_etat * (uv + cos_theta*n);
    vec3 r_out_parallel = -sqrt(fabs(1.0 - r_out_perp.length_squared())) * n;
    return r_out_perp + r_out_parallel;
}
```

- The **dielectric material** that *always refract is*

```c++
class dielectric : public material {
    public:
        dielectric(double index_of_refraction) : ir(index_of_refraction) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            attenuation = color(1.0, 1.0, 1.0);
            double refraction_ratio = rec.front_face ? (1.0/ir) : ir;

            vec3 unit_direction = unit_vector(r_in.direction());
            vec3 refracted = refract(unit_direction, rec.normal, refraction_ratio);

            scattered = ray(rec.p, refracted);
            return true;
        }

    public:
        double ir; // Index of Refraction
};

```

- We will **update** the **scene** to **change** the *left / center spheres* to **glass**

```c++
auto material_ground = make_shared<lambertian>(color(0.8, 0.8, 0.0));
auto material_center = make_shared<dielectric>(1.5);
auto material_left   = make_shared<dielectric>(1.5);
auto material_right  = make_shared<metal>(color(0.8, 0.6, 0.2), 1.0);
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129052457987.png)

### Total internal reflection

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129054544950.png)

- Summary $\to$ certain values mean **refraction cannot occur.**

```c++
if (refraction_ratio * sin_theta > 1.0) {
    // Must Reflect
    ...
} else {
    // Can Refract
    ...
}
```

- All the **light ** is **reflected** here and in practice this is often **inside solid objects**
  - Its called **==total internal reflection==**
    - This is sometimes why the **water air boundary** acts as a **perfect** mirror when you are **submerged**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129060130703.png)

```C++
double cos_theta = fmin(dot(-unit_direction, rec.normal), 1.0);
double sin_theta = sqrt(1.0 - cos_theta*cos_theta);

if (refraction_ratio * sin_theta > 1.0) {
    // Must Reflect
    ...
} else {
    // Can Refract
    ...
}
```

- And the **dielectric material** that *always refracts* when *possible* is:

```c++
class dielectric : public material {
    public:
        dielectric(double index_of_refraction) : ir(index_of_refraction) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            attenuation = color(1.0, 1.0, 1.0);
            double refraction_ratio = rec.front_face ? (1.0/ir) : ir;

            vec3 unit_direction = unit_vector(r_in.direction());
            double cos_theta = fmin(dot(-unit_direction, rec.normal), 1.0);
            double sin_theta = sqrt(1.0 - cos_theta*cos_theta);

            bool cannot_refract = refraction_ratio * sin_theta > 1.0;
            vec3 direction;

            if (cannot_refract)
                direction = reflect(unit_direction, rec.normal);
            else
                direction = refract(unit_direction, rec.normal, refraction_ratio);

            scattered = ray(rec.p, direction);
            return true;
        }

    public:
        double ir; // Index of Refraction
};
```

- Attenuation is **always** $1$ $\to$ *glass surface* **absorbs nothing**
  - If we **try** *that out* these **parameters**

```c++
auto material_ground = make_shared<lambertian>(color(0.8, 0.8, 0.0));
auto material_center = make_shared<lambertian>(color(0.1, 0.2, 0.5));
auto material_left   = make_shared<dielectric>(1.5);
auto material_right  = make_shared<metal>(color(0.8, 0.6, 0.2), 0.0);
```

- Obtain:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129060436668.png)

### Schlick Approximation

- Now real glass **has reflectivity** that **varies** with **angle** — look at a **window at a steep angle and it becomes a mirror**. 
- There is a big ugly equation for that, but almost everybody uses a **cheap and surprisingly accurate polynomial** **approximation** by Christophe Schlick. This yields our full glass material:

```c++
class dielectric : public material {
    public:
        dielectric(double index_of_refraction) : ir(index_of_refraction) {}

        virtual bool scatter(
            const ray& r_in, const hit_record& rec, color& attenuation, ray& scattered
        ) const override {
            attenuation = color(1.0, 1.0, 1.0);
            double refraction_ratio = rec.front_face ? (1.0/ir) : ir;

            vec3 unit_direction = unit_vector(r_in.direction());
            double cos_theta = fmin(dot(-unit_direction, rec.normal), 1.0);
            double sin_theta = sqrt(1.0 - cos_theta*cos_theta);

            bool cannot_refract = refraction_ratio * sin_theta > 1.0;
            vec3 direction;
            if (cannot_refract || reflectance(cos_theta, refraction_ratio) > random_double())
                direction = reflect(unit_direction, rec.normal);
            else
                direction = refract(unit_direction, rec.normal, refraction_ratio);

            scattered = ray(rec.p, direction);
            return true;
        }

    public:
        double ir; // Index of Refraction

    private:
        static double reflectance(double cosine, double ref_idx) {
            // Use Schlick's approximation for reflectance.
            auto r0 = (1-ref_idx) / (1+ref_idx);
            r0 = r0*r0;
            return r0 + (1-r0)*pow((1 - cosine),5);
        }
};
```

#### Modelling a hollow glass sphere

- An interesting and easy trick with **dielectric spheres** is to note that if you use a **negative radius**, the **geometry** is **unaffected**, but the **surface normal points** inward. This can be used as a bubble to make a hollow glass sphere:

```c++
world.add(make_shared<sphere>(point3( 0.0, -100.5, -1.0), 100.0, material_ground));
world.add(make_shared<sphere>(point3( 0.0,    0.0, -1.0),   0.5, material_center));
world.add(make_shared<sphere>(point3(-1.0,    0.0, -1.0),   0.5, material_left));
world.add(make_shared<sphere>(point3(-1.0,    0.0, -1.0),  -0.4, material_left));
world.add(make_shared<sphere>(point3( 1.0,    0.0, -1.0),   0.5, material_right));
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129061130433.png)

## Positionable Camera

- First, let’s allow an **adjustable field of view** (*fov*). 
- This is the **angle** you **see through the portal**. Since our **image is not square**, the fov is different horizontally and vertically. 
- I always use vertical fov. I also usually specify it in degrees and change to radians inside a constructor — a matter of personal taste.

### Camera viewing geometry

- First keep the **rays** coming from the **origin** and heading to the $z = 1$ 
  - Could make it the $z = -2$ plane or **whatever**, or long as we **made** $h$ a **ratio** to that **distance**
    - Here is *our*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129061703887.png)

- Thus $h = \tan(\frac{\theta}{2})$ 

```c++
class camera {
    public:
        camera(
            double vfov, // vertical field-of-view in degrees
            double aspect_ratio
        ) {
            auto theta = degrees_to_radians(vfov);
            auto h = tan(theta/2);
            auto viewport_height = 2.0 * h;
            auto viewport_width = aspect_ratio * viewport_height;

            auto focal_length = 1.0;

            origin = point3(0, 0, 0);
            horizontal = vec3(viewport_width, 0.0, 0.0);
            vertical = vec3(0.0, viewport_height, 0.0);
            lower_left_corner = origin - horizontal/2 - vertical/2 - vec3(0, 0, focal_length);
        }

        ray get_ray(double u, double v) const {
            return ray(origin, lower_left_corner + u*horizontal + v*vertical - origin);
        }

    private:
        point3 origin;
        point3 lower_left_corner;
        vec3 horizontal;
        vec3 vertical;
};
```

- When calling with camera `cam(90, aspect_ratio)` and **these spheres**

```c++
int main() {
    ...
    // World

    auto R = cos(pi/4);
    hittable_list world;

    auto material_left  = make_shared<lambertian>(color(0,0,1));
    auto material_right = make_shared<lambertian>(color(1,0,0));

    world.add(make_shared<sphere>(point3(-R, 0, -1), R, material_left));
    world.add(make_shared<sphere>(point3( R, 0, -1), R, material_right));

    // Camera

    camera cam(90.0, aspect_ratio);

    // Render

    std::cout << "P3\n" << image_width << " " << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
    ...
```

- obtains

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220129061946793.png)

### Position / Orientation

- To get an **arbitrary viewpoint**, let’s **first name** the **points** we **care about**.
-  We’ll call the **position** where we **place the camera** ==*lookfrom*== and the point we look at *lookat*. 
- We also need a way to specify the roll, or sideways tilt, of the camera: the rotation around the lookat-lookfrom axis. 
- If you keep `lookfrom` and `lookat` constant, you can still **rotate your head around your nose**. What we need is a way to **specify an “up” vector** for the camera. 
- This **up vector** should **lie** in the **plane orthogonal** to the **view direction**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130104006677.png)

- We can actually use **any up vector we want**, and simply **project it onto this plane** to get an **up vector for the camera**.
-  I use the common convention of **naming a “view up” (*vup*) vector**. A **couple of cross products**, and we now have a **complete orthonormal basis** $(u,v,w)$ to **describe** our camera’s orientation.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130104146504.png)

- Remember that `vup`, `v`, and `w` are all in the **same plane**. 
- Note that, like **before** when our **fixed camera** faced $-Z$, our arbitrary view camera faces $-w$.
- Can use world up $(0,1,0)$ to specify vup. This is **convenient** and will **naturally** keep your **camera horizontally** level until you **decide** to **experiment** with **crazy camera** angles.

```c++
class camera {
    public:
        camera(
            point3 lookfrom,
            point3 lookat,
            vec3   vup,
            double vfov, // vertical field-of-view in degrees
            double aspect_ratio
        ) {
            auto theta = degrees_to_radians(vfov);
            auto h = tan(theta/2);
            auto viewport_height = 2.0 * h;
            auto viewport_width = aspect_ratio * viewport_height;

            auto w = unit_vector(lookfrom - lookat);
            auto u = unit_vector(cross(vup, w));
            auto v = cross(w, u);

            origin = lookfrom;
            horizontal = viewport_width * u;
            vertical = viewport_height * v;
            lower_left_corner = origin - horizontal/2 - vertical/2 - w;
        }

        ray get_ray(double s, double t) const {
            return ray(origin, lower_left_corner + s*horizontal + t*vertical - origin);
        }

    private:
        point3 origin;
        point3 lower_left_corner;
        vec3 horizontal;
        vec3 vertical;
};
```

- Change back to the prior scene, and use the **new viewpoint**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130110700212.png)

- Change the **field of view**:

```c++
camera cam(point3(-2,2,1), point3(0,0,-1), vec3(0,1,0), 20, aspect_ratio);
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130110744320.png)

## Defocus Blur

https://www.youtube.com/watch?v=oolZEgQict0

- **Defocus blur**. Note, all photographers will call it **“depth of field”** so be aware of only using **“defocus blur”** among friends.
- The reason we **defocus blur** in real cameras is because **they need a big hole** (rather than just a pinhole) to **gather light**. 
- This would **defocus everything**, but if we **stick a lens** in the **hole**, there will be a certain distance where everything is in focus. 
- You can think of a lens this way: all **light** **rays** coming *from* a **specific point at the focus distance** — and **that hit the lens** — will be **bent back *to* a single point** on the **image sensor**.
- We call the **distance between the projection point** and the **plane** where everything is **in perfect focus the *focus distance*.** Be aware that the **focus distance** is **not** the same as the **focal length** — the *focal length* is the **distance between the projection** **point** and the **image plane.**
- In a physical camera, the **focus distance** is controlled by the **distance** **between** the **lens** and **the film/sensor**. That is why you see the **lens move relative** to the **camera** when you **change what is in focus** (that may **happen** in your **phone camera too,** but the **sensor moves**). 
- The **“aperture”** is a **hole to control** how **big** the **lens** is effectively.
-  For a **real camera**, if you **need more light** you **make the aperture bigger**, and will **get more defocus blur.** 
- For our **virtual camera**, we can **have a perfect sensor** and **never need more light**, so we only **have an aperture** when we want **defocus blur**.

### Thin lens Approximation

- A real camera has a **complicated compound lens**.

-  For our code we could **simulate the order**: 

  1. sensor
  2. lens  
  3. aperture. 

- Then we could figure out **where to send the rays**, and **flip** the **image** after it's **computed**

  >  (the image is projected upside down on the film). Graphics people, however, usually use a thin lens approximation:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130111723815.png)

- Do **not need to simulate** any of the **inside of the camera**
  - Purpose of rendering an **image outside** the *camera* $\to$ this would be **unnecessary**  *complexity*
    - Usually start rays from the **lens** then send **them towards** the *focus plane* (`focus_dist` away from the lens)
      - Where **everything** on that plane is **in perfect focus**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130111907588.png)

### Generating sample rays

- Normally, all scene **rays** **originate** from the `lookfrom` point.
-  In order to **accomplish defocus blur**, generate **random scene rays** originating from **inside a disk centered** at the `lookfrom` point.
-  The **larger the radius**, the **greater the defocus blur**. You can think of our **original camera** as having a **defocus** **disk** of radius **zero** (no blur at all), so **all rays originated at the disk center** (`lookfrom`).

```c++
/* generate random point inside unit disk */
vec3 random_in_unit_disk() {
    while (true) {
        auto p = vec3(random_double(-1,1), random_double(-1,1), 0);
        if (p.length_squared() >= 1) continue;
        return p;
    }
}
```

```c++
/* camera with adjustable depth of field */
class camera {
    public:
        camera(
            point3 lookfrom,
            point3 lookat,
            vec3   vup,
            double vfov, // vertical field-of-view in degrees
            double aspect_ratio,
            double aperture,
            double focus_dist
        ) {
            auto theta = degrees_to_radians(vfov);
            auto h = tan(theta/2);
            auto viewport_height = 2.0 * h;
            auto viewport_width = aspect_ratio * viewport_height;

            w = unit_vector(lookfrom - lookat);
            u = unit_vector(cross(vup, w));
            v = cross(w, u);

            origin = lookfrom;
            horizontal = focus_dist * viewport_width * u;
            vertical = focus_dist * viewport_height * v;
            lower_left_corner = origin - horizontal/2 - vertical/2 - focus_dist*w;

            lens_radius = aperture / 2;
        }


        ray get_ray(double s, double t) const {
            vec3 rd = lens_radius * random_in_unit_disk();
            vec3 offset = u * rd.x() + v * rd.y();

            return ray(
                origin + offset,
                lower_left_corner + s*horizontal + t*vertical - origin - offset
            );
        }

    private:
        point3 origin;
        point3 lower_left_corner;
        vec3 horizontal;
        vec3 vertical;
        vec3 u, v, w;
        double lens_radius;
};
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130112709918.png)

## Where next

```c++
ittable_list random_scene() {
    hittable_list world;

    auto ground_material = make_shared<lambertian>(color(0.5, 0.5, 0.5));
    world.add(make_shared<sphere>(point3(0,-1000,0), 1000, ground_material));

    for (int a = -11; a < 11; a++) {
        for (int b = -11; b < 11; b++) {
            auto choose_mat = random_double();
            point3 center(a + 0.9*random_double(), 0.2, b + 0.9*random_double());

            if ((center - point3(4, 0.2, 0)).length() > 0.9) {
                shared_ptr<material> sphere_material;

                if (choose_mat < 0.8) {
                    // diffuse
                    auto albedo = color::random() * color::random();
                    sphere_material = make_shared<lambertian>(albedo);
                    world.add(make_shared<sphere>(center, 0.2, sphere_material));
                } else if (choose_mat < 0.95) {
                    // metal
                    auto albedo = color::random(0.5, 1);
                    auto fuzz = random_double(0, 0.5);
                    sphere_material = make_shared<metal>(albedo, fuzz);
                    world.add(make_shared<sphere>(center, 0.2, sphere_material));
                } else {
                    // glass
                    sphere_material = make_shared<dielectric>(1.5);
                    world.add(make_shared<sphere>(center, 0.2, sphere_material));
                }
            }
        }
    }

    auto material1 = make_shared<dielectric>(1.5);
    world.add(make_shared<sphere>(point3(0, 1, 0), 1.0, material1));

    auto material2 = make_shared<lambertian>(color(0.4, 0.2, 0.1));
    world.add(make_shared<sphere>(point3(-4, 1, 0), 1.0, material2));

    auto material3 = make_shared<metal>(color(0.7, 0.6, 0.5), 0.0);
    world.add(make_shared<sphere>(point3(4, 1, 0), 1.0, material3));

    return world;
}

int main() {

    // Image

    const auto aspect_ratio = 3.0 / 2.0;
    const int image_width = 1200;
    const int image_height = static_cast<int>(image_width / aspect_ratio);
    const int samples_per_pixel = 500;
    const int max_depth = 50;

    // World

    auto world = random_scene();

    // Camera

    point3 lookfrom(13,2,3);
    point3 lookat(0,0,0);
    vec3 vup(0,1,0);
    auto dist_to_focus = 10.0;
    auto aperture = 0.1;

    camera cam(lookfrom, lookat, vup, 20, aspect_ratio, aperture, dist_to_focus);

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        ...
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130112729319.png)

- An interesting thing you might note is the **glass balls** don’t **really have shadows** which makes them look like t**hey are floating**. This is not a bug — you don’t see glass balls much in real life, where they also look a bit strange, and indeed seem to float on cloudy days. A point on the big sphere under a glass ball still has lots of light hitting it because the sky is re-ordered rather than blocked.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220130115834415.png)
