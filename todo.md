- weekly todo
    - main battle ui
        <!-- - skill switch(ui and code) -->
        <!-- - round data show(ui and code) -->
        - optimization
            <!-- - grenades auto layout -->
            - mood bar
                - black seg
                <!-- - margin -->
            <!-- - font substitute -->
            - flashbang color
        - trouble
            - mood multiple progress
    - main ui prefab and code(wait for UI)
    - trouble
        - progress bar end shining
        - skill cooldown bar white edge
        - back end
    - custom icon set (resource load)

- weekly recode
    - Mon
        - weapon/skill switch animation code
    - Tue
        - weapon/skill and round statistic ui 
        - round statistic show animation code 
    - Wed
        - animtion code optimization
        - mood multiple progress 
        - progress seg/end shining

- todo
    - a strict weak order, a total order
    - data command, proxy template 
    - lua dofile <-> gameobject
    - doc 
        - client send message
        - msgresponce name error
    - lua
        - class:function func(...)
            - func is a global variable ?
    - C# reflection
    - tailed progress 
        - editor
    - list view
        - visual effect
        - content size -> min/max pos, index
    - python
        - re.sub

- got
    - Canvas.ForceUpdateCanvases() -- for calculate the position of player status switch motion
    - unity guide 
        - gameobject, component, recttransform(anchor, pivot)
        - mvc test panel
        - ui auto layout
    - layout group
    - scroll view
        - content rect
        - content layout
    - cooperate 
    - think interrupt 
    - code review regularly 
    - ease
        - the key of ease is change speed at a instant time
        - ease in
            - start slow, end fast
        - ease out
            - start fase, end slow
            - when you want show something, the low change speed of end give serious feeling
    - Mask sprite selection
        - unity built-in sprite : "UIMask"
            - UIMask is a "void" sprite
                - "void" sprite
                    - rule one:  the of rgb contribution "void" sprite is very small
            - when your front graphic is exist, if Mask is single(no front graphic), the UIMask has a very tint color 
        - no sprite, set color's alpha to 1
            - almost fit every case, but it will affect alpha fade effect

- csgo project
    - mainBattle UI(core battle)
        - building layer animation
        - skill
        - passive skill
    - config
        - config design, config deploy, config load 
    
- net game workflow
    - request init data 
    - use init data to init ui
    - click request / server request
        - client listen and refresh 

4. 楼层切换效果

- todo
    - new project
        - google proto
        - game state machine
            - reset state
            - step state
        - ui -> a heap of data drived by data
    - asset bundle
        - variant
        - build
            - how to build assetbundles 
        - load
            - load asset from asset bundle
                - LoadAsset(assetName, type) 
                - LoadAssetWithSubAssets(assetName, type)
            - load asset bundle
                - LoadFromMemory(bytes)
                - LoadFromFile(path)
    - map

- unity
    - canvas renderer: what is it?
    - canvas
        - reference resolution
        - render mode
    - canvas scaler
        - ui scale mode
