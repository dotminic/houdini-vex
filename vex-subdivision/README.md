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

![test_b](https://user-images.githubusercontent.com/740173/154046617-a0b5a4b7-652d-4f6e-b53b-dc23f1aec90d.jpg)
![test_a](https://user-images.githubusercontent.com/740173/154046618-ff4a4792-3b69-4d2c-8659-9c04387b983b.jpg)
