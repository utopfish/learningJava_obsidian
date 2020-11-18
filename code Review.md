

如何做好代码 Review
## 什么时候发起 Review
在代码 Review 上，Author 需要意识到：Reviewer 的时间是昂贵的。因此在正式邀请 Reviewer 发起代码 Review 前，Author 有几项需要注意的点，这些都能提高代码 Review 的效率，节省 Reviewer 的时间。
#### MR (Merge Request)
也称为 PR（Pull Request）， MR 是我们进行代码 Review 的地方，它记录着代码的具体改动，参与者具体的讨论过程。好的 MR 应该做到以下几点：
- 单一：一个 MR 应该只解决一个单一的问题，无论是修复一个 bug，还是实现一个新 feature。Author 应该避免一个 MR 包含不同意图的代码改动。单一的 MR 能帮助 Reviewer 快速地了解代码改动的动机，能有针对性地进行 Review。 
- 短小：MR 应该尽量地小，比如一个 feature 引入了较多的改动，需要考虑是否可以拆成独立的几块实现，分开提 MR，比如接口定义、接口实现、逻辑对接等拆分开。 
- 详细: 这里说的详细是指 MR 应该尽可能地详细描述它的背景和动机，可以是在 MR 的描述中详细体现，也可以是连接到具体 issue 或 tapd 单中。需要达到的目的是，其他人翻开一个 MR 能知道当时做这个改动的背景以及动机。
#### Commit Message
之前翻看了不少现存的项目代码，看到不少的 Commit Message 写得比较简单，例如一连串的 "update", "fix"，从这些 Commit Message 中完全看不出做了什么改动，想想如果之后想要定位之前的某个改动，该从哪里下手。
目前 Commit Message 规范比较常见的有 [Angular 团队的规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)，并由此衍生出了 [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/)，可以参照此 Specification 约定 Commit Message 格式规范。

`<type>(<scope>): <subject><BLANK LINE><body><BLANK LINE><footer>`

大体分三行：
- 【标题行】 必填, 描述主要修改类型和内容。
- 【主题内容】 描述为什么修改, 做了什么样的修改, 以及这么做的思路等等。
- 【页脚注释】 放 Breaking Changes 或 Closed Issues。
其中 type 是 Commit 的类型，可以有以下取值：
- feat：新特性
- fix：修改 bug
- refactor：代码重构
- docs：文档更新
- style：代码格式修改
- test：测试用例修改
- chore：其他修改, 比如构建流程, 依赖管理

其中 scope 表示的是 Commit 影响的范围，比如 ui，utils，build 等，是一个可选内容。
其中 subject 是 Commit 的概述，body 是 Commit 的具体内容。
例如：
`fix: correct minor typos in codesee the issue for details on typos fixed.Refs #133`
Commit Message 可以在 git 中配置模板，这样可以在 vim 中展示出模板，另外可有工具帮助我们生成和约束 Commit Message，例如 commitizen/cz-cli，这里不再具体说明。

#### CI 通过
CI（Continuous Integration），持续集成可以帮助我们自动发现很多代码中的基本问题，在合适的静态代码检查（lint）配置和良好的单元测试覆盖下，CI 可以有效地提高代码的质量。很多人都低估了静态代码检查的能力，实际上现在常见语言的静态代码检查已经能帮助发现不少的 bug 和隐患。对于 Go 语言，可以配置 golangci-lint 来做代码检查，单元测试根据实际情况可以制定相应的标准，比如覆盖率 60%，其中关键的代码逻辑尽量全面覆盖。

提交代码 Review 前需要确保 CI 执行通过，这也是为了节省 Reviewer 的时间，能够通过自动化解决的事情，尽量不要让 Reviewer 来做，而 Reviewer 发现 CI 未过的 MR 也可以要求 Author 先解决 CI 问题。
#### Self-Review
我们一般代码 Review 都是找他人来进行 Review，其实负责任的 Author 在邀请他人来代码 Review 前也需要自己简单 Review 一遍，即 Self-Review。

Self-Review 的目的包括：
- 发现那些明显的疏忽，如代码 debug 过程中留下的不必要的痕迹，比如 fmt.Println(...)，不小心注释掉的代码。
- 之前被 Reviewer 多次提出过的问题。
- Commits 是否正常，在多人协作的情况下 MR 中否带入了不相关的 Commit。
- Commit Message 是否合适。Self-Review 是一个非常快速的过程，从我个人的经验，一般 1-2 分钟即可完成，所以推荐大家养成 Self-Review 的习惯。


