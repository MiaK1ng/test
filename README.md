-- Kaitun Script para Blox Fruits com Auto Clicker e Derrota de Bosses
-- Use com responsabilidade e dentro das regras do jogo.

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local userInputService = game:GetService("UserInputService")
local runService = game:GetService("RunService")

local autoClicking = false

-- Função para ativar o auto-clicker
local function autoClick()
    while autoClicking do
        local mouse = player:GetMouse()
        mouse1click() -- Simula o clique do mouse
        wait(math.random(0.08, 0.12)) -- Intervalo aleatório para cliques
    end
end

-- Função para coletar frutas
local function collectFruits()
    for _, item in pairs(workspace:GetChildren()) do
        if item:IsA("Tool") and item.Name:match("Fruit") then
            item.Parent = player.Backpack
            wait(math.random(0.1, 0.2)) -- Intervalo aleatório para coletar frutas
        end
    end
end

-- Função para farmar NPCs
local function farmNPCs()
    local npcs = workspace:GetChildren()
    for _, npc in pairs(npcs) do
        if npc:IsA("Model") and npc:FindFirstChild("Humanoid") then
            character:MoveTo(npc.PrimaryPart.Position)
            wait(math.random(1.5, 2.5)) -- Intervalo aleatório antes de atacar
            -- Simule um ataque aqui, utilizando a sua função de ataque real
            -- player:Kick("Ataque realizado!") -- Aqui você deve chamar sua função de ataque
            wait(math.random(2, 4)) -- Intervalo aleatório entre os ataques
        end
    end
end

-- Função para derrotar todos os bosses
local function defeatBosses()
    local bosses = {"Stone", "Island Empress","Captain Elephant","Beautiful Pirate","Longma","Cake Queen", "Kilo Admiral"} -- Substitua pelos nomes reais dos bosses
    for _, bossName in pairs(bosses) do
        local boss = workspace:FindFirstChild(bossName)
        if boss and boss:IsA("Model") and boss:FindFirstChild("Humanoid") then
            character:MoveTo(boss.PrimaryPart.Position)
            wait(math.random(1.5, 2.5)) -- Intervalo aleatório antes de atacar
            -- Aqui você deve chamar sua função de ataque real
            -- player:Kick("Ataque ao boss: " .. bossName) -- Chame sua função de ataque real
            wait(math.random(2, 4)) -- Intervalo aleatório entre os ataques
        end
    end
end

-- Função para monitorar e evitar kick
local function antiKick()
    while true do
        wait(1) -- Verifica a cada segundo
        if character.Humanoid.Health <= 0 then
            character:BreakJoints() -- Tenta evitar o kick ao reiniciar o personagem
        end
    end
end

-- Iniciar o Auto Clicker
autoClicking = true
coroutine.wrap(autoClick)() -- Executa o auto-clicker em uma coroutine
coroutine.wrap(antiKick)() -- Executa a função anti-kick em outra coroutine

-- Loop principal
while true do
    collectFruits()
    farmNPCs()
    defeatBosses() -- Adiciona a chamada para derrotar os bosses
    wait(math.random(5, 10)) -- Aguarde um intervalo aleatório antes de repetir o loop
end
