using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class EnemyVision : MonoBehaviour
{
    [SerializeField] private string targetTag = "Player";
    [SerializeField] private int rays = 8;
    [SerializeField] private float distance = 50;
    [SerializeField] private float angle = 50;
    [SerializeField] private Vector3 offset;
    [SerializeField] private Transform target;
    [SerializeField] private NavMeshAgent Nana;
    
    
    void Start()
    {
        target = GameObject.FindGameObjectWithTag(targetTag).transform;
        Nana = GetComponent<NavMeshAgent>();
        
    }

    // Update is called once per frame
    void Update()
    {
        if (Vector3.Distance(transform.position, target.position) < distance)
        {
            if (RayToScan())
            {
                Nana.enabled = true;
            }
            else
            {
                ////
            }
        }
    }

    bool GetRaycast(Vector3 dir)
    {
        bool result = false;
        RaycastHit hit = new RaycastHit();

        Vector3 pos = transform.position + offset;

        if (Physics.Raycast(pos, dir, out hit, distance))
        {
            if (hit.transform == target)
            {
                result = true;
                Debug.DrawLine(pos, hit.point, Color.green);
            }
            else
            {
                Debug.DrawLine(pos, hit.point, Color.blue);
            }
        }
        else
        {
            Debug.DrawRay(pos, dir*distance, Color.red);
        }

        return result;

    }
    bool RayToScan()
    {
        bool result = false;
        bool a = false;
        bool b = false;

        float j = 0;


        for (int i = 0; i < rays; i++)
        {
            var x = Mathf.Sin(j);
            var y = Mathf.Cos(j);
            j += angle * Mathf.Deg2Rad / rays;
            Vector3 dir = transform.TransformDirection(new Vector3(x, 0, y));
            if (GetRaycast(dir)) a = true;

            if (x != 0)
            {
                dir = transform.TransformDirection(new Vector3(-x, 0, y));
                if (GetRaycast(dir)) b = true;
            }
        }

        if (a || b) result = true;
        return result;
    }
    
}


