-- [[ NO COUNTER TEAM - ULTIMATE FINAL REMAKE ]]
-- Fix: Teks Marquee Tenggelam di Bawah Layar

local P = game:GetService("Players")
local LP = P.LocalPlayer
local TS = game:GetService("TweenService")
local RepS = game:GetService("ReplicatedStorage")
local UIS = game:GetService("UserInputService")
local SS = game:GetService("SoundService")

local SG_Parent = LP:WaitForChild("PlayerGui")

if SG_Parent:FindFirstChild("NoCounter_Ultimate_System") then
    SG_Parent:FindFirstChild("NoCounter_Ultimate_System"):Destroy()
end

local SG = Instance.new("ScreenGui")
SG.Name = "NoCounter_Ultimate_System"
SG.Parent = SG_Parent
SG.ResetOnSpawn = false

-- [[ AUDIO FUNCTION ]]
local function playSound(id)
    local s = Instance.new("Sound", SS)
    s.SoundId = "rbxassetid://"..tostring(id)
    s:Play()
    task.delay(1, function() s:Destroy() end)
end

-- [[ MAIN FRAME ]]
local MF = Instance.new("Frame", SG)
MF.AnchorPoint = Vector2.new(0.5, 0.5)
MF.Position = UDim2.new(0.5, 0, 0.5, 0)
MF.Size = UDim2.new(0, 750, 0, 360)
MF.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
MF.BorderSizePixel = 0
MF.Visible = false

local MF_Stroke = Instance.new("UIStroke", MF)
MF_Stroke.Thickness = 2

-- [[ TOP BAR ]]
local TopBar = Instance.new("Frame", MF)
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TopBar.BorderSizePixel = 0

local Title = Instance.new("TextLabel", TopBar)
Title.Size = UDim2.new(1, -80, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "NoCounter Backdoor System | Ultimate Remake"
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BackgroundTransparency = 1

local CloseBtn = Instance.new("TextButton", TopBar)
CloseBtn.Size = UDim2.new(0, 30, 0, 25)
CloseBtn.Position = UDim2.new(1, -35, 0.5, -12)
CloseBtn.Text = "x"
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
CloseBtn.TextColor3 = Color3.new(1,1,1)
CloseBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 5)

-- DRAG SYSTEM
local dragging, dragInput, dragStart, startPos
TopBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MF.Position
    end
end)
UIS.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        MF.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
UIS.InputEnded:Connect(function() dragging = false end)

-- [[ CONTENT - BOXES ]]
local Box = Instance.new("TextBox", MF)
Box.Size = UDim2.new(0.45, 0, 0, 220)
Box.Position = UDim2.new(0.03, 0, 0, 65)
Box.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Box.TextColor3 = Color3.new(0.9, 0.9, 0.9)
Box.TextSize = 14
Box.Text = ""
Box.PlaceholderText = "-- Put Script Here --"
Box.MultiLine = true
Box.ClearTextOnFocus = false
Box.Font = Enum.Font.Code
Box.TextXAlignment = Enum.TextXAlignment.Left
Box.TextYAlignment = Enum.TextYAlignment.Top
Instance.new("UIStroke", Box).Color = Color3.fromRGB(50, 50, 50)

local ScanResultBox = Instance.new("ScrollingFrame", MF)
ScanResultBox.Size = UDim2.new(0.45, 0, 0, 220)
ScanResultBox.Position = UDim2.new(0.52, 0, 0, 65)
ScanResultBox.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
ScanResultBox.BorderSizePixel = 0
ScanResultBox.ScrollBarThickness = 4
Instance.new("UIStroke", ScanResultBox).Color = Color3.fromRGB(50, 50, 50)

local ScanText = Instance.new("TextLabel", ScanResultBox)
ScanText.Size = UDim2.new(1, -10, 0, 0)
ScanText.Position = UDim2.new(0, 5, 0, 5)
ScanText.BackgroundTransparency = 1
ScanText.TextColor3 = Color3.fromRGB(0, 255, 0)
ScanText.TextSize = 12
ScanText.Font = Enum.Font.Code
ScanText.Text = "Waiting for scan..."
ScanText.TextXAlignment = Enum.TextXAlignment.Left
ScanText.TextYAlignment = Enum.TextYAlignment.Top
ScanText.AutomaticSize = Enum.AutomaticSize.Y

-- [[ BUTTONS ]]
local function createBtn(text, pos, color)
    local b = Instance.new("TextButton", MF)
    b.Size = UDim2.new(0.25, 0, 0, 35)
    b.Position = pos
    b.Text = text
    b.BackgroundColor3 = color
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 12
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 4)
    return b
end

local ScanBtn = createBtn("SCAN REMOTE", UDim2.new(0.52, 0, 0, 300), Color3.fromRGB(150, 0, 0))
local ExecBtn = createBtn("EXECUTE", UDim2.new(0.03, 0, 0, 300), Color3.fromRGB(200, 0, 0))
local ClearBtn = createBtn("CLEAR EXEC", UDim2.new(0.3, 0, 0, 300), Color3.fromRGB(40, 40, 40))

-- [[ LOGIC ]]
ScanBtn.MouseButton1Click:Connect(function()
    playSound(7593297942)
    ScanText.Text = "--- SCANNING... ---\n"
    local found = 0
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("RemoteEvent") or v:IsA("RemoteFunction") then
            found = found + 1
            ScanText.Text = ScanText.Text .. "[" .. found .. "] " .. v.ClassName .. ":\n" .. v:GetFullName() .. "\n\n"
        end
    end
    if found == 0 then ScanText.Text = "No Remotes Found." end
end)

