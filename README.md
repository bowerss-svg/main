-- Server‑side: Income and Purchase System

local ClickRemote = Instance.new("RemoteEvent")
ClickRemote.Name = "ClickRemote"
ClickRemote.Parent = game.ReplicatedStorage

-- Store player stats
local function setupPlayer(player)
    local data = Instance.new("Folder")
    data.Name = "PlayerData"
    data.Parent = player

    local points = Instance.new("IntValue")
    points.Name = "Points"
    points.Value = 0
    points.Parent = data

    local cps = Instance.new("NumberValue")
    cps.Name = "CPS"
    cps.Value = 0  -- clicks per second passive generation
    cps.Parent = data

    game.Players.PlayerAdded:Connect(function(pl) end)
end

game.Players.PlayerAdded:Connect(function(player)
    setupPlayer(player)
end)

-- Handle click events
ClickRemote.OnServerEvent:Connect(function(player)
    local pd = player:FindFirstChild("PlayerData")
    if pd then
        pd.Points.Value += 1
    end
end)

-- Passive income loop
while true do
    task.wait(1)
    for _, player in pairs(game.Players:GetPlayers()) do
        local pd = player:FindFirstChild("PlayerData")
        if pd then
            pd.Points.Value += pd.CPS.Value
        end
    end
end
