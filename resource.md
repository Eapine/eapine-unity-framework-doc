# 资源 (Resource)

为了保证玩家的体验，我们不推荐再使用同步的方式加载资源，由于框架自身使用了一套完整的异步加载资源体系，因此只提供了异步加载资源的接口。不论简单的数据表、本地化字典，还是复杂的实体、场景、界面，我们都将使用异步加载。同时，框架提供了默认的内存管理策略（当然，你也可以定义自己的内存管理策略）。多数情况下，在使用 GameObject 的过程中，你甚至可以不需要自行进行 Instantiate 或者是 Destroy 操作。

## 常规用法

```csharp
// 加载
private LoadAssetCallbacks m_loadAssetCallback;
if (m_loadAssetCallback == null)
{
    m_loadAssetCallback = new LoadAssetCallbacks(LoadAssetSuccessCallback, LoadAssetFailureCallback);
}

string resourcePath;
GameManager.Resource.LoadAsset(resourcePath, m_loadAssetCallback);

// 卸载
GameManager.Resource.UnloadAsset(asset);
```