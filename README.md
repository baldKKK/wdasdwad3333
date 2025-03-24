-- 🟢 Este script deve ser executado no Xeno Executor

local Players = game:GetService("Players")

-- 🔹 Função para puxar um jogador e sincronizar com o servidor
local function pullPlayer(targetName)
    local localPlayer = Players.LocalPlayer
    if not localPlayer.Character then return end
    local humanoidRootPart = localPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    for _, target in pairs(Players:GetPlayers()) do
        if target.Name:lower() == targetName:lower() and target.Character then
            local targetRoot = target.Character:FindFirstChild("HumanoidRootPart")
            if targetRoot then
                -- 🔥 Tentativa de forçar o movimento pelo servidor
                targetRoot.Anchored = true
                targetRoot.CFrame = humanoidRootPart.CFrame + Vector3.new(0, 3, 0)
                wait(0.1)
                targetRoot.Anchored = false
            end
        end
    end
end

-- 🔹 Monitorar o Chat para Comandos
Players.LocalPlayer.Chatted:Connect(function(msg)
    local args = string.split(msg, " ")

    if args[1] == "!pull" and args[2] then
        pullPlayer(args[2])
    end
end)
