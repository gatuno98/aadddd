-- Script colocado no LocalScript do jogador

local player = game.Players.LocalPlayer
local camera = workspace.CurrentCamera
local players = game.Players:GetPlayers()

-- Função para desenhar a linha entre dois pontos
local function drawLine(from, to)
    -- Criando a linha
    local line = Instance.new("Part")
    line.Size = Vector3.new(0.1, 0.1, (from - to).Magnitude)
    line.Anchored = true
    line.CanCollide = false
    line.Color = Color3.fromRGB(255, 255, 255) -- Linha branca
    line.Position = (from + to) / 2
    line.CFrame = CFrame.new(from, to) -- Alinha a linha para ir de 'from' a 'to'
    line.Parent = workspace

    -- Criando o nome
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Adornee = line
    billboardGui.Size = UDim2.new(0, 200, 0, 50)
    billboardGui.StudsOffset = Vector3.new(0, 5, 0) -- Ajuste da posição do nome

    local textLabel = Instance.new("TextLabel")
    textLabel.Text = player.Name
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.BackgroundTransparency = 1
    textLabel.Parent = billboardGui

    billboardGui.Parent = line

    -- Limpando a linha depois de um tempo (por exemplo, 5 segundos)
    game.Debris:AddItem(line, 5)
end

-- Função principal para verificar todos os jogadores
while true do
    for _, otherPlayer in ipairs(players) do
        -- Ignorar o jogador local
        if otherPlayer ~= player then
            local character = otherPlayer.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local otherPlayerPosition = character.HumanoidRootPart.Position
                local playerPosition = player.Character.HumanoidRootPart.Position
                drawLine(playerPosition, otherPlayerPosition)
            end
        end
    end
    wait(1) -- Espera um segundo antes de checar novamente
end
