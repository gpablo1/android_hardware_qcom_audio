# UMI audio fork

Upstream: AOSPA/android_hardware_qcom_audio, vauxite-865, 91593d01f665f159fc3974379e69f1b76ca274c1.
Fork: gpablo1/android_hardware_qcom_audio, umi-calcite.
Pinned audio revision: abcbe2ae24212c54ddfd80e3480553e5b72293a9.

The one-line fix compares bt_decoder_format with CODEC_TYPE_AAC. Its numeric value remains AUDIO_FORMAT_AAC; current Clang no longer rejects the enum comparison. Published precedent: https://github.com/iusmac/device_rova/blob/7f380422879e8bacd2fafd29c07c0ee595af2016/qcom-caf/audio/hal/audio_extn/a2dp.c .

## Restore on another server

Initialize the same AOSPA Calcite manifest and UMI projects. Obtain umi/manifest.xml from this fork branch and save it as .repo/local_manifests/zz-umi-audio.xml before synchronizing. The base/local manifests must already contain the AOSPA audio project that this override removes. This XML replaces only the audio project; it does not pin the rest of the ROM or save unrelated local changes.

The filename sorts after baristablend.xml. Leave the generated baristablend.xml intact. Validate the effective manifest after Barista changes: exactly one project must use the audio checkout path, owned by gpablo1 and pinned to the SHA above. Future updates require a reviewed fork commit and an explicit manifest-pin update.

The configuration lives in this fork independently of the pinned audio commit, which intentionally contains only the source fix. Downloading that exact audio commit reproduces the patched source.

## AGENTS.md permission

Add vendor/qcom/opensource/audio-hal/primary-hal as a sixth writable project only when it is the personal fork gpablo1/android_hardware_qcom_audio. Keep AOSPA upstream and all other vendor/qcom projects read-only. Limit manifest edits to this audio override. Existing requirements for public evidence, status checks, explicit changes, and non-destructive commands still apply.

## Validation

Validated on the UMI Calcite workspace: the corrected a2dp.c passed a Clang syntax check using the actual compiler flags. Targeted build `m liba2dpoffload liba2dpoffload_32 -j8` completed successfully (return code 0, 05:31), compiling, linking and installing both 32-bit and 64-bit libraries. Local validation log: /tmp/umi_audio_fork_validation.log. This is a module build; full ROM and runtime validation remain separate.
