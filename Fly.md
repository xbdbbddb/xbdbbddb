local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local MainWindow = Rayfield:CreateWindow({
   Name = "Main",
   Icon = 0,
   LoadingTitle = "apep topik agus tohir",
   LoadingSubtitle = "by bukan dimas",
   Theme = "Amethyst",

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false,

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil,
      FileName = "Latina hub"
   },

   Discord = {
      Enabled = false,
      Invite = "noinvitelink",
      RememberJoins = false
   },

   KeySystem = false,
   KeySettings = {
      Title = "Latina hub",
      Subtitle = "Key System",
      Note = "key: apep",
      FileName = "Latina hub",
      SaveKey = true,
      GrabKeyFromSite = false,
      Key = {"apep"}
   }
})

local MainTab = MainWindow:CreateTab("Main", 4483362458)

-- Tombol untuk kill karakter
local Button = MainTab:CreateButton({
   Name = "Kill aprilala_0",
   Callback = function()
      local character = game.Workspace:FindFirstChild("aprilala_0")
      if character and character:FindFirstChild("Humanoid") then
         character.Humanoid.Health = 0
      end
   end,
})

-- Slider untuk WalkSpeed
local Slider = MainTab:CreateSlider({
   Name = "WalkSpeed",
   Range = {16, 250},
   Increment = 10,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "Slider1",
   Callback = function(Value)
      local player = game.Players.LocalPlayer
      local character = player.Character or player.CharacterAdded:Wait()
      if character:FindFirstChild("Humanoid") then
         character.Humanoid.WalkSpeed = Value
      end
   end,
})


-- Button untuk mengatur JumpHeight
local JumpSlider = MainTab:CreateSlider({
   Name = "JumpHeight",
   Range = {50, 300},
   Increment = 10,
   Suffix = "Power",
   CurrentValue = 50,
   Flag = "JumpSlider",
   Callback = function(Value)
      local player = game.Players.LocalPlayer
      local character = player.Character or player.CharacterAdded:Wait()
      if character:FindFirstChild("Humanoid") then
         character.Humanoid.JumpHeight = Value
      end
   end,
})

-- Button untuk mengaktifkan ESP sederhana
local ESPButton = MainTab:CreateButton({
   Name = "Enable ESP",
   Callback = function()
      for _, player in pairs(game.Players:GetPlayers()) do
         if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local billboard = Instance.new("BillboardGui")
            billboard.Name = "ESP"
            billboard.Size = UDim2.new(0, 100, 0, 40)
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            billboard.AlwaysOnTop = true
            billboard.Parent = player.Character.Head

            local label = Instance.new("TextLabel")
            label.Size = UDim2.new(1, 0, 1, 0)
            label.BackgroundTransparency = 1
            label.Text = player.Name
            label.TextColor3 = Color3.fromRGB(255, 0, 0)
            label.TextScaled = true
            label.Font = Enum.Font.SourceSansBold
            label.Parent = billboard
         end
      end
   end,
})


local flying = false
local moveDirection = Vector3.zero
local bodyVelocity = nil
local noclipConnection = nil
local runConnection = nil

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRoot = character:WaitForChild("HumanoidRootPart")

-- Buat GUI tombol arah
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FlyMobileGui"
screenGui.ResetOnSpawn = false
screenGui.Enabled = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local directions = {
   {Name = "↑", Offset = Vector3.new(0, 0, -1), Pos = UDim2.new(0.9, 0, 0.6, 0)},
   {Name = "↓", Offset = Vector3.new(0, 0, 1), Pos = UDim2.new(0.9, 0, 0.8, 0)},
   {Name = "←", Offset = Vector3.new(-1, 0, 0), Pos = UDim2.new(0.85, 0, 0.7, 0)},
   {Name = "→", Offset = Vector3.new(1, 0, 0), Pos = UDim2.new(0.95, 0, 0.7, 0)},
   {Name = "↑↑", Offset = Vector3.new(0, 1, 0), Pos = UDim2.new(0.9, 0, 0.5, 0)},
   {Name = "↓↓", Offset = Vector3.new(0, -1, 0), Pos = UDim2.new(0.9, 0, 0.9, 0)},
}

for _, dir in ipairs(directions) do
   local button = Instance.new("TextButton")
   button.Size = UDim2.new(0, 40, 0, 40)
   button.Position = dir.Pos
   button.Text = dir.Name
   button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
   button.TextColor3 = Color3.new(1, 1, 1)
   button.Font = Enum.Font.SourceSansBold
   button.TextScaled = true
   button.Parent = screenGui

   button.MouseButton1Down:Connect(function()
      moveDirection = moveDirection + dir.Offset
   end)

   button.MouseButton1Up:Connect(function()
      moveDirection = moveDirection - dir.Offset
   end)
end

-- Fungsi mulai fly
local function startFly()
   flying = true
   screenGui.Enabled = true

   bodyVelocity = Instance.new("BodyVelocity")
   bodyVelocity.MaxForce = Vector3.new(1,1,1) * 100000
   bodyVelocity.Velocity = Vector3.zero
   bodyVelocity.Parent = humanoidRoot

   -- Noclip aktif saat fly
   noclipConnection = game:GetService("RunService").Stepped:Connect(function()
      for _, part in pairs(character:GetDescendants()) do
         if part:IsA("BasePart") then
            part.CanCollide = false
         end
      end
   end)

   -- Pergerakan
   runConnection = game:GetService("RunService").RenderStepped:Connect(function()
      bodyVelocity.Velocity = moveDirection.Unit * 50
   end)
end

-- Fungsi stop fly
local function stopFly()
   flying = false
   screenGui.Enabled = false
   moveDirection = Vector3.zero

   if bodyVelocity then
      bodyVelocity:Destroy()
      bodyVelocity = nil
   end
   if noclipConnection then
      noclipConnection:Disconnect()
      noclipConnection = nil
   end
   if runConnection then
      runConnection:Disconnect()
      runConnection = nil
   end

   -- Balikin colliders
   for _, part in pairs(character:GetDescendants()) do
      if part:IsA("BasePart") then
         part.CanCollide = true
      end
   end
end

-- Toggle Fly dari Rayfield
local FlyToggle = MainTab:CreateToggle({
   Name = "Fly Mode (HP)",
   CurrentValue = false,
   Flag = "FlyHP",
   Callback = function(value)
      if value then
         startFly()
      else
         stopFly()
      end
   end,
})


