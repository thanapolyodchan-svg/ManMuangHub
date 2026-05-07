local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "🚀 มันม่วง Hub | Final Perfect Fix",
   LoadingTitle = "กำลังปรับปรุงระบบแยกแยะโดย มันม่วง...",
   LoadingSubtitle = "เวอร์ชันแก้เส้นโยงมอนสเตอร์ติดคน",
   ConfigurationSaving = { Enabled = true, FolderName = "ManMuangHub", FileName = "FinalConfig" }
})

-- [แถบ: ตัวละคร]
local PlayerTab = Window:CreateTab("👤 ตัวละคร", 4483345998)
PlayerTab:CreateSlider({
   Name = "ความเร็วเดิน (WalkSpeed)",
   Range = {16, 500},
   Increment = 1,
   CurrentValue = 16,
   Callback = function(v) 
      if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
         game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v 
      end
   end,
})
PlayerTab:CreateSlider({
   Name = "แรงกระโดด (JumpPower)",
   Range = {50, 1000},
   Increment = 1,
   CurrentValue = 50,
   Callback = function(v)
      if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
         game.Players.LocalPlayer.Character.Humanoid.UseJumpPower = true
         game.Players.LocalPlayer.Character.Humanoid.JumpPower = v
      end
   end,
})
PlayerTab:CreateButton({
   Name = "♻️ รีเซ็ตค่าตัวละคร (มันม่วง System)",
   Callback = function()
      if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
         game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
         game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50
      end
   end,
})

-- [แถบ: มองทะลุ]
local VisualTab = Window:CreateTab("👁️ มองทะลุ", 4483362458)
_G.PlayerESP = false
_G.MobESP = false

VisualTab:CreateSection("ตั้งค่าการมองเห็น")

VisualTab:CreateToggle({
   Name = "เปิดมองผู้เล่น (สีขาว)",
   CurrentValue = false,
   Callback = function(v) _G.PlayerESP = v end,
})

VisualTab:CreateToggle({
   Name = "เปิดมองมอนสเตอร์ (สีเทา - แก้ไขแล้ว)",
   CurrentValue = false,
   Callback = function(v) _G.MobESP = v end,
})

-- ฟังก์ชันสร้าง ESP (ระบบคัดกรองพิเศษโดย มันม่วง)
local function ApplyESP(object, isPlayerType)
    local Highlight = Instance.new("Highlight")
    Highlight.OutlineColor = isPlayerType and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(150, 150, 150)
    Highlight.FillTransparency = 1
    Highlight.Parent = object

    local Tracer = Drawing.new("Line")
    Tracer.Visible = false
    Tracer.Color = Highlight.OutlineColor
    Tracer.Thickness = 1
    Tracer.Transparency = 0.6

    local Billboard, HealthFill
    if isPlayerType then
        Billboard = Instance.new("BillboardGui")
        Billboard.Size = UDim2.new(3, 0, 0.4, 0)
        Billboard.AlwaysOnTop = true
        Billboard.ExtentsOffset = Vector3.new(0, 3, 0)
        Billboard.Parent = object:WaitForChild("Head", 10) or object:WaitForChild("HumanoidRootPart", 10)
        Billboard.Adornee = Billboard.Parent
        local Frame = Instance.new("Frame", Billboard)
        Frame.Size = UDim2.new(1, 0, 1, 0)
        Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        HealthFill = Instance.new("Frame", Frame)
        HealthFill.Size = UDim2.new(1, 0, 1, 0)
        HealthFill.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    end

    game:GetService("RunService").RenderStepped:Connect(function()
        if not object or not object.Parent then 
            Highlight:Destroy(); Tracer:Destroy(); return 
        end
        
        local hum = object:FindFirstChildOfClass("Humanoid")
        local root = object:FindFirstChild("HumanoidRootPart") or object:FindFirstChild("Head")
        
        -- ตรวจสอบซ้ำว่ายังเป็นผู้เล่นอยู่หรือไม่ (กันพลาด)
        local isActuallyAPlayer = game.Players:GetPlayerFromCharacter(object) ~= nil
        
        if hum and root and hum.Health > 0 then
            -- เงื่อนไขตัดสินใจ: ถ้าตัวละครเป็นคน ต้องเช็ค PlayerESP | ถ้าเป็นมอนสเตอร์ ต้องเช็ค MobESP
            local active = false
            if isActuallyAPlayer then
                active = _G.PlayerESP
            else
                active = _G.MobESP
            end
            
            Highlight.Enabled = active
            
            local screenPos, onScreen = game:GetService("Workspace").CurrentCamera:WorldToViewportPoint(root.Position)
            if active and onScreen then
                Tracer.From = Vector2.new(game:GetService("Workspace").CurrentCamera.ViewportSize.X / 2, game:GetService("Workspace").CurrentCamera.ViewportSize.Y)
                Tracer.To = Vector2.new(screenPos.X, screenPos.Y)
                Tracer.Visible = true
            else Tracer.Visible = false end

            if isActuallyAPlayer and Billboard then
                Billboard.Enabled = active
                HealthFill.Size = UDim2.new(math.clamp(hum.Health / hum.MaxHealth, 0, 1), 0, 1, 0)
            end
        else
            Highlight.Enabled = false; Tracer.Visible = false
            if Billboard then Billboard.Enabled = false end
        end
    end)
end

-- ระบบจัดการรายชื่อผู้เล่น (Whitelist)
local function HandlePlayer(plr)
    plr.CharacterAdded:Connect(function(char) ApplyESP(char, true) end)
    if plr.Character then ApplyESP(plr.Character, true) end
end

for _, p in pairs(game.Players:GetPlayers()) do
    if p ~= game.Players.LocalPlayer then HandlePlayer(p) end
end
game.Players.PlayerAdded:Connect(HandlePlayer)

-- ระบบสแกนมอนสเตอร์ (Blacklist คนออก)
local function ScanMob(obj)
    if obj:IsA("Model") and obj:FindFirstChildOfClass("Humanoid") then
        task.wait(0.5) -- หน่วงเวลาเพื่อให้แน่ใจว่าไม่ใช่คนเพิ่งเกิด
        if not game.Players:GetPlayerFromCharacter(obj) then
            ApplyESP(obj, false) -- เป็นมอนสเตอร์แน่นอน
        end
    end
end

workspace.DescendantAdded:Connect(ScanMob)
for _, v in pairs(workspace:GetDescendants()) do ScanMob(v) end

-- ระบบแก้สไลด์
game:GetService("RunService").PreSimulation:Connect(function()
    local char = game.Players.LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if char and root and char.Humanoid.MoveDirection.Magnitude == 0 then
        root.AssemblyLinearVelocity = Vector3.new(0, root.AssemblyLinearVelocity.Y, 0)
    end
end)
