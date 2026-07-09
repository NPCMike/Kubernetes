# Stage 3：Pod 異常排查

## 這一關的情境

客服頻道開始熱鬧了。

有人說直播間卡，有人說結帳慢，也有人說「是不是 Kubernetes 壞了」。前輩看了你一眼：

> 不要先猜。先從 Pod 狀態開始查。狀態是入口，事件和 logs 才是線索。

這一關你要學會 Pod 排查的基本路線。

## 你先知道這個就好

Pod 是 Kubernetes 裡最小的服務單位。對新手來說，你可以先把 Pod 想成一個服務小盒子。

`STATUS` 像急診分診牌。它會先告訴你目前看起來怎樣，但不會直接告訴你完整病因。

`describe pod` 像看病歷和現場事件，能看到排程、拉 image、重啟、錯誤事件。

`logs` 像打開服務自己的日記，最接近應用程式裡發生的錯誤。

`events` 像整個 cluster 的最近事故紀錄，可以看 Kubernetes 最近發生過什麼事。

## 看圖理解

![Pod 查案圖](./assets/pod-debug-detective.png)

先記住這條查案路線：

```text
先看有哪些 Pod
    |
    v
找到異常 STATUS
    |
    v
describe 看 Kubernetes 事件
    |
    v
logs 看應用程式輸出
    |
    v
events 看 cluster 最近線索
```

## 跟著做

這個指令會看所有 namespace 裡的 Pod：

```bash
kubectl get pods -A
```

你可能會看到類似結果：

```text
NAMESPACE   NAME                         READY   STATUS             RESTARTS
default     web-7c9d4f8c9f-abcde         1/1     Running            0
default     api-64d7bbf8cc-x1y2z         0/1     CrashLoopBackOff   5
default     worker-558dd99f8-pqrst       0/1     ImagePullBackOff   0
```

找到異常 Pod 後，先看 Kubernetes 對它的描述和事件：

```bash
kubectl describe pod <pod-name> -n <namespace>
```

如果 Pod 有啟動過，但程式一直崩潰，查看 logs：

```bash
kubectl logs <pod-name> -n <namespace>
```

如果 Pod 一直重啟，上一個 container 的 logs 有時更有用：

```bash
kubectl logs <pod-name> -n <namespace> --previous
```

查看最近 cluster 事件：

```bash
kubectl get events -A --sort-by=.lastTimestamp
```

## 看懂結果

先用狀態小卡判斷下一步：

| 看到什麼 | 白話意思 | 先查哪裡 |
| --- | --- | --- |
| `ImagePullBackOff` | Kubernetes 拉不到 image，服務盒子還沒成功打開 | `describe pod` 的 Events、image 名稱、tag、registry 權限 |
| `CrashLoopBackOff` | container 有啟動，但程式一直崩潰重開 | `logs` 和 `logs --previous` |
| `Pending` | Pod 還沒被安排到 Node | `describe pod` 裡的 scheduling events |
| `Running` 但服務不正常 | Pod 看起來在跑，但應用程式可能不健康 | `logs`，必要時再看健康檢查設定 |

`kubectl describe pod` 最下面通常有 `Events`。新手可以先養成習慣：看到異常狀態，就去 `Events` 找 Kubernetes 留下的原因。

## 常見誤會

- `Running` 不等於服務一定健康。
- `CrashLoopBackOff` 不是 image 拉不到，而是程式啟動後又崩潰。
- `ImagePullBackOff` 很常是 image 名稱、tag、權限或網路問題。
- 不要只看 `STATUS` 就下結論。`STATUS` 是入口，不是答案。

## 排查決策樹

做完一次基本查案後，再看這張完整小抄會更有感：

![Pod 故障排查決策樹](./assets/pod-debug-decision-tree.png)

## 小任務：確認你真的懂

如果你看到：

```text
STATUS             RESTARTS
CrashLoopBackOff   8
```

你會先看哪裡找最接近應用程式錯誤的線索？

A. `kubectl logs <pod-name> -n <namespace>`  
B. `kubectl cordon <node-name>`  
C. `kubectl get nodes`

建議答案是 A。`CrashLoopBackOff` 代表程式啟動後反覆崩潰，logs 通常最接近程式錯誤原因。
