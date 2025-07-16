-- ESP (destaca jogadores com caixa amarela)
local function createESP(player)
    if player ~= LocalPlayer and player.Character then
        local highlight = Instance.new("Highlight")
        highlight.Parent = player.Character
        highlight.FillColor = Color3.new(1, 1, 0) -- Amarelo
        highlight.OutlineColor = Color3.new(1, 1, 0)
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    end
end

-- Aplica ESP em todos os jogadores
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        createESP(player)
    end)
end)

-- Atualiza ESP se o personagem for recarregado
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        player.CharacterAdded:Connect(function()
            createESP(player)
        end)
        if player.Character then
            createESP(player)
        end
    end
end
-- Variáveis
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")

-- Aimbot (trava no jogador mais próximo)
local function findClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if distance < shortestDistance then
                closestPlayer = player
                shortestDistance = distance
            end
        end
    end
    
    return closestPlayer
end

-- Ativa o aimlock (segurar RightShift para mirar)
RunService.RenderStepped:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        local target = findClosestPlayer()
        if target and target.Character and target.Character:FindFirstChild("Head") then
            if UserInputService:IsKeyDown(Enum.KeyCode.RightShift) then -- Tecla para ativar
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Character.Head.Position)
            end
        end
    end
end)
