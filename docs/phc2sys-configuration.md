---
layout: default
title: phc2sys Configuration Guide
---

# phc2sys Configuration Guide

## Problem: `phc2sys: invalid option -- '-'`

When running `phc2sys` with a long option such as `--transportSpecific=1`, you may encounter the following error:

```
$ sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w --transportSpecific=1
phc2sys: invalid option -- '-'

usage: phc2sys [options]

 automatic configuration:
 -a             turn on autoconfiguration
 -r             synchronize system (realtime) clock
                repeat -r to consider it also as a time source
```

### Cause

`phc2sys` (from the [linuxptp](http://linuxptp.sourceforge.net/) package) does not support
GNU-style long options (i.e., `--option=value` format). Options such as `transportSpecific`
cannot be passed directly on the command line and must instead be specified in a configuration
file.

### Solution

**Step 1**: Create a configuration file (e.g., `/etc/phc2sys.conf`) that sets `transportSpecific`:

```
[global]
transportSpecific        1
```

**Step 2**: Pass the configuration file to `phc2sys` using the `-f` flag:

```
sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w -f /etc/phc2sys.conf
```

This is equivalent to the intended command and correctly applies the `transportSpecific 1` setting.

### Alternative: Reuse an existing ptp4l configuration file

If you already have a `ptp4l` configuration file (e.g., `/etc/ptp4l.conf`) that contains
`transportSpecific 1` in its `[global]` section, you can reuse it directly:

```
sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w -f /etc/ptp4l.conf
```

### Summary

| Incorrect (not supported)                                                       | Correct                                                                                  |
|---------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| `sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w --transportSpecific=1`      | `sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w -f /etc/phc2sys.conf`                |

---

## 问题：`phc2sys: invalid option -- '-'`（中文说明）

使用 `--transportSpecific=1` 等长选项运行 `phc2sys` 时，会出现如下错误：

```
phc2sys: invalid option -- '-'
```

### 原因

`phc2sys`（来自 [linuxptp](http://linuxptp.sourceforge.net/) 软件包）**不支持** GNU 风格的长选项
（即 `--选项名=值` 的格式）。`transportSpecific` 等参数无法直接在命令行中指定，必须通过配置文件传入。

### 解决方法

**第一步**：创建配置文件，例如 `/etc/phc2sys.conf`，写入以下内容：

```
[global]
transportSpecific        1
```

**第二步**：使用 `-f` 参数将配置文件传给 `phc2sys`：

```
sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w -f /etc/phc2sys.conf
```

### 备选方案：复用已有的 ptp4l 配置文件

如果您已有包含 `transportSpecific 1` 的 `ptp4l` 配置文件（如 `/etc/ptp4l.conf`），可以直接使用：

```
sudo phc2sys -m -s enp0s31f6 -c CLOCK_REALTIME -w -f /etc/ptp4l.conf
```
