
-- 💩 mankefarm v1.0 - farm tới ỉa (Lite GUI, không phụ thuộc UI lib)

repeat wait() until game:IsLoaded()

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local ToggleFarm = Instance.new("TextButton")
local isFarming = false

ScreenGui.Name = "MankefarmGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Position = UDim2.new(0, 100, 0, 100)
Frame.Size = UDim2.new(0, 250, 0, 150)
Frame.Active = true
Frame.Draggable = true

Title.Name = "Title"
Title.Parent = Frame
Title.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "💩 mankefarm v1.0 - farm tới ỉa"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 14

ToggleFarm.Name = "ToggleFarm"
ToggleFarm.Parent = Frame
ToggleFarm.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
ToggleFarm.Position = UDim2.new(0.1, 0, 0.4, 0)
ToggleFarm.Size = UDim2.new(0.8, 0, 0.25, 0)
ToggleFarm.Font = Enum.Font.SourceSans
ToggleFarm.Text = "Bật Auto Farm: OFF"
ToggleFarm.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleFarm.TextSize = 14

ToggleFarm.MouseButton1Click:Connect(function()
	isFarming = not isFarming
	ToggleFarm.Text = "Bật Auto Farm: " .. (isFarming and "ON" or "OFF")
end)

-- Demo farm loop
spawn(function()
	while wait(1) do
		if isFarming then
			print("💩 Farm tới ỉa đang chạy...")
		end
	end
end)

-- 💩 mankefarm v1.0 - farm tới ỉa (Auto CDK / SA / SG + Pull Lever)

local function find_and_attack_boss(boss_name)
	for _, mob in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
		if mob.Name:lower():find(boss_name:lower()) and mob:FindFirstChild("HumanoidRootPart") then
			local hrp = mob.HumanoidRootPart
			game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = hrp.CFrame * CFrame.new(0, 10, 0)
			repeat
				wait(0.5)
				-- Giả lập đánh boss
				game:GetService("VirtualInputManager"):SendKeyEvent(true, "F", false, game)
				game:GetService("VirtualInputManager"):SendKeyEvent(false, "F", false, game)
			until not mob or not mob:FindFirstChild("Humanoid") or mob.Humanoid.Health <= 0 or not isFarming
			break
		end
	end
end

local function auto_pull_lever()
	local lever = workspace:FindFirstChild("Lever", true)
	if lever and lever:IsA("Model") then
		local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
		if hrp then
			hrp.CFrame = lever:GetModelCFrame() * CFrame.new(0, 3, 0)
			wait(1)
			fireproximityprompt(lever:FindFirstChildOfClass("ProximityPrompt"))
		end
	end
end

-- Thêm vào loop chính
spawn(function()
	while wait(2) do
		if isFarming then
			find_and_attack_boss("cake queen") -- CDK boss
			find_and_attack_boss("soul reaper") -- SG boss
			find_and_attack_boss("tide keeper") -- SA boss
			auto_pull_lever()
		end
	end
end)

local HttpService = game:GetService("HttpService")
local TPService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlaceId = game.PlaceId

local function server_hop()
	print("🔁 Không tìm thấy boss/trái, đang chuyển server...")
	local servers = {}
	local req = syn and syn.request or http_request or request
	if not req then return end

	local body = req({
		Url = string.format("https://games.roblox.com/v1/games/%d/servers/Public?sortOrder=Asc&limit=100", PlaceId),
		Method = "GET"
	})

	if body and body.Body then
		local decoded = HttpService:JSONDecode(body.Body)
		for _, server in pairs(decoded.data) do
			if type(server) == "table" and server.playing < server.maxPlayers then
				table.insert(servers, server.id)
			end
		end
	end

	if #servers > 0 then
		local randomServer = servers[math.random(1, #servers)]
		TPService:TeleportToPlaceInstance(PlaceId, randomServer, LocalPlayer)
	end
end

-- Kiểm tra boss hoặc trái tồn tại
local function check_boss_or_fruit()
	for _, mob in pairs(workspace.Enemies:GetChildren()) do
		if mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
			return true
		end
	end
	for _, item in pairs(workspace:GetChildren()) do
		if item:IsA("Tool") and item:FindFirstChild("Handle") and item.Name:lower():find("fruit") then
			return true
		end
	end
	return false
end

-- Auto Hop khi không thấy gì
spawn(function()
	while wait(30) do
		if isFarming and not check_boss_or_fruit() then
			server_hop()
			wait(10)
		end
	end
end)

-- 💥 Auto Upgrade Race V3

local function upgrade_race_v3()
	local localPlayer = game.Players.LocalPlayer
	if not localPlayer or not localPlayer:FindFirstChild("Backpack") then return end

	-- Kiểm tra nếu player đang ở V2, có đủ điều kiện lên V3
	local race = localPlayer:FindFirstChild("Data") and localPlayer.Data:FindFirstChild("Race")
	if race and race.Value:find("V2") then
		print("🎯 Đang cố gắng nâng Race lên V3...")
		-- Tìm NPC nâng race
		for _, npc in pairs(workspace:GetDescendants()) do
			if npc.Name == "MISC Island" and npc:FindFirstChild("Head") then
				local head = npc.Head
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = head.CFrame * CFrame.new(0, 2, 0)
				wait(2)
				local prompt = head:FindFirstChildOfClass("ProximityPrompt")
				if prompt then
					fireproximityprompt(prompt)
					wait(1)
					-- Giả lập chọn nâng cấp nếu có giao diện
					keypress(0x0D)
					keyrelease(0x0D)
				end
			end
		end
	end
end

-- Auto kiểm tra điều kiện nâng V3 mỗi 1 phút
spawn(function()
	while wait(60) do
		if isFarming then
			upgrade_race_v3()
		end
	end
end)

-- 🐟 Auto Roll Race - Chỉ chọn Fishman

local HttpService = game:GetService("HttpService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Player = game.Players.LocalPlayer

local function roll_race()
	if Player:FindFirstChild("Data") and Player.Data:FindFirstChild("Race") then
		local currentRace = Player.Data.Race.Value
		if currentRace:lower() ~= "fishman" then
			print("🔁 Đang roll lại Race... hiện tại là: " .. currentRace)
			local args = {
				[1] = "RollRace",
				[2] = {}
			}
			ReplicatedStorage.Remotes.CommF_:InvokeServer(unpack(args))
			wait(10)
			roll_race()
		else
			print("✅ Đã roll được Fishman!")
		end
	end
end

-- Tự động roll nếu không phải Fishman mỗi 1 phút
spawn(function()
	while wait(60) do
		if isFarming then
			roll_race()
		end
	end
end)
