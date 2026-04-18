-- [[ НАСТРОЙКИ СИСТЕМЫ ]] --
local API_URL = "https://script.google.com/macros/s/AKfycbyFMW4-6vUqjaFGdWQCEXF6c2_AnhktCupzqEwXm7Jx7yaRMcNK-7Z1_8kLsL064N38vQ/exec"
local GEN_LINK = API_URL .. "?gen=true" 

local Settings = {
    AimbotEnabled = true,
    ESP_Enabled = true,
    ESP_Rainbow = false, -- [🔒 ADMIN ONLY]
    ESP_Hue = 0.6,       -- [🔒 ADMIN ONLY]
    ShowFOV = false,     -- [🔒 ADMIN ONLY]
    AimRadius = 140,     -- [ДОСТУПНО ВСІМ]
    Sensitivity = 0.42,  -- [ДОСТУПНО ВСІМ]
    AimPart = "Head",
    MenuKey = Enum.KeyCode.Insert,
    ToggleKey = Enum.KeyCode.E
}

-- [[ СЕРВІСИ ]] --
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

local AimbotActive = false
local IsAdmin = false 

local Colors = {
    MainBlue = Color3.fromRGB(0, 100, 255),
    DarkBlue = Color3.fromRGB(0, 35, 100),
    Purple = Color3.fromRGB(120, 40, 180),
    AdminGold = Color3.fromRGB(255, 215, 0),
    LockedGrey = Color3.fromRGB(60, 60, 60),
    Background = Color3.fromRGB(15, 15, 15),
    TitleBar = Color3.fromRGB(25, 25, 25)
}

-- [[ ПЕРЕВІРКА КЛЮЧА ]] --
local function VerifyKeyRemote(input)
    local success, result = pcall(function()
        return game:HttpGet(API_URL .. "?check=" .. tostring(input))
    end)
    if success then
        if result == "admin" then IsAdmin = true return "ok"
        elseif result == "valid" then IsAdmin = false return "ok"
        elseif result == "expired_or_used" then return "used" end
    end
    return "fail"
end

-- [[ ІНТЕРФЕЙС ВХОДУ ]] --
if CoreGui:FindFirstChild("IlliaSystemV3") then CoreGui.IlliaSystemV3:Destroy() end
local MainSystemGui = Instance.new("ScreenGui", CoreGui)
local BG = Instance.new("Frame", MainSystemGui)
BG.Size = UDim2.new(1, 0, 1, 0)
BG.BackgroundColor3 = Colors.Background
BG.BorderSizePixel = 0

local Center = Instance.new("Frame", BG)
Center.Size = UDim2.new(0, 400, 0, 300)
Center.Position = UDim2.new(0.5, -200, 0.5, -150)
Center.BackgroundTransparency = 1

local LTitle = Instance.new("TextLabel", Center)
LTitle.Size = UDim2.new(1, 0, 0, 50)
LTitle.Text = "ILLIA V3 SYSTEM"
LTitle.TextColor3 = Color3.new(1, 1, 1)
LTitle.Font = Enum.Font.GothamBold
LTitle.TextSize = 35
LTitle.BackgroundTransparency = 1

local KeyContainer = Instance.new("Frame", Center)
KeyContainer.Size = UDim2.new(1, 0, 0, 160)
KeyContainer.Position = UDim2.new(0, 0, 0.45, 0)
KeyContainer.BackgroundTransparency = 1

local KeyInput = Instance.new("TextBox", KeyContainer)
KeyInput.Size = UDim2.new(0.85, 0, 0, 40)
KeyInput.Position = UDim2.new(0.075, 0, 0, 0)
KeyInput.PlaceholderText = "Введіть ключ..."
KeyInput.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
KeyInput.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", KeyInput)

local KeyBtn = Instance.new("TextButton", KeyContainer)
KeyBtn.Size = UDim2.new(0.85, 0, 0, 45)
KeyBtn.Position = UDim2.new(0.075, 0, 0.35, 0)
KeyBtn.BackgroundColor3 = Colors.MainBlue
KeyBtn.Text = "ПЕРЕВІРИТИ КЛЮЧ"
KeyBtn.TextColor3 = Color3.new(1, 1, 1)
KeyBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", KeyBtn)

local GetKeyBtn = Instance.new("TextButton", KeyContainer)
GetKeyBtn.Size = UDim2.new(0.85, 0, 0, 40)
GetKeyBtn.Position = UDim2.new(0.075, 0, 0.7, 0)
GetKeyBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
GetKeyBtn.Text = "ОТРИМАТИ КЛЮЧ (COPY LINK)"
GetKeyBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
Instance.new("UICorner", GetKeyBtn)

GetKeyBtn.MouseButton1Click:Connect(function()
    setclipboard(GEN_LINK)
    GetKeyBtn.Text = "СКОПІЙОВАНО!"
    task.wait(2)
    GetKeyBtn.Text = "ОТРИМАТИ КЛЮЧ (COPY LINK)"
end)

-- [[ МЕНЮ ЧИТА ]] --
local MainGui = Instance.new("ScreenGui", CoreGui)
MainGui.Enabled = false
local MainFrame = Instance.new("Frame", MainGui)
MainFrame.BackgroundColor3 = Colors.Background
MainFrame.Size = UDim2.new(0, 280, 0, 500)
MainFrame.Position = UDim2.new(0.5, -140, 0.5, -250)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Text = "ILLIA V3"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundColor3 = Colors.TitleBar
Title.Font = Enum.Font.GothamBold
Instance.new("UICorner", Title)

