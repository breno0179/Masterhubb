-- // MASTERHUB - VmasterScripts (Anúncio + Hub Completo)
-- GUARDA: este script cria primeiro um anúncio de 10s e só depois carrega o Hub.
-- GUI do Hub segue o estilo antigo (top bar com abas e botões lisos).

local Players            = game:GetService("Players")
local RunService         = game:GetService("RunService")
local ReplicatedStorage  = game:GetService("ReplicatedStorage")
local TweenService       = game:GetService("TweenService")
local LocalPlayer        = Players.LocalPlayer or Players.PlayerAdded:Wait()

------------------------------------------------------------
-- [1] ANÚNCIO (bloqueia até acabar) - 10 segundos
------------------------------------------------------------
do
    local screenGui = Instance.new("ScreenGui")
    screenGui.IgnoreGuiInset = true
    screenGui.Name = "VMaster_Ad"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0,0,0,0)
    frame.Position = UDim2.new(0.5,0,0.5,0)
    frame.AnchorPoint = Vector2.new(0.5,0.5)
    frame.BackgroundColor3 = Color3.fromRGB(25,25,25)
    frame.Parent = screenGui
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0,15)

    local stroke = Instance.new("UIStroke", frame)
    stroke.Thickness = 2
    stroke.Color = Color3.fromRGB(0,170,255)

    TweenService:Create(frame, TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0.42,0,0.26,0)}):Play()

    local title = Instance.new("TextLabel", frame)
    title.Size = UDim2.new(1,0,0.3,0)
    title.Position = UDim2.new(0,0,0.05,0)
    title.BackgroundTransparency = 1
    title.Text = "⭐ MEU CANAL DO YOUTUBE ⭐"
    title.TextColor3 = Color3.fromRGB(0,170,255)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold

    local subtitle = Instance.new("TextLabel", frame)
    subtitle.Size = UDim2.new(1,0,0.2,0)
    subtitle.Position = UDim2.new(0,0,0.35,0)
    subtitle.BackgroundTransparency = 1
    subtitle.Text = "VmasterScripts"
    subtitle.TextColor3 = Color3.new(1,1,1)
    subtitle.TextScaled = true
    subtitle.Font = Enum.Font.Gotham

    local countdown = Instance.new("TextLabel", frame)
    countdown.Size = UDim2.new(1,0,0.2,0)
    countdown.Position = UDim2.new(0,0,0.55,0)
    countdown.BackgroundTransparency = 1
    countdown.TextColor3 = Color3.new(1,1,1)
    countdown.TextScaled = true
    countdown.Font = Enum.Font.GothamBold

    local copyButton = Instance.new("TextButton", frame)
    copyButton.Size = UDim2.new(0.7,0,0.2,0)
    copyButton.Position = UDim2.new(0.15,0,0.8,0)
    copyButton.BackgroundColor3 = Color3.fromRGB(255,70,70) -- vermelho
    copyButton.Text = "🔗 COPIAR LINK DO CANAL"
    copyButton.TextColor3 = Color3.new(1,1,1)
    copyButton.TextScaled = true
    copyButton.Font = Enum.Font.GothamBold
    Instance.new("UICorner", copyButton).CornerRadius = UDim.new(0,8)

    copyButton.MouseButton1Click:Connect(function()
        if setclipboard then
            setclipboard("https://youtube.com/@vmasterscripts?si=Q31EKAwobbttKha0")
        end
        copyButton.BackgroundColor3 = Color3.fromRGB(0,200,0)
        copyButton.Text = "✅ COPIADO!"
        task.wait(1.5)
        copyButton.BackgroundColor3 = Color3.fromRGB(255,70,70)
        copyButton.Text = "🔗 COPIAR LINK DO CANAL"
    end)

    local timeLeft = 10
    while timeLeft > 0 do
        countdown.Text = "Abrindo Hub em "..timeLeft.."s..."
        timeLeft -= 1
        task.wait(1)
    end

    -- saída
    TweenService:Create(frame, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0,0,0,0)}):Play()
    task.wait(0.5)
    screenGui:Destroy()
end

------------------------------------------------------------
-- [2] HUB (estilo antigo original)
------------------------------------------------------------
-- Helpers
local function getHRP()
    local ch = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    return ch:FindFirstChild("HumanoidRootPart")
