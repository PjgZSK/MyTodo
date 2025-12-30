- todo
     bug
        - 配置bug
            - EsportsAthlete表里的quality有个值为0,代码不支持
            - 下包炸弹倒计时进度条缺一个颜色资源
        - 程序bug
            - 前端
                - 下包ui状态缺少一个停止下包的接口
                - 最后包炸了  要隐藏下地图上的C4
                - 下包进度条扩散时没有光标
                - 下包进度条在被调用停止接口时, 会主动显示
                - 爆炸死亡警示在player血量变化时没有触发检查
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
        - 下包斜向进度条换正向进度条
        - ~~购买动画优化(总时间5s, 道具购买/护甲/钳子购买增加间隔, 道具购买0.5s间隔,经济数字跳动后再播放购买动画)~~
    - function
        - 战斗镜头移动 
            - 设计思路
                - 事件和事件的触发分开来: 发事件在战斗逻辑,事件的处理是镜头移动逻辑
            - 实现分层
                - 事件定义(CameraFollowEvent)
                    - id, name, priority, canInterrupt, maxTriggerCount, followedGOs
                - 发事件lua接口类(CameraFollowEventTrigger)
                - 事件处理(CameraFollowEventHandler)
                - 镜头移动逻辑(CameraFollow)
                    - 在update里处理移动逻辑
                        1. 计算target (多个跟随目标计算或者单个跟随目标计算)
                        2. 计算camera和target的方向向量,乘以速度就是本次update移动的距离
                        3. camera的位置加上移动的距离
            - 工作量
                - 发事件(触发时机) 1.5天
                - 移动逻辑 + 事件处理 1天
                
        - 邮件后端 (接收邮件,更新邮件,已读功能,领取奖励功能)
        ~~- 战斗场景道具交互~~ (废弃,给别人做了)
    - alert
        - scene change: special effect

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
