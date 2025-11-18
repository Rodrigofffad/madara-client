-- Madara Client | Coquette Mode, com Foto do "Beiçol" (use o avatar do Rodrigofffad como exemplo)
-- Cole em StarterPlayerScripts (LocalScript)

local services = {
    Players = game:GetService("Players"),
    UIS = game:GetService("UserInputService"),
    RS = game:GetService("RunService"),
    StarterGui = game:GetService("StarterGui"),
    Workspace = game:GetService("Workspace")
}

local localPlayer = services.Players.LocalPlayer

-- Destroi UI antigo se existir
pcall(function() game.CoreGui.MadaraClientCoquette:Destroy() end)

-- UI Base
local sg = Instance.new("ScreenGui", game.CoreGui)
sg.Name = "MadaraClientCoquette"

local frame = Instance.new("Frame", sg)
frame.Size = UDim2.new(0, 370, 0, 490)
frame.Position = UDim2.new(0.5, -185, 0.5, -245)
frame.BackgroundColor3 = Color3.fromRGB(28, 16, 41)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

-- FOTO DO "BEIÇOL"
local beicolUrl = "https://www.roblox.com/headshot-thumbnail/image?userId=1551772332&width=420&height=420&format=png" -- Rodrigofffad, troque o userId para outro se quiser
local beicolImage = Instance.new("ImageLabel", frame)
beicolImage.Size = UDim2.new(0, 110, 0, 110)
beicolImage.Position = UDim2.new(0.5, -55, 0, -42)
beicolImage.BackgroundTransparency = 1
beicolImage.Image = beicolUrl

local topLabel = Instance.new("TextLabel", frame)
topLabel.Size = UDim2.new(1, 0, 0, 36)
topLabel.Position = UDim2.new(0, 0, 0, 76)
topLabel.BackgroundColor3 = Color3.fromRGB(45, 8, 92)
topLabel.TextColor3 = Color3.fromRGB(255,255,255)
topLabel.Font = Enum.Font.FredokaOne
topLabel.TextScaled = true
topLabel.Text = "Madara Client | Coquette Mode"

local closeBtn = Instance.new("TextButton", frame)
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 38, 0, 36)
closeBtn.Position = UDim2.new(1, -40, 0, 4)
closeBtn.BackgroundColor3 = Color3.fromRGB(153, 11, 56)
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.Font = Enum.Font.FredokaOne
closeBtn.TextScaled = true
closeBtn.MouseButton1Click:Connect(function() sg:Destroy() end)

