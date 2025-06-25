# 引用对象池 (Reference Pool)

提供引用类型对象缓存池的功能，仅限可通过new()直接实例化的类，避免频繁地创建和销毁各种引用类型的对象，提高游戏性能。除了框架自身使用了对象池，用户还可以很方便地创建和管理自己的引用对象池。

引用池分为两种，Safe和Unsafe。区别在于Safe的对象需要继承IReference接口，在入池时会在内部调用IReference的Clear函数进行清理，相对安全。而Unsafe则针对那些项目中已经存在的类，不打算增加子类去继承IReference接口，那么需要用户自己保证入池的对象被手动清理。

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

### Safe Pool
```csharp
//继承自 IReference 的类。
public class MyClass : IReference
{
    public string name;
    public int value;
    public GameObject gameObject;
    public Vector3 position;
    
    // 继承 IReference 后，需要实现 Clear()。
    public void Clear()
    {
        name = null;
        value = 0;
        gameObject = null;
        position = Vector3.zero;
    }

    // 根据需要增加一个Create 函数，复用初始化赋值代码。
    public static MyClass Create(string name, int value, GameObject gameObject, Vector3 position)
    {
        MyClass instance = ReferencePool.Acquire<MyClass>();
        instance.name = name;
        instance.value = value;
        instance.gameObject = gameObject;
        instance.position = position;
        return instance;
    }
}

//通过 ReferencePool.Acquire<T>() 获取实例。
MyClass instance = ReferencePool.Acquire<MyClass>();
instance.name = "eapine";
instance.value = 100;
instance.gameObject = gameObject;
instance.position = Vector3.one;

//或利用封装好的Create 函数创建，减少复用时的代码量。
MyClass instance = MyClass.Create("eapine", 100, gameObject, Vector3.one);

//通过 Acquire(Type type) 也能获取实例，但性能不如 Acquire<T>()。
instance = ReferencePool.Acquire(typeof(MyClass));
//等同于
instance = ReferencePool.Safe.Acquire(typeof(MyClass));

//通过 ReferencePool.Release() 释放实例。
ReferencePool.Release(instance);
//等同于
ReferencePool.Safe.Release(instance);
```

### Unsafe Pool
```csharp
public class MyClass
{
    public string name;
    public int value;
    public GameObject gameObject;
    public Vector3 position;

    // 根据需要增加一个Create 函数，复用初始化赋值代码。
    public static MyClass Create(string name, int value, GameObject gameObject, Vector3 position)
    {
        MyClass instance = ReferencePool.Unsafe.Acquire<MyClass>();
        instance.name = name;
        instance.value = value;
        instance.gameObject = gameObject;
        instance.position = position;
        return instance;
    }

    // 根据需要增加一个Release 函数，复用释放代码。
    public static void Release(MyClass instance)
    {
        name = null;
        value = 0;
        gameObject = null;
        position = Vector3.zero;
        ReferencePool.Unsafe.Release(instance);
    }
}

//通过 ReferencePool.Unsafe.Acquire<T>() 获取实例。
MyClass instance = ReferencePool.Unsafe.Acquire<MyClass>();
instance.name = "eapine";
instance.value = 100;
instance.gameObject = gameObject;
instance.position = Vector3.one;

//或利用封装好的Create 函数创建，减少复用时的代码量。
MyClass instance = MyClass.Create("eapine", 100, gameObject, Vector3.one);

//先手动清理，然后通过 ReferencePool.Unsafe.Release() 把实例放回对象池。
instance.name = null;
instance.value = 0;
instance.gameObject = null;
instance.position = Vector3.zero;
ReferencePool.Unsafe.Release(instance);
//或利用封装好的Release 函数，在内部实现清理逻辑，减少复用时的代码量。
MyClass.Release(instance);
```

### 其他
```csharp
//释放池子内所有实例，一般在切场景时调用。
ReferencePool.ClearAll();

//相当于Safe.ClearAll + Unsafe.ClearAll
ReferencePool.Safe.ClearAll();
ReferencePool.Unsafe.ClearAll();
```