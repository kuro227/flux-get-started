# weaveflux-sample

- minikube で weave flux を動かしてみる
- 以下の公式の手順に従って進める
  https://docs.fluxcd.io/en/latest/tutorials/get-started/

- minikube 起動

```
minikube start

minikube status
# minikube
# type: Control Plane
# host: Running
# kubelet: Running
# apiserver: Running
# kubeconfig: Configured
```

- fluxctl のインストール

```
brew install fluxctl
```

- flux の namespace を作る

```
kubectl create ns flux
```

- k8s Cluster に flux をインストールする

```
export GHUSER="yosuke.k22@gmail.com"
fluxctl install \
--git-user=${GHUSER} \
--git-email=${GHUSER}@users.noreply.github.com \
--git-url=git@github.com:${GHUSER}/flux-get-started \
--git-path=namespaces,workloads \
--namespace=flux | kubectl apply -f -
```

- Flux が開始するのを待つ

```
kubectl -n flux rollout status deployment/flux
```

- 起動時に Flux は SSH キーを生成し、公開鍵をログに記録する。
- 以下を実行して公開鍵を見つける

```
fluxctl identity --k8s-fwd-ns flux
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCrN+Wn+ShzjbU4wYu2Js8VBUEl5OngB4f9EO9b7T76aerEtjnggI7Up0SJJ5WGG2yuSo29JbpQW+Rbrog5jDeeIpc4Aba6J0aJMjKTu90S7jUt2eWnzKKETs+vG9Hqzown/e+0ISv4/OWbebEA7MoNH1QbhB3SQhEPK1Y5bd+PryzoPofUxDy4BfHNizTz5OR1P1qpeibq1qMVl/MC3Yz2fb7ztT3GUOepuXT6Dw19+azM0IizOwuRhkif9pXQTwEy7/E+yhFkOtJJs/847xP1Qw4ZAh/PHWfAQ/JqrMq13s2PeRQ7a1/zPapLduGR/7xljlqqHaFxCmNwdFExclSUs1uEj/PWRUb3AX1ta9SpuoqETH39ye7t6H4saHLzr5iwjKqo016ETMHfwyUiUbBHzSmq8oiop36rw6zmMpQHgGOM40xKGjrVn/XCNRzg1Z0UyMR1Ler3wDh3YWCk8rSnsBTnpMimEe9sfkQWxQPiCQXFUZ0kIvLfpCFulb9TKus= root@flux-6c49c9f75f-j6vq6
```

- cluster と git を同期するためには、github リポジトリに書き込みアクセス権を持つデプロイ鍵を作る必要がある。
- github.com のリポジトリの設定から deploy key を追加する。



## 参考

https://qiita.com/sotoiwa/items/ef6913653b5d4958e89c