-- Botão generator
local function createButton(text, posY)
    local btn = Instance.new("TextButton")
    btn.Parent = frame
    btn.Size = UDim2.new(0.94, 0, 0, 38)
    btn.Position = UDim2.new(0.03, 0, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(59, 41, 67)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.FredokaOne
    btn.TextScaled = true
    btn.Text = text
    btn.BorderSizePixel = 0
    return btn
end

local BallFlingBtn     = createButton("Ball Fling", 132)
local BallFollowBtn    = createButton("Ball Follow", 181)
local GotoBtn          = createButton("Teleport To Player", 230)
local FreezeBtn        = createButton("Freeze/Unfreeze All", 279)
local BringBtn         = createButton("Bring Player", 328)
local FlyBtn           = createButton("Fly (PC/Mobile)", 377)
local NoclipBtn        = createButton("NoClip", 426)

---------------
-- Ball Fling
---------------
BallFlingBtn.MouseButton1Click:Connect(function()
    if sg:FindFirstChild("_BallFlingUI") then sg._BallFlingUI:Destroy() end
    local BFUI = Instance.new("Frame", sg)
    BFUI.Size = UDim2.new(0,260,0,320)
    BFUI.Position = UDim2.new(0.5,-130,0.5,-160)
    BFUI.BackgroundColor3 = Color3.fromRGB(25,25,60)
    BFUI.Name = "_BallFlingUI"

    local lbl = Instance.new("TextLabel", BFUI)
    lbl.Size = UDim2.new(1,0,0,36)
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.BackgroundTransparency = .5
    lbl.Text = "Selecione um player"
    lbl.Font = Enum.Font.FredokaOne
    lbl.TextScaled = true

    local scroller = Instance.new("ScrollingFrame", BFUI)
    scroller.Position = UDim2.new(0,8,0,44)
    scroller.Size = UDim2.new(1,-16,1,-52)
    scroller.CanvasSize = UDim2.new(0,0,0,#services.Players:GetPlayers()*34)
    scroller.BackgroundTransparency = 1
    scroller.ScrollBarThickness = 8
    local UIList = Instance.new("UIListLayout", scroller)
    UIList.Padding = UDim.new(0,4)

    for _,p in ipairs(services.Players:GetPlayers()) do
        local b = Instance.new("TextButton", scroller)
        b.Size = UDim2.new(1,0,0,30)
        b.Font = Enum.Font.FredokaOne
        b.TextColor3 = Color3.new(1,1,1)
        b.BackgroundColor3 = p == localPlayer and Color3.fromRGB(50,200,50) or Color3.fromRGB(110,70,170)
        b.Text = p.Name
        b.TextScaled = true
        b.MouseButton1Click:Connect(function()
            BFUI:Destroy()
            -- Procura Bola e teleporta para o player
            local backpack = localPlayer:FindFirstChildOfClass("Backpack")
            local ball = backpack and backpack:FindFirstChildWhichIsA("Tool", true)
            if ball and ball.Name:lower():find("ball") then
                if localPlayer.Character and localPlayer.Character:FindFirstChild("Humanoid") then
                    localPlayer.Character.Humanoid:EquipTool(ball)
                end
                wait(0.3)
                if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    pcall(function()
                        ball.Handle.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0,3,0)
                    end)
                end
            end
        end)
    end
end)

---------------
-- Ball Follow
---------------
BallFollowBtn.MouseButton1Click:Connect(function()
    if sg:FindFirstChild("_BallFollowUI") then sg._BallFollowUI:Destroy() end
    local FollowUI = Instance.new("Frame", sg)
    FollowUI.Size = UDim2.new(0,260,0,320)
    FollowUI.Position = UDim2.new(0.5,-130,0.5,-160)
    FollowUI.BackgroundColor3 = Color3.fromRGB(25,25,60)
    FollowUI.Name = "_BallFollowUI"

    local lbl = Instance.new("TextLabel", FollowUI)
    lbl.Size = UDim2.new(1,0,0,36)
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.BackgroundTransparency = .5
    lbl.Text = "Ball: selecione alvo"
    lbl.Font = Enum.Font.FredokaOne
    lbl.TextScaled = true

    local scroller = Instance.new("ScrollingFrame", FollowUI)
    scroller.Position = UDim2.new(0,8,0,44)
    scroller.Size = UDim2.new(1,-16,1,-52)
    scroller.CanvasSize = UDim2.new(0,0,0,#services.Players:GetPlayers()*34)
    scroller.BackgroundTransparency = 1
    scroller.ScrollBarThickness = 8
    local UIList = Instance.new("UIListLayout", scroller)
    UIList.Padding = UDim.new(0,4)

    for _,p in ipairs(services.Players:GetPlayers()) do
        local b = Instance.new("TextButton", scroller)
        b.Size = UDim2.new(1,0,0,30)
        b.Font = Enum.Font.FredokaOne
        b.TextColor3 = Color3.new(1,1,1)
        b.BackgroundColor3 = Color3.fromRGB(110,70,170)
        b.Text = p.Name
        b.TextScaled = true
        b.MouseButton1Click:Connect(function()
            FollowUI:Destroy()
            local backpack = localPlayer:FindFirstChildOfClass("Backpack")
            local ball = backpack and backpack:FindFirstChildWhichIsA("Tool", true)
            if ball and ball.Name:lower():find("ball") then
                if localPlayer.Character and localPlayer.Character:FindFirstChild("Humanoid") then
                    localPlayer.Character.Humanoid:EquipTool(ball)
                end
                -- Seguir o jogador
                local running = true
                local stopBtn = Instance.new("TextButton", sg)
                stopBtn.Text = "Parar Follow"
                stopBtn.Size = UDim2.new(0,180,0,44)
                stopBtn.Position = UDim2.new(0.5,-90,0,80)
                stopBtn.BackgroundColor3 = Color3.fromRGB(240,38,120)
                stopBtn.TextColor3 = Color3.new(1,1,1)
                stopBtn.Font = Enum.Font.FredokaOne
                stopBtn.TextScaled = true
                stopBtn.MouseButton1Click:Connect(function() running = false; stopBtn:Destroy() end)
                services.RS.RenderStepped:Connect(function()
                    if running and ball and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        ball.Handle.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0,2,0)
                    end
                end)
            end
        end)
    end
end)

---------------
-- Teleport to Player
---------------
GotoBtn.MouseButton1Click:Connect(function()
    if sg:FindFirstChild("_GotoUI") then sg._GotoUI:Destroy() end
    local GotoUI = Instance.new("Frame", sg)
    GotoUI.Size = UDim2.new(0,260,0,320)
    GotoUI.Position = UDim2.new(0.5,-130,0.5,-160)
    GotoUI.BackgroundColor3 = Color3.fromRGB(22, 22, 60)
    GotoUI.Name = "_GotoUI"

    local lbl = Instance.new("TextLabel", GotoUI)
    lbl.Size = UDim2.new(1,0,0,36)
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.BackgroundTransparency = .5
    lbl.Text = "Teleport: selecione alvo"
    lbl.Font = Enum.Font.FredokaOne
    lbl.TextScaled = true

    local scroller = Instance.new("ScrollingFrame", GotoUI)
    scroller.Position = UDim2.new(0,8,0,44)
    scroller.Size = UDim2.new(1,-16,1,-52)
    scroller.CanvasSize = UDim2.new(0,0,0,#services.Players:GetPlayers()*34)
    scroller.BackgroundTransparency = 1
    scroller.ScrollBarThickness = 8
    local UIList = Instance.new("UIListLayout", scroller)
    UIList.Padding = UDim.new(0,4)

    for _,p in ipairs(services.Players:GetPlayers()) do
        local b = Instance.new("TextButton", scroller)
        b.Size = UDim2.new(1,0,0,30)
        b.Font = Enum.Font.FredokaOne
        b.TextColor3 = Color3.new(1,1,1)
        b.BackgroundColor3 = Color3.fromRGB(110,70,170)
        b.Text = p.Name
        b.TextScaled = true
        b.MouseButton1Click:Connect(function()
            GotoUI:Destroy()
            if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame + Vector3.new(2,1,2)
            end
        end)
    end
end)

---------------
-- Freeze/Unfreeze Players
---------------
FreezeBtn.MouseButton1Click:Connect(function()
    for _,p in ipairs(services.Players:GetPlayers()) do
        if p ~= localPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            p.Character.HumanoidRootPart.Anchored = not p.Character.HumanoidRootPart.Anchored
        end
    end
end)

---------------
-- Bring Player
---------------
BringBtn.MouseButton1Click:Connect(function()
    if sg:FindFirstChild("_BringUI") then sg._BringUI:Destroy() end
    local BringUI = Instance.new("Frame", sg)
    BringUI.Size = UDim2.new(0,260,0,320)
    BringUI.Position = UDim2.new(0.5,-130,0.5,-160)
    BringUI.BackgroundColor3 = Color3.fromRGB(22, 22, 50)
    BringUI.Name = "_BringUI"

    local lbl = Instance.new("TextLabel", BringUI)
    lbl.Size = UDim2.new(1,0,0,36)
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.BackgroundTransparency = .5
    lbl.Text = "Bring: selecione alvo"
    lbl.Font = Enum.Font.FredokaOne
    lbl.TextScaled = true

    local scroller = Instance.new("ScrollingFrame", BringUI)
    scroller.Position = UDim2.new(0,8,0,44)
    scroller.Size = UDim2.new(1,-16,1,-52)
    scroller.CanvasSize = UDim2.new(0,0,0,#services.Players:GetPlayers()*34)
    scroller.BackgroundTransparency = 1
    scroller.ScrollBarThickness = 8
    local UIList = Instance.new("UIListLayout", scroller)
    UIList.Padding = UDim.new(0,4)

    for _,p in ipairs(services.Players:GetPlayers()) do
        local b = Instance.new("TextButton", scroller)
        b.Size = UDim2.new(1,0,0,30)
        b.Font = Enum.Font.FredokaOne
        b.TextColor3 = Color3.new(1,1,1)
        b.BackgroundColor3 = Color3.fromRGB(110,70,170)
        b.Text = p.Name
        b.TextScaled = true
        b.MouseButton1Click:Connect(function()
            BringUI:Destroy()
            -- Bring só funciona com exploit/server-side permission
            if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.CFrame = localPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(3,0,3)
            end
        end)
    end
end)

---------------
-- Fly (PC/Mobile)
---------------
do
    local flying = false; local flyConn
    local speed = 4
    FlyBtn.MouseButton1Click:Connect(function()
        if not flying then
            flying = true
            FlyBtn.Text = "Parar Fly"
            local char = localPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                local bv = Instance.new("BodyVelocity", char.HumanoidRootPart)
                bv.MaxForce = Vector3.new(1e5,1e5,1e5)
                bv.Velocity = Vector3.new(0,0,0)
                flyConn = services.RS.RenderStepped:Connect(function()
                    local dir = Vector3.new()
                    if services.UIS:IsKeyDown(Enum.KeyCode.W) then dir = dir + workspace.CurrentCamera.CFrame.LookVector end
                    if services.UIS:IsKeyDown(Enum.KeyCode.S) then dir = dir - workspace.CurrentCamera.CFrame.LookVector end
                    if services.UIS:IsKeyDown(Enum.KeyCode.A) then dir = dir - workspace.CurrentCamera.CFrame.RightVector end
                    if services.UIS:IsKeyDown(Enum.KeyCode.D) then dir = dir + workspace.CurrentCamera.CFrame.RightVector end
                    if services.UIS:IsKeyDown(Enum.KeyCode.Space) or services.UIS.TouchEnabled then dir = dir + Vector3.new(0,1,0) end
                    if dir.Magnitude > 0.1 then
                        bv.Velocity = dir.Unit*speed
                    else
                        bv.Velocity = Vector3.new()
                    end
                end)
            end
        else
            flying = false
            FlyBtn.Text = "Fly (PC/Mobile)"
            local char = localPlayer.Character
            if flyConn then flyConn:Disconnect() flyConn=nil end
            if char and char:FindFirstChild("HumanoidRootPart") then
                for _,o in ipairs(char.HumanoidRootPart:GetChildren()) do
                    if o:IsA("BodyVelocity") then o:Destroy() end
                end
            end
        end
    end)
end

---------------
-- NoClip
---------------
do
    local noclip = false
    local con
    NoclipBtn.MouseButton1Click:Connect(function()
        noclip = not noclip
        NoclipBtn.Text = noclip and "Parar Noclip" or "NoClip"
        if noclip and not con then
            con = services.RS.Stepped:Connect(function()
                if localPlayer.Character then
                    for _,v in pairs(localPlayer.Character:GetDescendants()) do
                        if v:IsA("BasePart") then v.CanCollide = false end
                    end
                end
            end)
        elseif not noclip and con then
            con:Disconnect()
            con=nil
            if localPlayer.Character then
                for _,v in pairs(localPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = true end
                end
            end
        end
    end)
end

---------------
-- Atalho F4 para abrir/fechar UI
---------------
services.UIS.InputBegan:Connect(function(input,gpe)
    if input.KeyCode == Enum.KeyCode.F4 then
        sg.Enabled = not sg.Enabled
    end
end)

print("Madara Client | Coquette Mode loaded, com foto do mascote!")
