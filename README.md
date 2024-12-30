local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/Namoqp1/dump/refs/heads/main/them"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/ZAZA.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/InterfaceManager.lua.txt"))()

local ScreenGui1 = Instance.new("ScreenGui")
local ImageButton1 = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

ScreenGui1.Name = "ImageButton"
ScreenGui1.Parent = game.CoreGui
ScreenGui1.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

ImageButton1.Parent = ScreenGui1
ImageButton1.BackgroundTransparency = 1
ImageButton1.BorderSizePixel = 0
ImageButton1.Position = UDim2.new(0.120833337, 0, 0.0952890813, 0)
ImageButton1.Size = UDim2.new(0, 50, 0, 50)
ImageButton1.Draggable = true
ImageButton1.Image = "rbxassetid://10723396107"
ImageButton1.MouseButton1Down:Connect(function()
	game:GetService("VirtualInputManager"):SendKeyEvent(true,305,false,game)
	game:GetService("VirtualInputManager"):SendKeyEvent(false,305,false,game)
end)
UICorner.Parent = ImageButton1

local Window = Fluent:CreateWindow({
	Title = "Old Toilet TD",
	SubTitle = "ddcmmc",
	TabWidth = 130,
	Size = UDim2.fromOffset(480, 400),
	Acrylic = false, 
	Theme = "Dark",
	MinimizeKey = Enum.KeyCode.RightControl
})

local Tabs = {
	Setting = Window:AddTab({ Title = "Setting", Icon = "settings" })
}

local settings = {} -- ตารางตั้งค่าของตัวเอง

local function saveSettings(folderName, fileName)
	local filePath = folderName .. "/" .. fileName .. ".txt"

	-- สร้างโฟลเดอร์ถ้ายังไม่มี
	if not isfolder(folderName) then
		makefolder(folderName)
		print("สร้างโฟลเดอร์: " .. folderName)
	end

	-- สร้างไฟล์ถ้ายังไม่มี
	if not isfile(filePath) then
		writefile(filePath, "")
		print("สร้างไฟล์: " .. filePath)
	end

	-- เซฟค่า settings ที่เป็น boolean ลงไฟล์
	local content = ""
	for key, value in pairs(settings) do
		if type(value) == "boolean" then
			content = content .. key .. " = " .. tostring(value) .. "\n"
		end
	end
	writefile(filePath, content)
	print("บันทึกค่า settings ลงไฟล์: " .. filePath)
end

local function loadSettings(folderName, fileName)
	local filePath = folderName .. "/" .. fileName .. ".txt"

	-- โหลดค่าจากไฟล์กลับเข้า settings
	if isfile(filePath) then
		local content = readfile(filePath)
		for line in string.gmatch(content, "[^\r\n]+") do
			local key, value = string.match(line, "^(%w+) = (true|false)$")
			if key and value then
				settings[key] = (value == "true")
			end
		end
		print("โหลดค่าจากไฟล์กลับเข้า settings")
	else
		print("ไม่พบไฟล์: " .. filePath)
	end
end


local folderName = "MyFolder"
local fileName = "lol"

task.spawn(function()
	while true do task.wait(1)
		if true then
			saveSettings(folderName, fileName)
		end
	end
end)

loadSettings(folderName, fileName)


Tabs.Setting:AddToggle("Host Bot", {
	Title = "Host Bot",
	Default = false,
	Callback = function(Host)
		_G.Host= Host
	end
})

spawn(function()
	while task.wait() do
		if _G.Host then
			pcall(function()
				print("Ez")
			end)
		end
	end
end)

