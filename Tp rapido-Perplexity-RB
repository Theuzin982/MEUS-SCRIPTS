-- =======================
-- CONFIGURAÇÃO DO LAYOUT
-- =======================
local FRAME_WIDTH  = 100   -- largura total (mude aqui)
local FRAME_HEIGHT = 50    -- altura total (mude aqui)

-- =======================
-- CONFIG DE TECLA RÁPIDA
-- =======================
local SAVE_KEY = Enum.KeyCode.K   -- salvar posição
local TP_KEY   = Enum.KeyCode.L   -- teleportar para posição salva

-- =======================
-- SCRIPT
-- =======================

local UserInputService = game:GetService("UserInputService")

-- salva posição atual
local savedCFrame

local function getHRP()
    local plr = game.Players.LocalPlayer
    if not plr then return end
    local char = plr.Character or plr.CharacterAdded:Wait()
    return char:FindFirstChild("HumanoidRootPart")
end

local gui = Instance.new("ScreenGui")
gui.ResetOnSpawn = false
gui.Name = "SetTP_GUI"
gui.Parent = game.CoreGui

-- FRAME PRINCIPAL
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.fromOffset(FRAME_WIDTH, FRAME_HEIGHT)
frame.Position = UDim2.new(0.5, -FRAME_WIDTH/2, 0.1, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 0

-- =======================
-- BARRA SUPERIOR + X
-- =======================

local topBarHeight = 18

local topBar = Instance.new("Frame", frame)
topBar.Size = UDim2.new(1, 0, 0, topBarHeight)
topBar.Position = UDim2.new(0, 0, 0, 0)
topBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
topBar.BorderSizePixel = 0

local closeBtn = Instance.new("TextButton", topBar)
closeBtn.Size = UDim2.new(0, topBarHeight, 0, topBarHeight)
closeBtn.Position = UDim2.new(1, -topBarHeight, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.BorderSizePixel = 0
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextSize = 16

-- =======================
-- ÁREA DOS BOTÕES
-- =======================

local buttonsArea = Instance.new("Frame", frame)
buttonsArea.Size = UDim2.new(1, -10, 1, -topBarHeight-10)
buttonsArea.Position = UDim2.new(0, 5, 0, topBarHeight+5)
buttonsArea.BackgroundTransparency = 1

local setBtn = Instance.new("TextButton", buttonsArea)
setBtn.Size = UDim2.new(0.5, -5, 1, 0)
setBtn.Position = UDim2.new(0, 0, 0, 0)
setBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
setBtn.BorderSizePixel = 0
setBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
setBtn.Font = Enum.Font.SourceSansBold
setBtn.TextSize = 24
setBtn.Text = "Set"

local tpBtn = Instance.new("TextButton", buttonsArea)
tpBtn.Size = UDim2.new(0.5, -5, 1, 0)
tpBtn.Position = UDim2.new(0.5, 5, 0, 0)
tpBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
tpBtn.BorderSizePixel = 0
tpBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
tpBtn.Font = Enum.Font.SourceSansBold
tpBtn.TextSize = 24
tpBtn.Text = "TP"

-- =======================
-- FUNÇÕES DE AÇÃO
-- =======================

local function SavePosition()
    local hrp = getHRP()
    if hrp then
        savedCFrame = hrp.CFrame
    end
end

local function TeleportToSaved()
    if not savedCFrame then return end
    local hrp = getHRP()
    if hrp then
        hrp.CFrame = savedCFrame
    end
end

-- Botões da GUI
setBtn.MouseButton1Click:Connect(SavePosition)
tpBtn.MouseButton1Click:Connect(TeleportToSaved)

-- Botão X (fechar GUI)
closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- =======================
-- BINDS DE TECLADO
-- =======================

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end  -- não interfere com chats/menus

    if input.KeyCode == SAVE_KEY then
        SavePosition()
    elseif input.KeyCode == TP_KEY then
        TeleportToSaved()
    end
end)

-- =======================
-- DEIXAR O FRAME ARRASTÁVEL
-- =======================

local dragging = false
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    frame.Position = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
end

frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

frame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        if dragging then
            update(input)
        end
    end
end)
