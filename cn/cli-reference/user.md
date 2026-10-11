# 用户

## 列出用户电子邮件

此命令允许您列出系统中注册的所有用户电子邮件。

### 本地使用

```bash
pnpm user
```

或者如果使用 npm

```bash
npx flowise user
```

### Docker 使用

如果您在 Docker 容器中运行 Flowise，请使用以下命令：

```bash
docker exec -it FLOWISE_CONTAINER_NAME pnpm user
```

将 `FLOWISE_CONTAINER_NAME` 替换为您的实际 Flowise 容器名称。

## 重置用户密码

该命令允许您重置用户的密码。

### 本地使用

```bash
pnpm user --email "admin@admin.com" --password "myPassword1!"
```

或者如果使用 npm

```
npx flowise user --email "admin@admin.com" --password "myPassword1!"
```

### Docker 使用

如果您在 Docker 容器中运行 Flowise，请使用以下命令：

```bash
docker exec -it FLOWISE_CONTAINER_NAME pnpm user --email "admin@admin.com" --password "myPassword1!"
```

将 `FLOWISE_CONTAINER_NAME` 替换为您的实际 Flowise 容器名称。

### 参数

* `--email`：您要重置其密码的用户的电子邮件地址
* `--password`：为用户设置的新密码
