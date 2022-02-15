# VEX subdivision

Assuming each primitive in the geometry has an integer attribute i@subdiv, this wrangle will iterate over the primitives and subdivide them a number of times equal to i@subdiv. This works on ngons, not just triangles or quads.
```C
float dispaceScale = chf("displacescale");

int ppoints[] = primpoints(0, @primnum);
int ppointCount = len(ppoints);

vector centroid = @P;
int iteration = detail(1, "iteration");
centroid += @N * (iteration + i@subdiv) * dispaceScale;
int ci = addpoint(0, centroid);

for (int i = 0; i < ppointCount; i++)
{
    int newprim = addprim(0, "poly", ppoints[i], ppoints[(i + 1) % ppointCount], ci);
    setprimattrib(0, "N", newprim, @N);
    setprimattrib(0, "subdiv", newprim, i@subdiv);
}

removeprim(0, @primnum, 0);
```
![test1](https://user-images.githubusercontent.com/740173/154046188-98420193-83a5-43e7-920f-21b81101694e.jpg)
![test](https://user-images.githubusercontent.com/740173/154046193-8c83b30c-dbd9-4a69-bd0a-b83b2ec8fbeb.jpg)
