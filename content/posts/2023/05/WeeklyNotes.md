---
title: 周总结-slumr作业系统/AUC计算/传输数据
author: Feng Sang
description: 本周内的ticks总结。
date: 2023-05-13T00:00:00.000Z
categories:
  - 系统/工具
tags:
  - slurm
  - linux
  - gretna
  - scp
  - rsync
format: hugo-md
---


## slumr作业调度系统

当因作业名太长，而无法用squeue命令看到对应作业的name时，可采用以下方法：

1.  用更短的且更容易区分的名字给作业命名（PS. 这不是废话嘛）；

2.  用命令`squeue --yaml | grep <作业名的部分字符串>`查询；

## 图片分辨率和尺寸

图片的DPI表示单位英寸上的像素数，DPI越高图像显示越细腻。在给定像素数和DPI的情况下可以用像素数除以DPI得到英寸数，再乘以2.54厘米得到厘米单位下的图片某一边的长度。

## GRETNA中AUC指标计算

GRETNA中当设置了多个threshold后，会自动输出一个对应指标的AUC值。计算方法是将指标作为y轴，threshold作为x轴作折线图。然后计算每一个梯形的面积后求和得到AUC值。但是当threshold设置过小是，部分网络指标计算会出现Inf，而对应的AUC中会出现缺失值。这里提供一种解决方法，即去掉Inf对应阈值下的指标，再去计算AUC。代码如下：

``` python
import os
from os.path import join as opj
from glob import glob
import numpy as np
import logging
logging.basicConfig(level=logging.INFO, format="%(asctime)s %(message)s")

der = "results/NodalShortestPath"
nodalPaths = [f"{der}/NLp_Thres{i+1:03d}.txt" for i in range(36)]
nodalData = []
for i in nodalPaths:
    nodalData.append(np.loadtxt(i))
nodalData = np.array(nodalData)

# each sub
tmpRes = []
for i in range(nodalData.shape[1]):
    # each region
    tmpSub = []
    for j in range(nodalData.shape[2]):
        tmpSubData = nodalData[:, i, j]
        tmpSubData = tmpSubData[~np.isnan(tmpSubData)]
        tmpSubData = tmpSubData[~np.isinf(tmpSubData)]
        tmpSubAuc = np.trapz(tmpSubData, dx=0.01)    
        tmpSub.append(tmpSubAuc)
    tmpRes.append(tmpSub)
tmpRes = np.array(tmpRes)
# logging.info(tmpRes.shape)
np.savetxt(opj(der, "NLp_AUC.txt"), tmpRes, fmt="%.16e", delimiter="\t")
logging.info("Done.")
```

## Linux系统的机器间互传数据

**scp**和**rsync**命令可以实现在不同机器（linux/unix系统）之间的文件传输。假设当前要把A机器的a用户的文件/a/src，传输到B机器的b用户的/b/dst/文件夹下，具体用法如下：`scp /a/src b@B:/b/dst/`

如果是用B机器上b用户登陆的，则需要将命令的源文件中添加用户名和机器IP地址，例如：`scp a@A:/a/src /b/dst/`

如果/a/src是文件夹，则需要给scp命令加上参数-r；如果B机器ssh的端口号不是默认的22，则需要给scp用-P参数指定目标机器的ssh服务端口号。例如：`scp -P <目标机器（B机器）的ssh服务的端口号> -r /a/src b@B:/b/dst/`

详细的参数说明请参考：<https://www.runoob.com/linux/linux-comm-scp.html>

**rsync**命令也可以实现在不同机器之间文件的同步传输。用法如下：`rsync <src> <dst>`

但通常都会使用-av参数来递归地进行同步（同步文件夹）和文件的元信息（-a参数的效果，-v则是显示当前正在同步的文件，方便监视进度）。另外可以使用`-e 'ssh -P xxx'`来指定目标机器ssh服务的端口号。更详细的说明请参考：<https://www.ruanyifeng.com/blog/2020/08/rsync.html>
