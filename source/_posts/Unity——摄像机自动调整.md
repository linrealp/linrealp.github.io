---
title: Unity——摄像机自动调整
date: 2020-04-21
categories:
- 游戏开发
tags:
- Unity
- C#
---

本次介绍一个摄像机跟随的方式，摄像机会自动判断视线是否受到阻碍，自动调节到能观察到目标物体的位置。

<!--more-->

# 实际效果

![效果展示](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/%5BUnity%5D%E6%91%84%E5%83%8F%E6%9C%BA%E8%87%AA%E5%8A%A8%E8%B0%83%E8%8A%82%E8%A7%86%E8%A7%92%E8%B7%9F%E9%9A%8F/%E6%95%88%E6%9E%9C%E5%B1%95%E7%A4%BA.gif)



# 原理

增加多个摄像机观察点，遍历每个观察点，如果观察点能够无障碍的观察到Player，则将该观察点设置为摄像机的位置。

观察点的确定方式(插值)：

```c#
//摄像机原始位置
Vector3 startViewPostion = player.transform.position-offset;
//最后的观察点  Player正上方
Vector3 endViewPostion = player.transform.position + Vector3.up*distance;

//插值确定各个观察点位置
viewPostions[i] = Vector3.Slerp(startViewPostion, endViewPosition, [0~1]);
```



![原理图](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/%5BUnity%5D%E6%91%84%E5%83%8F%E6%9C%BA%E8%87%AA%E5%8A%A8%E8%B0%83%E8%8A%82%E8%A7%86%E8%A7%92%E8%B7%9F%E9%9A%8F/%E5%8E%9F%E7%90%86%E5%B1%95%E7%A4%BA.png)



# 代码

该代码只表示了两个摄像机观察点。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraFollow : MonoBehaviour
{
    //摄像机的移动速度
    public float cameraSpeed;
    //摄像机的旋转速度
    public float cameraRotate;

    private GameObject player;
    //摄像机与Player的偏移量
    private Vector3 offset;
    //摄像机与Player的距离
    private float distance;

    private void Awake()
    {
        player = GameObject.FindWithTag("Player");
    }

    private void Start()
    {
        //摄像机与Player的偏移量
        offset = player.transform.position - transform.position;
        //摄像机与Player的距离
        distance = Vector3.Distance(transform.position, player.transform.position);
    }

    private void Update()
    {
        //两个观察点，一个为摄像机正常位置，一个为Player正上方
        Vector3 startViewPostion = player.transform.position-offset;
        Vector3 endViewPostion = player.transform.position + Vector3.up * distance;

        //目标观察点位置
        Vector3 viewPostion;
        if (CheckView(startViewPostion))
        {
            viewPostion = startViewPostion;
        }
        else
        {
            viewPostion = endViewPostion;
        }

        //摄像机调整位置
        transform.position = Vector3.Slerp(transform.position, viewPostion, Time.deltaTime*cameraSpeed);

        //摄像机调整旋转角度
        SmoothCameraRotate();

    }

    /// <summary>
    /// 观察点是否满足没有碰撞体阻碍，可以直接观察到人物
    /// </summary>
    /// <param name="checkPointPos"></param>
    /// <returns></returns>
    private bool CheckView(Vector3 checkPointPos)
    {
        //得到观察点到Player的向量
        Vector3 dir = player.transform.position - checkPointPos;

        if (Physics.Raycast(checkPointPos, dir, out RaycastHit hit))
        {
            if (hit.collider.CompareTag("Player") || hit.collider.CompareTag("Tree"))
            {
                return true;
            }
        }
        return false;
    }


    /// <summary>
    /// 摄像机光滑调整角度
    /// </summary>
    private void SmoothCameraRotate()
    {
        //摄像机到Player的方向向量
        Vector3 dir = player.transform.position - transform.position;
        //要旋转的角度
        Quaternion quaternion = Quaternion.LookRotation(dir);
        transform.rotation = Quaternion.Lerp(transform.rotation, quaternion, Time.deltaTime*cameraRotate);  /*其实建议第三个参数设置成1,也就是直接同步位置（无插值），感觉效果更好*/
        //将摄像机x,y轴锁死
        transform.eulerAngles = new Vector3(transform.eulerAngles.x, 0, 0);
    }
}

```