local UIList = Instance.new("UIListLayout", MainFrame)
UIList.Padding = UDim.new(0, 8)
UIList.HorizontalAlignment = Enum.HorizontalAlignment.Center
Instance.new("Frame", MainFrame).Size = UDim2.new(1,0,0,50)

-- УНІВЕРСАЛЬНА ФУНКЦІЯ ДЛЯ ПЕРЕМИКАЧІВ
local function AddToggle(text, prop, adminOnly)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(0.9, 0, 0, 38)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    btn.TextColor3 = Color3.new(1, 1, 1)
    
    local function update()
        if adminOnly and not IsAdmin then
            btn.Text = text .. " [🔒 ADMIN]"
            btn.BackgroundColor3 = Colors.LockedGrey
        else
            btn.Text = text .. ": " .. (Settings[prop] and "ON" or "OFF")
            btn.BackgroundColor3 = Settings[prop] and Colors.DarkBlue or Colors.Purple
        end
    end

    btn.MouseButton1Click:Connect(function()
        if adminOnly and not IsAdmin then
            btn.Text = "ONLY FOR ADMINS!"
            task.wait(1)
            update()
        else
            Settings[prop] = not Settings[prop]
            update()
        end
    end)
    update()
    Instance.new("UICorner", btn)
end

-- УНИВЕРСАЛЬНА ФУНКЦІЯ ДЛЯ СЛАЙДЕРІВ/ВИБОРУ
local function AddOption(text, prop, min, max, adminOnly)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(0.9, 0, 0, 38)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    btn.TextColor3 = Color3.new(1, 1, 1)

    local function update()
        if adminOnly and not IsAdmin then
            btn.Text = text .. " [🔒 ADMIN]"
            btn.BackgroundColor3 = Colors.LockedGrey
        else
            btn.Text = text .. ": " .. tostring(Settings[prop])
            btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        end
    end

    btn.MouseButton1Click:Connect(function()
        if adminOnly and not IsAdmin then
            btn.Text = "ONLY FOR ADMINS!"
            task.wait(1)
            update()
        else
            if Settings[prop] >= max then Settings[prop] = min else Settings[prop] = Settings[prop] + (max-min)/5 end
            Settings[prop] = math.round(Settings[prop] * 100) / 100
            update()
        end
    end)
    update()
    Instance.new("UICorner", btn)
end

-- [[ СПИСОК ФУНКЦІЙ ]] --
AddToggle("ESP Players", "ESP_Enabled", false)       -- Всім
AddToggle("ESP Rainbow", "ESP_Rainbow", true)        -- АДМІН
AddOption("ESP Color (Hue)", "ESP_Hue", 0.1, 1, true) -- АДМІН
AddToggle("Show FOV Circle", "ShowFOV", true)        -- АДМІН
AddOption("FOV Radius", "AimRadius", 50, 400, false)  -- Всім
AddOption("Aim Speed", "Sensitivity", 0.1, 1.5, false)-- Всім
AddToggle("Aimbot Master", "AimbotEnabled", false)   -- Всім

-- [[ ЛОГІКА ВХОДУ ]] --
KeyBtn.MouseButton1Click:Connect(function()
    KeyBtn.Text = "ПЕРЕВІРКА..."
    local status = VerifyKeyRemote(KeyInput.Text)
    if status == "ok" then
        KeyBtn.Text = "УСПІШНО"
        task.wait(0.5)
        MainSystemGui:Destroy()
        if IsAdmin then Title.Text = "ILLIA V3 [ADMIN]" end
        MainGui.Enabled = true
    else
        KeyBtn.Text = "ПОМИЛКА КЛЮЧА"
        task.wait(1)
        KeyBtn.Text = "ПЕРЕВІРИТИ КЛЮЧ"
    end
end)

-- [[ ОСНОВНИЙ ЦИКЛ ]] --
local FOVCircle = Drawing.new("Circle")
FOVCircle.Thickness = 1.5

RunService.RenderStepped:Connect(function()
    if not MainGui.Enabled then FOVCircle.Visible = false return end
    
    FOVCircle.Visible = Settings.ShowFOV
    FOVCircle.Radius = Settings.AimRadius
    FOVCircle.Position = UserInputService:GetMouseLocation()
    FOVCircle.Color = Color3.new(1,1,1)

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            local h = p.Character:FindFirstChild("ILLIA_ESP")
            if Settings.ESP_Enabled then
                if not h then h = Instance.new("Highlight", p.Character) h.Name = "ILLIA_ESP" end
                h.Enabled = true
                -- Колір та Веселка доступні тільки адмінам (через зміну Settings)
                h.FillColor = Settings.ESP_Rainbow and Color3.fromHSV((tick() * 0.5) % 1, 0.8, 1) or Color3.fromHSV(Settings.ESP_Hue, 0.8, 1)
            elseif h then
                h.Enabled = false
            end
        end
    end
end)

UserInputService.InputBegan:Connect(function(i, g)
    if i.KeyCode == Settings.MenuKey and MainGui.Enabled then MainFrame.Visible = not MainFrame.Visible end
end)
