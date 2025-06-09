# 界面 (UI)

提供管理界面和界面组的功能，如显示隐藏界面、激活界面、改变界面层级等。不论是 Unity 内置的 uGUI 还是其它类型的 UI 插件（如 NGUI），只要派生自 UIFormLogic 类并实现自己的界面类即可使用。界面使用结束后可以不立刻销毁，从而等待下一次重新使用。

外部教程
https://zhuanlan.zhihu.com/p/434142767

## 常规用法

### 创建UI Prefab

> 编辑UI内容，增加对应控制类，并继承自UGUIFormLogic，保存prefab。

### 打开UI

```csharp
string path;    // UI Prefab 的路径，Assets/xxxxx.prefab
string uiGroup; // UI 组

// 1.直接打开
GameManager.UI.OpenUIForm(path, uiGroup);
// 2.打开时保存SerialId，ID会自增，保证唯一性
int serialId = GameManager.UI.OpenUIForm(path, uiGroup);
```

### 关闭UI

```csharp
// 1.通过UI Prefab路径获取UI，然后关闭
string path;
IUIForm ui = GameManager.UI.GetUIForm(path);
if (ui != null)
{
    GameManager.UI.CloseUIForm(ui);
}

// 2.通过打开时记录的SerialId来关闭
GameManager.UI.CloseUIForm(serialId);

// 3.在UI对应的逻辑类内部，直接调自身的关闭函数
CloseUI();
```

## 可重载函数

重点关注 OnInit、OnOpen、OnClose、OnUpdate

```csharp
// 界面初始化
void OnInit(object userData)
// 调用UIManager的OpenUIForm初次打开UI时被调用

// 界面回收
void OnRecycle()
// 调用UIManager的CloseUIForm关闭UI时被调用

// 界面打开
void OnOpen(object userData)
// 调用UIManager的OpenUIForm初次打开UI时被调用，在OnInit之后

// 界面关闭
void OnClose(bool isShutdown, object userData)
// 调用UIManager的CloseUIForm关闭UI时被调用，在OnRecycle之前

// 界面遮挡
void OnCover()
// 同一个UIGroup中，有其他界面覆盖到这个界面后，被调用

// 界面遮挡恢复
void OnReveal()
// 同一个UIGroup中，这个界面从被覆盖的状态恢复到处于最上层后，被调用

// 界面暂停
void OnPause()
// 当UIGroup的Pause属性被设置为True时，UIGroup中的所有UIForm的OnPause都被调用，或者当界面被另一个属性PauseCoveredUIForm为True的界面覆盖且当前界面Pause状态为False时，OnPause被调用

// 界面暂停恢复
void OnResume()
// 当界面Pause状态为True时，从被其他属性PauseCoveredUIForm为True的界面取消覆盖，且所属UIGroup的Pause属性也为False的时候，OnResume被调用

// 界面轮询
void OnUpdate(float elapseSeconds, float realElapseSeconds)
// 界面打开后，只要不处于Pause状态，就会被每帧调用

// 界面深度改变
void OnDepthChanged(int uiGroupDepth, int depthInUIGroup)
// 当界面在当前的UIGroup中的深度发生变化时，被调用

// 界面激活
void OnRefocus(object userData)
// 当界面被强制（不遵循栈的规则，从不靠近栈顶的位置，直接回到栈顶）聚焦时，会被调用
```