```
SSDT-PLUG-DRTNIA.aml：用于启用CPU电源管理和睿频。
SSDT-EC.aml：用于修复嵌入式控制器，以避免ACPI错误。
SSDT-GPRW.aml：用于修复睡眠问题。

SSDT-USB(x).aml：这是每套 EFI 都应该有的
SSDT-RHUB.aml：某些主板的 ACPI 确定存在问题，需要使用此文件进行修复调整，例如华硕 B460、部分微星 B460 等

SSDT-plug.aml 加载CPU原生电源管理（开启节能四项
SSDT-EC-USBX（同SSDT-EC）：禁用EC（Embedded Controller）和修复USB充电问题，可选
SSDT-AWAC.aml 修复RTC禁止AWAC（或者使用RTC0.aml补丁，二者选其一），必须
SSDT-PMC.aml 开启NVRAM （Z370、Z490和B460原生支持，可以不需要此补丁，其他主板基本都需要）可选
SSDT-GPRW.aml 解决大部分PC睡眠即醒问题，需搭配重命名使用（rename 47505257 02 to 58505257 02）
SSDT-PM.aml 加载节能第五项（断电后自动重启生效，PC基本通用的补丁）
SSDT-OC-XOSI.aml 操作系统补丁，配合重命名rename 5F4F5349 to 584F5349，技嘉主板需要

SSDT-EC.aml        用于防止卡afps_module_start
SSDT-AWAC.aml   用于防止卡afps_module_start
SSDT-XCPM.aml   用于开启CPU的SpeedStep
SSDT-PMCR.aml   开启节能五项
```


```
显卡：Lilu.kext, WhateverGreen.kext
声卡：AppleALC.kext
网卡：IntelMausi.kext
USB：USBInjectAll.kext
独立显卡：WhateverGreen.kext, NvidiaGraphicsFixup.kext, NvidiaWebDriverInjector.kext / WhateverGreen.kext, AGDPFix.kext / WhateverGreen.kext, RadeonDeInit.kext
```