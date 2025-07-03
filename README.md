local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local isGUIOpen = false
local speedValue = 16
local speedConnection = nil

local function createFloatingBall()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "SpeedHubBall"
    screenGui.Parent = playerGui
    screenGui.ResetOnSpawn = false
    
    local ball = Instance.new("TextButton")
    ball.Name = "Ball"
    ball.Size = UDim2.new(0, 80, 0, 80)
    ball.Position = UDim2.new(0, 20, 0, 100)
    ball.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    ball.BorderSizePixel = 0
    ball.Text = "Speed Hub"
    ball.TextColor3 = Color3.fromRGB(255, 100, 100)
    ball.TextSize = 12
    ball.Font = Enum.Font.SourceSans
    ball.Parent = screenGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0.5, 0)
    corner.Parent = ball
    
    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(255, 0, 0)
    stroke.Thickness = 2
    stroke.Parent = ball
    
    local pulseInfo = TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    local pulseTween = TweenService:Create(stroke, pulseInfo, {Thickness = 4})
    pulseTween:Play()
    
    local draggingBall = false
    local dragStart = nil
    local startPos = nil
    
    ball.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            draggingBall = true
            dragStart = input.Position
            startPos = ball.Position
        end
    end)
    
    ball.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            if draggingBall then
                local delta = input.Position - dragStart
                ball.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end
    end)
    
    ball.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            draggingBall = false
        end
    end)
    
    return ball
end

local function createMainGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "SpeedHackGUI"
    screenGui.Parent = playerGui
    screenGui.ResetOnSpawn = false
    
    local mainFrame = Instance.new("Frame")
    mainFrame.Name = "MainFrame"
    mainFrame.Size = UDim2.new(0, 300, 0, 200)
    mainFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
    mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    mainFrame.BorderSizePixel = 0
    mainFrame.Visible = false
    mainFrame.Parent = screenGui
    
    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 10)
    mainCorner.Parent = mainFrame
    
    local mainStroke = Instance.new("UIStroke")
    mainStroke.Color = Color3.fromRGB(0, 200, 255)
    mainStroke.Thickness = 2
    mainStroke.Parent = mainFrame
    
    local title = Instance.new("TextLabel")
    title.Name = "Title"
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 0, 0, 5)
    title.BackgroundTransparency = 1
    title.Text = "🚀 Speed Hub"
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.TextScaled = true
    title.Font = Enum.Font.SourceSansBold
    title.Parent = mainFrame
    
    local credits = Instance.new("TextLabel")
    credits.Name = "Credits"
    credits.Size = UDim2.new(1, 0, 0, 15)
    credits.Position = UDim2.new(0, 0, 0, 30)
    credits.BackgroundTransparency = 1
    credits.Text = "By MTS13GAMER"
    credits.TextColor3 = Color3.fromRGB(150, 150, 150)
    credits.TextScaled = true
    credits.Font = Enum.Font.SourceSans
    credits.Parent = mainFrame
    
    local closeButton = Instance.new("TextButton")
    closeButton.Name = "CloseButton"
    closeButton.Size = UDim2.new(0, 30, 0, 30)
    closeButton.Position = UDim2.new(1, -35, 0, 5)
    closeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    closeButton.Text = "X"
    closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    closeButton.TextScaled = true
    closeButton.Font = Enum.Font.SourceSansBold
    closeButton.Parent = mainFrame
    
    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 5)
    closeCorner.Parent = closeButton
    
    local speedLabel = Instance.new("TextLabel")
    speedLabel.Name = "SpeedLabel"
    speedLabel.Size = UDim2.new(1, -20, 0, 25)
    speedLabel.Position = UDim2.new(0, 10, 0, 55)
    speedLabel.BackgroundTransparency = 1
    speedLabel.Text = "Velocidade: " .. speedValue
    speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    speedLabel.TextScaled = true
    speedLabel.Font = Enum.Font.SourceSans
    speedLabel.Parent = mainFrame
    
    local sliderFrame = Instance.new("Frame")
    sliderFrame.Name = "SliderFrame"
    sliderFrame.Size = UDim2.new(1, -20, 0, 20)
    sliderFrame.Position = UDim2.new(0, 10, 0, 85)
    sliderFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    sliderFrame.BorderSizePixel = 0
    sliderFrame.Parent = mainFrame
    
    local sliderCorner = Instance.new("UICorner")
    sliderCorner.CornerRadius = UDim.new(0, 10)
    sliderCorner.Parent = sliderFrame
    
    local sliderButton = Instance.new("TextButton")
    sliderButton.Name = "SliderButton"
    sliderButton.Size = UDim2.new(0, 20, 1, 0)
    sliderButton.Position = UDim2.new(0, 0, 0, 0)
    sliderButton.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
    sliderButton.Text = ""
    sliderButton.Parent = sliderFrame
    
    local sliderButtonCorner = Instance.new("UICorner")
    sliderButtonCorner.CornerRadius = UDim.new(0, 10)
    sliderButtonCorner.Parent = sliderButton
    
    local speedButtons = {
        {text = "Normal (16)", value = 16},
        {text = "Rápido (50)", value = 50},
        {text = "Muito Rápido (100)", value = 100},
        {text = "Extremo (200)", value = 200}
    }
    
    for i, buttonData in ipairs(speedButtons) do
        local speedButton = Instance.new("TextButton")
        speedButton.Name = "SpeedButton" .. i
        speedButton.Size = UDim2.new(0.48, 0, 0, 25)
        speedButton.Position = UDim2.new(((i-1) % 2) * 0.52, 0, 0, 115 + math.floor((i-1) / 2) * 30)
        speedButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        speedButton.Text = buttonData.text
        speedButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        speedButton.TextScaled = true
        speedButton.Font = Enum.Font.SourceSans
        speedButton.Parent = mainFrame
        
        local buttonCorner = Instance.new("UICorner")
        buttonCorner.CornerRadius = UDim.new(0, 5)
        buttonCorner.Parent = speedButton
        
        speedButton.MouseButton1Click:Connect(function()
            setSpeed(buttonData.value)
            speedLabel.Text = "Velocidade: " .. buttonData.value
            updateSliderPosition(buttonData.value)
        end)
    end
    
    function updateSliderPosition(speed)
        local percentage = math.clamp((speed - 16) / (200 - 16), 0, 1)
        sliderButton.Position = UDim2.new(percentage, -10, 0, 0)
    end
    
    local dragging = false
    
    sliderButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    local function updateSlider(inputPosition)
        if dragging then
            local sliderPosition = sliderFrame.AbsolutePosition.X
            local sliderSize = sliderFrame.AbsoluteSize.X
            local inputX = inputPosition.X
            
            local relativeX = math.clamp(inputX - sliderPosition, 0, sliderSize - 20)
            local percentage = relativeX / (sliderSize - 20)
            
            sliderButton.Position = UDim2.new(0, relativeX, 0, 0)
            
            local newSpeed = math.floor(16 + (percentage * (200 - 16)))
            speedValue = newSpeed
            speedLabel.Text = "Velocidade: " .. newSpeed
            setSpeed(newSpeed)
        end
    end
    
    RunService.Heartbeat:Connect(function()
        if dragging then
            local mouse = Players.LocalPlayer:GetMouse()
            updateSlider(Vector2.new(mouse.X, mouse.Y))
        end
    end)
    
    sliderFrame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch and dragging then
            updateSlider(input.Position)
        end
    end)
    
    closeButton.MouseButton1Click:Connect(function()
        toggleGUI()
    end)
    
    local draggingGUI = false
    local dragStart = nil
    local startPos = nil
    
    mainFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            draggingGUI = true
            dragStart = input.Position
            startPos = mainFrame.Position
        end
    end)
    
    mainFrame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            if draggingGUI then
                local delta = input.Position - dragStart
                mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end
    end)
    
    mainFrame.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            draggingGUI = false
        end
    end)
    
    return mainFrame
