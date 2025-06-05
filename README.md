-- LocalScript (coloque em StarterPlayerScripts)
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

-- Função para criar o ESP com BillboardGui
local function criarESP(jogador)
    local personagem = jogador.Character
    if not personagem or not personagem:FindFirstChild("Head") then return end

    -- Evita duplicar ESP
    if personagem:FindFirstChild("ESP") then return end

    local gui = Instance.new("BillboardGui")
    gui.Name = "ESP"
    gui.Adornee = personagem.Head
    gui.Size = UDim2.new(0, 100, 0, 20)
    gui.StudsOffset = Vector3.new(0, 2, 0)
    gui.AlwaysOnTop = true
    gui.Parent = personagem

    local texto = Instance.new("TextLabel")
    texto.Size = UDim2.new(1, 0, 1, 0)
    texto.BackgroundTransparency = 1
    texto.TextColor3 = Color3.new(1, 0, 0) -- vermelho = inimigo
    texto.TextScaled = true
    texto.Text = "INIMIGO: " .. jogador.Name
    texto.Parent = gui
end

-- Verifica e cria ESP para jogadores inimigos
local function verificarJogadores()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Team ~= localPlayer.Team then
            player.CharacterAdded:Connect(function()
                wait(1) -- espera carregar o personagem
                criarESP(player)
            end)

            -- caso o personagem já tenha carregado
            if player.Character then
                criarESP(player)
            end
        end
    end
end

-- Quando novos jogadores entrarem
Players.PlayerAdded:Connect(function(player)
    if player ~= localPlayer and player.Team ~= localPlayer.Team then
        player.CharacterAdded:Connect(function()
            wait(1)
            criarESP(player)
        end)
    end
end)

-- Roda uma vez para os jogadores que já estão na partida
verificarJogadores()
