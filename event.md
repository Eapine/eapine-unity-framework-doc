# 事件系统 (Event)

## 使用

### 注册事件
```csharp
//对应的函数最多支持16个参数
GameManager.Event.Subscribe(key, OnEvent);
GameManager.Event.Subscribe<int>(key, OnEvent_1);
GameManager.Event.Subscribe<string, GameObject>(key, OnEvent_2);

private void OnEvent() {}
private void OnEvent_1(int i) {}
private void OnEvent_2(string str, GameObject go) {}

//父子类情况
public class Parent { }
public class Child : Parent { }
private void OnEvent_Class(Parent obj) {}

GameManager.Event.Subscribe<Parent>(key, OnEvent_Class);
```

### 取消注册事件
```csharp
GameManager.Event.Unsubscribe(key, OnEvent);
GameManager.Event.Unsubscribe<int>(key, OnEvent_1);
GameManager.Event.Unsubscribe<string, GameObject>(key, OnEvent_2);

GameManager.Event.Unsubscribe<Parent>(key, OnEvent_Class);
```

### 派发事件
```csharp
GameManager.Event.Publish(key);
GameManager.Event.Publish<int>(key, 996);
GameManager.Event.Publish<string, GameObject>(key, "eapine", gameObject);

//父子类情况
Parent parent = new Parent();
Child child = new Child();
GameManager.Event.Publish<Parent>(key, parent);
GameManager.Event.Publish<Parent>(key, child);//显示定义<Parent>尤为重要
```
---

**Subscribe、Unsubscribe、Publish 对应函数的类型最好明文写清楚，避免类型对不上造成逻辑异常。**

---

### 语法糖
Subscribe 和 Unsubscribe 需要一一对应，在日常开发中，难免会出错，为降低犯错成本，增加以下接口。
```csharp
private EventInfoPool m_EventInfoPool = null;

//注册事件
m_EventInfoPool.Add(GameManager.Event.Subscribe(1, OnEvent_1));
m_EventInfoPool.Add(GameManager.Event.Subscribe(2, OnEvent_2));
m_EventInfoPool.Add(GameManager.Event.Subscribe(3, OnEvent_3));

//取消注册事件
m_EventInfoPool.Clear();
```