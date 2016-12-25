# Kinematic Actors之间的碰撞检测

## PxSceneDesc
eENABLE_KINEMATIC_PAIRS

```cpp
PxSceneDesc sceneDesc(gPhysx->getTolerancesScale());
sceneDesc.flags |= PxSceneFlag::eENABLE_KINEMATIC_PAIRS;
gScene = gPhysics->createScene(sceneDesc);
```

## PxRigidDynamic
eKINEMATIC

```cpp
body->setRigidBodyFlag(PxRigidBodyFlag::eKINEMATIC, true);
```

## PxPairFlag
PhysX生成碰撞信息有两种模式:
1. 离散碰撞检测
  ```cpp
  PxPairFlag::eDETECT_DISCRETE_CONTACT
  ```

2. 连续碰撞检测
  ```cpp
  PxPairFlag::eDETECT_CCD_CONTACT
  ```
这两个Flag决定了仿真的discrete或者CCD阶段是否进行碰撞生成。注意CCD是不支持kinematics的。当这两个flag被设置后，PhysX就会生成碰撞信息，接下来要决定如何处理这些碰撞信息。

碰撞信息有两种处理方式：被solved或者给出contact event notifications。

如果两个actor都是kinematic的话，是不支持被solved的。所以可以通过设置如下flag，来接收events。

eNOTIFY_CONTACT_POINTS设置后，根据需要设置eNOTIFY_TOUCH_FOUND, eNOTIFY_TOUCH_LOST, eNOTIFY_TOUCH_PERSIST这三个Flag。


```cpp
PxFilterFlags contactReportFilterShader(PxFilterObjectAttributes attributes0, PxFilterData filterData0,
                                        PxFilterObjectAttributes attributes1, PxFilterData filterData1,
                                        PxPairFlags& pairFlags, const void* constantBlock, PxU32 constantBlockSize)
{
    PX_UNUSED(attributes0);
    PX_UNUSED(attributes1);
    PX_UNUSED(filterData0);
    PX_UNUSED(filterData1);
    PX_UNUSED(constantBlockSize);
    PX_UNUSED(constantBlock);

    pairFlags |= PxPairFlag::eDETECT_DISCRETE_CONTACT;
    pairFlags |= PxPairFlag::eNOTIFY_CONTACT_POINTS;
    pairFlags |= PxPairFlag::eNOTIFY_TOUCH_FOUND;
    return PxFilterFlag::eDEFAULT;
}
```

## PxRigidBynamic
setKinematicTarget，不能用setGlobalPose
```cpp
gBodys[0]->setKinematicTarget(lastTransform.transform(stepTransform));
```
