Editor Script/Menu Item - Put in script in Editor Folder.
Allows for a single selected object to be dropped to the next nearest model below it. 
Model that is dropped to ground is dropped to the pivot (something to note if your pivots are center of model etc)
```c#
using System.Collections.Generic;
using System.Linq;
using Luna;
using UnityEditor;
using UnityEngine;

public class EditorMenuItems : MonoBehaviour
{
        [MenuItem("GameObject/Set Object On Ground - No Collider #G", false, 10)]
        private static void SetObjectOnGround()
        {
            GameObject selection  = Selection.activeGameObject;
            Transform[] allObjects = UnityEngine.Object.FindObjectsOfType<Transform>() ;
            //Try to find the objects that are closest to the current GO
            IEnumerable<Transform> closeObjects = allObjects.Where(o =>  o.position.y < selection.transform.position.y 
                                                     && o.gameObject != selection.gameObject
                                                     && o.GetComponent<MeshRenderer>() 
                                                     && PointContainedIn2DPlane(selection.transform.position, o.GetComponent<MeshRenderer>().bounds));

            Vector3 hitPoint = Vector3.zero;
            float closestDist = 10000;
            foreach (Transform closeObject in closeObjects)
            {
                RaycastHit hit = new RaycastHit();
                bool bHit = HandleUtilityWrapper.IntersectRayMesh(new Ray(selection.transform.position, Vector3.down), closeObject.GetComponent<MeshFilter>(), out hit);
                if (!bHit)
                {
                    continue;
                }
                if (hit.distance < closestDist)
                {
                    // Debug.Log($"Updating Closest: {closeObject.name} dist:{hit.distance}");
                    hitPoint = hit.point;
                    closestDist = hit.distance;
                }
            }

            Vector3 position = selection.transform.position;
            position = new Vector3(position.x, hitPoint.y, position.z);
            selection.transform.position = position;
        }

        private static bool PointContainedIn2DPlane(Vector3 point, Bounds bounds)
        {
            if ((point.x > bounds.min.x && point.x < bounds.max.x) && (point.z > bounds.min.z && point.z < bounds.max.z))
            {
                return true;
            }

            return false;
        }
}
```

Class that opens access to mesh ray casting (Code adapted from [Gist](https://gist.github.com/MattRix/9205bc62d558fef98045))
```
using System;
using System.Reflection;
using UnityEngine;
using UnityEditor;
using System.Linq;
​
// Allows for access to Unity function that they haven't made public (through reflection)
// Makes it so we can do mesh raycasting (no colliders)
[InitializeOnLoad]
    public static class HandleUtilityWrapper
	{
		private static readonly Type       HandleUtility;
		private static readonly MethodInfo IntersectRayMeshMethodInfo;
​
		static HandleUtilityWrapper()
		{
			var editorTypes = typeof(Editor).Assembly.GetTypes();
			HandleUtility              = editorTypes.FirstOrDefault(t => t.Name == "HandleUtility");
			IntersectRayMeshMethodInfo = HandleUtility.GetMethod("IntersectRayMesh", BindingFlags.Static | BindingFlags.NonPublic);
		}
​
		public static bool IntersectRayMesh(Ray ray, MeshFilter meshFilter, out RaycastHit hit)
		{
			return IntersectRayMesh(ray, meshFilter.sharedMesh, meshFilter.transform.localToWorldMatrix, out hit);
		}
​
		private static object[] _parms = new object[4];
​
		public static bool IntersectRayMesh(Ray ray, Mesh mesh, Matrix4x4 matrix, out RaycastHit hit)
		{
			_parms[0] = ray;
			_parms[1] = mesh;
			_parms[2] = matrix;
			_parms[3] = null;
​
			var result = (bool) IntersectRayMeshMethodInfo.Invoke(null, _parms);
			hit = (RaycastHit) _parms[3];
			return result;
		}
	}
​
```