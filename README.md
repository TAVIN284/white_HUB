-- Script feito para fins de testes e detecção autorizada
-- Coloque este LocalScript dentro de StarterPlayerScripts

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Team permitido (altere para o nome exato da equipe que pode usar)
local allowedTeamName = "Admin"

-- Interface
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "SpeedJumpGUI"

local function createButton(name, position, callback)
	local button = Instance.new("TextButton")
	button.Size = UDim2.new(0, 150, 0, 40)
	button.Position = position
	button.Text = name
	button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	button.TextColor3 = Color3.fromRGB(255, 255, 255)
	button.Parent = screenGui
	button.MouseButton1Click:Connect(callback)
	return button
end

-- Estados
local speedOn = false
local jumpOn = false

-- Funções
local function toggleSpeed()
	if player.Team and player.Team.Name ~= allowedTeamName then
		warn("Speed bloqueado - equipe não autorizada")
		return
	end

	speedOn = not speedOn
	if speedOn then
		character.Humanoid.WalkSpeed = 50 -- Velocidade aumentada
	else
		character.Humanoid.WalkSpeed = 16 -- Velocidade padrão
	end
end

local function toggleJump()
	if player.Team and player.Team.Name ~= allowedTeamName then
		warn("Super pulo bloqueado - equipe não autorizada")
		return
	end

	jumpOn = not jumpOn
	if jumpOn then
		character.Humanoid.JumpPower = 120 -- Pulo aumentado
	else
		character.Humanoid.JumpPower = 50 -- Pulo padrão
	end
end

-- Criar botões
createButton("Toggle Speed", UDim2.new(0, 20, 0, 100), toggleSpeed)
createButton("Toggle Super Pulo", UDim2.new(0, 20, 0, 150), toggleJump)

-- Atualizar caso o personagem morra/reapareça
player.CharacterAdded:Connect(function(char)
	character = char
	character:WaitForChild("Humanoid")

	-- Restaurar estados
	if speedOn then
		character.Humanoid.WalkSpeed = 50
	end
	if jumpOn then
		character.Humanoid.JumpPower = 120
	end
end)
