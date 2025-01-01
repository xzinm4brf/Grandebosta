-- Mega Script de Missões, EXP e Transições de Mares para Blox Fruits

local players = game:GetService("Players")
local missions = {
    -- First Sea (Níveis 1 a 700)
    firstSea = {
        {level = 1, target = "Bandit", count = 5, rewardExp = 100},
        {level = 10, target = "Monkey", count = 10, rewardExp = 500},
        {level = 50, target = "Gorilla", count = 15, rewardExp = 1000},
        {level = 100, target = "Pirate", count = 20, rewardExp = 2000},
        {level = 200, target = "Brute", count = 25, rewardExp = 5000},
        {level = 300, target = "Desert Bandit", count = 30, rewardExp = 7500},
        {level = 400, target = "Snow Soldier", count = 45, rewardExp = 20000},
        {level = 500, target = "Fishman Lord", count = 65, rewardExp = 75000},
    },
    -- Second Sea (Níveis 701 a 1500)
    secondSea = {
        {level = 701, target = "Raider", count = 10, rewardExp = 100000},
        {level = 800, target = "Mercenary", count = 15, rewardExp = 150000},
        {level = 1000, target = "Flamingo Soldier", count = 30, rewardExp = 400000},
        {level = 1500, target = "Dark Beast", count = 50, rewardExp = 1000000},
    },
    -- Third Sea (Níveis 1501 a 2600)
    thirdSea = {
        {level = 1501, target = "Savage Warrior", count = 10, rewardExp = 1500000},
        {level = 1600, target = "Dragon Guard", count = 15, rewardExp = 2000000},
        {level = 2000, target = "Ancient King", count = 35, rewardExp = 4000000},
        {level = 2600, target = "Celestial Emperor", count = 65, rewardExp = 10000000},
    }
}

-- Função para criar menu de missões
function createMissionMenu(player)
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player:WaitForChild("PlayerGui")
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 400, 0, 300)
    frame.Position = UDim2.new(0.5, -200, 0.5, -150)
    frame.Parent = screenGui

    local closeButton = Instance.new("TextButton")
    closeButton.Text = "Fechar Menu"
    closeButton.Size = UDim2.new(0, 100, 0, 50)
    closeButton.Position = UDim2.new(0.5, -50, 0.85, 0)
    closeButton.Parent = frame

    closeButton.MouseButton1Click:Connect(function()
        screenGui:Destroy()
    end)

    local missionList = Instance.new("UIListLayout")
    missionList.Parent = frame

    -- Criar botões para cada missão
    local playerLevel = player.leaderstats.Level.Value
    local currentSea = getCurrentSea(playerLevel)

    if currentSea then
        for _, mission in ipairs(missions[currentSea]) do
            if playerLevel >= mission.level then
                local missionButton = Instance.new("TextButton")
                missionButton.Text = "Missão: " .. mission.target .. " (Lvl " .. mission.level .. ")"
                missionButton.Size = UDim2.new(0, 350, 0, 40)
                missionButton.Parent = frame

                missionButton.MouseButton1Click:Connect(function()
                    startMission(player, mission)
                    screenGui:Destroy()
                end)
            end
        end
    end
end

-- Função para determinar qual Sea o jogador está
function getCurrentSea(playerLevel)
    if playerLevel <= 700 then
        return "firstSea"
    elseif playerLevel <= 1500 then
        return "secondSea"
    elseif playerLevel <= 2600 then
        return "thirdSea"
    else
        return nil
    end
end

-- Função para iniciar missão
function startMission(player, mission)
    player.leaderstats.Exp.Value = player.leaderstats.Exp.Value + mission.rewardExp
    print(player.Name .. " completou a missão e ganhou " .. mission.rewardExp .. " de EXP!")
end

-- Teleportadores para transição entre os mares
function teleportToSecondSea(player)
    if player.leaderstats.Level.Value >= 700 then
        local secondSeaSpawn = workspace.SecondSeaSpawn
        player.Character:SetPrimaryPartCFrame(secondSeaSpawn.CFrame)
    else
        print("Você precisa ser nível 700 para acessar o Second Sea!")
    end
end

function teleportToThirdSea(player)
    if player.leaderstats.Level.Value >= 1500 then
        local thirdSeaSpawn = workspace.ThirdSeaSpawn
        player.Character:SetPrimaryPartCFrame(thirdSeaSpawn.CFrame)
    else
        print("Você precisa ser nível 1500 para acessar o Third Sea!")
    end
end

-- Função para criar as transições de mares
function createSeaTeleportation(player)
    if player.leaderstats.Level.Value >= 1500 then
        teleportToThirdSea(player)
    elseif player.leaderstats.Level.Value >=
