-- Crear el botón en la pantalla
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
local button = Instance.new("TextButton", screenGui)

-- Configuración del botón
button.Size = UDim2.new(0, 200, 0, 50) -- Tamaño del botón
button.Position = UDim2.new(1, -210, 1, -60) -- Posicionado en la parte inferior derecha
button.AnchorPoint = Vector2.new(1, 1) -- Anclar a la esquina inferior derecha
button.BackgroundColor3 = Color3.new(0, 1, 0) -- Color verde
button.Text = "Teletransportarse"
button.Font = Enum.Font.SourceSans
button.TextSize = 24
button.TextColor3 = Color3.new(1, 1, 1)

-- Hacer que el botón tenga bordes redondeados
local uICorner = Instance.new("UICorner", button)
uICorner.CornerRadius = UDim.new(0, 20)

-- Crear el texto de la versión
local versionLabel = Instance.new("TextLabel", screenGui)
versionLabel.Size = UDim2.new(0, 200, 0, 30) -- Tamaño del texto
versionLabel.Position = UDim2.new(1, -210, 1, -30) -- Posicionado justo debajo del botón
versionLabel.AnchorPoint = Vector2.new(1, 1) -- Anclar a la esquina inferior derecha
versionLabel.BackgroundTransparency = 1 -- Hacer el fondo transparente
versionLabel.Text = "Versión 1.0"
versionLabel.Font = Enum.Font.SourceSans
versionLabel.TextSize = 18
versionLabel.TextColor3 = Color3.new(1, 1, 1)

-- Función para teletransportar al jugador
local function teleportPlayer()
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        local hrp = character.HumanoidRootPart
        local direction = hrp.CFrame.LookVector
        local newPosition = hrp.Position + direction * 30

        -- Detectar si hay una pared en el camino
        local ray = Ray.new(hrp.Position, direction * 30)
        local hit, hitPosition = workspace:FindPartOnRay(ray, character)

        if hit then
            -- Si hay una pared, teletransportar al jugador justo antes de la pared
            newPosition = hitPosition - direction * (hrp.Size.Z / 2)
        end

        -- Mover el HumanoidRootPart a la nueva posición
        local movePosition = CFrame.new(newPosition, newPosition + direction)
        character:SetPrimaryPartCFrame(movePosition)
    end
end

-- Conectar la función al evento de clic del botón
button.MouseButton1Click:Connect(teleportPlayer)
