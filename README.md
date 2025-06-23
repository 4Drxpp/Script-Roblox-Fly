# Script-Roblox-Fly
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local root = char:WaitForChild("HumanoidRootPart")

-- GUI principale
local gui = Instance.new("ScreenGui")
gui.Name = "VolerV1"
gui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 110)
frame.Position = UDim2.new(0.1, 0, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.BackgroundTransparency = 0.1
frame.Active = true
frame.Draggable = true
frame.ClipsDescendants = true

local uicorner = Instance.new("UICorner", frame)
uicorner.CornerRadius = UDim.new(0, 8)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 25)
title.BackgroundTransparency = 1
title.Text = "🚀  Volée V1"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 18

local closeBtn = Instance.new("TextButton", frame)
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 20, 0, 20)
closeBtn.Position = UDim2.new(1, -25, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 14
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 4)

closeBtn.MouseButton1Click:Connect(function()
	gui:Destroy()
end)

local minimizeBtn = Instance.new("TextButton", frame)
minimizeBtn.Text = "-"
minimizeBtn.Size = UDim2.new(0, 20, 0, 20)
minimizeBtn.Position = UDim2.new(1, -50, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 14
Instance.new("UICorner", minimizeBtn).CornerRadius = UDim.new(0, 4)

local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	for _, child in ipairs(frame:GetChildren()) do
		if child ~= title and child ~= minimizeBtn and child ~= closeBtn and child ~= uicorner then
			child.Visible = not minimized
		end
	end
end)

local function createButton(name, position, text)
	local button = Instance.new("TextButton", frame)
	button.Name = name
	button.Position = position
	button.Size = UDim2.new(0, 60, 0, 25)
	button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	button.TextColor3 = Color3.fromRGB(255, 255, 255)
	button.Font = Enum.Font.Gotham
	button.TextSize = 14
	button.Text = text
	Instance.new("UICorner", button).CornerRadius = UDim.new(0, 6)
	return button
end

local flyBtn = createButton("FlyToggle", UDim2.new(0, 10, 0, 35), "Volée")
local upBtn = createButton("Haut", UDim2.new(0, 80, 0, 35), "↑ Haut")
local downBtn = createButton("Bas", UDim2.new(0, 150, 0, 35), "↓ Bas")
local plusBtn = createButton("SpeedPlus", UDim2.new(0, 10, 0, 70), "+ Vitesse")
local minusBtn = createButton("SpeedMinus", UDim2.new(0, 80, 0, 70), "- Vitesse")

local speedLabel = Instance.new("TextLabel", frame)
speedLabel.Size = UDim2.new(0, 60, 0, 25)
speedLabel.Position = UDim2.new(0, 150, 0, 70)
speedLabel.BackgroundTransparency = 1
speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedLabel.Font = Enum.Font.Gotham
speedLabel.TextSize = 15
speedLabel.Text = "Vitesse: 1"

-- Message en bas à droite au lancement
local messageGui = Instance.new("ScreenGui")
messageGui.Name = "MessageVoler"
messageGui.Parent = player:WaitForChild("PlayerGui")

local messageLabel = Instance.new("TextLabel", messageGui)
messageLabel.Size = UDim2.new(0, 220, 0, 40)
messageLabel.Position = UDim2.new(1, -230, 1, -50)  -- bas droite, 10 px marge à droite
messageLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
messageLabel.BackgroundTransparency = 0.3
messageLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
messageLabel.Font = Enum.Font.GothamBold
messageLabel.TextSize = 20
messageLabel.Text = "🚀 Volée V1 pars 4Drxpp"
messageLabel.TextStrokeTransparency = 0.7
messageLabel.BorderSizePixel = 0
messageLabel.TextXAlignment = Enum.TextXAlignment.Left
messageLabel.TextYAlignment = Enum.TextYAlignment.Center
local msgCorner = Instance.new("UICorner", messageLabel)
msgCorner.CornerRadius = UDim.new(0, 8)

local function fadeOutLabel(label, duration)
    local steps = 20
    local delayTime = duration / steps
    for i = 1, steps do
        local alpha = i / steps
        label.BackgroundTransparency = 0.3 + alpha * 0.7  -- passe de 0.3 à 1
        label.TextTransparency = alpha
        label.TextStrokeTransparency = 0.7 + alpha * 0.3 -- passe de 0.7 à 1
        wait(delayTime)
    end
    label.Parent:Destroy()
end