end

local function safeTP(cf)
    local hrp = getHRP()
    if hrp and cf then hrp.CFrame = cf end
end

local function msg(text, color, timeSec)
    local gui = LocalPlayer:FindFirstChild("PlayerGui")
    if not gui then return end
    local m = Instance.new("TextLabel")
    m.Size = UDim2.new(0, 320, 0, 40)
    m.Position = UDim2.new(0.5, -160, 0.1, 0)
    m.BackgroundColor3 = color or Color3.fromRGB(30,30,30)
    m.TextColor3 = Color3.new(1,1,1)
    m.Text = text
    m.Font = Enum.Font.GothamBold
    m.TextSize = 16
    m.Parent = gui
    Instance.new("UICorner", m).CornerRadius = UDim.new(0,10)
    task.delay(timeSec or 3, function() if m then m:Destroy() end end)
end

-- GUI principal (igual estrutura base, com 4 abas)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "VMasterPainel"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Botão abrir/fechar
local OpenBtn = Instance.new("ImageButton")
OpenBtn.Size = UDim2.new(0,50,0,50)
OpenBtn.Position = UDim2.new(0,10,0,10)
OpenBtn.BackgroundColor3 = Color3.fromRGB(0,170,255)
OpenBtn.Image = "rbxassetid://133647114342582" -- seu ID
OpenBtn.Parent = ScreenGui
Instance.new("UICorner", OpenBtn).CornerRadius = UDim.new(1,0)

-- Frame principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 320, 0, 420)
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Visible = true -- já libera após anúncio
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

OpenBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Título
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Title.Text = "MasterHUB - VMasterScripts"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Instance.new("UICorner", Title).CornerRadius = UDim.new(0, 12)

-- Abas (4)
local TabsFrame = Instance.new("Frame", MainFrame)
TabsFrame.Size = UDim2.new(1, 0, 0, 35)
TabsFrame.Position = UDim2.new(0,0,0,40)
TabsFrame.BackgroundColor3 = Color3.fromRGB(25,25,25)

local TeleportsFrame = Instance.new("Frame", MainFrame)
TeleportsFrame.Size = UDim2.new(1,0,1,-75)
TeleportsFrame.Position = UDim2.new(0,0,0,75)
TeleportsFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
TeleportsFrame.Visible = true

local TrabalhosFrame = Instance.new("Frame", MainFrame)
TrabalhosFrame.Size = UDim2.new(1,0,1,-75)
TrabalhosFrame.Position = UDim2.new(0,0,0,75)
TrabalhosFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
TrabalhosFrame.Visible = false

local AutoFarmsFrame = Instance.new("Frame", MainFrame)
AutoFarmsFrame.Size = UDim2.new(1,0,1,-75)
AutoFarmsFrame.Position = UDim2.new(0,0,0,75)
AutoFarmsFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
AutoFarmsFrame.Visible = false

local PlayerFrame = Instance.new("Frame", MainFrame)
PlayerFrame.Size = UDim2.new(1,0,1,-75)
PlayerFrame.Position = UDim2.new(0,0,0,75)
PlayerFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
PlayerFrame.Visible = false

local function switchTab(tab)
    TeleportsFrame.Visible = false
    TrabalhosFrame.Visible = false
    AutoFarmsFrame.Visible = false
    PlayerFrame.Visible = false
    tab.Visible = true
end

local function createTabButton(name, xPos, tabFrame)
    local btn = Instance.new("TextButton", TabsFrame)
    btn.Size = UDim2.new(0,80,1,0)
    btn.Position = UDim2.new(0,xPos,0,0)
    btn.BackgroundColor3 = Color3.fromRGB(0,170,255)
    btn.Text = name
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,8)
    btn.MouseButton1Click:Connect(function()
        switchTab(tabFrame)
    end)
end

createTabButton("Teleportes",  0,  TeleportsFrame)
createTabButton("Trabalhos",  80,  TrabalhosFrame)
createTabButton("Auto Farms", 160,  AutoFarmsFrame)
createTabButton("Player",     240,  PlayerFrame)

