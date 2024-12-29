
print("XD")
if not game:IsLoaded() then game.Loaded:Wait() end
repeat task.wait() until game:GetService("Players")
repeat task.wait() until game:GetService("Players").LocalPlayer
repeat task.wait() until game:GetService("ReplicatedStorage")
repeat task.wait() until game:GetService("ReplicatedFirst")

local ResetSetting = false
local bindable = Instance.new("BindableFunction")

function bindable.OnInvoke(response)
	if response == "Yes" then
		ResetSetting = true
	end
end

game:GetService("StarterGui"):SetCore("SendNotification", {
	Title = "Royx Setting",
	Text = "You Need To Reset Setting?",
	Duration = 1.8,
	Callback = bindable,
	Button1 = "Yes",
	Button2 = "No"
})

task.wait(2)


for _, v in next, getconnections(game:GetService("Players").LocalPlayer.Idled) do
	v:Disable()
end

game:GetService("Players").LocalPlayer.Idled:connect(function()
	game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
	wait(0.1)
	game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

local function checkLine(s)
	local a = 0
	for i in string.gmatch(s, "[^\n]+") do
		a = a+1
	end
	return a
end
local start = 1
local function addspace(a)
	local st = ""
	for i = 0,a*2 do
		st = st.." "
	end
	return st
end
local function usd(v,i)
	local turn

	if typeof(v) == "Axes" then
		turn = "Axes.new("..tostring(v)..")"
	elseif typeof(v) == "BrickColor" then
		turn = "BrickColor.new("..tostring(v)..")"
	elseif typeof(v) == "CFrame" then
		turn = "CFrame.new("..tostring(v)..")"
	elseif typeof(v) == "Vector3" then
		turn = "Vector3.new("..tostring(v)..")"
	elseif typeof(v) == "Color3" then
		if v.r<=1 and v.g<= 1 and v.b<= 1 and v.r >= 0 and v.g >= 0 and v.b >= 0 then
			local R = v.r * 255
			local G = v.g * 255
			local B = v.b * 255
			turn = 'Color3.fromRGB('..R..", "..G..", "..B..")"..'    --Color3.new('..tostring(v)..")"
		else
			turn = "Color Error"
		end
	elseif typeof(v) == "Instance" then
		turn = "game."..tostring(v:GetFullName())
	elseif typeof(v) == "Enum" then
		turn = tostring(v).."   -- Enum"
	else
		turn = tostring(typeof(v))..".new("..tostring(v)..")"
	end
	if i then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(turn)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(turn)
	end
	return tostring(turn)
end

local convert
local startSpace = 1
function convert(v,i,a)
	if not a then
		a = startSpace    
	end
	if type(v) == "string" then
		if checkLine(v) >= 2 then
			if type(i) == "number" then 
				return '['..tostring(i)..']'.. " = "..'[['..tostring(v)..']]'
			end
			return '["'..tostring(i)..'"]'.. " = "..'[['..tostring(v)..']]'
		else
			if type(i) == "number" then 
				return '['..tostring(i)..']'.. " = "..'"'..tostring(v)..'"'
			end
			return '["'..tostring(i)..'"]'.. " = "..'"'..tostring(v)..'"'
		end
	elseif type(v) == "number" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	elseif type(v) == "boolean" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	elseif type(v) == "nil" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = nil"
	elseif type(v) == "function" then
		local Name = ""
		if debug.getinfo(v).name == "" then
			Name = tostring(v)
		else
			Name = debug.getinfo(v).name
		end
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..'function()end  '..'--'.."[["..tostring(Name).."]]"
		end
		return '["'..tostring(i)..'"]'.. " = "..'function()end  '..'--'.."[["..tostring(Name).."]]"
	elseif type(v) == "userdata" or type(v) == "vector" then
		return usd(v,i)
	elseif type(v) == "table" then
		if i~= nil then
			if type(i) == "number" then
				stt = '['..tostring(i)..']'.." = "..'{'
			else
				stt = '["'..tostring(i)..'"]'.." = "..'{'
			end

		else
			stt = '{'
		end
		local count_table = 0
		for i,v in pairs(v) do
			count_table = count_table+1
			if count_table == 1 then
				stt = stt .. "\n" .. tostring(addspace(a)) .. tostring(convert(v, i, a + 1))
			else    
				stt = stt .. ",\n" .. tostring(addspace(a)) .. tostring(convert(v, i, a + 1))
			end
		end
		return stt.."\n}"
	elseif type(v) == "thread" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	end
end

local config = getgenv().Configs or {

	["Full Auto Play"] = false, -- soon

	["Select Map"] = "IDK THE NAME",
	["Select Difficult"] = "Nightmare", -- Easy Medium Hard Nightmare
	["Auto Join"] = false,
	["Automatically leave"] = true,

	["Auto Spin"] = false,
	["Auto Summon"] = false,

	["Delays Next Step"] = "0.2", -- "0.1","0.2","0.3","0.4","0.5"
	["Auto Play Macro"]  = false,
	["Select Macro"] = "AA",

	["Auto Claim Quest"] = true,
	["Auto Claim Gift"] = true,
	["Auto Claim Daily rewards"] = true,
	["Auto Skip"] = true,
	["Auto x2 Speed"] = true,

	["Black screen"] = false,
	["Fps boots"] = false,

}

--[[file service]]
local Http = game:GetService("HttpService")

local MainPath = "Royx_PTD"

task.spawn(function()
	if not isfolder(MainPath) then 
		makefolder(MainPath)
	end

	if not isfolder(MainPath.."/"..game.GameId) then 
		makefolder(MainPath.."/"..game.GameId)
	end

	if not isfolder(MainPath.."/Marcro/") then 
		makefolder(MainPath.."/Marcro/")
	end
end)

do
	if not isfolder(MainPath) then 
		makefolder(MainPath)
	end

	if not isfolder(MainPath.."/"..game.GameId) then 
		makefolder(MainPath.."/"..game.GameId)
	end

	if not isfolder(MainPath.."/Marcro/") then 
		makefolder(MainPath.."/Marcro/")
	end
end

local InsidePath = MainPath.."/"..game.GameId.."/"
local InsidePath_Marcro = MainPath.."/Marcro/"

if not isfile(InsidePath_Marcro.."Default.lua") then 
	writefile(InsidePath_Marcro.."Default.lua","")
end

local DefaultConfig = {
	["Save Settings"] = true,
	["Select Config"] = "Default"
}

if not isfile(MainPath.."/".."maiconfigkubwwadasdadasdasdsa.lua") then 
	writefile(MainPath.."/".."maiconfigkubwwadasdadasdasdsa.lua",Http:JSONEncode(DefaultConfig))
else
	DefaultConfig = Http:JSONDecode(readfile(MainPath.."/".."maiconfigkubwwadasdadasdasdsa.lua"))
end

--if not isfile(MainPath.."/Marcro/") then
--	writefile(MainPath.."/Marcro/",Http:JSONEncode(DefaultConfig))
--else
--	DefaultConfig = Http:JSONDecode(readfile(MainPath.."/Marcro/"))
--end

if not isfile(InsidePath.."Default"..".json") or ResetSetting then 
	writefile(InsidePath.."Default"..".json",Http:JSONEncode(DefaultConfig))
end

if not ResetSetting then
	if isfile(InsidePath..DefaultConfig["Select Config"]..".json") then 
		config = Http:JSONDecode(readfile(InsidePath..DefaultConfig["Select Config"]..".json"))
		print("YED")
		print(PrintTable(config))
	end
end

config = getgenv().Configs or config

local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/REALSALFES/uwuware_final/bc02d7f029d46738bb08a415a12bea0e6dd8b31a/base64"))():new();
local RoyXUi = game.CoreGui:WaitForChild("Roxy")
if RoyXUi then RoyXUi.Enabled = false end
getgenv().Key = "ez"
loadstring(game:HttpGet'https://raw.githubusercontent.com/NightsTimeZ/Donate-Me/main/Thx.lua')(RoyXUi)

function RemoveSomeThing(str)
	return tostring(tostring(tostring(str:gsub(InsidePath, "")):gsub(".json", "")):gsub("\\",""))
end

function RemoveSomeThing2(str)
	return tostring(tostring(tostring(str:gsub(InsidePath_Marcro, "")):gsub(".lua", "")):gsub("\\",""))
end

local RecordMacroTable = {}

local allconfiglist = {}
for _,v in pairs(listfiles(InsidePath)) do
	table.insert(allconfiglist,RemoveSomeThing(v))
end

local interact = function(v2)
	local events = {"Activated","MouseButton1Down","MouseButton1Click","MouseButton1Up","MouseButton1Down"}
	for i,v in pairs(events) do
		for i,v in pairs(getconnections(v2[v])) do
			v.Function()
		end
	end
end

local MainTab = Lib:CreateTap("Main");

local LeftPageMain = MainTab:CreatePage("Main",1)

local claim_event = game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("QuestsService"):WaitForChild("RF"):WaitForChild("Claim")

local allmapdata = {}
if game.PlaceId == 15939808257 then
	for i,v in pairs(workspace.Lobby.JoinPads.Elevators:GetChildren()) do 
		if v.Name ~= "nil" then 
			table.insert(allmapdata,v.Name)
		end
	end
end
function createPartBetween(part1Position, part2Position, thickness)
	local midpoint = (part1Position + part2Position) / 2
	local distance = (part1Position - part2Position).Magnitude

	local connectorPart = Instance.new("Part")
	connectorPart.Anchored = true
	connectorPart.Size = Vector3.new(distance, thickness, thickness)
	connectorPart.CFrame = CFrame.new(midpoint, part2Position) * CFrame.Angles(0, math.pi / 2, 0) -- Adjust rotation
	connectorPart.BrickColor = BrickColor.new("Bright green")
	connectorPart.Parent = workspace

	return connectorPart
end

function checkifallvl2()
	for i,v in pairs(workspace.Game.Map.PlacedUnits:GetChildren()) do 
		if v:GetAttribute("Level") == 1 then
			return false
		end
	end
	return true
end

LeftPageMain:AddToggle("Full Auto Play",{Stats = (config["Full Auto Play"]) , callback = function(va)
	config["Full Auto Play"] = va
	spawn(function()
		if game.PlaceId == 15939811130 then
			local forslot = require(game:GetService("Players").LocalPlayer.PlayerScripts.Controllers.DataMirrorController)
			local all_slot = forslot["Profiles"][game.Players.LocalPlayer].Inventory.Units.Slots
			local all_unit_use = {}
			for i,v in pairs(all_slot) do 
				if v.UnitType then
					table.insert(all_unit_use,v.UnitType)
				end
			end
			local all_poslist = {}
			
			local centerPart = createPartBetween(workspace.Game.Map.Waypoints["1"].Position, workspace.Game.Map.Waypoints["2"].Position, 1.5)
			local numParts = 12 
			local partSize = Vector3.new(1, 1, 1)
			local offsetDistance = 3.5

			local length = centerPart.Size.X / 2
			local width = centerPart.Size.Z / 2
			local height = centerPart.Position.Y

			local centerCFrame = centerPart.CFrame

			for i = 0, numParts - 1 do
				
				local t = i / (numParts - 1)

				local z = -width + t * 2 * width
				local x = -length + t * 2 * length

				table.insert(all_poslist,CFrame.new(centerCFrame * Vector3.new(-length - offsetDistance, 0, z)))
				
				table.insert(all_poslist,CFrame.new(centerCFrame * Vector3.new(length + offsetDistance, 0, z)))

				table.insert(all_poslist,CFrame.new(centerCFrame * Vector3.new(x, 0, width + offsetDistance)))

				table.insert(all_poslist,CFrame.new(centerCFrame * Vector3.new(x, 0, -width - offsetDistance)))
			end
			
			spawn(function()
				while config["Full Auto Play"] and wait() do 
					if checkifallvl2() then 
						local randomplace = all_unit_use[math.random(1,#all_unit_use)]
						local ranpos = all_poslist[math.random(1,#all_poslist)]

						local args = {
							[1] = randomplace,
							[2] = ranpos*CFrame.new(0,2,0)
						}

						game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("UnitService"):WaitForChild("RF"):WaitForChild("PlaceUnit"):InvokeServer(unpack(args))
					end
					wait(1)
				end
			end)
			spawn(function()
				while config["Full Auto Play"] and wait() do 
					local wasd = 9e9
					local kuy = nil
					for i,v in pairs(workspace.Game.Map.PlacedUnits:GetChildren()) do 
						if v:GetAttribute("Level") < wasd then
							wasd = v:GetAttribute("Level")
							kuy = v
						end
					end
					local args = {
						[1] = kuy
					}

					game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("UnitService"):WaitForChild("RF"):WaitForChild("UpgradeUnit"):InvokeServer(unpack(args))
				end
			end)
		end
	end)
end})


local fuck2 = LeftPageMain:AddDropdown("Select Map",{Values = {config["Select Map"]} , callback = function(va)
	config["Select Map"] = va
end})
fuck2:Clear()
fuck2:Refresh(allmapdata)

local fuck1 = LeftPageMain:AddDropdown("Select Difficult",{Values = {config["Select Difficult"]} , callback = function(va)
	config["Select Difficult"] = va
	print(va)
end})

fuck1:Clear()
fuck1:Refresh({"Easy", "Medium", "Hard", "Nightmare"})
local interact = function(v2)
	local events = {"Activated","MouseButton1Down","MouseButton1Click","MouseButton1Up","MouseButton1Down"}
	for i,v in pairs(events) do
		for i,v in pairs(getconnections(v2[v])) do
			v.Function()
		end
	end
end
LeftPageMain:AddToggle("Auto Join",{Stats = (config["Auto Join"]) , callback = function(va)
	config["Auto Join"] = va
	spawn(function()
		while config["Auto Join"] and wait() do 
			if config["Select Map"] then
				if game.PlaceId == 15939808257 then
					local map = workspace.Lobby.JoinPads.Elevators:FindFirstChild(config["Select Map"])
					if map then
						for i,v in pairs(map:GetChildren()) do 
							game.Players.LocalPlayer.Character:PivotTo(v.CFrame)
							wait(0.1)
							if game:GetService("Players").LocalPlayer.PlayerGui.MainUI.StartElevator.Visible == true then
								interact(game:GetService("Players").LocalPlayer.PlayerGui.MainUI.StartElevator.Main.Button)
							end
							wait(1)
							if game:GetService("Players").LocalPlayer.PlayerGui.MainUI.StartElevator.Visible == true then
								interact(game:GetService("Players").LocalPlayer.PlayerGui.MainUI.StartElevator.Main.Button)
							end
							wait(1)
						end
					end
				else
					if game:GetService("Players").LocalPlayer.PlayerGui.MainUI.Frames.Voting.Visible then
						local args = {
							[1] = (config["Select Difficult"] or "Easy")
						}

						game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("VotingService"):WaitForChild("RF"):WaitForChild("Vote"):InvokeServer(unpack(args))
					end 
				end
			end
		end
	end)
end})

LeftPageMain:AddToggle("Automatically leave",{Stats = (config["Automatically leave"]) , callback = function(va)
	config["Automatically leave"] = va
	spawn(function()
		while config["Automatically leave"] and wait() do 
			pcall(function()
				if game.PlaceId == 15939811130 then
					if #game:GetService("Players"):GetChildren() > 1 then
						game:GetService("TeleportService"):Teleport(15939808257)
					end
				end
			end)
		end
	end)
end})

LeftPageMain:AddToggle("Auto Play Again",{Stats = (config["Auto Play Again"]) , callback = function(va)
	config["Auto Play Again"] = va
	spawn(function()
		while config["Auto Play Again"] and game.PlaceId == 15939811130 and wait(1) do 
			if game:GetService("Players").LocalPlayer.PlayerGui.MainUI.Frames.Results.Visible == true then
				interact(game:GetService("Players").LocalPlayer.PlayerGui.MainUI.Frames.Results.Main.Foreground.Buttons.PlayAgain)
			end
		end
	end)
end,})

LeftPageMain:AddToggle("Auto Claim Quest",{Stats = (config["Auto Claim Quest"]) , callback = function(va)
	config["Auto Claim Quest"] = va
	spawn(function()
		if game.PlaceId == 15939808257 then
			while config["Auto Claim Quest"] and wait(1) do

				local knit = game:GetService("ReplicatedStorage").Packages._Index["sleitnick_knit@1.7.0"].knit
				local questsService = knit.Services.QuestsService.RF.Claim

				for i = 1, 10 do
					for _, questType in pairs({"Daily", "Weekly"}) do
						local args = {
							[1] = {
								["QuestId"] = i,
								["Type"] = questType
							}
						}
						questsService:InvokeServer(unpack(args))
					end
				end

			end
		end
	end)
end,})

LeftPageMain:AddToggle("Auto Claim Daily rewards",{Stats = (config["Auto Claim Daily rewards"]) , callback = function(va)
	config["Auto Claim Daily rewards"] = va
	spawn(function()
		if game.PlaceId == 15939808257 then
			while config["Auto Claim Daily rewards"] and wait(1) do
				local knit = game:GetService("ReplicatedStorage"):WaitForChild("Packages")._Index["sleitnick_knit@1.7.0"].knit
				knit.Services.DailyRewardService.RF.ClaimRewards:InvokeServer()
			end
		end
	end)
end,})

LeftPageMain:AddToggle("Auto Claim Gift",{Stats = (config["Auto Claim Gift"]) , callback = function(va)
	config["Auto Claim Gift"] = va
	spawn(function()
		if game.PlaceId == 15939808257 then
			while config["Auto Claim Gift"] and wait(1) do
				local knit = game:GetService("ReplicatedStorage").Packages._Index["sleitnick_knit@1.7.0"].knit
				local questsService = knit.Services.QuestsService.RF.Claim
				for i = 1, 9 do
					local args = {
						[1] = i
					}

					game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GiftsService"):WaitForChild("RF"):WaitForChild("Claim"):InvokeServer(unpack(args))

				end
			end
		end
	end)
end,})

LeftPageMain:AddToggle("Auto Skip",{Stats = (config["Auto Skip"]) , callback = function(va)
	config["Auto Skip"] = va
	spawn(function()
		if game.PlaceId == 15939811130 then
			while config["Auto Skip"] and wait(1) do
				if not game:GetService("Players").LocalPlayer:GetAttribute("AutoSkip") then
					game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("WaveService"):WaitForChild("RF"):WaitForChild("AutoSkip"):InvokeServer()
				end
				wait(10)
			end
		end
	end)
end,})

LeftPageMain:AddToggle("Auto Spin",{Stats = (config["Auto Spin"]) , callback = function(va)
	config["Auto Spin"] = va
	spawn(function()
		if game.PlaceId == 15939808257 then
			while config["Auto Spin"] and wait(1) do
				local knit = game:GetService("ReplicatedStorage").Packages._Index["sleitnick_knit@1.7.0"].knit
				knit.Services.SpinWheelService.RF.RequestSpin:InvokeServer() 			
			end
		end
	end)
end,})

LeftPageMain:AddToggle("Auto Summon",{Stats = (config["Auto Summon"]) , callback = function(va)
	config["Auto Summon"] = va
	spawn(function()
		if game.PlaceId == 15939808257 then
			while config["Auto Summon"] and wait(1) do
				game:GetService("ReplicatedStorage").Packages._Index:FindFirstChild("sleitnick_knit@1.7.0").knit.Services.SummonService.RF.Summon:InvokeServer()
			end
		end
	end)
end,})
LeftPageMain:AddToggle("Auto x2 Speed",{Stats = (config["Auto x2 Speed"]) , callback = function(va)
	config["Auto x2 Speed"] = va
	spawn(function()
		if game.PlaceId == 15939811130 then
			while config["Auto x2 Speed"] and wait(1) do

				if not game:GetService("Players").LocalPlayer:GetAttribute("SpeedUp") then
					game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("WaveService"):WaitForChild("RF"):WaitForChild("SpeedUp"):InvokeServer()
				end
				wait(10)
			end
		end
	end)
end,})

LeftPageMain:AddToggle("Black screen",{Stats = (config["Black screen"]) , callback = function(va)
	config["Black screen"] = va
end,})

LeftPageMain:AddToggle("Fps boots",{Stats = (config["Fps boots"]) , callback = function(va)
	config["Fps boots"] = va
	if config["Fps boots"] then 
		if not dfgihoadsdfshklglk then 
			-- [Lighting]
			local lgh =  game:GetService("Lighting")
			sethiddenproperty(lgh,"Technology",2)
			lgh.FogStart = 1/0
			lgh.FogEnd = 1/0

			for i,v in ipairs(lgh:GetChildren()) do
				if v:IsA("BloomEffect") or v:IsA("BlurEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("SunRaysEffect") or v:IsA("DepthOfFieldEffect") then
					v.Enabled = false
				end
			end

			-- [Terrain]
			local wsc = game:GetService("Workspace")
			wsc.Terrain.CastShadow = false
			wsc.Terrain.WaterReflectance = 0
			wsc.Terrain.WaterWaveSize = 0
			wsc.Terrain.WaterTransparency = 0
			wsc.Terrain.WaterWaveSpeed = 0
			sethiddenproperty(wsc, 'StreamingTargetRadius', 64)
			sethiddenproperty(wsc, 'StreamingPauseMode', 2)
			sethiddenproperty(wsc.Terrain, 'Decoration', false)

			-- [Disabled]

			local boostmap = function(v)
				if v:IsA("Shirt") or v:IsA("Pants") then
					v[v.ClassName.."Template"] = ""
				elseif v:IsA("MeshPart") then
					v.TextureID = ""
				elseif v:IsA("BasePart") then
					v.Material = Enum.Material.SmoothPlastic
					v.Reflectance = 0
					v.CastShadow = false
				elseif v:IsA("SpecialMesh") then
					v.TextureId = 0 
				elseif v:IsA("Texture") or v:IsA("Decal") then
					v.Texture = ""
					v.Transparency = 1
				elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
					v.Enabled = false
					v.Lifetime = NumberRange.new(0)
				elseif v:IsA("Fire") or v:IsA("SpotLight") or v:IsA("Smoke") or v:IsA("Sparkles") or v:IsA("PostEffect") then
					v.Enabled = false
				elseif v:IsA("ShirtGraphic") then
					v.Graphic = ''
				elseif v:IsA("Model") then
					sethiddenproperty(v, 'LevelOfDetail', 1)
				end
			end
			for i,v in pairs(wsc:GetDescendants()) do
				pcall(boostmap,v)
			end
			wsc.DescendantAdded:Connect(function(v)
				pcall(boostmap,v)
			end)

			-- [Settings]
			--game:GetService("NetworkClient"):SetOutgoingKBPSLimit(100)
			settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
			settings().Rendering.MeshPartDetailLevel = Enum.MeshPartDetailLevel.Level01
			settings().Rendering.GraphicsMode = Enum.GraphicsMode.NoGraphics;
			settings().Rendering.EagerBulkExecution = false

			settings().Network.IncomingReplicationLag = -1000
			settings().Physics.PhysicsEnvironmentalThrottle = 1

			UserSettings():GetService("UserGameSettings").MasterVolume = 0
			UserSettings():GetService("UserGameSettings").SavedQualityLevel = Enum.SavedQualitySetting.QualityLevel1
			dfgihoadsdfshklglk = true
		end
	end
end,})

local RightPageMacro = MainTab:CreatePage("Macro",2)

local get_macro = {}
for _,v in pairs(listfiles(InsidePath_Marcro)) do
	table.insert(get_macro,RemoveSomeThing2(v))
end

ConfigMacro = RightPageMacro:AddDropdown("Select Macro",{Values = {config["Select Macro"]} , callback = function(va)
	MacroSelect = va
	config["Select Macro"] = va
end})

ConfigMacro:Clear()
ConfigMacro:Refresh(get_macro)

RightPageMacro:AddButton("Refresh Macro",function()
	get_macro = {}
	ConfigMacro:Clear()
	for _,v in pairs(listfiles(InsidePath_Marcro)) do
		table.insert(get_macro,RemoveSomeThing2(v))
	end
	ConfigMacro:Refresh(get_macro)
end)

RightPageMacro:AddTextBox("Name Macro",{Placeholder = "Insert Name Macro",callback = function(va)
	MacroName = va
end})

RightPageMacro:AddButton("Create New Macro",function()
	if MacroName == nil or MacroName == "" then 
		Notify({
			Title = "Alert",
			Text = "Put The Name Macro Now.",
			Timer = 3
		},"Warn")
		return
	end
	writefile(MainPath.."/Marcro/"..tostring(MacroName)..".lua",Http:JSONEncode({}))
	get_macro = {}
	ConfigMacro:Clear()
	for _,v in pairs(listfiles(InsidePath_Marcro)) do
		table.insert(get_macro,RemoveSomeThing2(v))
	end
	ConfigMacro:Refresh(get_macro)
end)

RightPageMacro:AddToggle("Record Macro",{Stats = false, callback = function(va)
	RecordMacro = va
	if va then 
		RecordMacroTable = {}
	else
		Notify({
			Title = "Alert",
			Text = "Done.",
			Timer = 3
		},"Success")
	end
end})

local basetime = 0
if game.PlaceId == 15939811130 then
	spawn(function()
		while true do 
			local realtime_wait = wait()
			if not game:GetService("Players").LocalPlayer.PlayerGui.MainUI.Frames.Voting.Visible then 
				basetime = basetime + realtime_wait
			end
		end
	end)
end

local remote_record = {
	"PlaceUnit",
	"UpgradeUnit",
	"SellUnit"
}
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt,false)
mt.__namecall = function(self,...)
	if RecordMacro then
		if not checkcaller() then 
			local args = {...}
			if self.Name == "PlaceUnit" then

				table.insert(RecordMacroTable,{
					['time'] = basetime,
					['type'] = self.Name,
					['data'] = {
						['name'] = args[1],
						['position'] = args[2]
					}
				})
				writefile(MainPath.."/Marcro/"..tostring(MacroSelect)..".lua",convert(RecordMacroTable))
			elseif table.find(remote_record,self.Name) then 
				table.insert(RecordMacroTable,{
					['time'] = basetime,
					['type'] = self.Name,
					['data'] = {
						['position'] = args[1].HumanoidRootPart.CFrame
					}
				})
				writefile(MainPath.."/Marcro/"..tostring(MacroSelect)..".lua",convert(RecordMacroTable))
			end
		end
	end
	return old(self,...)
end

local macroloopnodup = false
function Play_Macro()
	if not macroloopnodup then

		macroloopnodup = true

		xpcall(function()
			local fake =  readfile(MainPath.."/Marcro/"..tostring(MacroSelect)..".lua")
			local datajson = loadstring("return "..fake)()
			if (basetime) < 0 or basetime > datajson[#datajson].time+2 then
				macroloopnodup = false
				task.wait(1)
				return
			end
			for i ,v in pairs(datajson) do 

				if not PlayMacro then break end
				print("1")
				repeat wait() until basetime >= v.time
				print("2")
				if v["type"] == "PlaceUnit" then 
					local args = {
						[1] = v.data.name,
						[2] = v.data.position
					}

					game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("UnitService"):WaitForChild("RF"):WaitForChild("PlaceUnit"):InvokeServer(unpack(args))
				elseif v["type"] == "UpgradeUnit" then

					local current = 10
					local current_instance = nil

					for i,unit in pairs(workspace:WaitForChild("Game"):WaitForChild("Map"):WaitForChild("PlacedUnits"):GetChildren()) do 
						local dis = (v.data.position.Position-unit.HumanoidRootPart.Position).Magnitude
						if dis < current then
							current = dis
							current_instance = unit
						end
					end
					if current_instance then
						print("UPDISAD")
						local args = {
							[1] = current_instance
						}

						game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("UnitService"):WaitForChild("RF"):WaitForChild("UpgradeUnit"):InvokeServer(unpack(args))
					end

				elseif v["type"] == "SellUnit" then
					local current = 10
					local current_instance = nil

					for i,unit in pairs(workspace:WaitForChild("Game"):WaitForChild("Map"):WaitForChild("PlacedUnits"):GetChildren()) do 
						local dis = (v.data.position.Position-unit.HumanoidRootPart.Position).Magnitude
						if dis < current then
							current = dis
							current_instance = unit
						end
					end
					if current_instance then
						local args = {
							[1] = current_instance
						}

						game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("UnitService"):WaitForChild("RF"):WaitForChild("SellUnit"):InvokeServer(unpack(args))
					end

				end

				task.wait(tonumber(delaytime) or 1)
			end
			if PlayMacro then
				print("MACRO FINISH")
			end
		end,function()
			macroloopnodup = false
		end)

		macroloopnodup = false

	end
end

RightPageMacro:AddToggle("Auto Play Macro",{Stats = config["Auto Play Macro"], callback = function(va)
	PlayMacro = va
	config["Auto Play Macro"] = va
	task.spawn(function()
		while task.wait() and PlayMacro do 
			Play_Macro()
		end
	end)
end})

RightPageMacro:AddDropdown("Delay Step",{Values = {"0.1","0.2","0.3","0.4","0.5"},setup = config["Delays Next Step"] , callback = function(va)
	delaytime = va
	config["Delays Next Step"] = va
end})

local SettingTab = Lib:CreateTap("Setting");

local SettingPage = SettingTab:CreatePage("Setting",1)


SettingPage:AddButton("Join Discord",function()
	setclipboard("https://discord.com/invite/uhnwMCtR")
	Notify({
		Title = "Alert",
		Text = "Clipboard Set",
		Timer = 3
	},"Warn")
end)
SettingPage:AddButton("Rejoin",function()
	game:GetService("TeleportService"):Teleport(game.PlaceId)
end)
SettingPage:AddButton("Server Hop",function()
	game:GetService("TeleportService"):Teleport(game.PlaceId)
end)
SettingPage:AddButton("Server Hop Less People",function()
	game:GetService("TeleportService"):Teleport(game.PlaceId)
end)

local SettingConfigPage = SettingTab:CreatePage("Setting Config",2)

ConfigSelected = SettingConfigPage:AddDropdown("Select Config",{Values = {DefaultConfig["Select Config"]} , callback = function(va)
	DefaultConfig["Select Config"] = va
	writefile(MainPath.."/".."maiconfigkubwwadasdadasdasdsa.lua",Http:JSONEncode(DefaultConfig))
end})

SettingConfigPage:AddButton("Refresh Config",function()
	TableConfigSelect = {}
	ConfigSelected:Clear()
	for i,v in pairs(listfiles(InsidePath)) do
		table.insert(TableConfigSelect,RemoveSomeThing(v))
	end
	ConfigSelected:Refresh(TableConfigSelect)
end)

SettingConfigPage:AddToggle("Auto Save Config",{Stats = (DefaultConfig["Save Settings"]) , callback = function(va)
	auto_save = va
	DefaultConfig["Save Settings"] = va
	writefile(MainPath.."/".."maiconfigkubwwadasdadasdasdsa.lua",Http:JSONEncode(DefaultConfig))
	task.spawn(function()
		while auto_save do task.wait(1)
			writefile(InsidePath..DefaultConfig["Select Config"]..".json",Http:JSONEncode(config))
		end
	end)
end})

SettingConfigPage:AddTextBox("Name Config",{Placeholder = "Insert Name Config",callback = function(starts)
	NameConfig = starts
end})

SettingConfigPage:AddButton("Create New Config",function()
	if NameConfig == nil then return end
	local nameconfig = InsidePath..NameConfig or "whatareu"..".json"
	writefile(nameconfig,Http:JSONEncode(DefaultConfig))
	table.insert(TableConfigSelect,NameConfig)
	ConfigSelected:Refresh(TableConfigSelect)
end)

do
	local UserInputService = game:GetService("UserInputService")
	local RunService = game:GetService("RunService")

	local function makeScreenWhite()
		local screenGui = Instance.new("ScreenGui")
		screenGui.Name = "WhiteScreen"
		screenGui.ResetOnSpawn = false
		screenGui.IgnoreGuiInset = true

		local whiteFrame = Instance.new("Frame")
		whiteFrame.Size = UDim2.new(1, 0, 1, 0)
		whiteFrame.BackgroundColor3 = Color3.new(0,0,0) 
		whiteFrame.BorderSizePixel = 0
		whiteFrame.Parent = screenGui
		screenGui.Enabled = DefaultConfig["Black screen"]
		screenGui.Parent = game:GetService('CoreGui')

		local function toggleWhiteScreen()
			screenGui.Enabled = not screenGui.Enabled
		end

		local inputService = game:GetService("UserInputService")
		inputService.InputBegan:Connect(function(input)
			if input.KeyCode == Enum.KeyCode.L then 
				toggleWhiteScreen()
			end
		end)
		spawn(function()
			while wait(0.1) do 
				local old = DefaultConfig["Black screen"]
				repeat wait() until DefaultConfig["Black screen"] ~= old
				screenGui.Enabled = DefaultConfig["Black screen"]
			end
		end)
	end
	spawn(function()
		repeat wait() until game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("Notify")
		game:GetService("Players").LocalPlayer.PlayerGui.Notify.Enabled = false
	end)

	makeScreenWhite()
	game:GetService("RunService"):Set3dRenderingEnabled(true)
end
Notify({
	Title = "Script RoyXHub_BF",
	Text = "Script Successfully Loaded",
	Timer = 3
},"Success")
LoadingScriptSuccess = true
if RoyXUi and not (_G.Auto_Close_ui or DefaultConfig["Close Ui Main"]) then RoyXUi.Enabled = true end
