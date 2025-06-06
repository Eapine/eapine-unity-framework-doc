# 数据表 (Data Table)

可以将游戏数据以表格（如 Microsoft Excel）的形式进行配置后，使用此模块使用这些数据表。数据表的格式是可以自定义的。

## 使用

### 创建并加载数据表
```csharp
GameManager.DataTable.CreateAndReadDataTable<T>(Path);
```

### 创建并加载数据表
```csharp
T t = GameManager.DataTable.GetDataTable<T>().GetDatas();
```