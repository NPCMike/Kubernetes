# Stage 1：認識你的 Cluster

## 任務背景

你剛加入 SRE 團隊，主管只給你一個 Kubernetes cluster，沒有文件。

## 任務目標

查出這個 cluster 有幾台 Node，誰是 control plane，誰是 worker。

## 開始調查

```bash
kubectl get nodes
kubectl get nodes -o wide
```

## 思考一下

哪一台 Node 負責管理 cluster？哪一台 Node 比較像是負責執行 workload 的工作機器？

