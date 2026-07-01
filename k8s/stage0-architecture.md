# Stage 0：Kubernetes 架構短講

## 任務目標

先建立心智模型：Kubernetes 不是一台機器，而是一個負責管理多台機器與服務狀態的平台。

## 你要先記住的事

Kubernetes 的核心概念是「宣告你想要的狀態，讓系統持續把現況修回去」。

```text
kubectl
  |
  v
API Server
  |
  +-- etcd：記住 cluster 狀態
  +-- Scheduler：決定 Pod 放哪台 Node
  +-- Controller：持續修正狀態
  |
  v
Node
  |
  +-- kubelet：確保 Pod 在 Node 上跑起來
  +-- container runtime：真正啟動 container
  +-- Pod：Kubernetes 最小運行單位
```

## 思考一下

如果 Pod 壞掉後又自動長回來，是誰在維持這個狀態？

