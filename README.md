# HugoAura-Market

HugoAura 插件市场的**独立发布仓库**: 插件清单 (`plugins.json`) 与插件包
(`packages/*.zip`)。与主仓库 [HugoAura-Enhanced](https://github.com/blingbling-bow/HugoAura-Enhanced) 分离——插件发布不需要动主仓库。

## 清单分发

HugoAura 客户端按以下顺序拉取清单 (全部失败时回退到包内离线清单):

1. jsDelivr: `https://cdn.jsdelivr.net/gh/blingbling-bow/HugoAura-Market@main/plugins.json`
2. raw: `https://raw.githubusercontent.com/blingbling-bow/HugoAura-Market/main/plugins.json`

插件包下载地址与 sha256 均在清单中声明; 客户端安装时强制校验一致性。

## 发布一个插件

1. 把插件 zip (内含 `manifest.json` + `main.js`, 规格见主仓库
   [PLUGIN_DEVELOPMENT.md](https://github.com/blingbling-bow/HugoAura-Enhanced/blob/main/PLUGIN_DEVELOPMENT.md))
   放入 `packages/`, 文件名使用插件 id: `<id>.zip`;
2. 在 `plugins.json` 的 `items` 里登记一条:

```json
{
  "id": "com.yourname.plugin",
  "name": "显示名",
  "version": "1.0.0",
  "author": "作者",
  "description": "一句话描述",
  "type": "main",
  "homepage": "https://...",
  "downloadUrl": "https://cdn.jsdelivr.net/gh/blingbling-bow/HugoAura-Market@main/packages/com.yourname.plugin.zip",
  "sha256": "包文件的 SHA-256 (必填, 客户端强制校验)"
}
```

3. 计算 sha256: `certutil -hashfile packages/<id>.zip SHA256` 或
   `sha256sum packages/<id>.zip`;
4. 提交并 Pull Request。合并后 jsDelivr 约 10 分钟内生效。

## 约定

- 覆盖安装时低于已装版本会被客户端拒绝, 升级请递增 semver;
- 插件在客户端 vm 能力沙箱中运行 (无 require/fs/process), 能力以
  manifest.permissions 声明为准, 用户可在客户端逐项关闭;
- 清单中 `encryptConfig` 等宿主配置字段与市场无关, 请勿在插件里请求
  超出其功能的权限。
