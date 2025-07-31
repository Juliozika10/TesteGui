local UILib = {}
UILib.__index = UILib

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

function UILib:CreateWindow(name)
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = name or "CustomUILib"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = PlayerGui

    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 400, 0, 300)
    MainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
    MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    MainFrame.BorderSizePixel = 0
    MainFrame.Parent = ScreenGui

    local TabContainer = Instance.new("Frame")
    TabContainer.Name = "TabContainer"
    TabContainer.Size = UDim2.new(0, 120, 1, 0)
    TabContainer.Position = UDim2.new(0, 0, 0, 0)
    TabContainer.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    TabContainer.Parent = MainFrame

    local ContentFrame = Instance.new("Frame")
    ContentFrame.Name = "ContentFrame"
    ContentFrame.Size = UDim2.new(1, -120, 1, 0)
    ContentFrame.Position = UDim2.new(0, 120, 0, 0)
    ContentFrame.BackgroundTransparency = 1
    ContentFrame.Parent = MainFrame

    local UIList = Instance.new("UIListLayout")
    UIList.Padding = UDim.new(0, 5)
    UIList.SortOrder = Enum.SortOrder.LayoutOrder
    UIList.Parent = TabContainer

    self.ScreenGui = ScreenGui
    self.MainFrame = MainFrame
    self.Tabs = {}
    self.ContentFrame = ContentFrame

    return setmetatable(self, UILib)
end

function UILib:CreateTab(name)
    local TabButton = Instance.new("TextButton")
    TabButton.Size = UDim2.new(1, 0, 0, 40)
    TabButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    TabButton.TextColor3 = Color3.new(1, 1, 1)
    TabButton.Text = name
    TabButton.Parent = self.MainFrame.TabContainer

    local TabContent = Instance.new("Frame")
    TabContent.Name = name
    TabContent.Size = UDim2.new(1, 0, 1, 0)
    TabContent.BackgroundTransparency = 1
    TabContent.Visible = false
    TabContent.Parent = self.ContentFrame

    local Layout = Instance.new("UIListLayout")
    Layout.Padding = UDim.new(0, 8)
    Layout.SortOrder = Enum.SortOrder.LayoutOrder
    Layout.Parent = TabContent

    TabButton.MouseButton1Click:Connect(function()
        for _, tab in pairs(self.Tabs) do
            tab.Frame.Visible = false
        end
        TabContent.Visible = true
    end)

    table.insert(self.Tabs, {Button = TabButton, Frame = TabContent})

    return TabContent
end

function UILib:AddButton(tab, text, callback)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, -10, 0, 40)
    Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    Button.TextColor3 = Color3.new(1, 1, 1)
    Button.Text = text
    Button.Parent = tab

    Button.MouseButton1Click:Connect(callback)
end

function UILib:AddToggle(tab, text, callback)
    local Toggle = Instance.new("TextButton")
    Toggle.Size = UDim2.new(1, -10, 0, 40)
    Toggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Toggle.TextColor3 = Color3.new(1, 1, 1)
    Toggle.Text = "[ OFF ] " .. text
    Toggle.Parent = tab

    local enabled = false

    Toggle.MouseButton1Click:Connect(function()
        enabled = not enabled
        Toggle.Text = (enabled and "[ ON ] " or "[ OFF ] ") .. text
        callback(enabled)
    end)
end

return UILib
