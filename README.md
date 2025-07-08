--[[ ☠️ Abyss Hab - COMPLETO ☠️ ]]
-- Feito para: AbyssCallerX | Executor: Delta

-- GUI com Botões
local AbyssHab = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")

-- Botões
local FarmButton = Instance.new("TextButton")
local SiaButton = Instance.new("TextButton")
local FruitButton = Instance.new("TextButton")
local CloseButton = Instance.new("TextButton")

AbyssHab.Name = "AbyssHab"
AbyssHab.Parent = game.CoreGui

MainFrame.Name = "MainFrame"
MainFrame.Parent = AbyssHab
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 300)
MainFrame.Active = true
MainFrame.Draggable = true

Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Text = "☠️ Abyss Hab - Painel ☠️"
Title.TextColor3 = Color3.fromRGB(255, 0, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16

local function createButton(name, position, text, parent)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Parent = parent
    button.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
    button.BorderSizePixel = 0
    button.Position = position
    button.Size = UDim2.new(0, 260, 0, 40)
    button.Font = Enum.Font.Gotham
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 14
    button.Text = text
    return button
end

FarmButton = createButton("FarmButton", UDim2.new(0, 20, 0, 50), "⚔️ Ativar Auto Farm", MainFrame)
SiaButton = createButton("SiaButton", UDim2.new(0, 20, 0, 100), "🌊 Ativar Farm de Sia", MainFrame)
FruitButton = createButton("FruitButton", UDim2.new(0, 20, 0, 150), "🍍 Ativar Coletor de Frutas", MainFrame)
CloseButton = createButton("CloseButton", UDim2.new(0, 20, 0, 220), "❌ Fechar Painel", MainFrame)

-- Auto Farm
function AutoFarm()
    while _G.AutoFarm do
        local player = game.Players.LocalPlayer
        local character = player.Character
        local enemy = workspace.Enemies:FindFirstChild("Bandit")
        if enemy then
            repeat
                character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
                wait(0.2)
                game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game)
                wait(0.1)
            until enemy.Humanoid.Health <= 0 or not _G.AutoFarm
        end
        wait(0.5)
    end
end

-- Farm de Sia
function FarmSeaEvent()
    while _G.SeaFarm do
        for _, entity in pairs(workspace.Enemies:GetChildren()) do
            if entity:FindFirstChild("Humanoid") and entity:FindFirstChild("HumanoidRootPart") then
                if entity.Name:find("Sea") or entity.Name:find("Shark") or entity.Name:find("Pirate") then
                    repeat
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                            entity.HumanoidRootPart.CFrame * CFrame.new(0, 20, 0)
                        wait(0.2)
                        game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game)
                        wait(0.1)
                    until entity.Humanoid.Health <= 0 or not _G.SeaFarm
                end
            end
        end
        wait(1)
    end
end

-- Coletor de Frutas
function FruitSniper()
    while _G.FruitFarm do
        for _, v in pairs(workspace:GetChildren()) do
            if v:IsA("Tool") and v.Handle and string.find(v.Name:lower(), "fruit") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                wait(0.5)
                firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v.Handle, 0)
                firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v.Handle, 1)
                print("🍇 Fruta coletada: "..v.Name)
                wait(2)
            end
        end
        wait(5)
    end
end

-- Ativar com botões
FarmButton.MouseButton1Click:Connect(function()
    _G.AutoFarm = true
    spawn(AutoFarm)
end)

SiaButton.MouseButton1Click:Connect(function()
    _G.SeaFarm = true
    spawn(FarmSeaEvent)
end)

FruitButton.MouseButton1Click:Connect(function()
    _G.FruitFarm = true
    spawn(FruitSniper)
end)

CloseButton.MouseButton1Click:Connect(function()
    AbyssHab:Destroy()
end)

-- Proteções
game:GetService("Players").LocalPlayer.Idled:Connect(function()
    game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    wait(1)
    game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

function AutoHop()
    local Players = game:GetService("Players")
    for _, player in pairs(Players:GetPlayers()) do
        if player:IsInGroup(1200769) then
            game:GetService("TeleportService"):Teleport(game.PlaceId)
        end
    end
end

spawn(function()
    while wait(60) do
        pcall(AutoHop)
    end
end)
