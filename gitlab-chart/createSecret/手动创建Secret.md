##  手动创建secret(可选)

##  初始 root 密码

创建用于存储初始 root 密码的 Kubernetes secret。密码长度至少应为 6 个字符。 将 <name> 替换为发行版的名称。

```
kubectl create secret generic <name>-gitlab-initial-root-password --from-literal=password=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 32)
```

##  Redis 密码

为 Redis 生成一个随机的 64 个字符的包含字母和数字的密码。 将 <name> 替换为发行版的名称。
```
kubectl create secret generic <name>-redis-secret --from-literal=secret=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)
```
如果使用已有的 Redis 集群部署，请使用 base64 编码的 Redis 集群访问密码，不要使用随机生成的密码。

##  GitLab Shell secret
为 GitLab Shell 生成一个随机的 64 个字符的包含字母和数字的 secret。 将 <name> 替换为发行版的名称。
```
kubectl create secret generic <name>-gitlab-shell-secret --from-literal=secret=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)
```
此 secret 由global.shell.authToken.secret 设置引用。

##  Gitaly secret
为 Gitaly 生成一个随机的 64 个字符的包含字母和数字的令牌。 将 <name> 替换为发行版的名称。
```
kubectl create secret generic <name>-gitaly-secret --from-literal=token=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)
```
此 secret 由 global.gitaly.authToken.secret 设置引用。

##  Praefect secret
为 Praefect 生成一个随机的 64 个字符的包含字母和数字的令牌。将 <name> 替换为发行版的名称：
```
kubectl create secret generic <name>-praefect-secret --from-literal=token=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)
```
此 secret 由global.praefect.authToken.secret 设置引用。

##  MinIO secret
为 MinIO 生成一组随机的 20 和 64 个字符的包含字母和数字的密钥。 将 <name> 替换为发行版的名称。
```
kubectl create secret generic <name>-minio-secret --from-literal=accesskey=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 20) --from-literal=secretkey=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)
```
此 secret 由 global.minio.credentials.secret 设置引用。

##  PostgreSQL 密码
生成一个随机的 64 个字符的包含字母和数字的密码。 将 <name> 替换为发行版的名称。
```
kubectl create secret generic <name>-postgresql-password \
    --from-literal=postgresql-password=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64) \
    --from-literal=postgresql-postgres-password=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 64)
```
将字符串string编码为base64的字符串然后输出； echo -n “password” | base64 4oCcbHUxMjM0NTbigJ0=
kubectl create secret generic gitlab-postgresql-password \
    --from-literal=postgresql-password=4oCcbHUxMjM0NTbigJ0=\
    --from-literal=postgresql-postgres-password=4oCcbHUxMjM0NTbigJ0=

##  外部服务
一些 chart 有更多的 secret 来启用无法自动生成的功能。

##  LDAP 密码
如果您需要密码认证来连接您的 LDAP 服务器，您必须将密码存储在 Kubernetes secret 中。
```
kubectl create secret generic ldap-main-password --from-literal=password=yourpasswordhere
```
然后使用 --set global.appConfig.ldap.servers.main.password.secret=ldap-main-password 将密码注入到您的配置中。

在配置 Helm 属性时使用 Secret 名称，而不是实际密码。

##  SMTP 密码
如果您使用的是需要身份验证的 SMTP 服务器，请将密码存储在 Kubernetes secret中。
```
kubectl create secret generic smtp-password --from-literal=password=yourpasswordhere
```
然后在你的 Helm 命令中使用 --set global.smtp.password.secret=smtp-password。
在配置 Helm 属性时使用 Secret 名称，而不是实际密码。

##  接收电子邮件的 IMAP 密码
让极狐GitLab 可以访问收到的电子邮件 将 IMAP 帐户的密码存储在 Kubernetes secret 中。
```
kubectl create secret generic incoming-email-password --from-literal=password=yourpasswordhere
```
然后在 Helm 命令中使用 --set global.appConfig.incomingEmail.password.secret=incoming-email-password 以及在文档中指定的其它必需设置。
在配置 Helm 属性时使用 Secret 名称，而不是实际密码。

##  服务台电子邮件的 IMAP 密码
为了让极狐GitLab 可以访问服务台电子邮件，将 IMAP 帐户的密码存储在 Kubernetes secret 中。
```
kubectl create secret generic service-desk-email-password --from-literal=password=yourpasswordhere
```
然后在 Helm 命令中使用 --set global.appConfig.serviceDeskEmail.password.secret=service-desk-email-password 以及文档中指定的其它必需设置。
在配置 Helm 属性时使用 Secret 名称，而不是实际密码。

##  接收电子邮件身份验证令牌
当接收电子邮件配置为使用 webhook 传递方法时，mail_room 服务和 web 服务之间应该有一个共享密钥，其必须具有 32 个字符的长度和 base64 编码。将 <name> 替换为发布版本的名称。
```
kubectl create secret generic <name>-incoming-email-auth-token --from-literal=authToken=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 32 | base64)
```
global.incomingEmail.authToken 设置引用了此 secret。

##  服务台电子邮件身份验证令牌
当服务台电子邮件配置为使用 webhook 传递方法时，mail_room 服务和 web 服务之间应该有一个共享密钥，其必须具有 32 个字符的长度和 base64 编码。将 <name> 替换为发布版本的名称。
```
kubectl create secret generic <name>-service-desk-email-auth-token --from-literal=authToken=$(head -c 512 /dev/urandom | LC_CTYPE=C tr -cd 'a-zA-Z0-9' | head -c 32 | base64)
```
global.serviceDeskEmail.authToken 设置引用了此 secret。

##  接收电子邮件的 Microsoft Graph 客户端 secret
让极狐GitLab 可以访问接收电子邮件，将 IMAP 帐户的密码存储在 Kubernetes secret 中。
```
kubectl create secret generic incoming-email-client-secret --from-literal=secret=your-secret-here
```
然后，在 Helm 命令中使用 --set global.appConfig.incomingEmail.clientSecret.secret=incoming-email-client-secret 以及文档中指定的其它必需设置.
在配置 Helm 属性时使用 Secret 名称，而不是实际密码。