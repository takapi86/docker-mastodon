# Docker Compose 環境インストール手順

ソースコードの取得します。

```bash
# docker/srcディレクトリで実行
git clone --depth 1 https://github.com/tootsuite/mastodon.git -b v3.3.0
```

Dockerfileのビルドを行います。

```bash
# dockerディレクトリで実行
docker-compose build
```

bundle installを行います

```bash
# dockerディレクトリで実行
docker-compose run --rm app bundle install
```

アプリケーションを立ち上げます。

```bash
# dockerディレクトリで実行
docker-compose up -d
```

AssetsPrecompile, DBのセットアップを行います。

```bash
# dockerディレクトリで実行
docker-compose exec app bundle exec rails assets:precompile
docker-compose exec app bundle exec rails db:setup
```

管理者のログイン情報を設定します。

```bash
# dockerディレクトリで実行
docker-compose exec app bundle exec rails r 'User.first.update!(email: "admin@localhost:3000", password: "password",)'
```

* URL: http://localhost:3000/
* ログイン: admin@localhost:3000/password

# Kubernetes 環境インストール手順

※Docker Compose 環境インストール手順で作成したイメージを使用します。

context を `docker-desktop` に設定します。

```
kubectl config use-context docker-desktop
```

マニフェストを適用します。

```
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

DBのマイグレーションを行います。

```bash
kubectl exec -it --container app (kubectl get pod -o name | grep app) -- bundle exec rails db:setup
```

管理者のパスワードを設定します。

```bash
kubectl exec -it --container app (kubectl get pod -o name | grep app) -- bundle exec rails r 'User.first.update!(email: "admin@localhost:3000", password: "password",)'
```

* URL: http://localhost:3000/
* ログイン: admin@localhost:3000/password