-- Message centré (instructions clavier)
local centerGui = Instance.new("ScreenGui")
centerGui.Name = "InstructionFly"
centerGui.Parent = player:WaitForChild("PlayerGui")

local instructionLabel = Instance.new("TextLabel", centerGui)
instructionLabel.Size = UDim2.new(0, 420, 0, 40)
instructionLabel.Position = UDim2.new(0.5, -210, 0.4, -20)
instructionLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
instructionLabel.BackgroundTransparency = 0.2
instructionLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
instructionLabel.Font = Enum.Font.GothamBold
instructionLabel.TextSize = 20
instructionLabel.Text = "⚙️  F = Volée , Espace = Haut , Ctrl = Bas"
instructionLabel.TextStrokeTransparency = 0.7
instructionLabel.BorderSizePixel = 0
instructionLabel.TextXAlignment = Enum.TextXAlignment.Center
instructionLabel.TextYAlignment = Enum.TextYAlignment.Center
Instance.new("UICorner", instructionLabel).CornerRadius = UDim.new(0, 8)

spawn(function()
	wait(5)
	fadeOutLabel(instructionLabel, 1.5)
end)


spawn(function()
	wait(3)
	fadeOutLabel(messageLabel, 1.5)
end)

-- Fly Logic
local flying = false
local speed = 1
local bodyGyro, bodyVelocity
local controls = {W=false,A=false,S=false,D=false,Space=false,Ctrl=false}

local UIS = game:GetService("UserInputService")

local function toggleFly()
	if flying then
		flying = false
		humanoid.PlatformStand = false
		if bodyGyro then bodyGyro:Destroy() end
		if bodyVelocity then bodyVelocity:Destroy() end
	else
		flying = true
		humanoid.PlatformStand = true

		bodyGyro = Instance.new("BodyGyro", root)
		bodyGyro.P = 9e4
		bodyGyro.maxTorque = Vector3.new(9e9,9e9,9e9)

		bodyVelocity = Instance.new("BodyVelocity", root)
		bodyVelocity.maxForce = Vector3.new(9e9,9e9,9e9)

		spawn(function()
			while flying and char and char.Parent do
				local cam = workspace.CurrentCamera.CFrame
				bodyGyro.CFrame = cam
				local dir = Vector3.zero
				if controls.W then dir += cam.LookVector end
				if controls.S then dir -= cam.LookVector end
				if controls.A then dir -= cam.RightVector end
				if controls.D then dir += cam.RightVector end
				if controls.Space then dir += Vector3.new(0,1,0) end
				if controls.Ctrl then dir -= Vector3.new(0,1,0) end
				if dir.Magnitude > 0 then
					bodyVelocity.Velocity = dir.Unit * speed * 20
				else
					bodyVelocity.Velocity = Vector3.zero
				end
				wait()
			end
		end)
	end
end

UIS.InputBegan:Connect(function(input, gpe)
	if gpe then return end
	if input.KeyCode == Enum.KeyCode.W then controls.W = true end
	if input.KeyCode == Enum.KeyCode.A then controls.A = true end
	if input.KeyCode == Enum.KeyCode.S then controls.S = true end
	if input.KeyCode == Enum.KeyCode.D then controls.D = true end
	if input.KeyCode == Enum.KeyCode.Space then controls.Space = true end
	if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then controls.Ctrl = true end
	if input.KeyCode == Enum.KeyCode.F then
		toggleFly()
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.W then controls.W = false end
	if input.KeyCode == Enum.KeyCode.A then controls.A = false end
	if input.KeyCode == Enum.KeyCode.S then controls.S = false end
	if input.KeyCode == Enum.KeyCode.D then controls.D = false end
	if input.KeyCode == Enum.KeyCode.Space then controls.Space = false end
	if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then controls.Ctrl = false end
end)

-- GUI Events
flyBtn.MouseButton1Click:Connect(toggleFly)
upBtn.MouseButton1Click:Connect(function()
	root.CFrame += CFrame.new(0, 3, 0)
end)
downBtn.MouseButton1Click:Connect(function()
	root.CFrame -= CFrame.new(0, 3, 0)
end)
plusBtn.MouseButton1Click:Connect(function()
	speed += 1
	speedLabel.Text = "Vitesse: " .. speed
end)
minusBtn.MouseButton1Click:Connect(function()
	if speed > 1 then
		speed -= 1
		speedLabel.Text = "Vitesse: " .. speed
	end
end)
