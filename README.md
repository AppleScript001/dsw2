local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()
local Mobs_Table = {}
for _, v in ipairs(workspace.Npc:GetDescendants()) do
    if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and not table.find(Mobs_Table, v.Name) then
        table.insert(Mobs_Table, v.Name)
        table.sort(Mobs_Table)
    end
end
local PlayerTP1
local TweenService = game:GetService("TweenService")
local X = Material.Load({
	Title = "🗡️ Damage 🎉",
	Style = 3,
	SizeX = 320,
	SizeY = 320,
	Theme = "Dark",
	ColorOverrides = {
		MainFrame = Color3.fromRGB(0,9,55)
	}
})
local Y = X.New({
	Title = "Main"
})
local ii = Y.Dropdown({
	Text = "Select Mobs!",
	Callback = function(t)
        PlayerTP1 = t
	end,
	Options = Mobs_Table
})
Y.Toggle({
    Text = "Auto Farm/Mobs",
    Callback = function(Value)
        _G.Attack = Value  -- Global variable to control the attack loop
        while _G.Attack do
            task.wait()  -- Wait for 1 second; adjust time as needed
            pcall(function()
                local toolName = "RainbowSword"  -- Replace with the name of your tool
                local LocalPlayer = game:GetService("Players").LocalPlayer
                
                -- Check if the tool is in the Backpack
                for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
                    if v:IsA('Tool') and v.Name == toolName then
                        v.Parent = LocalPlayer.Character
                        break
                    end
                end
                
                -- Activate the tool if it exists in the character
                if LocalPlayer.Character:FindFirstChild(toolName) then
                    LocalPlayer.Character:FindFirstChild(toolName):Activate()
                end
                for i, v in pairs(workspace.Npc:GetDescendants()) do
                    if v.Name == PlayerTP1 and v.Humanoid.Health > 0 then
                        -- Move towards NPC's position and attack
                        local targetCFrame = v.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = targetCFrame
                        -- Ensure character's gravity is maintained
                        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, -workspace.Gravity, 50)
                    end
                end
            end)
        end
    end,
    Enabled = false  -- Initially disabled until activated
})