-- Botão padrão
local function createTeleportButton(parent, text, pos, y)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0,250,0,35)
    btn.Position = UDim2.new(0,35,0,y)
    btn.BackgroundColor3 = Color3.fromRGB(0,170,255)
    btn.Text = text
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,8)
    btn.MouseButton1Click:Connect(function()
        local v = Vector3.new(pos[1],pos[2],pos[3])
        safeTP(CFrame.new(v))
    end)
end

-- TELEPORTES
createTeleportButton(TeleportsFrame,"Prefeitura",{-2811.069,-4.686,1969.642},20)
createTeleportButton(TeleportsFrame,"Praça",{-267.189,1.357,399.212},70)
createTeleportButton(TeleportsFrame,"Garagem",{-478.882,1.035,358.987},120)
createTeleportButton(TeleportsFrame,"Concessionária",{-99.048,1.814,484.828},170)
createTeleportButton(TeleportsFrame,"Banco",{-91.816,7.129,323.592},220)
createTeleportButton(TeleportsFrame,"Lojinha",{-95.035,4.959,-114.903},270)

-- TRABALHOS (TPs)
createTeleportButton(TrabalhosFrame,"Lixeiro",{-533.28,6.649,-49.867},20)
createTeleportButton(TrabalhosFrame,"Gas",{-478.303,4.736,-36.381},70)
createTeleportButton(TrabalhosFrame,"Mecânica",{-228.568,5.439,-552.194},120)
createTeleportButton(TrabalhosFrame,"Fazenda",{957.698,4.289,-123.291},170)
createTeleportButton(TrabalhosFrame,"Mineração",{380.075,3.977,88.745},220)
createTeleportButton(TrabalhosFrame,"Posto de Gasolina",{-478.384,5.79,176.703},270)

-- TOGGLE genérico (Azul OFF / Vermelho ON)
local function createToggleButton(parent,text,y,callback)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0,250,0,35)
    btn.Position = UDim2.new(0,35,0,y)
    btn.BackgroundColor3 = Color3.fromRGB(0,170,255) -- OFF (azul)
    btn.Text = text
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0,8)
    local state = false
    btn.MouseButton1Click:Connect(function()
        state = not state
        btn.BackgroundColor3 = state and Color3.fromRGB(255,0,0) or Color3.fromRGB(0,170,255) -- ON vermelho
        callback(state, btn)
    end)
    return btn
end

------------------------------------------------------------
-- [3] AUTO FARMS
------------------------------------------------------------
-- GARI melhorado: desativa colisão dos “lixos”, tenta prompt próximo, se não alcançar, troca de alvo
local gariThread
createToggleButton(AutoFarmsFrame,"Gari",20,function(on)
    if on then
        if gariThread and coroutine.status(gariThread) ~= "dead" then return end
        gariThread = coroutine.create(function()
            while true do
                local mapa = workspace:FindFirstChild("MapaGeral")
                local lixosFolder = mapa and mapa:FindFirstChild("Gari") and mapa.Gari:FindFirstChild("Lixos")
                if lixosFolder then
                    local lixos = lixosFolder:GetChildren()
                    -- Embaralhar para não travar num único alvo
                    for i = #lixos, 2, -1 do
                        local j = math.random(i)
                        lixos[i], lixos[j] = lixos[j], lixos[i]
                    end

                    local doneThisLoop = false
                    for _, alvo in ipairs(lixos) do
                        local hrp = getHRP()
                        if not hrp then break end

                        -- desativa colisões do alvo
                        for _,d in ipairs(alvo:GetDescendants()) do
                            if d:IsA("BasePart") then d.CanCollide = false end
                        end

                        local prompt = alvo:FindFirstChildWhichIsA("ProximityPrompt", true)
                        local cf = (alvo:IsA("BasePart") and alvo.CFrame) or (alvo.PrimaryPart and alvo.PrimaryPart.CFrame) or nil
                        if prompt and cf then
                            -- aumenta o alcance do prompt
                            pcall(function() prompt.MaxActivationDistance = 25 end)

                            -- tenta acionar em posição atual
                            local dist = (hrp.Position - cf.Position).Magnitude
                            if dist > 10 then
                                safeTP(cf * CFrame.new(0,0,3))
                                task.wait(0.25)
                            end

                            -- checa se ainda existe (às vezes já coletou)
                            if prompt and prompt.Parent then
                                prompt:InputHoldBegin()
                                task.wait(3)
                                prompt:InputHoldEnd()
                                doneThisLoop = true
                                break
                            end
                        end
                        task.wait(0.1)
                    end

                    if not doneThisLoop then
                        task.wait(1.5)
                    end
                else
                    task.wait(1.5)
                end
                task.wait(0.35)
            end
        end)
        coroutine.resume(gariThread)
    else
        gariThread = nil -- loop terminará naturalmente ao perder referência
    end
end)

