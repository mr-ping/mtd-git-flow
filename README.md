# MTD 版本管理系统使用规范

MTD日常开发采用Git作为版本管理系统。

MTD版本管理系统的分支管理策略（工作流）采用AVH扩展版本的git-flow工作流方式。



## AVH版git-flow分支类型

- 开发分支（主线）：develop_flow (MTD_Chem_Server基于develop_5.0分支)

- 部署分支（主线）：master

- 支持分支（临时）：
  - 功能分支 - feature：用于功能开发（开发阶段）的临时性分支
  - 发布分支 - release：用于版本发布（测试阶段）的临时性分支
  - BUG修复分支 - bugfix：用于常规Bug修复
  - BUG修复分支 - hotfix：用于线上版本Bug的热修复

**工作流图示：**

![http://nvie.com/img/git-model@2x.png](http://nvie.com/img/git-model@2x.png)



## 开发阶段的分支使用——"feature"分支

开发过程中请开发人员建立相应的 *feature* 分支，并于其上进行开发工作，勿直接操作 *develop_flow* 或者 *master* 分支。

1. 开发人员在开发相关功能前，请使用 `git flow feature start branch-name` 命令创建临时的功能性分支。 

2. 具体开发工作请于创建的相应 *feature* 分支中进行。

3. 待功能开发完成后，请使用 `git flow featrue finish branch-name --no-ff` 命令将此分支并入 *develop_flow* 分支，并删除此功能分支。

相关命令文档：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-feature


## 测试阶段的分支使用——"release"分支

测试用临时分支的操作由项目负责人（或技术负责人）进行，开发人员请与已经建好的*release*分支中进行bug修复等相关操作。

1. 测试开始前，请使用 `git flow release start version-code` 命令创建临时的发布分支。
2. 使用 `git flow release publish version-code` 命令将其推送至远端服务，供开发人员协作使用。
3. 开发人员请使用 `git flow release track`  &  `git flow relase checkout`  &  `git flow relase pull` 等命令进行分支操作。详情请参考文档。
4. 测试期间的bug修复工作于此分支行中进行，bug修复完成后可随时并入 `develop_flow` 分支当中，以便于新的下游分支同步。
5. 测试结束后，请使用 `git flow release finish version-code -p --ff-master` 命令将发布分支同时合并入 *develop_flow* 和 *master* 分支中并推送至远端，且同时将此发布分支删除。

相关命令文档：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-release



## 常规Bug修复——"bugfix"分支

此Bug修复行为适用于相关 *feature* 分支已开发完成的情况之下，请勿直接与develop_flow分支中修改。

1. 进行Bug修复之前，请使用 `git flow bugfix start branch-name` 命令创建临时的修复分支。
2. 相关修复行为请于此分支上进行。
3. 修复完成后，请使用 `git flow bugfix finish branch-name --no-ff` 命令结束临时修复分支，并将其并入 *develop_flow* 分支当中。

相关命令文档：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-bugfix



## 线上Bug修复——"hotfix"分支

线上Bug修复请创建临时 *hotfix* 分支，勿直接于 *master分支* 上进行修改。

1. 线上bug修复，请相关开发人员于 master 分支上使用 `git flow hotfix start branch-name` 命令创建临时修复分支。
2. 具体BUG修复工作于此分支中进行。
3. 线上bug修复完成后，使用`git flow hotfix finish branch-name`命令结束临时分支生命周期，并将其合并入*master*分支中。

相关命令文档：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-hotfix



## 注：如因合作开发，需要共享"支持分支"，请依照下列建议操作：

1. 创建者使用 `git flow feature publish branch-name` (命令中'feature'可根据需要替换为其他支持分支)将分支推送至远端服务；
2. 协作者在完成 *git-flow* 初始化前提下，使用 `git flow feature track branch-name` (命令中'feature'可根据需要替换为其他支持分支)对远端分支进行追踪（其他如`checkout`, `pull`, `rebase`, `diff` 等操作方法，详见上方各自命令文档）；
3. 完成本功能开发后请将相应分支从远端及本地删除。
