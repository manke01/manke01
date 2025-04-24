
-- 💩 mankefarm v1.0 - farm tới ỉa
-- Bản mới: +Auto Farm Level + Nhiệm Vụ, +Farm Rip Indra

repeat wait() until game:IsLoaded()

-- UI Library
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/watermark"))()
local Window = Library:CreateWindow("💩 mankefarm v1.0 - farm tới ỉa")

-- Tabs
local MainTab = Window:CreateTab("Main")
local WebhookTab = Window:CreateTab("Webhook")

-- Sections
local FarmSection = MainTab:CreateSection("Tính năng Auto Farm")
local WebhookSection = WebhookTab:CreateSection("Webhook Discord")

-- Global toggles
_G.AutoFarm = true
_G.AutoHop = true
_G.FarmGodhuman = true
_G.FarmMirrorFractal = true
_G.FarmAdminHat = false
_G.AutoFarmLevel = true
_G.AutoFarmIndra = true
_G.WebhookEnabled = false
_G.WebhookURL = ""

-- Toggles
FarmSection:CreateToggle("Bật Auto Farm", false, function(v) _G.AutoFarm = v end)
FarmSection:CreateToggle("Auto Hop Server", false, function(v) _G.AutoHop = v end)
FarmSection:CreateToggle("Auto Farm Godhuman", false, function(v) _G.FarmGodhuman = v end)
FarmSection:CreateToggle("Auto Farm Mảnh Gương (Mirror Fractal)", false, function(v) _G.FarmMirrorFractal = v end)
FarmSection:CreateToggle("Auto Farm Mũ Admin", false, function(v) _G.FarmAdminHat = v end)
FarmSection:CreateToggle("Auto Farm Level + Nhận Nhiệm Vụ", false, function(v) _G.AutoFarmLevel = v end)
FarmSection:CreateToggle("Auto Farm Rip Indra", false, function(v) _G.AutoFarmIndra = v end)

-- Webhook settings
WebhookSection:CreateToggle("Bật gửi Webhook khi ra Kitsune", false, function(v) _G.WebhookEnabled = v end)
WebhookSection:CreateTextBox("Dán Webhook Discord tại đây", "https://discord.com/api/webhooks/...", true, function(v) _G.WebhookURL = v end)

-- Demo loop (replace with real logic)
spawn(function()
    while task.wait(1) do
        if _G.AutoFarm then print("▶ Farm tới ỉa...") end
        if _G.FarmGodhuman then print("🥋 Đang farm Godhuman...") end
        if _G.FarmMirrorFractal then print("🔮 Farm Mirror Fractal...") end
        if _G.FarmAdminHat then print("🎩 Farm Mũ Admin...") end
        if _G.AutoHop then print("💨 Tự hop server khi cần...") end
        if _G.AutoFarmLevel then print("📈 Auto farm level và nhận nhiệm vụ...") end
        if _G.AutoFarmIndra then print("👑 Tìm và farm Rip Indra...") end
    end
end)