-- GÁS: BotGas (3s), equipa Botijão, percorre blocos do jogador e entrega (2s)
local function equipBotijao()
    local char = LocalPlayer.Character
    local humanoid = char and char:FindFirstChildOfClass("Humanoid")
    local backpack = LocalPlayer:FindFirstChild("Backpack")
    if not humanoid or not backpack then return end
    local tool = backpack:FindFirstChild("Botijão de Gás")
    if not tool then
        local toolsFolder = ReplicatedStorage:FindFirstChild("tools")
        if toolsFolder then
            local src = toolsFolder:FindFirstChild("Botijão de Gás")
            if src then
                tool = src:Clone()
                tool.Parent = backpack
            end
        end
    end
    if tool then humanoid:EquipTool(tool) task.wait(0.2) end
end

local gasThread
createToggleButton(AutoFarmsFrame,"Gas",70,function(on)
    if on then
        if gasThread and coroutine.status(gasThread) ~= "dead" then return end
        gasThread = coroutine.create(function()
            while true do
                local mapa = workspace:FindFirstChild("MapaGeral")
                local gasPlace = mapa and mapa:FindFirstChild("GasPlace")
                local botGas = gasPlace and gasPlace:FindFirstChild("BotGas")
                local blocosFolder = gasPlace and gasPlace:FindFirstChild("blocos")
                if botGas and blocosFolder then
                    local cf = botGas:IsA("BasePart") and botGas.CFrame or (botGas.PrimaryPart and botGas.PrimaryPart.CFrame)
                    if cf then
                        safeTP(cf * CFrame.new(0,0,3))
                        task.wait(0.25)
                        local promptBot = botGas:FindFirstChildWhichIsA("ProximityPrompt", true)
                        if promptBot then
                            pcall(function() promptBot.MaxActivationDistance = 25 end)
                            promptBot:InputHoldBegin()
                            task.wait(3)
                            promptBot:InputHoldEnd()
                        end

                        equipBotijao()

                        -- espera modelo do player
                        local myModel
                        for i=1,50 do -- até ~5s
                            myModel = blocosFolder:FindFirstChild(LocalPlayer.Name)
                            if myModel then break end
                            task.wait(0.1)
                        end
                        if myModel then
                            for _,bloco in ipairs(myModel:GetChildren()) do
                                local prox = bloco:FindFirstChildWhichIsA("ProximityPrompt", true)
                                local bcf = (bloco:IsA("BasePart") and bloco.CFrame) or (bloco.PrimaryPart and bloco.PrimaryPart.CFrame)
                                if prox and bcf then
                                    pcall(function() prox.MaxActivationDistance = 20 end)
                                    safeTP(bcf * CFrame.new(0,0,3))
                                    task.wait(0.2)
                                    prox:InputHoldBegin()
                                    task.wait(2)
                                    prox:InputHoldEnd()
                                end
                                task.wait(0.2)
                            end
                        end
                    end
                end
                task.wait(0.5)
            end
        end)
        coroutine.resume(gasThread)
    else
        gasThread = nil
    end
end)

------------------------------------------------------------
-- [4] PLAYER: ESP + Slider / Sem Queda / Abrir Carro
------------------------------------------------------------
-- ESP
local espOn = false
local espConns = {}
local ESP_HL_TRANSP = 0.25 -- default (0 transparente? FillTransparency 0-1)
local function applyESPTo(plr)
    if not espOn then return end
    if not plr or plr == LocalPlayer or not plr.Character then return end
    if plr.Character:FindFirstChildOfClass("Highlight") then return end
    local hl = Instance.new("Highlight")
    hl.FillColor = Color3.fromRGB(0,170,255)
    hl.OutlineColor = Color3.fromRGB(255,255,255)
    hl.FillTransparency = ESP_HL_TRANSP
    hl.OutlineTransparency = 0
    hl.Adornee = plr.Character
    hl.Parent = plr.Character
