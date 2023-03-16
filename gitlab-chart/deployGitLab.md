```
helm repo add gitlab-jh https://charts.gitlab.cn
helm repo update
helm install gitlab gitlab-jh/gitlab \
--version 6.9.3 \
--timeout 600s \
--set global.hosts.domain=bfylu.top \
--set global.hosts.externalIP=10.244.0.10 \
--set certmanager-issuer.email=18279544224@163.com \
--set global.gitaly.authToken.secret=gitlab-gitaly-secret \
--set global.praefect.authToken.secret=gitlab-praefect-secret \
--set global.minio.credentials.secret=gitlab-minio-secret \
--set postgresql.install=false \
--set global.psql.host=192.168.2.194 \
--set global.psql.password.secret=gitlab-postgresql-password \
--set global.psql.password.key=postgresql-password \
--set global.psql.port=5432 \
--set global.psql.database=gitlabhq_production \
--set global.psql.username=gitlab \
--set redis.install=false \
--set global.redis.host=10.110.249.100 \
--set global.redis.password.enabled=false \
--set global.redis.port=6379 \
-n gitlab-jh
```



kubectl create -n gitlab-jh secret generic gitlab-gitlab-initial-root-password --from-literal=password=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 32)

kubectl create -n gitlab-jh secret generic gitlab-postgresql-password \
--from-literal=postgresql-password=4oCcbHUxMjM0NTbigJ0=\
--from-literal=postgresql-postgres-password=4oCcbHUxMjM0NTbigJ0=

kubectl create -n gitlab-jh secret generic gitlab-minio-secret --from-literal=accesskey=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 20) --from-literal=secretkey=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)

kubectl create -n gitlab-jh secret generic gitlab-praefect-secret --from-literal=token=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)

kubectl create -n gitlab-jh secret generic gitlab-gitaly-secret --from-literal=token=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)

kubectl create -n gitlab-jh secret generic smtp-password --from-literal=password=lu123456

kubectl create -n gitlab-jh secret generic prod-db-secret --from-literal=username=gitlabhq_production --from-literal=password=Y4nys7f11

