# Stage 1：認識你的 Cluster

## 這一關的情境

你拿到地圖後，前輩說：

> 好，現在點名。先不要修服務，先看這個 Kubernetes 現場裡到底有幾台機器。

這一關你要學會看 Node。Node 就像直播後台裡的一台台工作機器，有些負責管理，有些負責跑服務。

## 你先知道這個就好

Cluster 是整個 Kubernetes 現場。

Node 是 cluster 裡的一台機器。你可以先把 Node 想成一張工作桌，有些桌子負責指揮，有些桌子負責實際處理服務。

Control plane node 比較像指揮中心，負責管理 cluster。

Worker node 比較像工作站，負責跑實際服務。

`STATUS` 是第一個健康訊號。看到 `Ready` 代表 Kubernetes 目前認為這台 Node 可以工作，但這只是入口，不是完整診斷。

## 看圖理解

```text
Cluster：整個直播後台
  |
  +-- controlplane：比較像指揮中心
  |
  +-- node01：比較像工作機器
```

你現在要做的事不是修壞掉的服務，而是先問：

> 這個後台裡有幾台機器？誰像指揮中心？誰像工作機器？

## 跟著做

這個指令要查 cluster 裡有哪些 Node：

```bash
kubectl get nodes
```

你可能會看到類似結果：

```text
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   20m   v1.30.0
node01         Ready    <none>          19m   v1.30.0
```

這個指令會顯示更多資訊，例如內部 IP：

```bash
kubectl get nodes -o wide
```

你可能會看到類似結果：

```text
NAME           STATUS   ROLES           VERSION   INTERNAL-IP
controlplane   Ready    control-plane   v1.30.0   192.168.0.10
node01         Ready    <none>          v1.30.0   192.168.0.11
```

## 看懂結果

先看這幾欄：

| 欄位 | 怎麼看 |
| --- | --- |
| `NAME` | Node 的名字 |
| `STATUS` | `Ready` 代表目前可用 |
| `ROLES` | `control-plane` 代表它像指揮中心 |
| `VERSION` | 這台 Node 的 Kubernetes 版本 |
| `INTERNAL-IP` | Cluster 內部使用的 IP |

如果 `ROLES` 是 `<none>`，新手可以先理解成：它不是 control plane，通常更像 worker node。

## 常見誤會

- `<none>` 不是壞掉，只是沒有顯示特殊角色。
- `Ready` 不代表應用程式一定正常，只代表 Node 本身目前看起來可工作。
- 一開始不要急著看所有細節，先會分辨「指揮中心」和「工作機器」就很好。

## 小任務：確認你真的懂

看到下面輸出時，哪一台比較像指揮中心？

```text
NAME           STATUS   ROLES
controlplane   Ready    control-plane
node01         Ready    <none>
```

答案是 `controlplane`。線索是 `ROLES` 欄位裡的 `control-plane`。
