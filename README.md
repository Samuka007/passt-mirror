# passt-mirror

Ubuntu 22.04 (jammy) 的 passt/pasta 非官方镜像源。jammy 归档不含 passt，而
iso-cc 依赖它；本仓在钉定的上游 tag 上以 jammy 环境源码编译，发布到
apt.samuka007.com 的 `passt` 组件。

## 客户端接入（jammy）

```sh
curl -fsSL https://apt.samuka007.com/KEY.gpg | sudo tee /etc/apt/trusted.gpg.d/isocc.asc
echo "deb [signed-by=/etc/apt/trusted.gpg.d/isocc.asc] https://apt.samuka007.com stable main passt" | sudo tee /etc/apt/sources.list.d/isocc.list
sudo apt update && sudo apt install passt
```

## 发布

GitHub Actions → passt → Run workflow（填上游 tag，默认 `2024_02_20.1e6f92b`）。
流程：ubuntu:22.04 容器内 `make` → dpkg-deb 打包
`passt_<ver>-1iso1_amd64.deb` → 增量合并发布到 R2 registry（拉现有 tree →
补 pool/passt → 全量重建索引 → GPG 重签 → 推回）。

## 安全更新义务

上游（https://passt.top/passt）或 Ubuntu 发布 passt 安全修复时，改 tag 重跑
本 workflow。registry 的 Consumers 通过 `apt update` 获得更新。
