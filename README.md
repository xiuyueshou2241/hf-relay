# HF Relay - HuggingFace Download via GitHub Actions

通过 GitHub Actions 中转下载 HuggingFace 模型文件，绕过 GFW 限制。

## 使用方法

### 方式一：使用本地脚本（推荐）

```bash
# 下载单个文件
~/scripts/hf-relay.sh <hf_repo> <hf_file> [release_tag] [branch] [target_dir]

# 示例
~/scripts/hf-relay.sh DeepBeepMeep/Wan2.1 wan2.1_14B.safetensors
~/scripts/hf-relay.sh stabilityai/stable-diffusion-3.5-large sd35.safetensors sd35 main ~/models
```

### 方式二：手动触发

1. 访问 [Actions 页面](https://github.com/xiuyueshou2241/hf-relay/actions/workflows/relay.yml)
2. 点击 "Run workflow"
3. 填写参数：
   - `hf_repo`: HuggingFace 仓库名 (如 `DeepBeepMeep/Wan2.1`)
   - `hf_file`: 文件路径 (如 `wan2.1_14B.safetensors`)
   - `release_tag`: Release 标签 (自定义)
   - `hf_branch`: 分支 (默认 `main`)

### 方式三：API 触发

```bash
gh api repos/xiuyueshou2241/hf-relay/dispatches \
  -X POST \
  -f event_type=hf-relay \
  -f "client_payload[hf_repo]=DeepBeepMeep/Wan2.1" \
  -f "client_payload[hf_file]=wan2.1_14B.safetensors" \
  -f "client_payload[release_tag]=wan21-14b"
```

## 下载文件

Workflow 完成后，文件会上传到 GitHub Release，可通过以下方式下载：

```bash
# 代理加速（推荐国内用户）
curl -L -O "https://gh-proxy.com/https://github.com/xiuyueshou2241/hf-relay/releases/download/<tag>/<filename>"

# 直连
curl -L -O "https://github.com/xiuyueshou2241/hf-relay/releases/download/<tag>/<filename>"
```

## 特性

- ✅ 自动处理 HuggingFace LFS 文件
- ✅ 大文件自动分片（>1.9GB）
- ✅ 支持 gated 仓库（需配置 HF_TOKEN secret）
- ✅ MD5 校验文件
- ✅ 代理加速支持

## 限制

- GitHub Release 单文件限制 2GB（超过会自动分片）
- Gated 仓库需要在仓库 Settings → Secrets 中添加 `HF_TOKEN`
- Fine-grained Token 需要 `contents:write` 权限（用于触发 workflow）

## 代理推荐

| 代理 | 速度 | 稳定性 |
|------|------|--------|
| gh-proxy.com | ~3-4 MB/s | ⭐⭐⭐⭐ |
| ghfast.top | ~50-100 KB/s | ⭐⭐⭐ |
