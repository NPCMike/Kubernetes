# Stage 1：認識你的 Cluster

## 任務簡報

你剛進 SRE 戰情室，主管沒有丟架構文件，只丟了一個 kubeconfig。

現在先別急著修服務。第一步是確認這個 Kubernetes cluster 裡有哪些 Node，以及每台 Node 在現場扮演什麼角色。

## 你現在要懂的事

- Cluster 是一組由 Kubernetes 管理的機器
- Node 是實際承載 Pod 的機器
- Control plane 負責管理 cluster 狀態
- Worker node 主要負責執行 workload
- `STATUS` 是第一個健康訊號，但不是完整診斷

## 開始操作

```bash
kubectl get nodes
kubectl get nodes -o wide
```

## 觀察重點

看 `kubectl get nodes` 時，先盯住這幾欄：

- `NAME`：Node 名稱
- `STATUS`：Node 是否 Ready
- `ROLES`：是否為 control-plane
- `VERSION`：Node 使用的 Kubernetes 版本
- `INTERNAL-IP`：Node 在 cluster 內部使用的 IP

## 卡關提示

如果你看到 `control-plane`，那台 Node 負責 cluster 管理任務。

如果某台 Node 沒有 control-plane 角色，它通常更接近 worker node：負責執行實際 workload。

## 思考一下

你現在看到的是幾台 Node？哪一台比較像 cluster 的指揮中心？哪一台比較像真正跑服務的工作機器？