end

function setSpeed(speed)
    speedValue = speed
    
    if speedConnection then
        speedConnection:Disconnect()
    end
    
    speedConnection = RunService.Heartbeat:Connect(function()
        local character = player.Character
        if character then
            local humanoid = character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = speed
            end
        end
    end)
end

function toggleGUI()
    local gui = playerGui:FindFirstChild("SpeedHackGUI")
    if gui then
        local mainFrame = gui:FindFirstChild("MainFrame")
        if mainFrame then
            isGUIOpen = not isGUIOpen
            mainFrame.Visible = isGUIOpen
            
            if isGUIOpen then
                mainFrame.Size = UDim2.new(0, 0, 0, 0)
                mainFrame.Visible = true
                
                local openTween = TweenService:Create(mainFrame, 
                    TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out),
                    {Size = UDim2.new(0, 300, 0, 200)}
                )
                openTween:Play()
            else
                local closeTween = TweenService:Create(mainFrame,
                    TweenInfo.new(0.2, Enum.EasingStyle.Back, Enum.EasingDirection.In),
                    {Size = UDim2.new(0, 0, 0, 0)}
                )
                closeTween:Play()
                
                closeTween.Completed:Connect(function()
                    mainFrame.Visible = false
                end)
            end
        end
    end
end

local function initialize()
    local floatingBall = createFloatingBall()
    local mainGUI = createMainGUI()
    
    local clickStartTime = 0
    local clickThreshold = 0.2
    
    floatingBall.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            clickStartTime = tick()
        end
    end)
    
    floatingBall.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            local clickDuration = tick() - clickStartTime
            if clickDuration < clickThreshold then
                toggleGUI()
            end
        end
    end)
    
    floatingBall.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            local hoverTween = TweenService:Create(floatingBall,
                TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
                {Size = UDim2.new(0, 90, 0, 90)}
            )
            hoverTween:Play()
        end
    end)
    
    floatingBall.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            local leaveTween = TweenService:Create(floatingBall,
                TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
                {Size = UDim2.new(0, 80, 0, 80)}
            )
            leaveTween:Play()
        end
    end)
    
    setSpeed(speedValue)
    
    print("🚀 Speed Hub carregado com sucesso!")
    print("👆 Clique na bolinha 'Speed Hub' para abrir a interface")
    print("📱 Compatível com Mobile e PC")
    print("🖱️ Arraste a bolinha e a GUI para mover")
    print("📝 Criado por MTS13GAMER")
end

if player.Character then
    initialize()
else
    player.CharacterAdded:Connect(initialize)
end

player.CharacterAdded:Connect(function()
    wait(1)
    setSpeed(speedValue)
end)
