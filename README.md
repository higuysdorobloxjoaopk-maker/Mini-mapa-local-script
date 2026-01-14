# Mini-mapa-local-script
hey para que funcione corretamente primeiro crie uma pasta(folder) no worspace nomeia para "MiniMap">exatamente assim dentro od arquivo coloque as parts visíveis no mini mapa depoi vai em startplayer dentro de startplayer preucure por startplayerscripts dentro crie um local script e coloque o script

-- obrigado pela atenção★

script:

```lua
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local rootPart = character:WaitForChild("HumanoidRootPart")

local gui = Instance.new("ScreenGui")
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local miniFrame = Instance.new("Frame")
miniFrame.Size = UDim2.new(0,75,0,75)
miniFrame.Position = UDim2.new(0,695,0,0)
miniFrame.BackgroundColor3 = Color3.fromRGB(15,15,15)
miniFrame.BorderSizePixel = 0
miniFrame.ClipsDescendants = true
miniFrame.Parent = gui

local miniCorner = Instance.new("UICorner")
miniCorner.CornerRadius = UDim.new(0,10)
miniCorner.Parent = miniFrame

local miniViewport = Instance.new("ViewportFrame")
miniViewport.Size = UDim2.new(1,0,1,0)
miniViewport.BackgroundTransparency = 1
miniViewport.Parent = miniFrame

local miniCam = Instance.new("Camera")
miniCam.CameraType = Enum.CameraType.Scriptable
miniCam.FieldOfView = 1
miniCam.Parent = miniViewport
miniViewport.CurrentCamera = miniCam

local myMiniDot = Instance.new("Frame")
myMiniDot.Size = UDim2.new(0,6,0,6)
myMiniDot.Position = UDim2.new(0.5,0,0.5,0)
myMiniDot.AnchorPoint = Vector2.new(0.5,0.5)
myMiniDot.BackgroundColor3 = Color3.fromRGB(0,255,0)
myMiniDot.BorderSizePixel = 0
myMiniDot.Parent = miniFrame

local myMiniCorner = Instance.new("UICorner")
myMiniCorner.CornerRadius = UDim.new(1,0)
myMiniCorner.Parent = myMiniDot

local openMap = Instance.new("TextButton")
openMap.Size = UDim2.new(0,75,0,20)
openMap.Position = UDim2.new(0,695,0,80)
openMap.Text = "Mapa"
openMap.BackgroundColor3 = Color3.fromRGB(25,25,25)
openMap.TextColor3 = Color3.new(1,1,1)
openMap.BorderSizePixel = 0
openMap.Parent = gui

local openCorner = Instance.new("UICorner")
openCorner.CornerRadius = UDim.new(0,8)
openCorner.Parent = openMap

local fullFrame = Instance.new("Frame")
fullFrame.Size = UDim2.new(1,0,1,0)
fullFrame.BackgroundColor3 = Color3.fromRGB(0,0,0)
fullFrame.BackgroundTransparency = 0.75
fullFrame.Visible = false
fullFrame.BorderSizePixel = 0
fullFrame.ClipsDescendants = true
fullFrame.Parent = gui

local fullCorner = Instance.new("UICorner")
fullCorner.CornerRadius = UDim.new(0,30)
fullCorner.Parent = fullFrame

local fullViewport = Instance.new("ViewportFrame")
fullViewport.Size = UDim2.new(1,0,1,0)
fullViewport.BackgroundTransparency = 1
fullViewport.Parent = fullFrame

local fullCam = Instance.new("Camera")
fullCam.CameraType = Enum.CameraType.Scriptable
fullCam.FieldOfView = 1
fullCam.Parent = fullViewport
fullViewport.CurrentCamera = fullCam

local close = Instance.new("TextButton")
close.Size = UDim2.new(0,40,0,40)
close.Position = UDim2.new(1,-50,0,10)
close.Text = "X"
close.TextScaled = true
close.BackgroundColor3 = Color3.fromRGB(180,0,0)
close.TextColor3 = Color3.new(1,1,1)
close.BorderSizePixel = 0
close.Parent = fullFrame

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(1,0)
closeCorner.Parent = close

local myFullDot = Instance.new("Frame")
myFullDot.Size = UDim2.new(0,10,0,10)
myFullDot.Position = UDim2.new(0.5,0,0.5,0)
myFullDot.AnchorPoint = Vector2.new(0.5,0.5)
myFullDot.BackgroundColor3 = Color3.fromRGB(0,255,0)
myFullDot.BorderSizePixel = 0
myFullDot.Parent = fullFrame

local myFullCorner = Instance.new("UICorner")
myFullCorner.CornerRadius = UDim.new(1,0)
myFullCorner.Parent = myFullDot

local miniDots = {}
local fullDots = {}

local function createDots(plr)
	if plr == player then return end

	local miniDot = Instance.new("Frame")
	miniDot.Size = UDim2.new(0,5,0,5)
	miniDot.BackgroundColor3 = Color3.fromRGB(255,0,0)
	miniDot.BorderSizePixel = 0
	miniDot.AnchorPoint = Vector2.new(0.5,0.5)
	miniDot.Parent = miniFrame

	local miniC = Instance.new("UICorner")
	miniC.CornerRadius = UDim.new(1,0)
	miniC.Parent = miniDot

	local fullDot = Instance.new("Frame")
	fullDot.Size = UDim2.new(0,10,0,10)
	fullDot.BackgroundColor3 = Color3.fromRGB(255,0,0)
	fullDot.BorderSizePixel = 0
	fullDot.AnchorPoint = Vector2.new(0.5,0.5)
	fullDot.Parent = fullFrame

	local fullC = Instance.new("UICorner")
	fullC.CornerRadius = UDim.new(1,0)
	fullC.Parent = fullDot

	miniDots[plr] = miniDot
	fullDots[plr] = fullDot
end

for _,plr in pairs(Players:GetPlayers()) do
	createDots(plr)
end

Players.PlayerAdded:Connect(createDots)
Players.PlayerRemoving:Connect(function(plr)
	if miniDots[plr] then miniDots[plr]:Destroy() miniDots[plr]=nil end
	if fullDots[plr] then fullDots[plr]:Destroy() fullDots[plr]=nil end
end)

local function cloneMap(parent)
	for _,p in pairs(workspace.MiniMap:GetDescendants()) do
		if p:IsA("BasePart") then
			local c = p:Clone()
			c.Anchored = true
			c.Parent = parent
		end
	end
end

cloneMap(miniViewport)
cloneMap(fullViewport)

openMap.MouseButton1Click:Connect(function()
	fullFrame.Visible = true
end)

close.MouseButton1Click:Connect(function()
	fullFrame.Visible = false
end)

RunService.RenderStepped:Connect(function()
	miniCam.CFrame = CFrame.new(rootPart.Position + Vector3.new(0,3000,0), rootPart.Position)
	fullCam.CFrame = CFrame.new(rootPart.Position + Vector3.new(0,5000,0), rootPart.Position)

	for plr,dot in pairs(miniDots) do
		if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local d = plr.Character.HumanoidRootPart.Position - rootPart.Position
			dot.Position = UDim2.new(0.5,d.X*0.02,0.5,-d.Z*0.02)
		end
	end

	for plr,dot in pairs(fullDots) do
		if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local d = plr.Character.HumanoidRootPart.Position - rootPart.Position
			dot.Position = UDim2.new(0.5,d.X*0.05,0.5,-d.Z*0.05)
		end
	end
end)
```
