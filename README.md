# LineageOS 23.2 for Xiaomi 14 (houji)

这是在 LineageOS 主 manifest 上补充 houji 源码项目的 local manifest，包含
SM8650Dev 下的 21 个代码仓库及 3 个直接上游依赖。当前配置使用源码内核和外置模块，
搭配同版国行预编译 DTB/DTBO；部分模块及专有文件仍由 vendor 提供。

## 同步

需要 Git、Repo、Git LFS，以及可访问 GitHub 的 SSH 密钥。先初始化 LineageOS：

```sh
repo init -u https://github.com/LineageOS/android.git -b lineage-23.2 --git-lfs
manifest_dir=$(mktemp -d)
git clone --branch main git@github.com:SM8650Dev/android_manifest_houji.git "$manifest_dir"
mkdir -p .repo/local_manifests
cp "$manifest_dir/houji.xml" .repo/local_manifests/houji.xml
repo sync -c -j8
```

`houji.xml` 跟踪各组织仓库的 `lineage-23.2` 分支。`houji-pinned.xml` 固定本次发布的
24 个 houji 项目提交；若要复现此快照，改为复制它，并确保只安装这两份 XML 中的一份。
这份补充快照不锁定其余 LineageOS 工程；全工程版本应另外保存 `repo manifest -r`。
已有独立 Git 工作区时先保存本地修改，避免覆盖式同步；同一路径只定义一次。

## 构建

```sh
source build/envsetup.sh
breakfast houji
m bacon
```

完整 XML 已列出当前依赖，普通 `repo sync` 无需修改 roomservice。
如需 roomservice 自动发现后续新增依赖，请参阅设备仓库中的已有补丁说明。

- [仓库列表及上游来源](https://github.com/SM8650Dev/android_device_xiaomi_houji/blob/lineage-23.2/docs/dev/repositories.md)
- [依赖与 roomservice 说明](https://github.com/SM8650Dev/android_device_xiaomi_houji/blob/lineage-23.2/docs/dev/dependencies.md)
- [当前移植进度](https://github.com/SM8650Dev/android_device_xiaomi_houji/blob/lineage-23.2/docs/dev/bringup.md)

各驱动独立保留原上游 Git 历史。音频、GPU 和视频的发布仓库名为
`android_kernel_xiaomi_houji-multimedia`，源码路径仍映射到
`kernel/xiaomi/houji-modules/oneplus-sm8650`，保留 OnePlusOSS 来源。
