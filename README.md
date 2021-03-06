# 前言
本计算器基于dawnclass所写、韩械汉化并加入国服特色的一键史诗搭配计算器，在其上进行了一些性能优化、扩展功能、易用性改动及bugfix。

目前实现了搜索百变怪、升级工作服、跨界、多武器搭配等功能，并对启动和搜索性能做了一定优化，在操作易用性上加了一些调整。具体改动内容可见下文

# 网盘链接（更新于2020/4/28)
链接:https://pan.baidu.com/s/1-I8pMK6_yPH5qU4SWNMVog
提取码:238m

# 使用简介
## 第一批功能说明
![第一批功能说明](change_log_images/功能说明1.png)

## 第二批功能说明
![第二批功能说明](change_log_images/功能说明2.png)

# v3.2.8.2 2020.4.27
# bugfix
1. 经贴吧网友@王八老二 反馈，魔改后版本的神话装备图片没有原来那么闪，并给出了对比图，经排查发现作者在images目录中神话装备会有三个文件，
    分别是以n.gif/n.png/f.png结尾，对应超闪耀，点亮、暗淡三种，实际上原版神话装备只用了1和3，但是优化加载图片时是改为文件夹中有多少个图片就按需加载，
    由于前俩的key都是装备编码，而且超闪亮的是先加载的，所以会被普通闪给覆盖。目前已修复

# v3.2.8.1 2020.4.26
## bugfix:
1. 经贴吧网友@反正有大好时光 @Usingaa @丿俊哥丶，新版本中自定义功能出现问题，经排查定位到是因为多增加了一行列名含义后，保存自定义结构的代码中写死的坐标会找到错误的定位，已修复
2. 经贴吧网友@罗衣 @git，奶系选择百变怪后计算有时候会卡住，经排查发现是因为写法与输出那边不太一样，导致相同的处理办法直接拷贝过来会有问题，已修复
3. 发现新加的套装概览区域不会随查看详细信息和更改奶量标准变动，查了下，发现是做的时候太忙了，忘记了。。。已修复

## 开发工具
1. 新增脚本_export_excel_to_txt.py，用于将data.xlsx与preset.xlsx导出为文本格式，方便进行版本对比，避免误改动到数据文件

# v3.2.8 2020.4.26
## 新增功能
1. 增加装备跨界功能，因计数部分太复杂，不再维护
2. 增加选择多个武器的功能，因计数部分太复杂，不再维护
3. 增加区域展示更加方便阅读的当前搭配，如天擎3水果2这种
4. 结果栏中增加查看名称按钮，点击即可查看当前搭配的各部位装备名称，不用再看图认出装备了
5. 增加当前用时，方便看花了多少时间了
6. 在显示当前组合数的地方，在前面加上装备的收集进度
7. 把结果界面的层级不设置最顶端，这样出结果后切换其他角色时不需要额外折腾
8. 增加多选列表组件
9. 增加与贝奇邂逅称号（因为我的奶妈在用这个<_<)
10. 汉化data.xlsx
11. Data.xlsx增加首行，表示各列的中文含义，同时冻结第一行与前两列，这样编辑具体数值时容易定位

## bugfix
1. 根据b站网友@面粉馅包子 的反馈，之前版本的百变怪功能在特定装备级下，百变怪没有选择最合适的转换装备。
    经排查发现，是因为作者根据搭配套装数目预估价值量的函数f=sum([floor(x * 0.7) for x in set_val.values()]) + god,
    与其实际精确搭配的奶量伤害的函数g，不能满足在任意区间内都具有相同的函数单调性（递增或递减），
    从而可能或出现对于某些搭配x1,x2，f（x1）<f(x2)，而g(x1)>g（x2），在作者的原剪枝流程中，会导致x1搭配被视为无效搭配。

    解决办法：
    1. （非常耗时）计算每个搭配的精确伤害奶量来找到在该伤害奶量算法下的精确解
    2. （实现难度高）找到更合适的价值评估函数，在满足与该伤害奶量算法相同增减性的同时，且计算代价小
    3. （自行选择精确度还是速度）仍使用当前算法，忍受为了计算速度而可能错过一些精确解的bug，
        与此同时，增加超慢速选项，允许用户选择使用花费更多时间来得到更精确的解。
        但是这种情况下，由于无法进行提前剪枝，需要计算所有组合，其时间复杂度将是O(n1*n2*...n10*n11)，
        其中ni为部位i的可选装备数目，在各部位都有一定数目的情况下，用时将难以想象

    出于个人精力有限，目前暂时选择方案3，在速度选项再增加一个超慢速方案，当选择该方案时，所有剪枝与预判都会停用，对比装备的唯一标准
    就是作者实际计算伤害奶量的算法结果

    排查过程截图详见网盘或贴吧帖子
2. 经韩械反馈，属强多出来了80点，应该是之前他在data.xlsx中补正的数值我这边重新计算了一遍- -，暂时先在国服特色的函数中减去一定数值，保证两边属强计算结果一样
3. 经贴吧网友@飞花逐月反馈，在五个散件防具、两首饰、两特殊这种类似的情况下，原版只会给出唯一的史诗组成的搭配，而略过了实际战力更高的传说普雷搭配。
   为了计算结果更精确，永远将100传说防具、普雷首饰、普雷特殊加入备选方案
