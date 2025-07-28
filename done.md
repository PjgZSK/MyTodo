- 2025.07.28
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
        - add search path in "package.path" list 
- add auxiliary prefab funciton
    - when select a gameobject in prefab, you can select to print its path from root gameobject
    - upload to github
- 修改：
    1. 全甲（头盔+护甲），半甲（护甲），无甲切换
    2. 拆弹器/炸药包位置固定
- 需要增加的效果
    1. 闪光弹头像效果
    2. 掉血效果
    3. 死亡效果
    5. 右边选手头像缩放形变问题
        - 美术切的头像长宽比不一样
- MainBattle的自适应效果
- battle statistic panel
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



