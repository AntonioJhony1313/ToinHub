--// ToinHub v1
--// Uso: coloque este LocalScript em StarterPlayer > StarterPlayerScripts
--// Feito para experiências próprias no Roblox Studio.

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

--==================================================
-- CONFIGURAÇÕES
--==================================================

local Config = {
    Theme = Color3.fromRGB(120, 70, 255),
    Background = Color3.fromRGB(18, 18, 24),
    Secondary = Color3.fromRGB(28, 28, 38),
    Text = Color3.fromRGB(240, 240, 245),
    Muted = Color3.fromRGB(160, 160, 175),
}

--==================================================
-- GUI
--==================================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ToinHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = playerGui

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(700, 450)
Main.Position = UDim2.new(0.5, -350, 0.5, -225)
Main.BackgroundColor3 = Config.Background
Main.BorderSizePixel = 0
Main.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 14)
MainCorner.Parent = Main

-- Sombra
local Shadow = Instance.new("ImageLabel")
Shadow.Name = "Shadow"
Shadow.AnchorPoint = Vector2.new(0.5, 0.5)
Shadow.Position = UDim2.fromScale(0.5, 0.5)
Shadow.Size = UDim2.new(1, 50, 1, 50)
Shadow.BackgroundTransparency = 1
Shadow.Image = "rbxassetid://1316045217"
Shadow.ImageColor3 = Color3.new(0, 0, 0)
Shadow.ImageTransparency = 0.45
Shadow.ScaleType = Enum.ScaleType.Slice
Shadow.SliceCenter = Rect.new(10, 10, 118, 118)
Shadow.ZIndex = -1
Shadow.Parent = Main

--==================================================
-- TÍTULO
--==================================================

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -30, 0, 50)
Title.Position = UDim2.fromOffset(20, 5)
Title.BackgroundTransparency = 1
Title.Text = "ToinHub"
Title.TextColor3 = Config.Text
Title.TextSize = 25
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Main

local Subtitle = Instance.new("TextLabel")
Subtitle.Size = UDim2.new(1, -30, 0, 25)
Subtitle.Position = UDim2.fromOffset(20, 35)
Subtitle.BackgroundTransparency = 1
Subtitle.Text = "Hub para sua experiência"
Subtitle.TextColor3 = Config.Muted
Subtitle.TextSize = 12
Subtitle.Font = Enum.Font.Gotham
Subtitle.TextXAlignment = Enum.TextXAlignment.Left
Subtitle.Parent = Main

--==================================================
-- SIDEBAR
--==================================================

local Sidebar = Instance.new("Frame")
Sidebar.Size = UDim2.fromOffset(155, 355)
Sidebar.Position = UDim2.fromOffset(15, 75)
Sidebar.BackgroundColor3 = Config.Secondary
Sidebar.BorderSizePixel = 0
Sidebar.Parent = Main

local SidebarCorner = Instance.new("UICorner")
SidebarCorner.CornerRadius = UDim.new(0, 10)
SidebarCorner.Parent = Sidebar

local SideLayout = Instance.new("UIListLayout")
SideLayout.Padding = UDim.new(0, 7)
SideLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
SideLayout.VerticalAlignment = Enum.VerticalAlignment.Top
SideLayout.Parent = Sidebar

local SidePadding = Instance.new("UIPadding")
SidePadding.PaddingTop = UDim.new(0, 12)
SidePadding.Parent = Sidebar

--==================================================
-- CONTEÚDO
--==================================================

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -190, 1, -90)
Content.Position = UDim2.fromOffset(180, 75)
Content.BackgroundTransparency = 1
Content.Parent = Main

local Pages = {}

local function CreatePage(name)
    local page = Instance.new("ScrollingFrame")
    page.Name = name
    page.Size = UDim2.fromScale(1, 1)
    page.BackgroundTransparency = 1
    page.BorderSizePixel = 0
    page.ScrollBarThickness = 4
    page.ScrollBarImageColor3 = Config.Theme
    page.Visible = false
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.Parent = Content

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 10)
    layout.Parent = page

    local padding = Instance.new("UIPadding")
    padding.PaddingRight = UDim.new(0, 8)
    padding.Parent = page

    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        page.CanvasSize = UDim2.fromOffset(0, layout.AbsoluteContentSize.Y + 15)
    end)

    Pages[name] = page
    return page
end

local HomePage = CreatePage("Home")
local CombatPage = CreatePage("Combat")
local TeleportPage = CreatePage("Teleport")
local SettingsPage = CreatePage("Settings")

--==================================================
-- COMPONENTES
--==================================================

local function CreateSection(parent, text)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -5, 0, 35)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Config.Text
    label.TextSize = 17
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = parent

    return label
end

local function CreateButton(parent, text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -5, 0, 42)
    button.BackgroundColor3 = Config.Secondary
    button.BorderSizePixel = 0
    button.Text = text
    button.TextColor3 = Config.Text
    button.TextSize = 13
    button.Font = Enum.Font.GothamMedium
    button.AutoButtonColor = false
    button.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = button

    button.MouseEnter:Connect(function()
        TweenService:Create(
            button,
            TweenInfo.new(0.15),
            {BackgroundColor3 = Config.Theme}
        ):Play()
    end)

    button.MouseLeave:Connect(function()
        TweenService:Create(
            button,
            TweenInfo.new(0.15),
            {BackgroundColor3 = Config.Secondary}
        ):Play()
    end)

    button.MouseButton1Click:Connect(callback)

    return button
end