4. 经贴吧网友@萌萌的汉堡包反馈，在特定组合下选择百变怪计算奶量时程序会无法计算，提示时间很长，经排查，是之前遇到过的一个bug，只是修复了输出职业的那边，奶这边没有改。
   具体原因：目前select是默认初始化时将tg{1101-3336}[0,1]范围的key对应的值都设为0，而百变怪会根据select的值为0来筛选出未选择的集合，
   因此在获取装备属性切片时，如果因为这件装备时间不存在，导致切片为None而空指针访问程序无法正常执行。这种情况，直接判断空指针返回就可以了


# v3.2.7.2 2020.4.23
1. 经韩械反馈，原先版本已经实现了至尊宠物所带来的5%最终伤害，一级1-50级技能+1，只不过实现方式是在Data.xlsx中所有武器的这两个属性中分别加了相应值，
    因此，3.2.7版本的最终伤害会高5%，1-50级技能等级会多出1点来
    根据他的建议，目前从Data.xlsx中移除了这两个加成，并在那边的另一个1-50级技能+1（暂不明确来源）加到国服特色的代码中

# v3.2.7.1 2020.4.23
1. 经贴吧吧友@给QQ一巴掌 提示，发现在调整国服特色数值实现的过程中，把原有的初始属强给漏了，这里给补回来

# v3.2.7 2020.4.21
1. 增加支持更多春节宠物、称号和国庆称号，并尽可能将每个词条都考虑进来
2. 修复夜语黑瞳武器55技攻变成55速度的问题
3. 修复update_count和update_count2在tkinter.mainloop启动前就调用tkinter相关组件而导致计算倒计时的功能挂掉的bug
4. 增加每个词条的枚举，而不是使用magic number来访问- -
5. 汉化data中的部分装备名称

# v3.2.6 2020.4.20
1. 百变怪的备选集合中排除升级得到的工作服、智慧产物
2. 新增可配置最多升级n件工作服的功能
3. 目前select是默认初始化时将tg{1101-3336}[0,1]范围的key对应的值都设为0，而百变怪会根据select的值为0来筛选出未选择的集合，因此在这里如果为None，将其过滤掉，避免程序不运行
    3.1 bug来源：@我就水亿贴 贴吧网友的反馈
4. ui细节调整
5. 增加推荐使用步骤及免责声明
6. 增加一件发布脚本
7. 增加加入升级工作服功能后，剪枝时的精确计数，后因性能问题回滚


# v3.2.5 2020.4.20
1. 修复奶系职业切换排序标准时右侧搭配不刷新的问题
2. 输出结果界面额外汉化
3. 输出界面排版优化

# v3.2.4 2020.4.19
1. 将保存结果的结构体由列表改为最小堆（存储O(n)，排行O(1)），原先的排行消耗太大(存储O(n*logn),排行O(n^2))，尤其是在点亮全部装备的时候尤为显著

# v3.2.3 2019.4.19
1. 修复状态栏中剪枝时未计入后续组合中的百变怪的组合而导致算的增加的无效组合数低于实际剪枝数目
2. 修复下方统计总数时因将神话装备算入百变怪备选集合而导致总数与上方计得数字不一致的问题
3. 添加无提前剪枝和最宽泛上限的剪枝方案
3.1 测试数据无提前剪枝用时123s
3.2 测试数据最宽泛上限剪枝用时9s
3.2.1 每个剩余装备按1点增益计算，若目前序列尚无神话，且后续序列存在神话，则额外加一点
3.2 测试数据任意现有装备下新增若干个装备剪枝用时8.99s
3.2.2 当前已有装备不受限制，预先计算任意新增k个装备所能得到的最大增益，若目前序列尚无神话，且后续序列存在神话，则额外加一点

# v3.2.2 2019.4.19
由于下周或者下下周，基本上大部分人的百变怪都做出来了，那时候大家可能会烦恼如何使用计算器来看这个百变怪该选啥，为了不至于一个个去尝试，因此增加了下述功能
1. 将itertools.product改为自行实现，方便在中间过程进行剪枝
2. 增加百变怪功能，当选择拥有百变怪选项时，计算搭配时会将百变怪考虑进来

# v3.2.1 2019.4.14
1. 存档读档功能增加支持选择速度、武器、职业选择、输出时间、称号选择、宠物选择、冷却补正等信息，无需每次读档后再手动设定后才能进行计算，现在读档后可以直接点计算
2. 启动时自动读取首个存档，无需再自己去点一次读档才能去进行其余操作
3. 性能优化：
    3.1. 调整读取装备图片的流程，通过遍历文件夹来实现加载所需的图片，而不是穷举所有可能，最后导致启动时要卡顿两秒，根据测试，目前读取图片共使用0:00:01.780298秒, 总共尝试加载6749个， 有效的加载为351个
    3.2. 国内环境无法访问他那个更新版本的google网盘地址，所以直接移除相关代码
4. 干掉了在总组合数目超过5亿种时不允许玩家运行的限制，同时将遍历组合的流程由先生产所有改为使用生成器去遍历，使得在组合数非常大时内存也不会溢出，经测试即时点亮所有图标，新版本也能够正常计算
5. 增加计算预计剩余时间的功能，在计算栏中将初始化右侧已显示的当前总组合数移除，改为预计剩余计算时间，这样可以更容易知道进度
6. 初始状态设为停止状态，在成功开始计算时设为计算状态，结束计算时和按停止时设为停止状态
7. 保证职业列表按照excel表中的行顺序排列
8. 未选择职业或武器直接点计算时弹出错误框，使得更加易用
