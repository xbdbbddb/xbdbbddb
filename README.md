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