end
local function removeESPFrom(plr)
    if not plr or not plr.Character then return end
    local h = plr.Character:FindFirstChildOfClass("Highlight")
    if h then h:Destroy() end
end
local function espEnable()
    if espOn then return end
    espOn = true
    for _,p in ipairs(Players:GetPlayers()) do
        applyESPTo(p)
    end
    table.insert(espConns, Players.PlayerAdded:Connect(function(p)
        table.insert(espConns, p.CharacterAdded:Connect(function()
            task.wait(0.2)
            applyESPTo(p)
        end))
    end))
    for _,p in ipairs(Players:GetPlayers()) do
        table.insert(espConns, p.CharacterAdded:Connect(function()
            task.wait(0.2)
            applyESPTo(p)
        end))
    end
end
local function espDisable()
    espOn = false
    for _,p in ipairs(Players:GetPlayers()) do
        removeESPFrom(p)
    end
    for _,c in ipairs(espConns) do
        if typeof(c) == "RBXScriptConnection" then c:Disconnect() end
    end
    espConns = {}
end

-- Slider util
local function createSlider(parent, labelText, y, minV, maxV, defaultV, onChange)
    local label = Instance.new("TextLabel", parent)
    label.Size = UDim2.new(0,250,0,20)
    label.Position = UDim2.new(0,35,0,y)
    label.BackgroundTransparency = 1
    label.Text = labelText
    label.TextColor3 = Color3.new(1,1,1)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 14

    local bar = Instance.new("Frame", parent)
    bar.Size = UDim2.new(0,250,0,8)
    bar.Position = UDim2.new(0,35,0,y+24)
    bar.BackgroundColor3 = Color3.fromRGB(60,60,60)
    Instance.new("UICorner", bar).CornerRadius = UDim.new(0,4)

    local fill = Instance.new("Frame", bar)
    fill.Size = UDim2.new((defaultV-minV)/(maxV-minV),0,1,0)
    fill.Position = UDim2.new(0,0,0,0)
    fill.BackgroundColor3 = Color3.fromRGB(0,170,255)
    Instance.new("UICorner", fill).CornerRadius = UDim.new(0,4)

    local knob = Instance.new("Frame", bar)
    knob.Size = UDim2.new(0,12,0,12)
    knob.Position = UDim2.new((defaultV-minV)/(maxV-minV),-6,0.5,-6)
    knob.BackgroundColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", knob).CornerRadius = UDim.new(1,0)

    local value = defaultV
    local dragging = false

    local function setValueFromX(x)
        local rel = math.clamp((x - bar.AbsolutePosition.X)/bar.AbsoluteSize.X, 0, 1)
        value = minV + rel * (maxV-minV)
        fill.Size = UDim2.new(rel,0,1,0)
        knob.Position = UDim2.new(rel,-6,0.5,-6)
        if onChange then onChange(value) end
    end

    bar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            setValueFromX(input.Position.X)
        end
    end)
    bar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    bar.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            setValueFromX(input.Position.X)
        end
    end)

    if onChange then onChange(value) end
end

-- Player UI elements
-- 1) ESP Toggle
createToggleButton(PlayerFrame,"ESP",20,function(on)
    if on then espEnable() else espDisable() end
end)
-- 1.1) Slider Visibilidade do ESP (0 = opaco, 1 = invisível)
createSlider(PlayerFrame, "Visibilidade do ESP", 60, 0, 1, 0.25, function(v)
    ESP_HL_TRANSP = math.clamp(v,0,1)
    if espOn then
        for _,p in ipairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character then
                local h = p.Character:FindFirstChildOfClass("Highlight")
                if h then h.FillTransparency = ESP_HL_TRANSP end
            end
        end
    end
end)

-- 2) Sem Queda Toggle (reaplica no respawn)
local noFallOn = false
local function applyNoFall(char)
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not hum then return end
    pcall(function()
        hum:SetStateEnabled(Enum.HumanoidStateType.Freefall, not noFallOn and true or false)
        hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown, not noFallOn and true or false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, not noFallOn and true or false)
    end)
end
LocalPlayer.CharacterAdded:Connect(function(char)
    if noFallOn then
        char:WaitForChild("Humanoid")
        task.wait(0.2)
        applyNoFall(char)
    end
end)

