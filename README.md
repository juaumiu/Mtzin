local Players = game:GetService("Players")
local player = Players.LocalPlayer

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "JP Hub",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "JP Hub",
   LoadingSubtitle = "by Sirius",
   ShowText = "JP Hub", -- for mobile users to unhide rayfield, change if you'd like
   Theme = "Dark", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   ToggleUIKeybind = "K", -- The keybind to toggle the UI visibility (string like "K" or Enum.KeyCode)

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local Maintab = Window:CreateTab("Maintab", 4483362458) -- Title, Image

local Toggle = Maintab:CreateToggle({
	Name = "Esp",
	CurrentValue = false,
	Flag = "Esp",
	Callback = function(Value)
		if Value then
			-- Ativa: cria destaque azul em cada NPC
			for _, npc in pairs(CollectionService:GetTagged("NPC")) do
				if not npc:FindFirstChild("NPC_Highlight") then
					local highlight = Instance.new("Highlight")
					highlight.Name = "NPC_Highlight"
					highlight.OutlineColor = Color3.fromRGB(0, 150, 255) -- azul
					highlight.FillTransparency = 1
					highlight.Parent = npc
				end
			end
		else
			-- Desativa: remove todos os destaques
			for _, npc in pairs(CollectionService:GetTagged("NPC")) do
				local highlight = npc:FindFirstChild("NPC_Highlight")
				if highlight then
					highlight:Destroy()
				end
			end
		end
	end,
})

local Slider = Maintab:CreateSlider({
	Name = "Walkspeed",
	Range = {0, 100},      -- limite de valores
	Increment = 5,         -- aumenta de 5 em 5
	Suffix = "Speed",
	CurrentValue = 10,
	Flag = "Walkspeed",
	Callback = function(Value)
		-- Espera o personagem carregar
		local character = player.Character or player.CharacterAdded:Wait()
		local humanoid = character:WaitForChild("Humanoid")

		-- Define a velocidade
		humanoid.WalkSpeed = Value
	end,
})
