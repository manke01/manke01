local function getInventoryFromScrollingFrame(scrollingFrame)
    local items = {}
    for _, child in ipairs(scrollingFrame:GetChildren()) do
        if child:FindFirstChild("TierImage") then
            local tierImageURL = tostring(child.TierImage.Image)
            local rarity = ""
            if tierImageURL == "rbxassetid://9473887537" then
                rarity = "E"
            elseif tierImageURL == "rbxassetid://9481936456" then
                rarity = "L"
            end
            if rarity ~= "" then
                table.insert(items, child.TierImage.Parent.Name .. "(" .. rarity .. ")")
            end
        end
    end
    return items
end
local function decodeJSON(jsonString)
    local success, result = pcall(game:GetService("HttpService").JSONDecode, game:GetService("HttpService"), jsonString)
    if success then
        return result
    else
        warn("Error decoding JSON:", result)
        return nil
    end
end
local function checkFightingStyle(boughtDataString)
    local boughtData = decodeJSON(boughtDataString)
    if not boughtData then
        return nil
    end

    local trueCount = 0
    for _, value in pairs(boughtData) do
        if value == true then
            trueCount = trueCount + 1
        end
    end
    
    local style = ""
    if trueCount >= 3 and trueCount < 5 then
        style = "3-5"
    elseif trueCount < 3 then
        style = "0-2"
    elseif trueCount >= 5 then
        style = "God"
    end
    
    return style
end

local function extractRaceData(raceDataString)
    local raceData = decodeJSON(raceDataString)
    if not raceData then
        return nil
    end

    local appearance = raceData.Appearance or ""
    local race = raceData.Race or ""
    
    local raceString = string.gsub(race .. "V" .. appearance, "%s", "")
    return raceString
end

while true do
    local player = game.Players.LocalPlayer
    local weaponInventory = getInventoryFromScrollingFrame(player.PlayerGui.MainGui.StarterFrame.Inventory_Frame.ScrollingFrame)
    local accessoryInventory = getInventoryFromScrollingFrame(player.PlayerGui.MainGui.StarterFrame.Inventory_Frame.ScrollingFrameAccessories)
    local fruitInventory = getInventoryFromScrollingFrame(player.PlayerGui.MainGui.StarterFrame.Inventory_Frame.ScrollingFrameFruits)
    local fightingStyle = checkFightingStyle(player.PlayerStats.Bought.Value)
    local raceData = extractRaceData(player.PlayerStats.RaceTbl.Value)

    local data = {
        ['Basic Data'] = {
            Level = player.PlayerStats.lvl.Value,
            Beli = player.PlayerStats.beli.Value,
            Fragments = player.PlayerStats.Gem.Value,
            DevilFruit = accessoryInventory,
            Race = raceData or "Unknown",
            ['Bounty/Honor'] = 0,
            ['Fighting Style'] = fightingStyle or "Unknown"
        },
        ['Items Inventory'] = weaponInventory,
        ['Fruits Inventory'] = fruitInventory
    }

    local successWrite, errorMessage = pcall(function()
        writefile(string.format("%sData.json", player.Name), game:GetService("HttpService"):JSONEncode(data))
    end)

    if successWrite then
        print(string.format("The file with name %sData.json has been written", player.Name))
    else
        warn("Got error:", errorMessage)
    end

    wait(35)
end
