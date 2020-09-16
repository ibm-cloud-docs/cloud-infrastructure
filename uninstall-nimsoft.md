---

copyright:
  years: 2020
lastupdated: "2020-05-07"

keywords: uninstall advanced monitoring, uninstall nimsoft

subcollection: SLmonitoring

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:important: .important}
{:tip: .tip}
{:note: .note}

# Uninstalling Advanced Monitoring by Nimsoft
{: #uninstall-nimsoft}

To uninstall Advanced Monitoring by Nimsoft, use the following information.

## Uninstalling Advanced Monitoring by Nimsoft on Windows
{: uninstall-nimsoft-windows}

To uninstall Advanced Monitoring by Nimsoft from Windows, use the following commands.

| Action | Command |
|--------|---------|
| Start  | `C:\Program Files (x86)\nimsoft\bin\nimbus.exe –start` |
| Stop   | `C:\Program Files (x86)\nimsoft\bin\nimbus.exe –stop` |
| Remove | `C:\Program Files (x86)\nimsoft\unins000.exe` |
| Remove (cmd) | `C:\Program Files (x86)\nimsoft\bin\nimbus.exe -remove` |
{: caption="Table 1. Unistall Advanced Monitoring by Nimsoft from Windows" caption-side="top"}

## Uninstalling Advanced Monitoring by Nimsoft on Linux
{: uninstall-nimsoft-linux}

To uninstall Advanced Monitoring by Nimsoft from Linux (RHEL 6.0 and newer, CentOS and newer, Ubuntu), use the following commands.

| Action | Command |
|--------|---------|
| Confirm | `rpm -q nimsoft-robot` |
| Start   | `initctl start nimbus` |
| Stop    | `initctl stop nimbus`  |
| Remove  | `rpm -e RPM_package`(RPM_package = robot name minus .rpm) |
{: caption="Table 2. Unistall Advanced Monitoring by Nimsoft from Linux" caption-side="top"}

_Confirm_ and _Remove_ commands apply to robots that were installed by ADE. If you installed the robot by using `nimldr`, no RPM packages are installed. So, the confirm and remove commands aren't applicable.
{: important}

To remove a robot that was installed by `nimldr`, use the following commands.

`service nimbus stop`
`/opt/nimsoft/bin/inst_init.sh remove`





 
