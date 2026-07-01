# Stage 4：Deployment 修復與擴充

## 任務背景

公司服務不能只靠手動建立 Pod。你要讓 Kubernetes 自動維持服務副本數。

## 任務目標

建立 Deployment、調整副本數，並觀察 Pod 被刪除後如何自動補回來。

## 開始操作

```bash
kubectl create deployment web --image=nginx --replicas=3
kubectl get deployments
kubectl get pods -o wide
kubectl delete pod <pod-name>
kubectl get pods -o wide
```

## 思考一下

你刪掉的是 Pod，為什麼 Kubernetes 又補了一個新的 Pod 回來？

