# Wave Simulation

This simulation has been made in the ```Unity game engine``` and ```c#```

# Approach
To simulate a wave, a custom and adaptable plane will be required. A plane can be drawn trough script in the unity game engine. A plane exist out of squares, that is divided into triangles. This is shown in the image below.

<img src="https://www.patrykgalach.com/wp-content/uploads/2019/07/Zrzut-ekranu-2019-07-29-o-11.40.16-768x667.png" width="600" height="500">

In order to create a custom plane, there has to be a gameobject with a mesh filter and a mesh renderer. If both components are added to a gameobject, a script can be created as well. A mesh renderer can draw a plane if all the point in the above shown image are passed to the component. The variables needed are the vertices and the triangles connecting eachother.


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

 So in the image above, point 0, 1 and 5 are creating a triagle. This will be put into the triagles arrey in the form: 0, 1, 5. The amount of triagles in a plane is:
 
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
