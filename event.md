# 事件系统

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
```

### 取消注册事件
```csharp
GameManager.Event.Unsubscribe(key, OnEvent);
GameManager.Event.Unsubscribe<int>(key, OnEvent_1);
GameManager.Event.Unsubscribe<string, GameObject>(key, OnEvent_2);
```

### 派发事件
```csharp
GameManager.Event.Publish(key);
GameManager.Event.Publish<int>(key, 996);
GameManager.Event.Publish<string, GameObject>(key, "eapine", gameObject);
```