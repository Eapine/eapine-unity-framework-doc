# 显示对象池 (GameView Pool)

提供对象缓存池的功能，避免频繁地创建和销毁各种游戏对象，提高游戏性能。除了框架自身使用了对象池，用户还可以很方便地创建和管理自己的对象池。

## 使用

### 创建对象池
```csharp
//继承自 ObjectBase
class ObjClass : EapineFramework.Core.ObjectPool.ObjectBase { }

//创建允许单次获取的对象池
GameManager.ObjectPool.CreateSingleSpawnObjectPool<ObjClass>(name, autoReleaseInterval, capacity, expireTime, priority);

//创建允许多次获取的对象池
GameManager.ObjectPool.CreateMultiSpawnObjectPool<ObjClass>(name, autoReleaseInterval, capacity, expireTime, priority);
```

### 获取对象池
```csharp
IObjectPool<ObjClass> objectPool = GameManager.ObjectPool.GetObjectPool<ObjClass>(name);

//如果条件允许，在创建时缓存即可。
```

### 从对象池获取对象
```csharp


IObjectPool<ObjClass> objectPool = GameManager.ObjectPool.GetObjectPool<ObjClass>(name);
    
UIFormInstanceObject uiFormInstanceObject = m_InstancePool.Spawn(uiFormAssetName);
m_InstancePool.Unspawn(uiForm.Handle);
```