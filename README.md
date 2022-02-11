# world-chat feature
Community Server custom development world-chat functionality for Legends of Aria.

This feature permits users to type in a global chat for your community.  Users will be able to initiate chat by typing /wc, /world, /yell or by selecting the chat-channel button in the bottom left-hand corner of the screen and switching from 'Say' to 'World'.

- World chat is designed to show up in an orange font.

- World chat is not logged against the server logs.  Please use at your own discretion.

Produced by SCAN from HOPE Society


# Files requiring an update:

## player.lua

### Lines 364

    --SCAN ADDED

    local chatChannels = { {"Say","say"}, {"World","world"} } 


### Lines 2150 - 2153

    -- SCAN, ADDED

    RegisterEventHandler(EventType.Message,"World.Chat",

    function(name,title,msg)
    
        this:SystemMessage( "[FFBF00][World] " .. name .. " : " .. msg .."[-]","custom")
        
    end)


## scriptcommands_possessee.lua

### Lines 15 - 28

    -- SCAN ADDED

    WorldMessage = function(...)  

        if ( this:HasTimer("AntiWorldChatSpam") ) then
        
            return
            
        end
        
        this:ScheduleTimerDelay(TimeSpan.FromSeconds(2), "AntiWorldChatSpam")
        
        local onlineUsers = GlobalVarRead("User.Online")
        
        local message = CombineArgs(...)
        
        if(onlineUsers) then
        
            for user,y in pairs(onlineUsers) do
            
                --if ( GlobalVarReadKey("User.Online", memberObj) ) then
                
                user:SendMessageGlobal("World.Chat", this:GetCharacterName(), tostring(this:GetSharedObjectProperty("Title")), message)
                
                --end
                
            end
            
        end


### Lines 141

    --SCAN, ADDED

    RegisterCommand{ Command="world", AccessLevel = AccessLevel.Mortal, Func=PossesseeCommandFuncs.WorldMessage, Desc="Sends a world message", Aliases={"wd", "yell", "wc"} } 

