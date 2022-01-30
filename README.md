# Wave Simulation

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

 So in the image above, point 0, 1 and 5 are creating a triagle. This will be put into the triagles arrey in the form: 0, 1, 5. This means that the amount of triagles in a plane are ```jo``` jo
