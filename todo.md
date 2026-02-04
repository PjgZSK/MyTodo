- todo
    - bug
        - 配置bug
            <!-- - EsportsAthlete表里的quality有个值为0,代码不支持 -->
            - 下包炸弹倒计时进度条缺一个颜色资源
        - 程序bug
            - 前端
                <!-- - 下包ui状态缺少一个停止下包的接口 -->
                <!-- - 最后包炸了  要隐藏下地图上的C4 -->(换3d了)
                <!-- - 下包进度条扩散时没有光标 -->
                <!-- - 下包进度条在被调用停止接口时, 会主动显示 -->
                - 爆炸死亡警示在player血量变化时没有触发检查
                    - 等有时间再改吧
                - c4伤害示警用的伤害公式固定是dust2;不同地图伤害公式应该不一样
                    - 等后面其他3d地图实装时再改
                <!-- - 战斗主界面y轴翻转选手面板导致点击区域异常
                    - 这个等镜头跟随正式优化的时候再改,现在镜头跟随的不确定性太多 -->
                <!-- - fpsmainuicontroller.lua里的ondestroy拼错了 -->
                - ~~战斗时钳子图标会莫名其妙的消失~~
                - ~~换边重置护甲~~
            - 后端
                - ~~后端给的背包里出现的手雷数量有概率大于4(汉钊在改)~~
            - 前后端同步问题
                - 当ct方拆弹胜利,前端发送拆弹胜利消息给后端,后端移除所有已死亡的队员的物品,并向前端发送刷新物品的消息,这个时候战斗表现有概率还没结束,会导致前端ui显示问题
                - ~~战术背包同步有bug~~
        - 美术bug
            - 公告字体颜色
    - optimization
        - 战斗主界面实例化选手面板的时候每次都forceUpdateCanvases, 可以优化
        - 下包斜向进度条换正向进度条
        - ~~购买动画优化(总时间5s, 道具购买/护甲/钳子购买增加间隔, 道具购买0.5s间隔,经济数字跳动后再播放购买动画)~~
    - function
        - 战斗镜头移动 
            - 设计思路
                - 事件的处理和事件的触发分开来: 发事件在战斗逻辑,事件的处理是镜头移动逻辑
            - 实现分层
                - 事件定义(CameraFollowEvent)
                    <!-- - id, name, priority, canInterrupt, group, followedGOs
                    - CameraFollowEventGroup(Enum, 事件组)
                        - PlayerFollow  (玩家跟随)
                        - ThrowableFollow  (可抛掷道具跟随)
                        - BattleFollow  (战局跟随)
                        - C4Follow  (c4跟随) -->
                - 发事件lua接口类(CameraFollowEventTrigger)
                    <!-- - 触发逻辑(核心逻辑)
                         - TriggerFollowEvent(根据传进来的事件组, 优先级和go列表生成并触发事件) 
                            1. CameraFollowEventHandler实例检查,为空则不触发事件,逻辑结束
                            1. 检查go列表, 不通过则不触发事件,逻辑结束
                            2. 生成事件: 事件id, 事件名(group+id), 事件优先级,事件组,follow列表
                            3. 处理事件(使用CameraFollowEventHandler接口), 并返回结果 -->
                    <!-- - 成员变量
                        - CameraFollowEventHanlder实例
                        - 事件自增id -->
                    <!-- - 外部接口
                        - Init(初始化, 传入Camera)
                            1. 给camera增加CameraFollow,并缓存camerafollow
                            2. 通过camerafollow生成CameraFollowEventHandler实例, 并赋值给成员变量 -->
                - 事件处理(CameraFollowEventHandler)
                    <!-- - 触发逻辑(核心逻辑)
                        - HandleFollowEvent(处理跟随事件)
                            1. 检查CameraFollow, 为空则无法触发事件,逻辑结束
                            2. 事件有效性检查(实参判空,跟随go列表使用==判空),不通过则不触发事件,逻辑结束
                            3. 获取实参事件的事件组,事件组是否有次数限制,如果有次数限制并且已到达上限,不触发事件,逻辑结束
                            4. 获取当前在跑的跟随事件(从CameraFollow里获取), 如果存在事件并且当前事件的优先级比实参事件高,那么不触发跟随事件,逻辑结束
                            5. 将传进来的事件设为当前事件(在CameraFollow里设置)
                            6. 实参事件的触发次数加1 -->
                    <!-- - 成员变量
                        - <事件组, 触发次数>字典
                        - CameraFollow实例 -->
                    <!-- - 外部接口
                        - CameraFollow实例获取/设置
                        - 清除次数字典 -->
                - 镜头移动逻辑(CameraFollow)
                    <!-- - z轴运动
                        - 缓存值: cameraOriginZ
                        - z轴运动参数变量: originZ, targetZ, durationZ, zElapsed
                        - 触发
                            - 当设置新事件(如果新事件有z轴变化): originZ设置为相机当前的位置, targetZ设置为跟随事件的targetZ 
                            - 当事件被取消/无效(如果旧事件有z轴变化): originZ设置为相机当前的位置, targetZ设置为 cameraOriginZ, 其他z轴运动参数和事件保持一致 -->
                    <!-- - 基础跟随事件
                        - 当当前跟随事件结束或者失效时,如果有基础跟随事件,回到基础跟随事件 -->
                    <!-- - 移动逻辑(核心逻辑)
                        - update(处理移动逻辑)
                            1. 计算targetPos(多个跟随目标计算或者单个跟随目标计算)
                            2. 判断camara pos是否到达了targetPos, 如果到达了,本次逻辑结束;没有的话进入下一步
                            2. 计算从camera pos指向targetPos的向量v,v.normalized乘以速度和v.magnitude的小者就是本次update移动的距离
                            3. camera的位置加上移动的距离 -->
                    <!-- - field 
                        - currentCamera: 当前控制的camera
                        - speed: 镜头移动速度
                        - currentFollowEvent: 当前跟随事件
                        - previousFollowEvent: 前一个跟随事件的缓存
                        - followEnabled: 是否允许跟随 -->
                    <!-- - property
                        - isFollowEnabled -->
                    <!-- - 接口
                        - SetFollowEvent: 设置跟随事件
                        - GetCurrentEvent: 获取当前的跟随事件 -->
                    - 事件(Event)
                        - OnFollowFinished: 当跟随事件顺利结束
                        - OnFollowInterrupted: 当跟随事件被中断了
                        - OnFollowResumed: 当跟随事件恢复跟随
                        - OnFollowStoped: 当跟随事件停止跟随
            - 工作量
                <!-- - 发事件(触发时机) 1.5天: 实际用了2-3天 -->
                <!-- - 移动逻辑 + 事件处理 1天: 实际用了3天 -->
        - 战斗镜头移动优化
            - 重要
                <!-- - 增加基础事件 -->
                <!-- - 增加俯冲/拉升(z轴值), 俯冲/拉升速度 -->
                - 俯冲/拉升运动优化(和平移运动保持一致)
                - 增加俯冲/拉升运动模糊效果(备用)
            - 玩家跟随触发时机修改
        - 战斗镜头todo
            - feature
                - 延迟镜头数据恢复 
                - 跟随增加抖动检查(anti-shake): 这个比较难做,放在后面做
                - 增加朝向: 不一定会做,先等战斗那边实现
            - function
                <!-- - c4爆炸镜头跟随 -->
                <!-- - 跟踪投掷道具还没做: 现在投掷实在ui上做的,等换完3d在做 -->
            - optimization
                - 镜头锁定下包: 现在锁定的是人;优先级太低,被跟踪下包覆盖了
                <!-- - camfun0: 镜头有两种视角,镜头初始位置只有一个, 需要再添加一个初始位置 -->
                <!-- - 战斗太快,全是露头就秒 -->
            - bug
                <!-- - 镜头抖动bug(有视频) -->
                <!-- - 战局结束判定有错误 -->
            <!-- - c4爆炸后c4镜头跟随事件应该结束,且z轴返回正常值 -->(bug)
            <!-- - 计算斜视角的camera位置: 应该可以用缓存的matrix做一个数学优化 -->(做不了优化,这个matrix对应的坐标轴会变动)
            <!-- - bug: 现在下完包,跟随带包人员事件没取消 -->
		    <!-- - 现在武器类型(手枪/狙击步枪等)配表里只有服务端能获取,后面改 -->
            <!-- - 地编ctburst的AB区域属性配置有问题,需要地编那边修改 -->
        - 邮件后端 (接收邮件,更新邮件,已读功能,领取奖励功能)
        ~~- 战斗场景道具交互~~ (废弃,给别人做了)
        - 生涯赛接入战斗,创角和后端
            <!-- - 进入战斗会出现两个一样的队伍, 测试模式进入是正常的 -->
            <!-- - 进入战斗结束生涯赛没出生涯赛结算界面 -->
            <!-- - 比赛奖励结束后面的 段位结算  先屏蔽下 -->
            - 生涯赛bug
                <!-- - 未打最后一场比赛直接走结果流程了  -->(是模拟数据的问题, 已经做了多处的log输出)
                <!-- - bo3结果显示界面: 在对决概览加一个比分显示 -->
                <!-- - map bp 界面bp显示问题 -->
            - 接入战斗
                <!-- 1. 发战斗消息(mapid) -->
                <!-- 2. 战斗结束获取结果
                    - 增加一个立即结束战斗的gm命令
                    - 获取快速战斗的的结果 -->
                3. 同步cm管理器进度
            - 接入创角
                <!-- - 创角结束后直接进入生涯赛 -->
            - 接入后端
                <!-- - 接入普通后端 -->
                <!-- - 接入创角后生涯赛后端(直接进入淘汰赛第二轮bo3最后一场)
                    - 实现跳过快速战斗页面
                    - 测试progress code逻辑: code之前生效,code之后不生效  -->
                <!-- - 去除选手从后端传进来的选手信息, 改为从表格读取 -->
            - 服务器bug
                <!-- - 在瑞士轮: 服务器返回的比分数据和快速战斗返回的比分数据不一致(偶发) -->
                <!-- - 在瑞士轮: 服务器返回比分数据时没有返回下一轮的队伍对决数据(偶发) -->
                <!-- - 在瑞士轮: 前端进入战斗后发送requestSingleBattleResult消息给服务器,返回错误(必现) -->
        - 生涯赛进度保存
            <!-- - 根据服务端返回的checkpoint恢复相应的进度和数据 -->
    - 别人的bug
        <!-- - 快速战斗返回的teamID有一个是0 -->(已反馈)
        <!-- - 快速战斗的返回结果缺少: 上下半场分数, 地图id -->(已反馈)
    - 需要他人配合
        <!-- - 镜头事件:镜头跟随带包玩家进入近包区域,近包区域从地编配置获取的,现在近包区域的地编数据不对 -->
    - alert
        - scene change: special effect
    - 工具类
        <!-- - 去除cm里所有的print_dump -->
        <!-- - 增加print_dump的替代品
            - print_dump没啥大问题, 崩溃的原因是protobuf解码的某些消息table只记录了消息名和消息二进制数据,解码行为在metatable里,在c#端print二进制数据时抛出异常 -->(对未解码的table挨个使用protobuf的decode解码)
        - 在运行的场景中寻找特定instance id的component或者gameobject
    - 改版
        - 战斗主界面ui改版
            - todo-list
                - 玩家信息面板改版
                    <!-- - 款式大改
                        - t/ct面板换色
                        - 同步血条透明度设置为0 -->
                    <!-- - 增加卡牌
                        - 卡牌死亡效果
                            - 等卡牌改完版再做 -->
                    <!-- - kda改版 -->
                    <!-- - 出场动画改版 -->
                <!-- - 底部按钮改版
                    - 战报下移 -->
                    - 炸死警告(应该是改3d后的代码问题)
                        - 没资源,等资源到再改
            - changes
                - 玩家名字,玩家头像隐藏
                - 同步血条透明度为0(无资源)
        - unity web 打包
            - WebGL打包工具没有,需要找一下资源
                - 切换到webgl后打包,运行问题调试
            - 现在在windows平台跑打包流程
                - 需要单独打包assetbundle
                - 代码在运行时如何获取assetbundle

