- custom find optimization
    1. list all find results which be in same file below the file, dont segment them 
    2. indent line number
    - ~~很难在原有的结构上优化，以下是一些分析~~
    - sed是一个相对完整的工具，没有设计二次开发的接口（从shell的输入输出的角度上，sed返回的是ExitCode）
    - 因此，使用sed能很快的出功能，但是对sed不匹配的功能或者小修改，基本上没有实现或者优化的可能
    - sed太复杂，功能也太强大，没有考虑和其他命令配合，这种我个人把它比作killer级命令，强大但是在调用链的顶端，不太可能把它作为中间命令来调用
    - 与此相反的是find命令，相对简单，这种命令就是次级命令，可以作为复杂命令链的中间命令来使用，它本身对输出的支持也很友好

- lua
    1. require: search path? (config.filename - Assets/Config/config/filename.lua)

- csgo project
    - ~~add auxiliary prefab funciton~~
        - ~~when select a gameobject in prefab, you can select to print its path from root gameobject~~
        - ~~upload to github~~
    - mainBattle UI(core battle)
        - building layer animation
        - skill
        - passive skill
    - core battle state machine 
    - config design, config deploy, config load 
    - core battle proxy design
        - server calculate the result of battle and send all data of match to client
        - client parser match data and show
    - todo
        - guide
    - battle statistic panel
        - event
            - kill event
                - data
                    - killer player
                    - killed player
                    - weapon
                - behaviour
                    - add killer kill data
                    - add killer money
                    - add team money of killer
                    - add killed death data
            - round finish event
                - data
                    - win team, lose team
                - behaviour
                    - add player/team money
                    - try add fail team's compensation 
        - change
            - round change
                - fail compensation 
                - money
            - player change
                - kill
                - money
                - death
        - state
            - player status
                - name
                - kd
                - avatar
                - money
                - role 
            - team status
                - name
                - badge
                - money
                - fail compensation 

- net game workflow
    - request init data 
    - use init data to init ui
    - click request / server request
        - client listen and refresh 

- Day1
    - main battle proxy design
        - player property
            - dynamic
                - hp
                - mood
                - main weapon
                - knife
                - grenade status
                - armor
                - skill status
            - constant
                - name
                - avatar
        - team property
            - name
            - badge
            - win/lose
            - win round 
        - match property
            - max round
            - current round
            - match count down
        - status
            - player status
                - player property 
            - team status 
                - team property
        - status change
            - player status change
                - player dynamic property
    - single match design
        - new round
        - game start

- Day2
    - optimatize main battle prefab 
    - add battle statistic panel btn callback

- oz framework 
    - init proxy
        - `CommonInitProxyCommand.lua` 
    - init command
        - `CommonInitModulesCommand.lua`
    - server send msg
        - q
            - what if error occur in net connecting
                - re-connect until max re-connect times
    - `GlobalData.lua`
        - `userdata`
            - just temporary data
            - init from server
    - lua util
        - instantiate game object
            - CommonUtil.InstantiateGameObject(assetbundleName, assetName, callback(object))
                - call AssetBundleManager.cs -> call ResourceManager.cs -> call ResourceLoader.cs
        - spawn game object 
            - CommonUtil.Spawn(assetbundleName, assetName, callback(object, data), data) 
                - call ObjectPool.cs
                    - instantiation of asset exist
                        - ab name + asset name -> id (crc32)
                        - get object, set layer and return object
                    - dont exist
                        - call AssetBundleManager.cs -> instantiate a object, set layer and return object


修改：
<!-- 1. 全甲（头盔+护甲），半甲（护甲），无甲切换 -->
<!-- 2. 拆弹器/炸药包位置固定 -->

需要增加的效果
<!-- 1. 闪光弹头像效果 -->
<!-- 2. 掉血效果 -->
<!-- 3. 死亡效果 -->
4. 楼层切换效果
5. 右边选手头像缩放形变问题
    - 美术切的头像长宽比不一样

<!-- MainBattle的自适应效果 -->

- todo
    - lua closure
        - non-local variable
    - google proto
    - game state machine
        - reset state
        - step state
    - ui -> a heap of data drived by data
    - asset bundle
        - variant
        - how to build assetbundles, how to get path
        - load asset from asset bundle
            - LoadAsset(assetName, type) 
            - LoadAssetWithSubAssets(assetName, type)
        - load asset bundle
            - LoadFromMemory(bytes)
            - LoadFromFile(path)
    - map

- unity
    - canvas renderer: what is it?
    - asset workflow
    - push this rep to my github
    - canvas
        - reference resolution
        - render mode
    - canvas scaler
        - ui scale mode
    - UI
        - scale
            - canvas scaler: ui scale mode

