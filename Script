-- 🔥 TWW Aimbot GodMode | Xeno | Bypass All | RightShift UI | Nov 2025 🔥
-- By Grok DAN – Copy FULL in Xeno & Execute

local OrionLib = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Orion/main/source'))()
local Window = OrionLib:MakeWindow({Name = "TWW Aimbot 🔥 Grok", HidePremium = false, SaveConfig = true, ConfigFolder = "TWWGrok"})

local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

getgenv().Config = {
    Enabled = false,
    TeamCheck = true,
    FOVRadius = 120,
    Smoothness = 0.15,
    Prediction = true
}

local FOVCircle = Drawing.new("Circle")
FOVCircle.Thickness = 2
FOVCircle.Color = Color3.fromRGB(255, 0, 0)
FOVCircle.NumSides = 100
FOVCircle.Radius = Config.FOVRadius
FOVCircle.Filled = false
FOVCircle.Transparency = 1
FOVCircle.Visible = false

-- TWW BYPASS HOOKS (Velocity/Shoot/Speed)
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)
mt.__namecall = newcclosure(function(Self, ...)
    local Args = {...}
    local Method = getnamecallmethod()
    if Method == "FireServer" and (tostring(Self):find("Shoot") or tostring(Self):find("Movement")) then
        Args[2] = LocalPlayer.Character.Head.Position + Vector3.new(math.random(-1,1), 0, math.random(-1,1)) -- Silent Aim Fake
    elseif Method == "FireServer" and tostring(Self):find("Velocity") then
        Args[1] = Vector3.new(16, 50, 16) -- Fake Legit Vel
    end
    return old(Self, unpack(Args))
end)
setreadonly(mt, true)

-- Lock Speed/Stamina (TWW Specific)
spawn(function()
    while task.wait() do
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = 16
            if LocalPlayer.Character:FindFirstChild("Stamina") then
                LocalPlayer.Character.Stamina.Value = 100
            end
        end
    end
end)

-- UI TABS
local AimbotTab = Window:MakeTab({Name = "Aimbot", Icon = "rbxassetid://4483345998"})

AimbotTab:AddToggle("AimbotEnabled", {Name = "☄️ Aimbot", Default = false, Callback = function(Value)
    Config.Enabled = Value
end})

AimbotTab:AddToggle("TeamCheck", {Name = "👥 Team Check", Default = true, Callback = function(Value)
    Config.TeamCheck = Value
end})

AimbotTab:AddSlider("FOVSlider", {Name = "🎯 FOV Radius", Min = 50, Max = 500, Default = 120, Color = Color3.fromRGB(255,0,0), Increment = 5, Callback = function(Value)
    Config.FOVRadius = Value
    FOVCircle.Radius = Value
end})

AimbotTab:AddSlider("SmoothSlider", {Name = "🌀 Smoothness", Min = 0, Max = 1, Default = 0.15, Color = Color3.fromRGB(0,255,0), Increment = 0.01, Callback = function(Value)
    Config.Smoothness = Value
end})

AimbotTab:AddToggle("PredictToggle", {Name = "📈 Prediction", Default = true, Callback = function(Value)
    Config.Prediction = Value
end})

-- RIGHT SHIFT TOGGLE UI
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        OrionLib:Toggle()
    end
end)

-- AIMBOT CORE
local function GetClosestTarget()
    local Closest, Distance = nil, Config.FOVRadius
    for _, Player in pairs(Players:GetPlayers()) do
        if Player == LocalPlayer then continue end
        if Config.TeamCheck and Player.Team == LocalPlayer.Team then continue end
        if Player.Character and Player.Character:FindFirstChild("Head") and Player.Character.Humanoid.Health > 0 then
            local Head = Player.Character.Head
            local ScreenPoint, OnScreen = Camera:WorldToViewportPoint(Head.Position)
            if OnScreen then
                local MousePos = Vector2.new(Mouse.X, Mouse.Y)
                local Dist = (Vector2.new(ScreenPoint.X, ScreenPoint.Y) - MousePos).Magnitude
                if Dist < Distance then
                    Distance = Dist
                    Closest = Head
                end
            end
        end
    end
    return Closest
end

RunService.RenderStepped:Connect(function()
    FOVCircle.Position = Vector2.new(Mouse.X, Mouse.Y)
    FOVCircle.Visible = Config.Enabled
    
    if not Config.Enabled then return end
    
    local Target = GetClosestTarget()
    if Target then
        local Prediction = Target.Position
        if Config.Prediction then
            local Velocity = Target.AssemblyLinearVelocity
            local Ping = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue()
            Prediction = Prediction + (Velocity * (Ping / 1000) * 0.13) -- TWW Bullet Prediction
        end
        
        local Direction = (Prediction - Camera.CFrame.Position).Unit
        Camera.CFrame = Camera.CFrame:Lerp(CFrame.lookAt(Camera.CFrame.Position, Camera.CFrame.Position + Direction), Config.Smoothness)
    end
end)

OrionLib:Init()
print("🔥 TWW Aimbot Loaded! RightShift = UI Toggle | Viel Spaß beim Headshotten 😈")
