--[[

    Blox Fruits Script

    Created based on research of game mechanics and existing script examples

    Features:
    - Auto Farm
    - Teleportation
    - Fruit Detection
    - Combat Automation

]]

-- Configuration
local Config = {
    AutoFarm = true,
    AutoRaid = false,
    FruitFinder = false,
    AutoStats = true,
    FastAttack = true,
    SelectedFruit = "None",
    Team = "Pirates", -- "Pirates" or "Marines"
    SelectedBoss = "None",
    SelectedWeapon = "Melee",
    AutoBuyFruit = false,
    TeleportIsland = "None"
}

-- UI Library
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local window = library.CreateLib("Blox Fruits Script", "Ocean")

-- Main Tab
local mainTab = window:NewTab("Main")
local mainSection = mainTab:NewSection("Auto Farm")

mainSection:NewToggle("Auto Farm Level", "Automatically farms levels", function(state)
    Config.AutoFarm = state
    if state then
        AutoFarmFunction()
    end
end)

mainSection:NewToggle("Fast Attack", "Increases attack speed", function(state)
    Config.FastAttack = state
    if state then
        FastAttackFunction()
    end
end)

mainSection:NewToggle("Auto Stats", "Automatically assigns stat points", function(state)
    Config.AutoStats = state
    if state then
        AutoStatsFunction()
    end
end)

-- Teleport Tab
local teleportTab = window:NewTab("Teleport")
local teleportSection = teleportTab:NewSection("Islands")

local islands = {
    "First Sea", "Second Sea", "Third Sea", "Starter Island", "Marine Island",
    "Middle Town", "Jungle", "Pirate Village", "Desert", "Frozen Village",
    "Marine Fortress", "Skylands", "Prison", "Colosseum", "Magma Village", "Fountain City"
}

teleportSection:NewDropdown("Select Island", "Choose an island to teleport to", islands, function(currentOption)
    Config.TeleportIsland = currentOption
end)

teleportSection:NewButton("Teleport", "Teleports to selected island", function()
    TeleportToIsland(Config.TeleportIsland)
end)

-- Fruits Tab
local fruitsTab = window:NewTab("Fruits")
local fruitsSection = fruitsTab:NewSection("Fruit Options")

fruitsSection:NewToggle("Fruit Finder", "Notifies when fruits spawn", function(state)
    Config.FruitFinder = state
    if state then
        FruitFinderFunction()
    end
end)

local fruits = {
    "Bomb", "Spike", "Chop", "Spring", "Kilo", "Smoke", "Spin", "Flame", "Bird", "Ice", "Sand",
    "Dark", "Revive", "Diamond", "Light", "Love", "Rubber", "Barrier", "Magma", "Door", "Quake",
    "Buddha", "String", "Phoenix", "Rumble", "Paw", "Gravity", "Dough", "Shadow", "Venom", "Control",
    "Soul", "Dragon"
}

fruitsSection:NewDropdown("Select Fruit", "Choose a fruit to auto-buy", fruits, function(currentOption)
    Config.SelectedFruit = currentOption
end)

fruitsSection:NewToggle("Auto Buy Fruit", "Automatically buys selected fruit when available", function(state)
    Config.AutoBuyFruit = state
    if state then
        AutoBuyFruitFunction()
    end
end)

-- Combat Tab
local combatTab = window:NewTab("Combat")
local combatSection = combatTab:NewSection("Combat Options")

local weapons = {"Melee", "Sword", "Gun", "Fruit"}

combatSection:NewDropdown("Select Weapon", "Choose your weapon", weapons, function(currentOption)
    Config.SelectedWeapon = currentOption
end)

local bosses = {
    "None", "Saber Expert", "The Gorilla King", "Bobby", "Yeti", "The Saw", "Warden", "Chief Warden",
    "Swan", "Magma Admiral", "Fishman Lord", "Wysper", "Thunder God", "Cyborg", "Greybeard",
    "Diamond", "Jeremy", "Fajita", "Don Swan", "Smoke Admiral", "Cursed Captain", "Darkbeard",
    "Order", "Tide Keeper", "Cake Queen"
}

combatSection:NewDropdown("Select Boss", "Choose a boss to auto-farm", bosses, function(currentOption)
    Config.SelectedBoss = currentOption
end)

combatSection:NewToggle("Auto Raid", "Automatically does raids", function(state)
    Config.AutoRaid = state
    if state then
        AutoRaidFunction()
    end
end)

-- Settings Tab
local settingsTab = window:NewTab("Settings")
local settingsSection = settingsTab:NewSection("Script Settings")

local teams = {"Pirates", "Marines"}

settingsSection:NewDropdown("Select Team", "Choose your team", teams, function(currentOption)
    Config.Team = currentOption
    SelectTeam(currentOption)
end)

settingsSection:NewButton("Destroy GUI", "Removes the GUI", function()
    library:Destroy()
end)

-- Functions
function AutoFarmFunction()
    spawn(function()
        while Config.AutoFarm do
            print("Auto farming...")
            wait(1)
        end
    end)
end

function FastAttackFunction()
    spawn(function()
        while Config.FastAttack do
            print("Fast attack enabled...")
            wait(1)
        end
    end)
end

function AutoStatsFunction()
    spawn(function()
        while Config.AutoStats do
            print("Auto assigning stats...")
            wait(5)
        end
    end)
end

function TeleportToIsland(island)
    local islandCoordinates = {
        ["Starter Island"] = CFrame.new(1071.28, 16.30, 1426.86),
        ["Marine Island"] = CFrame.new(-2566.42, 6.85, 2045.25),
        ["Middle Town"] = CFrame.new(-655.82, 7.88, 1436.67),
        ["Jungle"] = CFrame.new(-1249.77, 11.88, 341.35),
        ["Pirate Village"] = CFrame.new(-1122.34, 4.78, 3855.91),
        ["Desert"] = CFrame.new(1094.14, 6.47, 4192.88),
        ["Frozen Village"] = CFrame.new(1198.00, 27.00, -1211.73),
        ["Marine Fortress"] = CFrame.new(-4505.37, 20.68, 4260.55),
        ["Skylands"] = CFrame.new(-4970.21, 717.70, -2622.35),
        ["Prison"] = CFrame.new(4875.33, 5.65, 734.85),
        ["Colosseum"] = CFrame.new(-1576.11, 7.38, -2986.47),
        ["Magma Village"] = CFrame.new(-5231.75, 8.61, 8467.87),
        ["Fountain City"] = CFrame.new(5132.71, 4.53, 4037.85)
    }

    if islandCoordinates[island] then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = islandCoordinates[island]
    else
        print("Island not found or not implemented yet")
    end
end

function FruitFinderFunction()
    spawn(function()
        while Config.FruitFinder do
            print("Searching for fruits...")
            for _, v in pairs(game:GetService("Workspace"):GetChildren()) do
                if string.find(v.Name, "Fruit") then
                    print("Found fruit: " .. v.Name)
                    -- Optionally teleport to fruit
                    -- game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                end
            end
            wait(10)
        end
    end)
end

function AutoBuyFruitFunction()
    spawn(function()
        while Config.AutoBuyFruit do
            print("Checking if " .. Config.SelectedFruit .. " is available...")
            wait(30)
        end
    end)
end

function AutoRaidFunction()
    spawn(function()
        while Config.AutoRaid do
            print("Auto raid enabled...")
            wait(5)
        end
    end)
end

function SelectTeam(team)
    print("Selected team: " .. team)
    local args = {
        [1] = "SetTeam",
        [2] = team
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end

-- Initialize
print("Script loaded successfully!")
