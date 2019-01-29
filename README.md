https://cldup.com/YgsLg7gM2L.png
こんなかんじです kubernetes って

# 自身がどのクラスタに接続してるんだなあと確認

```
kubectl config get-contexts
CURRENT   NAME                 CLUSTER                      AUTHINFO             NAMESPACE
          dind                 dind                                              default
*         docker-for-desktop   docker-for-desktop-cluster   docker-for-desktop
```

尚 `cat ~/.kube/config` に情報が記録されてる

# 作業用の namespace 作る

作る

`kubectl create namespace test`

確認
`kubectl get namespace`

作成した namespace を永続的に適用

`kubectl config set-context $(kubectl config current-context) --namespace=test`

# pod 立てる

立てる

`kubectl create -f pod-nginx.yaml`

確認

`kubectl get pod`

詳細確認

`kubectl describe pods nginx-pod`

# nginx 動作確認してみる

alpine の pod 立てて、

`kubectl exec -it alpine-pod /bin/sh`

describe で nginx-pod のIP確認の上

curl {nginx-pod の IP}

# deployment してみる

デプロイ
`kubectl apply -f v1.0/deployment.yml`

デプロイ完了されたなと確認
`kubectl get deployments`


# 確認してみる

kubectl proxy

[ダッシュボード](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/)にアクセス


# deployment を v2 に更新してみる

デプロイ
`kubectl apply -f v2.0/deployment.yml`

履歴を確認
`kubectl rollout history deployment nginx-deployment`

# deployment を ロールバック してみる

ロールバック
`kubectl rollout undo deployment nginx-deployment --to-revision=1`

# service を構築してみる

構築
`kubectl create -f service.yml`

サービス確認
`kubectl get services`

エンドポイント確認
`kubectl get endpoints`

# バランシングされてるか確認

nginx のログを監視する
kubectl log -f {pod 名}

その上で別のpod から curl nginx-service でリクエストしまくってみる。