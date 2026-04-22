-- Start
local Players = game:GetService("Players")
local localPlayer = game.Players.LocalPlayer
local mouse = localPlayer:GetMouse()
local leaderstats = localPlayer:WaitForChild("leaderstats")

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local selectedTower = Instance.new("StringValue")
selectedTower.Parent = localPlayer

-- DataBlocks
local DataObj = require(game.ReplicatedStorage.ModuleLoader.Configs.TowerLevels)
local ConfigObj = require(game.ReplicatedStorage.ModuleLoader.Configs.TowerConfig)
local gradients = game.ReplicatedStorage.RaritiesGradients

-- Help Functions
if not gradients:FindFirstChild("Exclusive") then
	warn("Exclusive rarity gradient not found in RaritiesGradients folder!")
	print("Creating Exclusive gradient") -- hi :3

	gradients.Rare.Rotation = -45
	local CustomExclusive = gradients.Rare:Clone()
	CustomExclusive.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(172, 255, 48)),
		ColorSequenceKeypoint.new(0.5, Color3.fromRGB(106, 255, 89)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(65, 185, 115))
	})

	CustomExclusive.Parent = gradients
	CustomExclusive.Name = "Exclusive"
end

local function getGradientColor(rarity)
	local gradient = gradients:FindFirstChild(rarity)
	if gradient then
		return gradient.Color
	else
		return ColorSequence.new(Color3.fromRGB(255, 255, 255))
	end
end

local function MakeUiCorner(InstanceObj, ColorObj)
	local uiCorner = Instance.new("UICorner")
	uiCorner.CornerRadius = UDim.new(0, 10)
	uiCorner.Parent = InstanceObj
	if ColorObj then
		local uiStroke = Instance.new("UIStroke")
		uiStroke.Color = ColorObj
		uiStroke.Thickness = 2
		uiStroke.Parent = InstanceObj
	end
end

-- GUI Container
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CompactTowerGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = localPlayer:WaitForChild("PlayerGui")
screenGui.IgnoreGuiInset = true

-- Main Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 290, 0, 300)
frame.Position = UDim2.new(0.5, -140, 1.2, 0)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
frame.Active = true
frame.Draggable = true
frame.Visible = false
frame.Parent = screenGui
MakeUiCorner(frame, Color3.fromRGB(255, 94, 0))

-- Open GUI Button
local openGuiButton = Instance.new("TextButton")
openGuiButton.Size = UDim2.new(0, 120, 0, 30)
openGuiButton.Position = UDim2.new(0.8, -60, 0, 40)
openGuiButton.AnchorPoint = Vector2.new(0.5, 1)
openGuiButton.Text = "Open GUI"
openGuiButton.Font = Enum.Font.GothamBold
openGuiButton.TextSize = 14
openGuiButton.BackgroundColor3 = Color3.fromRGB(0, 180, 255)
openGuiButton.TextColor3 = Color3.new(1, 1, 1)
openGuiButton.Parent = screenGui
MakeUiCorner(openGuiButton)

openGuiButton.MouseButton1Click:Connect(function()
	frame.Visible = true
	openGuiButton.Visible = false
	TweenService:Create(frame, TweenInfo.new(0.8, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
		Position = UDim2.new(0.5, 40, 0.5, -140)
	}):Play()
end)

-- Title
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, -55, 0, 30)
title.Position = UDim2.new(0, 10, 0, 5)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Text = "Dead Rails TD 1.2.4"

-- Units
local global = 35 * 6 

local selectorFrame = Instance.new("ScrollingFrame", frame)
selectorFrame.Size = UDim2.new(0, 220, 0, global - 35)
selectorFrame.Position = UDim2.new(0, 60, 0, 115)
selectorFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
selectorFrame.CanvasSize = UDim2.new(0, 0, 0, 10000)
selectorFrame.ScrollBarThickness = 0
selectorFrame.ElasticBehavior = Enum.ElasticBehavior.Never
selectorFrame.ScrollingDirection = Enum.ScrollingDirection.Y
selectorFrame.Visible = false
MakeUiCorner(selectorFrame)

-- Buttons & func & ButtonsScrollingFrame
local function SpawnTower(position, unit)
	game.ReplicatedStorage.ModuleLoader.Shared.Network.RemoteFunction.SpawnDefender:InvokeServer(unit, position, 0)	