## 该找谁来 Review
从目的出发，可以从以下几方面考虑 Reviewer：
- **提高代码质量**。所以首先应该找和代码改动紧密相关的研发人员参与 Review，比如一起开发某个功能，某个项目，或者一起参与了方案设计讨论并给出了有价值意见的研发。
- **获取意见**。找有相关经验的资深研发帮忙 Review，比如 Java 语言资深的研发、写过相同或类似系统/功能的研发。
- **形成共识**。如果涉及到不同团队或模块间的接口改动，或其他会影响其他人的改动，可以邀请相关团队或模块的接口人参与 Review，以对改动形成共识。
- **质量把关**。对于重要的代码库，可能会执行比较严格的质量把控，如果设置了必须的 Reviewer，这些 Reviewer 也会参与进来。
- **变动告知**。很多情况下一个代码库可能只有一个人维护，如果做了些比较特殊的变动，其他人很难发现。因此在做一些重要的但是理解起来不那么直接的地方的时候，最好告知一下相关的研发，以便他们能大概知道发生了什么。
## 都 Review 些什么
经常会有 Reviewer 拿到 MR 不知道该 Review 些什么，其实无论你参与对应项目的深入如何，都可以对代码进行 Review，也鼓励不同人从不同的深度、角度去帮助 Review。**代码 Review 没有固定的形式，它更像是一门艺术，唯一的提高办法就是实际参与进去**。

Review 的时候可以从以下几个方面入手：
**1.简单的 Review在 CI 通过的情况下，最简单的 Review 方式可能只需要这样：**
>Reviewer：在实际环境中都验证过了吗？ 
>Author：当然验证过了 
>Reviewer：LGTM(look good to me ,见[[常见提交缩写]])

这是一种提醒式的 Review。确认一句：是否在环境中验证过了，或者进一步把能想到的重要的验收点提出来确认一遍。即使是这种最简单的 Review 实际上也是有价值的，我们很难保证所有研发都会在提 MR 前实际在环境中验证自己所做的修改，也很难保证单元测试、e2e 测试能 Cover 住所有的情况，Reviewer 基本上也不可能都自己去环境上跑一遍。让 Author 去确认实际上就是提醒 Author 去确保改动至少是真实有效的，尤其是对一些已发布版本的 Bugfix，一定要提醒实际自测通过。

类似的提醒还包括：相关的文档（外部的）是否相应更新了、这个改动是否会有兼容性的问题、性能是否有影响。他们的本质就是提醒 Author 自己去思考他们可能遗漏的问题。

**2.常规的 Review**
代码 Review 一般都会从代码风格、变量命名、语法统一处入手，当然这些应该更多的借助于 CI 等自动化手段来保证，但是在相关流程还不是很完善的前提下还是有必要进行关注。

此外代码可读性、代码健壮性、代码可扩展性都是 Review 时关注的点。每一个关注的点都依赖于 Reviewer 的实际经验，这里只简单举几个例子：

**2.1 代码可读性** 
代码是写给人看的，因为没有不需要维护的代码，无论是 Author 自己后续维护代码，还是他人接手代码，能快速地理解代码逻辑是非常必要的。

代码 Review 的时候可以关注以下几点：
- **变量、方法的命名是否合适**，是否真实反映了他们的目的，这方面网上可以找到不少的资料说明。 
- **实现的逻辑是否已有现成的库可以替代**。如果有成熟的库可以使用，尽量不要自己去实现，因为可能会引入不必要的 bug。从我个人的角度，简洁（大白话就是代码少）是可读性一个很重要的指标。 
- 关于**注释**。代码注释不求多，而在于有效，以前也经历过代码注释要求至少达到 30% 以上的年代，现在看来过多依赖注释其实是对代码质量的不自信，好的代码应该尽量做到自解释。在实际过程中偶尔能看到代码逻辑实际已经清晰明了，但是用语句不怎么通的英文注释了一通，最后反而是看懂了代码才能理解注释到底说了啥。因此 Review 的时候，不必要的注释可以建议去掉。 
- **不好理解的地方要有恰当的注释**。代码中如果有特殊处理、特殊的常量、或者不符合一般用法的逻辑需要特别注释说明一下。 
- **代码的组织**。良好的代码应该有较好的封装以及层级，使得代码看起来清晰明了，比如 DAO 层、Service 层。

**2.2 代码健壮性** 
- 代码的改动是否会影响其他功能。
- 用户参数是否做和细致的校验。
- 有没有 Panic 的可能（针对 Go 的说法）。
- 是否会破坏 API 的兼容性。

**2.3 可扩展性**

当前的实现方式是否能兼容以后类似的扩展需求。比如在新增接口、API 或者调整参数以解决某一问题上，可以考虑是否后续会有其他类似情况发生。举几个例子：
- 假设我们需要定义一个 API 接口去获取一个用户的某些信息，例如联系方式等，我们定义的 API 就不能只返回这些信息，而是应该把用户相关的信息都返回。
- 假设我要定义一个参数，虽然当前定义成单个元素即可满足，例如 string, int，但是以后是否会涉及到多个元素的场景，是否定义成 []stirng, []int 是更优的。这里只是举了有限的一些例子，在实际 Review 过程中，Reviewer 可以根据自身的经验从各个角度提出优化的意见。

一般需要重点看看：
- 你看不懂或疑惑的地方。
- 打破你常规的地方。
- 复杂的地方。

## 如何进行

Review 过程中鼓励 Reviewer 大胆 Comment，有不理解的地方，或者觉得不合适的地方都直接表达出来，Author 对 MR 的 每个 Comment 也要做出反馈，无论是展开讨论还是简单的给个 OK 都是有效的反馈。