createToggleButton(PlayerFrame,"Sem Queda",110,function(on)
    noFallOn = on
    applyNoFall(LocalPlayer.Character)
    if on then
        msg("Sem Queda: ATIVADO", Color3.fromRGB(0,160,80), 2)
    else
        -- reativar estados
        local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            pcall(function()
                hum:SetStateEnabled(Enum.HumanoidStateType.Freefall, true)
                hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown, true)
                hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, true)
            end)
        end
        msg("Sem Queda: DESATIVADO", Color3.fromRGB(160,60,60), 2)
    end
end)

-- 3) Abrir Carro (com Slider de Raio)
local OPEN_RADIUS = 50
createSlider(PlayerFrame, "Raio Abrir Carro", 150, 10, 300, 50, function(v)
    OPEN_RADIUS = math.floor(v + 0.5)
end)

local function findNearestLockedCar(radius)
    -- caminho: workspace.MapaGeral.Estacionamentox.EstacionamentoModel.Spawns.CarroSpawnados
    local mg = workspace:FindFirstChild("MapaGeral")
    local estx = mg and mg:FindFirstChild("Estacionamentox")
    local estModel = estx and estx:FindFirstChild("EstacionamentoModel")
    local spawns = estModel and estModel:FindFirstChild("Spawns")
    local carros = spawns and spawns:FindFirstChild("CarroSpawnados")
    if not carros then return nil end

    local hrp = getHRP()
    if not hrp then return nil end

    local nearest, ndist = nil, radius
    for _,car in ipairs(carros:GetChildren()) do
        if car:IsA("Model") then
            local valueFolder = car:FindFirstChild("Value")
            local locked = valueFolder and valueFolder:FindFirstChild("Locked")
            if locked and locked:IsA("BoolValue") then
                -- posição de referência do carro
                local pos
                if car.PrimaryPart then pos = car.PrimaryPart.Position
                else
                    local anyPart = car:FindFirstChildWhichIsA("BasePart", true)
                    pos = anyPart and anyPart.Position or nil
                end
                if pos then
                    local d = (hrp.Position - pos).Magnitude
                    if d <= ndist then
                        nearest = car
                        ndist = d
                    end
                end
            end
        end
    end
    return nearest
end

local function showBigBanner(text, bgColor)
    local gui = LocalPlayer:FindFirstChild("PlayerGui")
    if not gui then return end
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 420, 0, 60)
    frame.Position = UDim2.new(0.5, -210, 0.15, 0)
    frame.BackgroundColor3 = bgColor or Color3.fromRGB(30,30,30)
    frame.Parent = gui
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0,12)

    local lbl = Instance.new("TextLabel", frame)
    lbl.Size = UDim2.new(1, -20, 1, 0)
    lbl.Position = UDim2.new(0,10,0,0)
    lbl.BackgroundTransparency = 1
    lbl.Text = text
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.Font = Enum.Font.GothamBold
    lbl.TextScaled = true

    task.delay(3, function() if frame then frame:Destroy() end end)
end

local openCarBtn = Instance.new("TextButton", PlayerFrame)
openCarBtn.Size = UDim2.new(0,250,0,35)
openCarBtn.Position = UDim2.new(0,35,0,190)
openCarBtn.BackgroundColor3 = Color3.fromRGB(0,170,255)
openCarBtn.Text = "Abrir Carro"
openCarBtn.TextColor3 = Color3.new(1,1,1)
openCarBtn.Font = Enum.Font.GothamBold
openCarBtn.TextSize = 14
Instance.new("UICorner", openCarBtn).CornerRadius = UDim.new(0,8)

openCarBtn.MouseButton1Click:Connect(function()
    local car = findNearestLockedCar(OPEN_RADIUS)
    if car then
        local valueFolder = car:FindFirstChild("Value")
        local locked = valueFolder and valueFolder:FindFirstChild("Locked")
        if locked then
            locked:Destroy()
            showBigBanner("Carro aberto! Thanks VmasterScript - youtube", Color3.fromRGB(20,160,80)) -- verde sucesso
        else
            showBigBanner("Sem carros por perto para abrir", Color3.fromRGB(160,40,40)) -- vermelho aviso
        end
    else
        showBigBanner("Sem carros por perto para abrir", Color3.fromRGB(160,40,40))
    end
end)