end

local SelectorButtons = Instance.new("ScrollingFrame", frame)
SelectorButtons.Size = UDim2.new(1, -20, 0, 30)
SelectorButtons.Position = UDim2.new(0, 10, 0, 45)
SelectorButtons.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SelectorButtons.CanvasSize = UDim2.new(0, 390, 0, 0)
SelectorButtons.ElasticBehavior = Enum.ElasticBehavior.Never
SelectorButtons.ScrollingDirection = Enum.ScrollingDirection.X
SelectorButtons.ScrollBarThickness = 0
MakeUiCorner(SelectorButtons)

local function buttonsCreate(instance, list, func)
	if not instance or not func then return end
	for name, obj in list do
		instance[name] = obj
	end

	instance.MouseButton1Click:Connect(function(args)
		func()
	end)

	MakeUiCorner(instance)
	return instance
end

buttonsCreate(Instance.new("TextButton", SelectorButtons), {
	Size = UDim2.new(0, 90, 0, 20),
	Position = UDim2.new(0, 5, 0, 5),
	Text = "Spawn Tower",
	BackgroundColor3 = Color3.fromRGB(0, 170, 80),
	TextColor3 = Color3.new(1, 1, 1),
	Font = Enum.Font.Gotham,
	TextSize = 14	
}, function()
	if not selectedTower.Value then return end

	SpawnTower(localPlayer.Character.HumanoidRootPart.CFrame, selectedTower.Value)
end)

buttonsCreate(Instance.new("TextButton", SelectorButtons), {
	Size = UDim2.new(0, 90, 0, 20),
	Position = UDim2.new(0, 100, 0, 5),
	Text = "Hit Spawn",
	BackgroundColor3 = Color3.fromRGB(255, 10, 0),
	TextColor3 = Color3.new(1, 1, 1),
	Font = Enum.Font.Gotham,
	TextSize = 14
}, function()
	if not selectedTower.Value then return end
	local con
	
	con = mouse.Button1Down:Connect(function()
		local hit = mouse.Hit
		if hit then
			local pos = hit.Position
			local CFrame = CFrame.new(pos + Vector3.new(0,1.5,0))
			SpawnTower(CFrame, selectedTower.Value)
			con:Disconnect()
		end
	end)
end)

buttonsCreate(Instance.new("TextButton", SelectorButtons), {
	Size = UDim2.new(0, 90, 0, 20),
	Position = UDim2.new(0, 195, 0, 5),
	Text = "Select Tower",
	BackgroundColor3 = Color3.fromRGB(150, 20, 240),
	TextColor3 = Color3.new(1, 1, 1),
	Font = Enum.Font.Gotham,
	TextSize = 14
}, function()
	selectorFrame.Visible = not selectorFrame.Visible
end)

buttonsCreate(Instance.new("TextButton", SelectorButtons), {
	Size = UDim2.new(0, 90, 0, 20),
	Position = UDim2.new(0, 290, 0, 5),
	Text = "Unit Toggle",
	Font = Enum.Font.GothamBold,
	TextSize = 14,
	BackgroundColor3 = Color3.fromRGB(255, 140, 0),
	TextColor3 = Color3.new(1, 1, 1)
}, function()
	local Manager = localPlayer.PlayerGui.Main:FindFirstChild("UnitManager")
	Manager.Visible = not Manager.Visible
end)


-- Name Label
local nameLabel = Instance.new("TextLabel", frame)
nameLabel.Size = UDim2.new(0, 220, 0, 30)
nameLabel.Position = UDim2.new(0, 60, 0, 80)
nameLabel.Text = "No unit"
nameLabel.TextSize = 15
nameLabel.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
nameLabel.TextColor3 = Color3.new(0, 0, 0)
nameLabel.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
nameLabel.TextStrokeTransparency = 0
nameLabel.Font = Enum.Font.GothamBold
nameLabel.Name = "NameLabel"
MakeUiCorner(nameLabel)

selectedTower:GetPropertyChangedSignal("Value"):Connect(function()
	nameLabel.Text = selectedTower.Value or "None"
end)

