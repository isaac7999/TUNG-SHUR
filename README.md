local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Função para encontrar o VehicleSeat mais próximo
local function findNearestVehicleSeat()
    local nearestSeat = nil
    local shortestDistance = math.huge
    for _, seat in pairs(workspace:GetDescendants()) do
        if seat:IsA("VehicleSeat") and seat.Occupant == nil then
            local distance = (seat.Position - character.PrimaryPart.Position).Magnitude
            if distance < shortestDistance then
                shortestDistance = distance
                nearestSeat = seat
            end
        end
    end
    return nearestSeat
end

-- Tentar sentar no assento mais próximo
local seat = findNearestVehicleSeat()
if seat then
    -- Teleportar o personagem ligeiramente acima do assento
    character:SetPrimaryPartCFrame(seat.CFrame + Vector3.new(0, 2, 0))
    wait(0.1) -- Pequena espera para garantir que o personagem esteja posicionado corretamente
    seat:Sit(humanoid)
else
    warn("Nenhum assento disponível encontrado.")
end
