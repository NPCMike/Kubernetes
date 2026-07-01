# Stage 2：Node 基本維運

## 任務背景

worker node 準備維護，不能再讓新的 Pod 被派到這台機器。

## 任務目標

練習讓 Node 暫停排程，再恢復排程。

## 開始操作

```bash
kubectl get nodes
kubectl cordon <node-name>
kubectl get nodes
kubectl uncordon <node-name>
kubectl get nodes
```

## 思考一下

`SchedulingDisabled` 代表 Node 壞掉了嗎？還是代表 Kubernetes 暫時不要把新的 Pod 放上去？

