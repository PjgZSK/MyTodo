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
            <!-- - mood multiple progress -->
    - main ui prefab and code(wait for UI)
    - trouble
        <!-- - progress bar end shining -->
        - skill cooldown bar white edge
        - back end
    - custom icon set (resource load)


- resource hot update workflow
    - version check
        - request remote version file
        - request local version file
        - check
        - version file
            - asset version
            - apk version
            - reousce list file md5
    - resource check
        - request remote resource list file
        - request local resource list file
        - check
        - resource list file
            - resource entry
                - name
                - path(remote and local)
                - size
                - md5
    - download 
        - download outdated resource

- how to use a cdn 
    - cdn is Content Deliver Network

- csgo resource update workflow
    - upload "cdn start" to 3rd sdk 
    - version check
        - request remote version info and resource address from server (UnityWebRequest, n try, doman/ip, resId.xml)
        - parser remote version file 
        - request local data/streming version file from local host
        - check remote version and local data/streaming version to determine whether update or not
            - no update, routine finish
    - resource check
        - request remote resource list from resource address 
        - request local data/streaming resource list from local host
        - check remote resource list and local resource list to get outdated resource list
        - request outdated resource list
    
- todo
    - player card implementation 
    - reset userdata
    - pack sprite atlas(may no atlas)
    - build ab resources
    - upload resources 

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

- check role exist(msg)
    - yes -> enter game
    - no
        - set role name (msg)
            - failed
            - success
                - set manager(msg)
                    - failed
                    - success
                        - create team(msg)
                            - failed
                            - success -> enter guide

- break down
    - integrate sdk(server)??
    - area server(auto) 
    - create/login account
        - 2 page (create page, login page)

    - create role 
        - 1 page (set name page) 
            - name spider??
        - create manager
            - 2 page(set manager page, welcome page)
        - create team
            - 2 page (set team page, support player page)
            - avatar??

- set role name breakdown
    - set name
        - random name(client or server)
        - set name manually(IME)
    - name check 
        - name is legal (check in client)
        - name is unique (check in server)
    - server message
        - send name to server

- account create/login 
    - logic
        - check whether there is a account
            - yes -> account login logic
                - get account info(name/id/phonenumber, password) and device info(device type, device id, platform)
                - check account command
                - return result
            - no -> account create logic
                - get account info(name/id/phonenumber, password)
                - create account command
                - return result
                    - suc - goto account login logic
                    - failed - fail tip
    - property
        - user id
    - client
        - account create/login page
            - two version? mainland and oversea?
        - unknown
    - server
        - account create
        - account login
        - unknown 
            - different servers: 
                - normal
                    - mainland, oversea 
                - special(different target net address in client code)
                    - intranet, mainland test, oversea test
            - sdk(where is server)
                - mainland
                    - real-name requirement ? 
                - oversea

- select server
    - client: select server page
    - server: ??
- role create
    - set role name
        - random name
            - club id + player id + weapon name/position + random number
            - club id, player id 
        - input name
            - IME 
    - name check
        - 14 byte
        - name is legal
        - unique name
    - name change
        - 30day cooldown
        - record
        - update new name for other players

- manager create
    - avatar, avatar frame
    - property
        - create date
        - gender, avatar, avatar frame, 
        - level, grade 
        - match season data(??)

- todo
    - a strict weak order, a total order
    - tool
        - data command, proxy template 
        - add weekly md
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
    - UnityEngine.SystemInfo
    - UnityEngine.Application
    - map


