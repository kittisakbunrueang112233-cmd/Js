-- โหลด Rayfield UI Library
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- สร้าง Window หลัก
local Window = Rayfield:CreateWindow({
   Name = "Dev Control Panel",
   LoadingTitle = "Game Settings",
   LoadingSubtitle = "by Developer",
   ConfigurationSaving = {
      Enabled = false
   }
})

-- สร้าง Tab สำหรับควบคุมตัวละคร
local Tab = Window:CreateTab("Player Settings", 4483362458)

-- Slider สำหรับปรับความเร็ว (WalkSpeed)
local SpeedSlider = Tab:CreateSlider({
   Name = "Walk Speed",
   Range = {16, 100},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "SpeedSlider",
   Callback = function(Value)
       local player = game.Players.LocalPlayer
       if player.Character and player.Character:FindFirstChild("Humanoid") then
           player.Character.Humanoid.WalkSpeed = Value
       end
   end,
})

-- Slider สำหรับปรับแรงกระโดด (JumpPower)
local JumpSlider = Tab:CreateSlider({
   Name = "Jump Power",
   Range = {50, 200},
   Increment = 5,
   Suffix = "Power",
   CurrentValue = 50,
   Flag = "JumpSlider",
   Callback = function(Value)
       local player = game.Players.LocalPlayer
       if player.Character and player.Character:FindFirstChild("Humanoid") then
           player.Character.Humanoid.UseJumpPower = true
           player.Character.Humanoid.JumpPower = Value
       end
   end,
})

-- Toggle สำหรับเปิด/ปิด Highlight สีฟ้า (สำหรับ Dev / ทีมเดียวกัน)
local HighlightToggle = Tab:CreateToggle({
   Name = "Blue Highlight",
   CurrentValue = false,
   Flag = "HighlightToggle",
   Callback = function(Value)
       local Players = game:GetService("Players")
       local LocalPlayer = Players.LocalPlayer

       if Value then
           -- เปิดใช้งาน Highlight สีฟ้าแก่ผู้เล่นอื่นในเกม
           for _, player in ipairs(Players:GetPlayers()) do
               if player ~= LocalPlayer and player.Character then
                   local character = player.Character
                   if not character:FindFirstChild("DevBlueHighlight") then
                       local highlight = Instance.new("Highlight")
                       highlight.Name = "DevBlueHighlight"
                       highlight.FillColor = Color3.fromRGB(0, 170, 255) -- สีฟ้า
                       highlight.OutlineColor = Color3.fromRGB(255, 255, 255) -- เส้นขอบสีขาว
                       highlight.FillTransparency = 0.5
                       highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                       highlight.Parent = character
                   end
               end
           end
       else
           -- ลบ Highlight ออกเมื่อปิด Toggle
           for _, player in ipairs(Players:GetPlayers()) do
               if player.Character and player.Character:FindFirstChild("DevBlueHighlight") then
                   player.Character.DevBlueHighlight:Destroy()
               end
           end
       end
   end,
})