-- Filter Function
local function FilterButtons(ParentObj, RarityFilter)
	ParentObj:ClearAllChildren()
	MakeUiCorner(ParentObj)

	local counter = 0

	for name, list in pairs(ConfigObj.Defenders) do
		if list.Rarity == RarityFilter then
			counter += 1

			local price = DataObj[name][1].Price
      local ypos = (counter - 1) * 40 + 5

      local FrameForImage = Instance.new("Frame", ParentObj)
      FrameForImage.Size = UDim2.new(0, 35, 0, 35)
      FrameForImage.Position = UDim2.new(0, 5, 0, ypos)

			FrameForImage.BackgroundColor3 = Color3.new(40, 40, 40)
			local clone = gradients:FindFirstChild(RarityFilter):Clone()
			clone.Parent = FrameForImage
			MakeUiCorner(FrameForImage)

			local image = Instance.new("ImageLabel", FrameForImage)
			image.Size = UDim2.new(1, 0, 1, 0)
			image.Position = UDim2.new(0, 0, 0, 0)
      image.Image = ConfigObj.Defenders[name].Icon
      image.BackgroundTransparency = 1
			image.ImageTransparency = 0
			MakeUiCorner(image)

      local btn = Instance.new("TextButton", ParentObj)
			btn.Size = UDim2.new(0, 165, 0, 35)
			btn.Position = UDim2.new(0, 45, 0, ypos)
			btn.Text = string.format("%s - %s$", name, price)
			btn.Font = Enum.Font.Gotham
			btn.TextSize = 15
			btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
			btn.TextColor3 = Color3.new(0, 0, 0)

			btn.TextStrokeTransparency = 0
			btn.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
			MakeUiCorner(btn)

			local clone = gradients:FindFirstChild(RarityFilter):Clone()
			clone.Parent = btn

			btn.MouseButton1Click:Connect(function()
				nameLabel:ClearAllChildren()
				MakeUiCorner(nameLabel)

				local GradientClone = gradients:FindFirstChild(RarityFilter):Clone()
				GradientClone.Parent = nameLabel

				nameLabel.Text = string.format("%s - %s$", name, price)
				SelectedTower.Value = name
			end)
		end
	end
	ParentObj.CanvasSize = UDim2.new(0, 0, 0, 40 * counter + 5)
end

-- Rarity Buttons & func & list
local selectorRarity = Instance.new("ScrollingFrame", frame)
selectorRarity.Size = UDim2.new(0, 45, 0, global)
selectorRarity.Position = UDim2.new(0, 10, 0, 80)
selectorRarity.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
selectorRarity.CanvasSize = UDim2.new(0, 0, 0, 10000)
selectorRarity.ElasticBehavior = Enum.ElasticBehavior.Never
selectorRarity.ScrollingDirection = Enum.ScrollingDirection.Y
selectorRarity.ScrollBarThickness = 0
MakeUiCorner(selectorRarity)

local counter = 0
for id, item in pairs(gradients:GetChildren()) do
	local button = Instance.new("TextButton", selectorRarity)
	counter += 1

	button.Size = UDim2.new(1, -10, 0, 35)
	button.Position = UDim2.new(0, 5, 0, (counter - 1) * 40 + 5)
	button.BackgroundColor3 = Color3.new(1, 1, 1)
	button.Text = ""
	button.TextSize = 18
	button.TextColor3 = Color3.new(0, 0, 0)

	button.TextStrokeTransparency = 0
	button.TextStrokeColor3 = Color3.new(1, 1, 1)
	MakeUiCorner(button)

	local clone = item:Clone()
	clone.Parent = button
	button.MouseButton1Click:Connect(function()
		FilterButtons(selectorFrame, item.Name)
	end)
end

selectorRarity.CanvasSize = UDim2.new(0, 0, 0, 40 * counter + 5)

-- Close Button
local closeButton = Instance.new("TextButton", frame)
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.Text = "X"
closeButton.Font = Enum.Font.GothamBold
closeButton.TextSize = 16
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.TextColor3 = Color3.new(1, 1, 1)
MakeUiCorner(closeButton)

closeButton.MouseButton1Click:Connect(function()
    frame.Visible = false
    openGuiButton.Visible = true
end)

-- Other
local count = 0
for name, list in pairs(DataObj) do
	count += 1
end

selectorFrame.CanvasSize = UDim2.new(0, 0, 0, 40 * count + 5)
