# 实体 (Entity)

我们将游戏场景中，动态创建的一切物体定义为实体。此模块提供管理实体和实体组的功能，如显示隐藏实体、挂接实体（如挂接武器、坐骑，或者抓起另一个实体）等。实体使用结束后可以不立刻销毁，从而等待下一次重新使用。

## 常规用法

### 创建Entity Prefab


### Show Entity

```csharp
string entityPath;  // Entity Prefab 的路径，Assets/xxxxx.prefab
string entityGroup; // Entity 组
object userData;    // 传入参数, 可不传

// 1.直接Show
GameManager.Entity.ShowEntity<T>(entityPath, entityGroup, userData);

// 2.Show时保存SerialId，ID会自增，保证唯一性
int serialId = GameManager.Entity.ShowEntity<T>(entityPath, entityGroup, userData);
```

### Hide Entity

```csharp
// 1.通过某种途径获取 Entity，然后Hide
Entity entity;
GameManager.Entity.HideEntity(entity);

// 2.通过Show时记录的SerialId来关闭
GameManager.Entity.HideEntity(serialId);
```


AttachEntity

DetachEntity