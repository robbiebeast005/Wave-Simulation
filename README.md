# Wave Simulation

This simulation has been made with the ```Unity game engine``` and ```c#```

# Approach
* Create adaptable plane 
* Modefy plane
* Use shader to create cool looks


To simulate a wave, a custom and adaptable plane will be required. A plane can be drawn trough script in the unity game engine. A plane exist out of squares, that is divided into triangles. This is shown in the image below.

<img src="https://www.patrykgalach.com/wp-content/uploads/2019/07/Zrzut-ekranu-2019-07-29-o-11.40.16-768x667.png" width="600" height="500">

In order to create a custom plane, there has to be a gameobject with a mesh filter and a mesh renderer. If both components are added to a gameobject, a script can be created as well. A mesh renderer can draw a plane if all the point in the above shown image are passed to the component. The variables needed are the position of the vertices and the points that create a triagle. All this information leads to the belown shown code.

---
```ruby
[RequireComponent(typeof(MeshFilter))]
public class MeshGenerator : MonoBehaviour
{
    Mesh mesh;

    public Vector3[] vertices;
    public int[] triangles;

    void Start()
    {
        mesh = new Mesh();
        GetComponent<MeshFilter>().mesh = mesh;
    }
```
---

We want to have full control over the size of the plane. Therefore, a ```height``` and ```width``` variable will be created. To position the points of the plane in the scene, a double for loop we be used. By looping through all the width values in a loop through all the height values, all points will get a value. This is becasue ```height x width``` is equal to the amount of vertices in a plane. By impelemting the code shown below, a array with all the vertices of the plane will be created.

---
```ruby
vertices = new Vector3[width * height];

for (int y = 0; y < height; y++){
    for(int x = 0; x < width; x++){
        vertices[(y * height) + x] = new Vector3(x,0,y);
    }
}
```
---
A similar aproach will be done for the triagles. Lets begin with determining the length of the array. The amount of squares in the plane is calculated by multiplying ```(width - 1) x (height - 1)```. The amount of triangles per square is 2 and the amount of vertices per triagle is 3. So in onder to create all triangles, an array with the lenght of ```((width - 1) x (height - 1)) x 6``` will be needed. Now all the vertices that create a triagle need to be added to the array. Insted of the image shown above, the plane that will be created will have a value of 0 in the top right insted of bottem left. This gives that if we begin at vertex 0, we can create triagle (0, 5, 4) and (0, 5, 1). It could also be discribed as ```(vertex, vertex + vertex + 1, width)``` and ```(vertex, vertex + width + 1, vertex + 1)```. This notation works for every vertex, so per vertex, two triagles can be created. With this approach we dont need

 
 ## Full Code
 
 ```ruby
 using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(MeshFilter))]
public class MeshGenerator : MonoBehaviour
{
    public int width;
    public int height;
    public float amplitude;
    public float speed;

    Mesh mesh;

    public Vector3[] vertices;
    public int[] triangles;
    float point = 0;

    void Start()
    {
        mesh = new Mesh();
        GetComponent<MeshFilter>().mesh = mesh;

        CreatShape();
        UpdateMesh();
    }

    void CreatShape()
    {
        vertices = new Vector3[width * height];
        for (int y = 0; y < height; y++){
            for(int x = 0; x < width; x++){
                vertices[(y * height) + x] = new Vector3(x,0,y);
            }
        }

        int i = 0;
        triangles = new int[((width - 1) * (height - 1)) * 6];
        for (int y = 0; y < (height - 1); y++){
            for(int x = 0; x < (width - 1); x++){
                triangles[i] = ((y * height) + x);
                triangles[i+1] = ((y * height) + x)+1+width;
                triangles[i+2] = ((y * height) + x)+width;
                
                triangles[i+3] = ((y * height) + x)+1;
                triangles[i+4] = ((y * height) + x)+1+width;
                triangles[i+5] = ((y * height) + x);
                i += 6;
            }
        }
    }

    void UpdateMesh()
    {
        mesh.Clear();
        vertices = new Vector3[0];
        vertices = new Vector3[width * height];

        for (int y = 0; y < height; y++){
            for(int x = 0; x < width; x++){
                if(y%2 == 0){
                    vertices[(y * height) + x] = new Vector3(x,(Mathf.Cos(point+(y * height) + x)) * amplitude,y);
                }
                else{
                    vertices[(y * height) + x] = new Vector3(x,(Mathf.Sin(point+(y * height) + x)) * amplitude,y);
                }
                
            }
        }

        mesh.vertices = vertices;
        mesh.triangles = triangles;

        mesh.RecalculateNormals();
    }

    void Update()
    {
        point += speed * Time.deltaTime;
        UpdateMesh();
    }
}

 ```
