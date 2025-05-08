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

