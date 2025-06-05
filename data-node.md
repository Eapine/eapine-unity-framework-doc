# 数据结点 (Data Node)

## 使用

### 读写数据

#### 引用类型
```csharp
string name = "eapine";
byte[] data = new byte[0];
GameObject gameObject = null;

//------------------------  写入  ------------------------
GameManager.DataNode["name"].SetData<string>(name);
GameManager.DataNode["data"].SetData<byte[]>(data);
GameManager.DataNode["target"].SetData<GameObject>(gameObject);

//当类型明确时，可省略为
GameManager.DataNode["name"].SetData(name);
GameManager.DataNode["data"].SetData(data);
GameManager.DataNode["target"].SetData(gameObject);

//------------------------  读取  ------------------------
name = GameManager.DataNode["name"].GetData<string>();
data = GameManager.DataNode["data"].GetData<byte[]>(data);
gameObject = GameManager.DataNode["target"].GetData<GameObject>();

//写入时可省略类型说明，读取时需要写明，否则取到的是object
name = GameManager.DataNode["name"].GetData() as string;

```
#### 值类型
```csharp
int lv = 100;
float buff = 0.5f;
bool isVip = true;
Vector3 pos = Vector3.zero;

//------------------------  写入  ------------------------
GameManager.DataNode["lv"].SetInt32(lv);
GameManager.DataNode["buff"].SetSingle(buff);
GameManager.DataNode["isVip"].SetBool(isVip);
GameManager.DataNode["pos"].SetVector3(pos);

// 以上写法等同于
GameManager.DataNode["lv"].SetData<VarInt32>(lv);
GameManager.DataNode["buff"].SetData<VarSingle>(buff);
GameManager.DataNode["isVip"].SetData<VarBoolean>(isVip);
GameManager.DataNode["pos"].SetData<VarVector3>(pos);
// 最终值类型数据会通过引用池转成引用类型再存入，引用池有复用对象功能


//------------------------  读取  ------------------------
lv = GameManager.DataNode["lv"].GetInt32();
buff = GameManager.DataNode["buff"].GetSingle();
isVip = GameManager.DataNode["isVip"].GetBool();
pos = GameManager.DataNode["pos"].GetVector3();

// 以上写法等同于
lv = GameManager.DataNode["lv"].GetData<VarInt32>();
buff = GameManager.DataNode["buff"].GetData<VarSingle>();
isVip = GameManager.DataNode["isVip"].GetData<VarBoolean>();
pos = GameManager.DataNode["pos"].GetData<VarVector3>();

// GetInt32() 是 GetData<VarInt32>() 的简化封装，
// VarInt32 继承自 Variable<int>，
// 当需要用到枚举和结构体，以及未封装的值类型时，
// 自行增加VarXXX 类，继承自Variable<T>，仿造VarInt32 的实现即可。
```

### 树结构
```csharp
// 可以设置多层结构，不限长，但使用时应尽量短。
GameManager.DataNode["player"]["name"].SetData("eapine");
GameManager.DataNode["player"]["lv"].SetInt32(100);

// 以上写法等同于。
GameManager.DataNode.Root["player"]["name"].SetData("eapine");
GameManager.DataNode.Root["player"]["lv"].SetInt32(100);
// 为了简短代码做了封装省略了Root，实际上整个DataNode是一个数结构，有一个Root节点。
```

### 树节点
```csharp
DataNode dataNode = GameManager.DataNode.Root["player"];
// 等价于
DataNode dataNode = GameManager.DataNode.Root.GetOrAddNode("player");
// 如果Root下没有"player"，会在内部自动创建，不会返回空。

// 如果不想自动创建，可以使用GetNode，如果节点不存在会返回空。
DataNode dataNode = GameManager.DataNode.Root.GetNode("player");

// 如果想删除节点，通过RemoveNode，会将它的子孙也一起删除。
GameManager.DataNode.Root.RemoveNode("player");
// 如果节点上的数据是Variable类型，会在内部放回引用池。
```