# VEX polylines

Iterate over each point and add a polyline between points that are less than a certain distance apart. **The iteration is done in reverse order reading in the geo stream in input 1 in order to build the polylines on geo stream 0**.
After each iteration the current point is deleted to avoid double connections.
```C
float md = chf("max_dist");
int pcount = npoints(1) - 1;

for (int i = pcount; i >= 0; i--)
{
    vector pos = point(1, "P", i);
    float conlen = point(1, "conlen", i);
    
    int npts[] = nearpoints(1, pos, md * conlen);
    
    foreach(int np; npts)
    {
        if (i != np)
        {
            vector nppos = point(1, "P", np);
            int npa = addpoint(0, nppos);
            int npb = addpoint(0, pos);
            int line = addprim(0, "polyline", npa, npb);
            
            float dist = 1.0f - (distance(nppos, pos) / md);
            setpointattrib(0, "opa", npa, dist);
            setpointattrib(0, "opa", npb, dist);
        }
    }
    removepoint(1, i);
}
```

![plexus-high](https://user-images.githubusercontent.com/740173/153961107-e925dc29-2466-4197-acd1-15eb0a6a2181.gif)
