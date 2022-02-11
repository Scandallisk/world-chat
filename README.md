# world-chat feature
Community Server custom development world-chat functionality for Legends of Aria.

This feature permits users to type in a global chat for your community.  Users will be able to initiate chat by typing /wc, /world, /yell or by selecting the chat-channel button in the bottom left-hand corner of the screen and switching from 'Say' to 'World'.

- World chat is designed to show up in an orange font.

- World chat is not logged against the server logs.  Please use at your own discretion.

Produced by SCAN from HOPE Society

## Instructions

Simply copy paste the code below into the file names listed at the specific line.  You will be required to restart your server for these changes to take effect.  If you make changes to the player.lua while other players are on your server you will risk crashing your server, forcing a restart.  To protect our community, please schedule downtime before deploying this change.

This code was produced for PublicServer version 1.4.7




# Files requiring an update:

## player.lua

### Lines 364-365

    --SCAN ADDED
    local chatChannels = { {"Say","say"}, {"World","world"} } 


### Lines 2150-2154

    -- SCAN, ADDED
    RegisterEventHandler(EventType.Message,"World.Chat",
    function(name,title,msg)
            this:SystemMessage( "[FFBF00][World] " .. name .. " : " .. msg .."[-]","custom")  
    end)


## scriptcommands_possessee.lua

### Lines 15-27

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
                user:SendMessageGlobal("World.Chat", this:GetCharacterName(), tostring(this:GetSharedObjectProperty("Title")), message)                          
            end            
        end

### Lines 141-142

    --SCAN, ADDED
    RegisterCommand{ Command="world", AccessLevel = AccessLevel.Mortal, Func=PossesseeCommandFuncs.WorldMessage, Desc="Sends a world message", Aliases={"wd", "yell", "wc"} } 

