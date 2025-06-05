# 对象池 (Object Pool)

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