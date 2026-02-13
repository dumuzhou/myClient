# GitHub Actions 工作流

本目录包含此项目的 GitHub Actions 工作流配置。

## iOS 构建和发布 (ios-build.yml)

此工作流自动构建 iOS IPA 文件并上传到 GitHub Releases。

### 触发方式

工作流可以通过以下两种方式触发：

1. **自动触发**：推送版本标签（如 `v1.0.0`）
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. **手动触发**：在 GitHub 仓库的 Actions 标签页手动运行
   - 可以选择指定版本号，也可以使用默认值（会自动生成带时间戳的版本号）

### 工作流步骤

1. **环境准备**
   - 检出代码
   - 安装 Node.js 18
   - 安装项目依赖

2. **构建 React Native Bundle**
   - 安装 duxappReactNative 模块依赖
   - 构建 iOS 平台的 React Native bundle

3. **iOS 原生构建**
   - 安装 CocoaPods
   - 安装 iOS 依赖（Pod install）
   - 使用 xcodebuild 构建 iOS Archive
   - 导出 IPA 文件

4. **发布**
   - 上传 IPA 到 GitHub Releases（仅对标签推送）
   - 上传 IPA 作为 GitHub Artifacts（所有构建，保留 30 天）

### 代码签名说明

当前工作流配置为无签名构建，生成的 IPA 为未签名版本。这对于以下场景很有用：

- CI/CD 环境测试
- 本地重新签名
- 使用企业证书的二次签名

**如需签名的 IPA**，请：
1. 在仓库 Settings > Secrets 中添加证书和配置文件
2. 修改工作流以使用签名选项

### 输出

- **GitHub Release**：对于标签推送，IPA 文件会附加到相应的 release
- **GitHub Artifacts**：所有构建的 IPA 都会作为 artifacts 上传，名称格式为 `duxapp-ios-{version}`

### 构建环境

- **运行器**：macOS latest
- **Node.js**：18.x
- **Ruby**：3.0
- **CocoaPods**：最新版本

### 权限

工作流使用最小权限原则，仅需要：
- `contents: write` - 用于上传 release 资源

---

# GitHub Actions Workflows

This directory contains GitHub Actions workflow configurations for this project.

## iOS Build and Release (ios-build.yml)

This workflow automatically builds iOS IPA files and uploads them to GitHub Releases.

### Trigger Methods

The workflow can be triggered in two ways:

1. **Automatic**: Push a version tag (e.g., `v1.0.0`)
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. **Manual**: Run manually from the Actions tab in the GitHub repository
   - You can specify a version number or use the default (which auto-generates a timestamped version)

### Workflow Steps

1. **Environment Setup**
   - Checkout code
   - Install Node.js 18
   - Install project dependencies

2. **Build React Native Bundle**
   - Install duxappReactNative module dependencies
   - Build React Native bundle for iOS platform

3. **Native iOS Build**
   - Install CocoaPods
   - Install iOS dependencies (Pod install)
   - Build iOS Archive with xcodebuild
   - Export IPA file

4. **Release**
   - Upload IPA to GitHub Releases (for tag pushes only)
   - Upload IPA as GitHub Artifacts (all builds, retained for 30 days)

### Code Signing Notes

The current workflow is configured for unsigned builds, generating unsigned IPAs. This is useful for:

- CI/CD environment testing
- Local re-signing
- Re-signing with enterprise certificates

**For signed IPAs**, please:
1. Add certificates and provisioning profiles in repository Settings > Secrets
2. Modify the workflow to use signing options

### Outputs

- **GitHub Release**: For tag pushes, IPA files are attached to the corresponding release
- **GitHub Artifacts**: All built IPAs are uploaded as artifacts with the name format `duxapp-ios-{version}`

### Build Environment

- **Runner**: macOS latest
- **Node.js**: 18.x
- **Ruby**: 3.0
- **CocoaPods**: Latest version

### Permissions

The workflow uses the principle of least privilege, requiring only:
- `contents: write` - For uploading release assets
