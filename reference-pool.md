# 引用对象池 (Reference Pool)

提供引用类型对象缓存池的功能，仅限可通过new()直接实例化的类，避免频繁地创建和销毁各种引用类型的对象，提高游戏性能。除了框架自身使用了对象池，用户还可以很方便地创建和管理自己的引用对象池。

## 常规用法

使用引用池的类需要继承 **IReference** 接口，并实现 **Clear()** 方法。

```csharp
/// <summary>
/// 引用接口。
/// </summary>
public interface IReference
{
    /// <summary>
    /// 清理引用。
    /// </summary>
    void Clear();
}
```

## 最佳实践

```csharp
//继承自 IReference 的类
public class MyClass : IReference
{
    public string name;
    public int value;
    public GameObject gameObject;
    public Vector3 position;
            
    public void Clear()
    {
        name = null;
        value = 0;
        gameObject = null;
        position = Vector3.zero;
    }
}

//通过 ReferencePool.Acquire<T>() 获取实例
MyClass instance = ReferencePool.Acquire<MyClass>();
instance.name = "eapine";
instance.value = 100;
instance.gameObject = gameObject;
instance.position = Vector3.one;

//通过 Acquire(Type type) 也能获取实例，但性能不如 Acquire<T>()
instance = ReferencePool.Acquire(typeof(MyClass));

//通过 ReferencePool.Release() 释放实例
ReferencePool.Release(instance);

//释放池子内所有实例，一般在切场景时调用
ReferencePool.ClearAll();
```