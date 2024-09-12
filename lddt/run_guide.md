# Usage Guide

## Installation

lddt是openstructure的其中一个模块。因此需要先安装openstructure。
> openstructure很强大，除了lddt还有很多指标可以算。有时间可以看看怎么算contact。

### Install by Docker

推荐使用docker安装，直接从远程拉取镜像

```shell
docker pull registry.scicore.unibas.ch/schwede/openstructure:<TAG>
```

> `tag`的选择可以从[GitLab registry page](https://git.scicore.unibas.ch/schwede/openstructure/container_registry/)找到。名为 `latest` 的标签始终可用，指向当前稳定的 OST 版本。

### Not Root

一般情况下我们不是root，无法使用sudo（docker需要root权限）。在集群上可能有一些工具代替docker。比如在delta上可以使用apptainer。apptainer是singularity的一个分支，专门用于HPC。具体怎么使用apptainer，这里不做讨论，可自行看官方文档或chatgpt。

## Run

### 命令

假设有文件`$YOUR_DIR/af3/model.pdb`和`$YOUR_DIR/reference.pdb`，你需要计算lddt，可以使用如下方法：

```shell
docker run --rm -v $YOUR_DIR:/home registry.scicore.unibas.ch/schwede/openstructure compare-structures \
    --model /home/af3/model.pdb \
    --reference /homde/reference.pdb \
    --output scores.json \
    --lddt \
    --lddt-no-stereochecks
```

这里`-v`的作用是把`$YOUR_DIR`挂载到`/home`，也就是说，在容器内部，`/home`就是`$YOUR_DIR`。这样就可以在容器内部访问到`$YOUR_DIR`下的文件了。

`compare-structures`的具体参数可以通过下面的命令查看：

```shell
docker run --rm registry.scicore.unibas.ch/schwede/openstructure compare-structures -h
```

### 脚本

openstructure提供了 [run_docker_ost](https://git.scicore.unibas.ch/schwede/openstructure/-/blob/master/docker/run_docker_ost?ref_type=heads) 脚本方便我们在 Docker 容器中运行指定的脚本或操作。具体可参考 [README](https://git.scicore.unibas.ch/schwede/openstructure/-/blob/master/docker/README.rst?ref_type=heads) 。我们可以自己编写一个python脚本，然后通过`run_docker_ost`运行。

以下命令可以进入交互式的ost python环境：

```shell
docker run --rm -ti registry.scicore.unibas.ch/schwede/openstructure
```

## 其他参考

- [openstructure](https://git.scicore.unibas.ch/schwede/openstructure)
- [官方文档](https://openstructure.org/docs)
- [monomer_lddt_no_stereocheck.py](https://git.scicore.unibas.ch/schwede/casp15_ema/-/blob/main/scoring/monomer_lddt_no_stereocheck.py?ref_type=heads)
- [compare-structures](https://openstructure.org/docs/2.8/actions/#ost-compare-structures)
- [lddt](https://openstructure.org/docs/2.8/mol/alg/molalg/#ost.mol.alg.lddt.lDDTScorer)