- bug info
NetWork, ReceiveCallback Error:{socket disconnect}
-----NetWork, Close------
OzGameClient, Update Error. (String conversion error: Illegal byte sequence encounted in the input.)

- unity build
    - variant

- 专项
    - 场景画面捕捉处理
    - 字符编码理解
        - lua 字符编码理解
    - slua
        - unity plugin-in (dll, bundle)
        - 如何与C#交互
    <!-- - lua metatable理解
        - event - metamethod -->

slua
path:
.\?.lua
C:\Program Files\Unity 2021.3.22f1\Editor\lua\?.lua
C:\Program Files\Unity 2021.3.22f1\Editor\lua\?\init.lua
C:\Program Files\Unity 2021.3.22f1\Editor\?.lua
C:\Program Files\Unity 2021.3.22f1\Editor\?\init.lua
d:\Program Files (x86)\Lua\5.1\lua\?.luac

cpath:
.\?.dll
C:\Program Files\Unity 2021.3.22f1\Editor\?.dll
C:\Program Files\Unity 2021.3.22f1\Editor\loadall.dll

- unity 行为偏差
    - UI
        - layout组件即使调用force update canvases接口, 在某些情况下也不会立即布局

- todo
    - ~~cm state: add battle data logic~~
    - ~~create role: set role name Chinese character bug fix~~
    - ~~edit team init code~~
    - ~~select battle init code~~
    - ~~fix main battle right team member change ani error~~
    - ~~optimate mainbattle ui code~~
    - ~~c4 ui~~
        - ~~初始状态(ui显隐)~~
        - ~~炸弹倒计时不同时间不同色彩~~
        - ~~炸弹倒计时两个闪烁动画~~
        - ~~拆弹测试~~
            - ~~拆弹条时间动态化~~
        - ~~动画 (拆弹钳呼吸动画*,*炸弹拆除/爆炸状态出现缩放动画)~~
        - ~~闪光条错位问题~~
        - ~~拆弹失败: 蓝色进度条，在爆炸前的最后1s时，会停止增长并晃动0.3s，然后消失~~
    - ~~c4相关~~
        - ~~c4掉落红圈动画提示 (*需要知道c4状态, c4掉落位置*) (这个需要消息)~~
        - ~~c4下包后提示~~
            - ~~当警家进入爆炸范围会有提示, 现在只有警家会被炸死时有提示 (*需要知道c4下包位置, 警家位置, 爆炸范围*) (这个需要增加战斗逻辑)~~
                - ~~ui上有提示~~
                - ~~小头像有提示~~
            - ~~以包体为中心扩散波纹提示/包体呼吸提示(*需要知道包体位置, 包体状态*) (先做这个, 这个不需要消息)~~
        - ~~下包/拆包声音范围提示(*需要知道下包/拆包位置, 声音范围*) (在做这个 这个也不需要消息)~~

