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
            - bp的最后一张地图保持left状态
        - 生涯赛要能保存状态(优先级靠后)
            - 玩家退出重进,要能回到上次的状态
            - 生涯赛界面的退出箭头就是暂时退出生涯赛
            - 解决思路
                - 保存进度
                    - 客户端有一个cm状态管理器,状态管理器里有一个state的变量负责记录cm当前的进度,在玩家退出的时候把这个状态(state变量)发送给服务器保存
                - 保存比赛记录
                    - 服务器根据需要保存当前的比赛记录(比如如果玩家处在淘汰赛阶段,只需要保存淘汰赛阶段的比赛结果就行了)
                - 恢复状态和记录
                    - 在玩家重进的时候,服务器下发保存的进度和记录,客户端根据下发的进度和记录恢复cm状态管理器
    - 创角
        - 创角名字为中文会一直提示错误
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

