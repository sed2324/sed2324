---abaixo estara lib da nossa UI

local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

local players = game:Getservice("Players")
local localplayer = players.localplayer
local Team = localplayer.teamcolor

local UI = Lib:Create{
    Theme = "Dark", -- or any other theme
    Size = UDim2.new(0, 555, 0, 400) -- default
 }
 
 local Main = UI:Tab{
    Name = "inicio"
 }
 
 local Divider = Main:Divider{
    Name = "inicio shit"
 }
 
 local QuitDivider = Main:Divider{
    Name = "sair"
 }
 
 local KillAll = Divider:Button{
    Name = "esp",
    Description = "esp",
    Callback = function()
        local UserInputService = game:GetService("UserInputService") -- Serviço para capturar entradas do usuário (como teclas pressionadas)
        local Players = game:GetService("Players") -- Serviço que lida com todos os jogadores no jogo
        local LocalPlayer = Players.LocalPlayer -- O jogador local (o jogador que está executando o script)
        local Camera = workspace.CurrentCamera -- A câmera atual do jogo
        local ToggleOn = false -- Variável que indica se a funcionalidade está ativada ou não
        local VisibilityToggled = false -- Variável que controla a visibilidade dos jogadores
    end
end
}

local KillAll = Divider:Button{
    Name = "visibilidade ",
    Description = "visibilidade ativada",
    Callback = function()
        local function TogglePlayersVisibility()
            VisibilityToggled = not VisibilityToggled -- Alterna o estado de visibilidade
            for _, player in ipairs(Players:GetPlayers()) do -- Itera por todos os jogadores no servidor
                if player.Character and player.Character:FindFirstChild("Head") then -- Verifica se o jogador tem um personagem e uma cabeça
                    local head = player.Character.Head
                    head.Transparency = VisibilityToggled and 0 or 1 -- Define a transparência da cabeça: 0 (visível) ou 1 (invisível)
                end
            end
        end
    end

    local LoopKillAll = Divider:Toggle{
        Name = "aim bot",
        Description = "aim bot ativado",
        Callback = function(State)
            local function FollowPlayerHead()
                while ToggleOn do -- Executa enquanto a funcionalidade estiver ativada
                    local mouse = LocalPlayer:GetMouse() -- Obtém a posição do mouse do jogador
                    local targetPlayer = nil -- Inicializa a variável do jogador alvo
                    local shortestDistance = math.huge -- Define a menor distância como infinita inicialmente
            
                    for _, player in ipairs(Players:GetPlayers()) do -- Itera por todos os jogadores no servidor
                        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then -- Verifica se o jogador não é o local e tem uma cabeça
                            local head = player.Character.Head
                            local screenPosition, onScreen = Camera:WorldToScreenPoint(head.Position) -- Converte a posição da cabeça para a tela
                            
                            if onScreen then -- Verifica se a cabeça está na tela
                                local distance = (Vector2.new(screenPosition.X, screenPosition.Y) - Vector2.new(mouse.X, mouse.Y)).Magnitude -- Calcula a distância entre a cabeça e o cursor
                                if distance < shortestDistance then -- Verifica se a distância é a menor encontrada até agora
                                    shortestDistance = distance -- Atualiza a menor distância
                                    targetPlayer = player -- Define o jogador alvo como o jogador mais próximo do cursor
                                end
                            end
                        end
                    end
            
                    if targetPlayer then -- Verifica se há um jogador alvo
                        local head = targetPlayer.Character.Head
                        Camera.CFrame = CFrame.new(Camera.CFrame.Position, head.Position) -- Faz a câmera olhar para a cabeça do jogador alvo
                    end
                    
                    wait(0.1) -- Espera 0.1 segundos antes de continuar o loop
                end
            end     
    
        end

        local function disableESP
            UserInputService.InputBegan:Connect(function(input, gameProcessedEvent)
                if not gameProcessedEvent and input.KeyCode == Enum.KeyCode.P then -- Verifica se o evento não foi processado pelo jogo e se a tecla pressionada é 'P'
                    ToggleOn = not ToggleOn -- Alterna o estado da funcionalidade
                    if ToggleOn then
                        TogglePlayersVisibility() -- Alterna a visibilidade dos jogadores
                        FollowPlayerHead() -- Inicia o seguimento da cabeça dos jogadores
                    else
                        TogglePlayersVisibility() -- Apenas alterna a visibilidade dos jogadores
                    end
                end
            end)
            
            local function disableVISIBILIDADE
                UserInputService.InputBegan:Connect(function(input, gameProcessedEvent)
                    if not gameProcessedEvent and input.KeyCode == Enum.KeyCode.P then -- Verifica se o evento não foi processado pelo jogo e se a tecla pressionada é 'P'
                        ToggleOn = not ToggleOn -- Alterna o estado da funcionalidade
                        if ToggleOn then
                            TogglePlayersVisibility() -- Alterna a visibilidade dos jogadores
                            FollowPlayerHead() -- Inicia o seguimento da cabeça dos jogadores
                        else
                            TogglePlayersVisibility() -- Apenas alterna a visibilidade dos jogadores
                        end
                    end
                end)