ExecBtn.MouseButton1Click:Connect(function()
    playSound(7593297942)
    local code = Box.Text
    for _, v in pairs(RepS:GetDescendants()) do
        if v:IsA("RemoteEvent") then pcall(function() v:FireServer(code) end) end
    end
end)

ClearBtn.MouseButton1Click:Connect(function() Box.Text = "" end)
CloseBtn.MouseButton1Click:Connect(function() SG:Destroy() end)

-- [[ TOGGLE BUTTON ]]
local Tgl = Instance.new("TextButton", SG)
Tgl.Name = "Toggle"
Tgl.Size = UDim2.new(0, 60, 0, 25)
Tgl.Position = UDim2.new(0, 100, 0, 5)
Tgl.BackgroundColor3 = Color3.new(0,0,0)
Tgl.Text = "MENU"
Tgl.TextColor3 = Color3.new(1,1,1)
Tgl.Font = Enum.Font.GothamBold
Tgl.TextSize = 12
Tgl.Visible = false
Instance.new("UICorner", Tgl)
local TStroke = Instance.new("UIStroke", Tgl)
TStroke.Thickness = 1.5

-- [[ LOADING SYSTEM + MARQUEE FIX ]]
local function StartLoading()
    local L_Frame = Instance.new("Frame", SG)
    L_Frame.Size = UDim2.new(0, 280, 0, 160)
    L_Frame.Position = UDim2.new(0.5, -140, 0.5, -80)
    L_Frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    local LS = Instance.new("UIStroke", L_Frame)
    LS.Thickness = 2
    
    local Logo = Instance.new("ImageLabel", L_Frame)
    Logo.AnchorPoint = Vector2.new(0.5, 0.5)
    Logo.Position = UDim2.new(0.5, 0, 0.35, 0)
    Logo.Size = UDim2.new(0, 80, 0, 80)
    Logo.BackgroundTransparency = 1
    Logo.Image = "rbxassetid://113430762146825"
    
    local L_Status = Instance.new("TextLabel", L_Frame)
    L_Status.Size = UDim2.new(1, 0, 0, 30)
    L_Status.Position = UDim2.new(0, 0, 0.65, 0)
    L_Status.BackgroundTransparency = 1
    L_Status.Text = "INITIALIZING..."
    L_Status.TextColor3 = Color3.new(1,1,1)
    L_Status.Font = Enum.Font.GothamBold
    L_Status.TextSize = 18

    local L_BarBG = Instance.new("Frame", L_Frame)
    L_BarBG.Size = UDim2.new(0.85, 0, 0, 6)
    L_BarBG.Position = UDim2.new(0.075, 0, 0.85, 0)
    L_BarBG.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    local L_Bar = Instance.new("Frame", L_BarBG)
    L_Bar.Size = UDim2.new(0, 0, 1, 0)
    L_Bar.BackgroundColor3 = Color3.new(1, 0, 0)

    -- [[ MARQUEE SYSTEM (RAINBOW & SPACING FIX) ]]
    local MarqueeFrame = Instance.new("Frame", SG)
    MarqueeFrame.Size = UDim2.new(1, 0, 0, 60)
    -- POSISI DIPERBAIKI: Menggunakan scale 1 untuk bawah, lalu offset -65 agar teks naik ke atas layar dan tidak tenggelam
    MarqueeFrame.Position = UDim2.new(0, 0, 1, -65) 
    MarqueeFrame.BackgroundTransparency = 1
    MarqueeFrame.ClipsDescendants = true
    
    local ScrollText = Instance.new("TextLabel", MarqueeFrame)
    ScrollText.Size = UDim2.new(6, 0, 1, 0)
    ScrollText.BackgroundTransparency = 1
    ScrollText.Text = "--   NO   COUNTER   BACKDOOR   |   BY   FERTOURXKENZOO   |   NO   COUNTER   TEAM   --                                                                         "
    ScrollText.Font = Enum.Font.GothamBold
    ScrollText.TextSize = 28
    
    -- Stroke Hitam agar teks Rainbow tetap terbaca di map terang
    local NS = Instance.new("UIStroke", ScrollText)
    NS.Thickness = 3.5
    NS.Color = Color3.new(0,0,0)

    -- RAINBOW LOOP
    task.spawn(function()
        while SG.Parent do
            local h = tick() % 5 / 5
            local rainbow = Color3.fromHSV(h, 1, 1)

            LS.Color = rainbow
            TStroke.Color = rainbow
            ScrollText.TextColor3 = rainbow
            MF_Stroke.Color = rainbow
            
            Logo.Rotation = Logo.Rotation + 3
            ScrollText.Position = ScrollText.Position - UDim2.new(0, 3, 0, 0)
            
            if ScrollText.Position.X.Offset < -4500 then 
                ScrollText.Position = UDim2.new(1, 0, 0, 0) 
            end
            task.wait()
        end
    end)

    for i = 0, 100 do
        L_Bar.Size = UDim2.new(i/100, 0, 1, 0)
        if i < 40 then L_Status.Text = "LOADING SYSTEM..."
        elseif i < 80 then L_Status.Text = "SCANNING BACKDOORS..."
        else L_Status.Text = "READY!" end
        task.wait(0.2)
    end
    
    L_Frame:Destroy()
    MarqueeFrame:Destroy()
    MF.Visible = true
    Tgl.Visible = true
end

Tgl.MouseButton1Click:Connect(function() MF.Visible = not MF.Visible end)
StartLoading()