- todo
    - server doc
    - client ui init/reset

- todo
    - thread 
    - update/fixupdate/lateupdate
    - socket: tcp

- todo
    - 现在的ai幻觉是真tm的多,还是得自己看官方文档

- todo
    - server
        - exit data save
        - database
            - cache: read/write, save

- todo
    - lua metatable
    - dictionary
    - lua/c call each other
    - slua bind c# class
    - assetbundle load
    - c# assembly/reflection
    - Esport Manager 2026
        - 签名功能
        - 点图
            - 线
            - 动画
    - net game pay interface

- todo
    - 图形算法
        - shake算法
        - blur算法(shader)

- todo
    - videoplayer.onSeekCompleted 回调的时候videoplayer.time没有刷新
    - high light timeline
        - *在c#层的事件原型写好,在lua层的事件触发代码写好 - (暂时做了个简略的)*
        - timeline video track优化
            - *时间优化,默认时间就是视频长度,不要后面手动赋值 - (做了个按钮)*
            - game视图的刷新, 把videoplayercontroller的刷新单独拿出来
        - *获取PlaybaleDirector,手动设置playbale(timeline asset),给playbal里的Game Event Track手动设置GameEventReceiver组件*
    - slua
        - *slua在导入UntiyEngine.Timeline.TimelineAsset时报错,像是导入泛型内容报错 - (不用导入Timeline空间,直接在c#层实现,lua调用c#层的东西就可以了)*
- todo 
    - 生涯赛
        - MsgReturnBattleResult
            - 接收并处理这个服务器消息
        - *瑞士轮 红蓝箭头一直显示*
        - bp界面 
            - *bo1 bp 没被选中的地图是ban状态*
            - *对局概览 程序不要自动设置横向滑动框的滑动位置,让用户自己滑动*
            - *ban/pick 按钮,两个按钮都要出现,不能按的按钮置灰*
            - *pick的地图前移*
            - *bp的最后一张地图保持left状态*
        - *生涯赛要能保存状态(优先级靠后)*
            - 玩家退出重进,要能回到上次的状态
            - *生涯赛界面的退出箭头就是暂时退出生涯赛*
            - 解决思路
                - 保存进度
                    - 客户端有一个cm状态管理器,状态管理器里有一个state的变量负责记录cm当前的进度,在玩家退出的时候把这个状态(state变量)发送给服务器保存
                - 保存比赛记录
                    - 服务器根据需要保存当前的比赛记录(比如如果玩家处在淘汰赛阶段,只需要保存淘汰赛阶段的比赛结果就行了)
                - 恢复状态和记录
                    - 在玩家重进的时候,服务器下发保存的进度和记录,客户端根据下发的进度和记录恢复cm状态管理器
    - 创角
        - *创角名字为中文会一直提示错误 - (客户端名字检测使用string.match的%w导致的, 把这条检测规则去掉了)*
        - 创角现在没有头像和头像框资源
    - 选手仓库
        - *现在只做了金色卡的ui,代码没做其他等级卡的资源替换*

- todo
    - a strict weak order, a total order

- got
    - Canvas.ForceUpdateCanvases() -- for calculate the position

- todo
    - asset bundle
        - build
            - how to build assetbundles 
        - load
            - load asset from asset bundle
                - LoadAsset(assetName, type) 
                - LoadAssetWithSubAssets(assetName, type)
            - load asset bundle
                - LoadFromMemory(bytes)
                - LoadFromFile(path)
    - UnityEngine.SystemInfo
    - UnityEngine.Application
    - map

- trs
    - translation, rotation, scale (matrix)
    - t = (tx, ty, tz), q = (qx, qy, qz, qw), s = (sx, sy, sz)
    - T(t)*R(q)*S(s)* v

- space
    - model, local, world 

- unity
    - Graphic
        - void OnPopulateMesh(VertexHelper vh)
    - shader, vertex, texture
    - ui render
        - Graphic
        - CanvasRenderer
    - normal render
        - MeshRenderer
        - Mesh
        - Material 
