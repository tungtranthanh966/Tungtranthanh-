--================================================
-- NPC GAMING HUB | BLOX FRUITS (TAB UI)
--================================================

local Players = game:GetService("Players")
local VirtualUser = game:GetService("VirtualUser")
local player = Players.LocalPlayer

--================= BIẾN =================
local AutoFarm = false
local SpeedOn = false
local JumpOn = false

--================= ANTI AFK =================
player.Idled:Connect(function()
	VirtualUser:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
	task.wait(1)
	VirtualUser:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

--================= AUTO FARM =================
function GetNearestMob()
	local dist, mob = math.huge, nil
	for _,v in pairs(workspace.Enemies:GetChildren()) do
		if v:FindFirstChild("Humanoid")
		and v.Humanoid.Health > 0
		and v:FindFirstChild("HumanoidRootPart") then
			local mag = (v.HumanoidRootPart.Position -
				player.Character.HumanoidRootPart.Position).Magnitude
			if mag < dist then
				dist = mag
				mob = v
			end
		end
	end
	return mob
end

task.spawn(function()
	while task.wait() do
		if AutoFarm and player.Character then
			local mob = GetNearestMob()
			if mob then
				player.Character.HumanoidRootPart.CFrame =
					mob.HumanoidRootPart.CFrame * CFrame.new(0,0,3)
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Attack")
			end
		end
	end
end)

--================= SPEED / JUMP =================
task.spawn(function()
	while task.wait() do
		if SpeedOn and player.Character then
			player.Character.Humanoid.WalkSpeed = 50
		end
		if JumpOn and player.Character then
			player.Character.Humanoid.JumpPower = 100
		end
	end
end)

--================================================
-- GUI TAB (BANANA HUB STYLE)
--================================================

local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "NPCGamingHub"

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0,420,0,260)
Main.Position = UDim2.new(0.3,0,0.25,0)
Main.BackgroundColor3 = Color3.fromRGB(20,20,20)
Main.Active = true
Main.Draggable = true

-- Title
local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1,0,0,40)
Title.Text = "NPC GAMING HUB | BLOX FRUITS"
Title.TextColor3 = Color3.fromRGB(255,170,0)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16

-- Tabs
local TabFrame = Instance.new("Frame", Main)
TabFrame.Size = UDim2.new(0,120,1,-40)
TabFrame.Position = UDim2.new(0,0,0,40)
TabFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)

local Content = Instance.new("Frame", Main)
Content.Size = UDim2.new(1,-120,1,-40)
Content.Position = UDim2.new(0,120,0,40)
Content.BackgroundColor3 = Color3.fromRGB(25,25,25)

--================= TAB CONTENT =================
local MainTab = Instance.new("Frame", Content)
MainTab.Size = UDim2.new(1,0,1,0)
MainTab.Visible = true
MainTab.BackgroundTransparency = 1

local PlayerTab = Instance.new("Frame", Content)
PlayerTab.Size = UDim2.new(1,0,1,0)
PlayerTab.Visible = false
PlayerTab.BackgroundTransparency = 1

--================= TAB BUTTON =================
local function CreateTab(text, pos, callback)
	local btn = Instance.new("TextButton", TabFrame)
	btn.Size = UDim2.new(1,0,0,40)
	btn.Position = UDim2.new(0,0,0,pos)
	btn.Text = text
	btn.BackgroundColor3 = Color3.fromRGB(255,140,0)
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.MouseButton1Click:Connect(callback)
end

CreateTab("MAIN", 0, function()
	MainTab.Visible = true
	PlayerTab.Visible = false
end)

CreateTab("PLAYER", 40, function()
	MainTab.Visible = false
	PlayerTab.Visible = true
end)

--================= MAIN TAB =================
local FarmBtn = Instance.new("TextButton", MainTab)
FarmBtn.Size = UDim2.new(0.9,0,0,40)
FarmBtn.Position = UDim2.new(0.05,0,0.15,0)
FarmBtn.Text = "Auto Farm : OFF"
FarmBtn.BackgroundColor3 = Color3.fromRGB(255,140,0)
FarmBtn.TextColor3 = Color3.new(1,1,1)

FarmBtn.MouseButton1Click:Connect(function()
	AutoFarm = not AutoFarm
	FarmBtn.Text = AutoFarm and "Auto Farm : ON" or "Auto Farm : OFF"
end)

--================= PLAYER TAB =================
local SpeedBtn = Instance.new("TextButton", PlayerTab)
SpeedBtn.Size = UDim2.new(0.9,0,0,40)
SpeedBtn.Position = UDim2.new(0.05,0,0.15,0)
SpeedBtn.Text = "Speed : OFF"
SpeedBtn.BackgroundColor3 = Color3.fromRGB(255,140,0)
SpeedBtn.TextColor3 = Color3.new(1,1,1)

SpeedBtn.MouseButton1Click:Connect(function()
	SpeedOn = not SpeedOn
	SpeedBtn.Text = SpeedOn and "Speed : ON" or "Speed : OFF"
	player.Character.Humanoid.WalkSpeed = SpeedOn and 50 or 16
end)

local JumpBtn = Instance.new("TextButton", PlayerTab)
JumpBtn.Size = UDim2.new(0.9,0,0,40)
JumpBtn.Position = UDim2.new(0.05,0,0.35,0)
JumpBtn.Text = "Jump : OFF"
JumpBtn.BackgroundColor3 = Color3.fromRGB(255,140,0)
JumpBtn.TextColor3 = Color3.new(1,1,1)

JumpBtn.MouseButton1Click:Connect(function()
	JumpOn = not JumpOn
	JumpBtn.Text = JumpOn and "Jump : ON" or "Jump : OFF"
	player.Character.Humanoid.JumpPower = JumpOn and 100 or 50
end)
