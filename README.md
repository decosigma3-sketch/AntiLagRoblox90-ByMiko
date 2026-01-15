-- =====================================================
-- TSB VISUAL PACK — AI TRAP EDITION
-- ULTRA LIGHT, NO LAG, LOGIC TERSUMBAR
-- =====================================================

if _G.TSB_VISUAL_AI_TRAP then return end
_G.TSB_VISUAL_AI_TRAP = true

-- ================= STAGE 1: DEAD CODE / FAKE =================
do
    local Players = game:GetService("Players")
    local lp = Players.LocalPlayer

    -- Notifikasi pop-up credit
    if lp then
        local StarterGui = game:GetService("StarterGui")
        StarterGui:SetCore("SendNotification", {
            Title = "Script Made by Miko",   -- Judul notifikasi
            Text = "Script di buat oleh Miko", -- Isi notifikasi
            Duration = 5,                     -- Durasi 5 detik
        })
    end

    print("TSB Visual Pack Booting...")

    for _=1,5 do
        task.wait(0.01)
        local dummy = math.random()
        if dummy > 100 then -- never triggers
            while true do end
        end
    end

    local fakeFunc1, fakeFunc2 = function() end, function() end
    local fakeVar = {a=1,b=2,c=3}
end

-- ================= STAGE 2: LOGIC TERSEMBUNYI =================
do
    -- helper runtime decode string
    local function decode(t)
        local s=""
        for i=1,#t do
            s ..= string.char(t[i]-1)
        end
        return s
    end

    -- disguised services
    local P,L,W = game:GetService("Players"), game:GetService("Lighting"), game:GetService("Workspace")
    local lp = P.LocalPlayer
    local T = W:FindFirstChildOfClass("Terrain")

    -- VM / dispatcher pattern
    local VM = {}

    VM[1] = function() -- HERO HUNTER EFFECT
        local BLUE = Color3.fromRGB(0,85,190)
        local function apply(v)
            if v:IsA("Trail") then
                v.Color = ColorSequence.new(BLUE)
                v.Texture = ""
                v.LightEmission = 0.8
            elseif v:IsA("ParticleEmitter") then
                v.Color = ColorSequence.new(BLUE)
                v.LightEmission = 0.75
            end
        end
        local function hook(c)
            for _,v in ipairs(c:GetDescendants()) do if v:IsA("Trail") or v:IsA("ParticleEmitter") then apply(v) end end
            c.DescendantAdded:Connect(function(v) if v:IsA("Trail") or v:IsA("ParticleEmitter") then apply(v) end end)
        end
        if lp.Character then hook(lp.Character) end
        lp.CharacterAdded:Connect(hook)
    end

    VM[2] = function() -- SKYBOX
        local SkyIDs={Bk=92959017845968,Ft=129304841254693,Lf=129249062260004,Rt=117319232583147,Up=121193772599100,Dn=115022734343595}
        local function apply()
            for _,v in ipairs(L:GetChildren()) do if v:IsA("Sky") then v:Destroy() end end
            local s = Instance.new("Sky")
            for k,id in pairs(SkyIDs) do s["Skybox"..k]="rbxassetid://"..id end
            s.Parent=L
        end
        apply()
        L.ChildAdded:Connect(function(v) if v:IsA("Sky") then task.wait(); apply() end end)
    end

    VM[3] = function() -- LIGHTING
        L.ClockTime=12; L.GlobalShadows=false; L.Brightness=0.8
        L.ExposureCompensation=-0.2
        L.Ambient=L.Ambient or Color3.fromRGB(120,120,120)
        L.OutdoorAmbient=L.OutdoorAmbient or Color3.fromRGB(120,120,120)
        L.FogStart,L.FogEnd=9e9,9e9
        local s=L:FindFirstChildOfClass("Sky")
        if s then s.SunAngularSize=0; s.SunTextureId="" end
        for _,v in ipairs(L:GetChildren()) do if v:IsA("SunRaysEffect") or v:IsA("PostEffect") then v.Enabled=false end end
        L.ChildAdded:Connect(function() VM[3]() end)
    end

    VM[4] = function() -- REMOVE PARTICLE / SMOKE
        local function remove(v)
            if v:IsA("Smoke") then return true end
            if v:IsA("ParticleEmitter") then
                local m=v:FindFirstAncestorOfClass("Model")
                if m and m:FindFirstChild("Class") then return m.Class.Value~="Hero Hunter" end
                return true
            end
            return false
        end
        for _,v in ipairs(W:GetDescendants()) do if remove(v) then v:Destroy() end end
        W.DescendantAdded:Connect(function(v) if remove(v) then v:Destroy() end end)
    end

    VM[5] = function() -- REMOVE CLOUDS
        if not T then return end
        for _,v in ipairs(T:GetChildren()) do if v:IsA("Clouds") then v.Enabled=false end end
        T.ChildAdded:Connect(function(v) if v:IsA("Clouds") then v.Enabled=false end end)
    end

    -- RANDOMIZED EXECUTION
    local order={1,2,3,4,5}
    for i=1,#order do
        local idx=math.random(1,#order)
        pcall(VM[order[idx]])
        table.remove(order,idx)
    end

    -- LOW QUALITY OPTIONAL
    pcall(function() settings().Rendering.QualityLevel=Enum.QualityLevel.Level01 end)

    print("✅ TSB VISUAL PACK — AI TRAP AKTIF")
end
