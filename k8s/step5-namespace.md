# Stage 5：Namespace 現場分區

## 這一關的情境

直播平台救回來後，前輩又丟給你一個問題：

> 現在只有一個服務還好。等測試環境、正式環境、後台工具、監控工具全部進來，你要怎麼知道哪一區真的在出事？

這一關你要學 Namespace。它不是新開一台機器，而是把同一個 Kubernetes 現場分成幾個清楚的區域。

## 你先知道這個就好

Namespace 可以先想成戰情室裡的分區白板。直播服務寫在一區，測試服務寫在一區，監控工具寫在一區。大家還在同一間戰情室，但不會全部混在同一張紙上。

`default` 是 Kubernetes 預設區域。新手剛開始常常把東西都丟進 `default`，但正式工作時，資源太多就會很難找。

`-n` 是指定 namespace。你可以把它想成「只看這一區」。

`-A` 是 all namespaces。你可以把它想成「全場巡一次」。

## 看圖理解

先看這張圖：三個小區域都在同一個 Kubernetes cluster 裡。Namespace 不是多開幾台機器，而是把同一個現場分成幾個管理區。

![Namespace 現場分區示意](./assets/namespace-zone-board.png)

```text
Cluster：整個 NPC 直播平台後台
  |
  +-- default       預設區，適合練習，不適合什麼都塞
  |
  +-- npc-live      直播服務區
  |
  +-- kube-system   Kubernetes 自己的系統區
```

這一關你要學會問兩個問題：

> 這個資源在哪一區？  
> 我現在查的是一區，還是全場？

## 跟著做

這個指令要看目前有哪些 Namespace：

```bash
kubectl get namespaces
```

也可以用短寫：

```bash
kubectl get ns
```

你可能會看到類似結果：

```text
NAME              STATUS   AGE
default           Active   35m
kube-system       Active   35m
kube-public       Active   35m
```

建立直播平台專用 Namespace：

```bash
kubectl create namespace npc-live
```

把測試服務放到 `npc-live` 這一區：

```bash
kubectl create deployment live-web --image=nginx --replicas=2 -n npc-live
```

只看 `npc-live` 裡的 Deployment 和 Pod：

```bash
kubectl get deployments -n npc-live
kubectl get pods -n npc-live -o wide
```

從全場角度看所有 Pod 分布在哪些 Namespace：

```bash
kubectl get pods -A
```

## 看懂結果

重點看 `NAMESPACE` 欄位：

```text
NAMESPACE   NAME                        READY   STATUS
default     web-7c9d4f8c9f-abcde        1/1     Running
npc-live    live-web-6c8b7c5f9d-x1y2z   1/1     Running
```

同樣是查 Pod，查的範圍會不一樣：

| 指令 | 白話意思 |
| --- | --- |
| `kubectl get pods` | 看目前所在 Namespace 的 Pod |
| `kubectl get pods -n npc-live` | 只看 `npc-live` 這一區 |
| `kubectl get pods -A` | 全場 Namespace 都看 |

如果你明明建立了 Pod，卻查不到，第一個要懷疑的不是 Pod 消失，而是你可能查錯區域。

## 常見誤會

- Namespace 不是一台新的機器，它只是 Kubernetes 裡的邏輯分區。
- `default` 不是錯，但不適合把所有正式資源都塞進去。
- 查資源時沒加 `-n`，不一定看得到其他 Namespace 的東西。
- `kube-system` 裡通常是 Kubernetes 自己的系統元件，新手不要隨便改。

## 小任務：確認你真的懂

你建立了：

```bash
kubectl create deployment live-web --image=nginx -n npc-live
```

但你執行下面指令看不到它：

```bash
kubectl get pods
```

下一步最合理的是什麼？

A. 立刻重開 cluster  
B. 改用 `kubectl get pods -n npc-live` 或 `kubectl get pods -A`  
C. 刪掉 Deployment 重做

建議答案是 B。你很可能只是查錯 Namespace。
