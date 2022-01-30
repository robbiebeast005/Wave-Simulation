# Wave Simulation

To simulate a wave, a custom and adaptable plane will be needed. A plane can be drawn trough script in the unity game engine. A plane exist out of squares, who are divided into triangles. This is shown in the image below.

<img src="https://www.patrykgalach.com/wp-content/uploads/2019/07/Zrzut-ekranu-2019-07-29-o-11.40.16-768x667.png" width="600" height="500">

In order to create a custom plane, there has to be a gameobject with a mesh filter and a mesh renderer. If both components are added to a gameobject, a script can be created as well.

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


# Civilisation-Simulation

This is a if statement
```ruby
List<int> numberList = new List<int>();
int total;

foreach(int number in numberList)
{
    total += numer;
}
```
