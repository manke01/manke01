-- Anti AFK + Auto leave khi hết Full Moon

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer

-- Anti AFK
LocalPlayer.Idled:Connect(function()
    VirtualUser:CaptureController()
    VirtualUser:ClickButton2(Vector2.new())
end)

-- Check Full Moon
local function isFullMoon()
    return Lighting:GetMoonPhase() == Enum.MoonPhase.FullMoon
end

-- Auto leave khi hết Full Moon
while task.wait(5) do
    if not isFullMoon() then
        LocalPlayer:Kick("Hết Full Moon -> đổi server")
    end
end
