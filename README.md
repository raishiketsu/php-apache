# php-apache

## コンテナの準備

Dockerfile
```
FROM php:5-apache
COPY index.php /var/www/html/index.php
RUN chmod a+rx index.php
```

index.php
```
<?php
  $x = 0.0001;
  for ($i = 0; $i <= 1000000; $i++) {
    $x += sqrt($x);
  }
  echo "OK!";
?>
```

## コンテナをPush
ビルドして、コンテナイメージレジストリにPush、
もしくはこちらを使います。
registry.k8s.io/hpa-example


## コンテナをデプロイ
割愛


## 状態の確認

```
kubectl get hpa
```


## 負荷をかける
```
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```


## 状態の確認
```
watch kubectl get deployment php-apache
```


## 負荷の停止
<Ctrl> + C
