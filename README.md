-- GUI
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local ToggleButton = Instance.new("TextButton")

ScreenGui.Name = "AutoFarmGUI"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

Frame.Name = "Main"
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Position = UDim2.new(0, 100, 0, 100)
Frame.Size = UDim2.new(0, 200, 0, 100)

ToggleButton.Name = "ToggleButton"
ToggleButton.Parent = Frame
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ToggleButton.Position = UDim2.new(0, 20, 0, 20)
ToggleButton.Size = UDim2.new(0, 160, 0, 60)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Text = "Ativar AutoFarm"
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.TextSize = 20

-- Auto Farm Lógica
local autoFarmAtivo = false
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

function autoFarm()
    while autoFarmAtivo do
        for _, npc in pairs(workspace:GetChildren()) do
            if npc:FindFirstChild("Humanoid") and npc.Name == "Dummy" then
                -- Teleporta o player até o NPC
                character:MoveTo(npc.Position + Vector3.new(0, 2, 0))

                -- Dá dano no NPC
                npc.Humanoid:TakeDamage(10)

                wait(0.5) -- Espera um pouco antes de atacar de novo
            end
        end
        wait(1)
    end
end

ToggleButton.MouseButton1Click:Connect(function()
    autoFarmAtivo = not autoFarmAtivo
    ToggleButton.Text = autoFarmAtivo and "Desativar AutoFarm" or "Ativar AutoFarm"

    if autoFarmAtivo then
        spawn(autoFarm)
    end
end)
