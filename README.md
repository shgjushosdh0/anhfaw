local part = script.Parent
local cooldown = {}

local function getXPNeeded(level)
	return 100 + (level - 1) * 50
end

part.Touched:Connect(function(hit)
	local character = hit.Parent
	local player = game.Players:GetPlayerFromCharacter(character)
	if not player then return end

	-- Anti-spam por jogador
	if cooldown[player] then return end
	cooldown[player] = true
	task.delay(2, function() cooldown[player] = false end)

	-- Garante que o jogador tenha leaderstats
	local leaderstats = player:FindFirstChild("leaderstats")
	local level = leaderstats and leaderstats:FindFirstChild("Level")
	local xp = player:FindFirstChild("XP")

	if level and xp then
		xp.Value += 50  -- Ganha 50 XP ao tocar
		while xp.Value >= getXPNeeded(level.Value) do
			xp.Value -= getXPNeeded(level.Value)
			level.Value += 1
			print(player.Name .. " subiu para o nível " .. level.Value)
		end
	end
end)
