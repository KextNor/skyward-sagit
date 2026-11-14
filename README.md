# Skyward Recovery · Xiaomi Mi 6 (sagit)

用 GitHub Actions 构建 [Skyward](https://github.com/kirklin/skyward) 的 recovery 镜像，机型配置在 [devices/sagit.toml](devices/sagit.toml)。

## 状态

**实验性，尚未在真机验证。**

上游 Skyward 目前只支持 A/B 机型（`_a/_b` 双槽位、`super` 动态分区、`vendor_boot`）。Mi 6 是 A-only 老机型，本仓库使用 fork [KextNor/skyward](https://github.com/KextNor/skyward) 的 `aonly` 分支：

- 新增 `sagit` / `polaris` 设备配置，`ab = false`
- A-only 启动时跳过 `super` 映射，直接使用原厂 recovery 的 ramdisk
- 原厂包从 MIUI fastboot 线刷包（`cdnorg.d.miui.com`）中取出 `images/recovery.img`，不使用 `payload.bin`

运行时的 A-only 适配仍在进行，镜像可能无法正常启动。刷入前请先备份 boot 与 recovery 分区。

## 构建

推送到任意分支触发构建，产物在 Actions 的 Artifacts 里；打 `v*` 标签会发布 Release。

## 刷入

```sh
unzip skyward-*.zip
adb reboot bootloader
fastboot flash recovery skyward-*.img
fastboot reboot recovery
```

镜像基于 MIUI `V11.0.5.0.PCACNXM`（中国版，Android 9）的原厂 recovery 构建。底包版本不一致时，建议先刷回该版本。

## 许可

Skyward 本体为 GPL-3.0-or-later，见上游仓库。
