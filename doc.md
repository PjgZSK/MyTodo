- csgo project
    1. config loading
        - lua script: `LuaDataMgr.lua`
        - concepts 
            - config lua script 
                - sample: `cfg_setting.lua`
                - code generating
                - return a list
            - config **id**
                - `LuaDataMgr` map: string_id -> **id**
                - map: map[string_id] -> config lua script
        - API
            - `LuaDataMgr.GetConfig(id)` -- return config list 
            - `LuaDataMgr.GetConfigByIndex(id, index)` -- return config[index]
            - `LuaDataMgr.SetConfigByIndex(id, index, value)`
    2. sprite loading
        - lua script: `GloabalData.lua`
        - ItemSprite Rule
            - asset name: **prefix_name**
            - asset bundle name: **prefixname** (remove '_' from asset name)
            - API: `AtlasItemFactory:getItemSprite(_, spriteName, spriteObject)`
    3. project key word
        - MainBattle
        - test