local function CreateToggle(parent, text, default, callback)
    local state = default

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -5, 0, 42)
    button.BackgroundColor3 = Config.Secondary
    button.BorderSizePixel = 0
    button.Text = ""
    button.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = button

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -70, 1, 0)
    label.Position = UDim2.fromOffset(14, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Config.Text
    label.TextSize = 13
    label.Font = Enum.Font.GothamMedium
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = button

    local indicator = Instance.new("Frame")
    indicator.Size = UDim2.fromOffset(40, 20)
    indicator.Position = UDim2.new(1, -52, 0.5, -10)
    indicator.BackgroundColor3 = Color3.fromRGB(70, 70, 80)
    indicator.BorderSizePixel = 0
    indicator.Parent = button

    local indicatorCorner = Instance.new("UICorner")
    indicatorCorner.CornerRadius = UDim.new(1, 0)
    indicatorCorner.Parent = indicator

    local function Update()
        TweenService:Create(
            indicator,
            TweenInfo.new(0.15),
            {
                BackgroundColor3 = state
                    and Config.Theme
                    or Color3.fromRGB(70, 70, 80)
            }
        ):Play()

        callback(state)
    end

    button.MouseButton1Click:Connect(function()
        state = not state
        Update()
    end)

    Update()

    return button
end

--==================================================
-- HOME
--==================================================

CreateSection(HomePage, "Bem-vindo ao ToinHub")

local Welcome = Instance.new("TextLabel")
Welcome.Size = UDim2.new(1, -5, 0, 80)
Welcome.BackgroundColor3 = Config.Secondary
Welcome.BorderSizePixel = 0
Welcome.Text = "Olá, " .. player.DisplayName .. "!\n\nEste hub foi criado para controlar sistemas da sua própria experiência."
Welcome.TextColor3 = Config.Text
Welcome.TextSize = 13
Welcome.Font = Enum.Font.Gotham
Welcome.TextWrapped = true
Welcome.Parent = HomePage

local WelcomeCorner = Instance.new("UICorner")
WelcomeCorner.CornerRadius = UDim.new(0, 8)
WelcomeCorner.Parent = Welcome

CreateSection(HomePage, "Informações")

CreateButton(HomePage, "Mostrar nome do jogador", function()
    print("Jogador:", player.Name)
end)

CreateButton(HomePage, "Mostrar UserId", function()
    print("UserId:", player.UserId)
end)

--==================================================
-- COMBAT
--==================================================

CreateSection(CombatPage, "Sistemas de combate")

CreateToggle(CombatPage, "Sistema de treino", false, function(enabled)
    print("Sistema de treino:", enabled)
    -- Coloque aqui a lógica legítima do seu jogo.
end)

CreateToggle(CombatPage, "Dano de treino", false, function(enabled)
    print("Dano de treino:", enabled)
    -- Integre com o sistema de combate do seu próprio jogo.
end)

CreateButton(CombatPage, "Treinar", function()
    print("Treino ativado.")
end)

--==================================================
-- TELEPORT
--==================================================

CreateSection(TeleportPage, "Teleporte")

-- Crie Parts no Workspace com estes nomes:
-- Spawn
-- Cidade
-- Ilha1
-- Ilha2

local function TeleportTo(partName)
    local character = player.Character
    if not character then
        return
    end

    local root = character:FindFirstChild("HumanoidRootPart")
    local destination = workspace:FindFirstChild(partName)

    if root and destination and destination:IsA("BasePart") then
        root.CFrame = destination.CFrame + Vector3.new(0, 4, 0)
    else
        warn("Destino não encontrado:", partName)
    end
end

CreateButton(TeleportPage, "Spawn", function()
    TeleportTo("Spawn")
end)

CreateButton(TeleportPage, "Cidade", function()
    TeleportTo("Cidade")
end)

CreateButton(TeleportPage, "Ilha 1", function()
    TeleportTo("Ilha1")
end)

CreateButton(TeleportPage, "Ilha 2", function()
    TeleportTo("Ilha2")
end)

--==================================================
-- SETTINGS
--==================================================

CreateSection(SettingsPage, "Configurações")

CreateToggle(SettingsPage, "Interface visível", true, function(enabled)
    if enabled then
        Main.Visible = true
    else
        Main.Visible = false
    end
end)

CreateButton(SettingsPage, "Restaurar posição da interface", function()
    Main.Position = UDim2.new(0.5, -350, 0.5, -225)
end)

--==================================================
-- NAVEGAÇÃO
--==================================================

local function ShowPage(name)
    for pageName, page in pairs(Pages) do
        page.Visible = pageName == name
    end
end

local function CreateTab(text, pageName)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -20, 0, 40)
    button.BackgroundColor3 = Config.Secondary
    button.BorderSizePixel = 0
    button.Text = text
    button.TextColor3 = Config.Text
    button.TextSize = 13
    button.Font = Enum.Font.GothamMedium
    button.AutoButtonColor = false
    button.Parent = Sidebar

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = button

    button.MouseButton1Click:Connect(function()
        ShowPage(pageName)

        for _, child in ipairs(Sidebar:GetChildren()) do
            if child:IsA("TextButton") then
                child.BackgroundColor3 = Config.Secondary
            end
        end

        button.BackgroundColor3 = Config.Theme
    end)

    return button
end

local HomeTab = CreateTab("Home", "Home")
CreateTab("Combat", "Combat")
CreateTab("Teleport", "Teleport")
CreateTab("Settings", "Settings")

HomeTab.BackgroundColor3 = Config.Theme
ShowPage("Home")

--==================================================
-- ARRASTAR JANELA
--==================================================

local dragging = false
local dragStart
local startPosition

Title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = true
        dragStart = input.Position
        startPosition = Main.Position
    end
end)

Title.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch then

        local connection
        connection = input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
                connection:Disconnect()
            end
        end)
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then
        local delta = input.Position - dragStart

        Main.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end
end)

print("ToinHub carregado com sucesso!")
