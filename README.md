local player = game.Players.LocalPlayer
local RunService = game:GetService("RunService")

local SUPER_SPEED = 150
local SUPER_JUMP = 180

local autoFarm = false
local autoRaid = false

local function getHumanoid()
    if player.Character then
        return player.Character:FindFirstChild("Humanoid"),
               player.Character:FindFirstChild("HumanoidRootPart")
    end
end

local function applyStats()
    local humanoid = getHumanoid()
    if humanoid then
        humanoid.WalkSpeed = SUPER_SPEED
        humanoid.JumpPower = SUPER_JUMP
    end
end

player.CharacterAdded:Connect(function()
    task.wait(0.5)
    applyStats()
end)

local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0.9,0,0.55,0)
frame.Position = UDim2.new(0.05,0,0.25,0)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1,0,0,50)
title.Text = "REDZ HUB"
title.TextScaled = true
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundColor3 = Color3.fromRGB(20,20,20)

local layout = Instance.new("UIListLayout", frame)
layout.Padding = UDim.new(0,8)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

local function button(text)
    local b = Instance.new("TextButton", frame)
    b.Size = UDim2.new(0.95,0,0,45)
    b.Text = text
    b.TextScaled = true
    b.TextColor3 = Color3.new(1,1,1)
    b.BackgroundColor3 = Color3.fromRGB(60,60,60)
    return b
end

button("Teleport: Castelo do Mar").MouseButton1Click:Connect(function()
    local _, hrp = getHumanoid()
    local p = workspace:FindFirstChild("CasteloDoMar")
    if hrp and p then hrp.CFrame = p.CFrame + Vector3.new(0,5,0) end
end)

button("Teleport: Mansão").MouseButton1Click:Connect(function()
    local _, hrp = getHumanoid()
    local p = workspace:FindFirstChild("Mansao")
    if hrp and p then hrp.CFrame = p.CFrame + Vector3.new(0,5,0) end
end)

button("Auto Farm ON/OFF").MouseButton1Click:Connect(function()
    autoFarm = not autoFarm
end)

button("Auto Raid ON/OFF").MouseButton1Click:Connect(function()
    autoRaid = not autoRaid
end)

button("Adicionar Dinheiro").MouseButton1Click:Connect(function()
    local stats = player:FindFirstChild("leaderstats")
    if stats and stats:FindFirstChild("Money") then
        stats.Money.Value += 10000
    end
end)

RunService.Heartbeat:Connect(function()
    applyStats()

    local _, hrp = getHumanoid()

    if autoFarm and hrp then
        local npc = workspace:FindFirstChild("NPC_Farm")
        if npc and npc:FindFirstChild("HumanoidRootPart") then
            hrp.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0,0,3)
        end
    end

    if autoRaid and hrp then
        local boss = workspace:FindFirstChild("RaidBoss")
        if boss and boss:FindFirstChild("HumanoidRootPart") then
            hrp.CFrame = boss.HumanoidRootPart.CFrame * CFrame.new(0,0,5)
        end
    end
end)
