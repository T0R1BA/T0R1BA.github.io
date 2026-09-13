---
title: "로블록스 스튜디오 스크립트"
categories: RobloxStudio
# excerpt: ""
---




# 📌 PlayerAdded:Connect
{: .notice}

```lua
local <Variable Name> = game.Workspace.<Instance Name>

game.Players.PlayerAdded:Connect(function(Player)
	<Variable Name>.Touched:Connect(function(Hit)
		if Hit.Parent:FindFirstChild("Humanoid") then
			Player.Team = game.Teams["<Team Name>"]
		end
	end)
end)
```




# 📌 Function
{: .notice}

```lua
local function <Function Name>()
	-- Fucntion Body--
end
```




# 📌 Local, Global
{: .notice}

```lua
local <Variable Name> = <Value> -- local
<Variable Name> = <Value> -- Global
```




# 📌 + String +
{: .notice}

```lua
local <Variable Name_1> = <Value>
local <Variable Name_2> = <Value>
local <Variable Name_3> = game.StarterGui.<ScreenGui Instance Name>.<TextLabel Instance Name>

<Variable Name_3>.Text = <Variable Name_1> .. "<String Value>" .. <Variable Name_2>
```




# 📌 for
{: .notice}

```lua
for i = <Start Value>, <End Value>, <Step Value> do
    --For Body--
end
```




# 📌 for i, in pairs
{: .notice}

```lua
local <Table Name with an s> = {
	game.Workspace.<Instance Name>,
	game.Workspace.<Instance Name>,
	game.Workspace.<Instance Name>
}

for i, <Table Name> in pairs(<Table Name with an s>) do
	<Table Name>.Touched:Connect(function(Hit)
		if Hit.Parent:FindFirstChild("Humanoid") and then
			<Table Name> = <Value>
		end
	end)
end
```




# 📌 Repeat
{: .notice}

```lua
repeat
	-- Repeat Code --
	task.wait(<Wait Time>)
until <Bool Value>
```




# 📌 IsA, GetChildren
{: .notice}

```lua
local <Table Name with an s> = game.Workspace.<Folder Name>

for i, <Table Name> in pairs(<Table Name with an s>:GetChildren()) do
	if <Table Name>:IsA("<Class Name>") then -- Check Class Type
		<Table Name>.Touched:Connect(function(Hit)
			local Humanoid = Hit.Parent:FindFirstChild("Humanoid")
			if Humanoid then
				Humanoid.Health = 0 -- Take away Health
			end
		end)
	end
end
```




# 📌 Script, Local Script, Module Script
{: .notice}

<span class="color-keyword">Script</span> 서버에서만 처리해야 할 스크립트. NPC AI, 맵 오브젝트 제어, 플레이어 데이터 처리, 데미지/체력 처리, 게임 룰 등

<span class="color-keyword">Local Script</span> 클라이언트(플레이어) 개인에게서만 필요한 처리를 담당하는 스크립트. 키보드/마우스 Input, UI, 카메라, 플레이어의 화면 효과, 로컬 애니메이션 등

<span class="color-keyword">Module Script</span> 데이터 스크립트로 다른 Script, LocalScript가 가져다 쓰는 코드 모듈

```lua
-- 다른 스크립트에서 이런식으로 사용
local <Variable Name> = require(game.Workspace.ModuleScript)
<Variable Name>.<Function Name>()
```





# 📌 Sound
{: .notice}

```lua
local <Variable Name> = game.<Sound Instance Name>

<Variable Name>:Play()
<Variable Name>:Pause()
<Variable Name>:Stop()
```



# 📌 Tool Equipped, Unequipped, Activated
{: .notice}

```lua
local <Variable Name> = game.StarterPack.<Tool Instance Name>

<Variable Name>.Equipped:Connect(function()
	-- Tool 장착 시
end)

<Variable Name>.Unequipped:Connect(function()
	-- Tool 장착 해제 시
end)

<Variable Name>.Activated:Connect(function()
	-- Tool 장착 상태로 화면 클릭(탭) 시
end)
```




# 📌 Script에서 Instance 접근
{: .notice}

```lua
game. -- Explorer 접근
script. -- Explorer 내 해당 Script 기준으로 접근
```




# 📌 MouseButton1Click
{: .notice}

```lua
local <Variable Name> = game.StarterGui.<ScreenGui Instance Name>.<TextButton Instance Name>

<Variable Name>.MouseButton1Click:Connect(function()
	-- 버튼이 클릭됐을 때
end)
```




# 📌 Leaderboard
{: .notice}

```lua
game.Players.PlayerAdded:Connect(function(Player)
	local LeaderStats = Instance.new("Folder", Player)
	LeaderStats.Name = "leaderstats" -- leaderstats 대소문자에 주의
	
	-- Value Instance를 쓰면 다른 Script에서 쉽게 값에 접근이 가능
	local <Variable Name> = Instance.new("IntValue", LeaderStats)
	<Variable Name>.Name = "<Instance Name>"
	<Variable Name>.Value = <Value>
end)
```




# 📌 ClickDetector
{: .notice}

<pre>
Part
└─ ClickDetector
       └─ Script
</pre>

```lua
local ClickDetector = script.Parent

ClickDetector.MouseClick:Connect(function(Player)
	-- 해당 Player가 Part를 클릭했을 때를 설정할 수 있음
end)
```




# 📌 MoveTo를 이용해 포탈 만들기
{: .notice}

<pre>
Portals
├─ Script
├─ Protal_1
└─ Protal_2
</pre>

```lua
local Portals = script.Parent

local Portal_1 = Portals.Portal_1
local Portal_2 = Portals.Portal_2

local Debounce = false

game.Players.PlayerAdded:Connect(function(Player)
	local Character = Player.Character or Player.CharacterAdded:Wait()
	
	Portal_1.Touched:Connect(function(Hit)
		if Hit.Parent:FindFirstChild("Humanoid") and not Debounce then
			Debounce = true
			Character:MoveTo(Portal_2.Position)
			task.wait(2)
			Debounce = false
		end
	end)
end)
```




# 📌 투명해지면서 사라지는 발판 만들기
{: .notice}

```lua
local Part = script.Parent

local Debounce = false

local function Touched(Duration, Interval) -- 닿으면 투명해지는 함수
	while Part.Transparency < 1 do
		Part.Transparency += (Interval / Duration)
		task.wait(Interval)
	end

	Part.Transparency = 1
end

Part.Touched:Connect(function(hit)
	if Debounce then return end
	
	local character = hit.Parent
	local humanoid = character:FindFirstChild("Humanoid")

	if humanoid then
		Debounce = true
		Touched(4, 0.2)
		Part.CanCollide = false
		task.wait(3)
		Part.CanCollide = true
		Part.Transparency = 0
		Debounce = false
	end
end)
```




# 📌 깜빡이는 발판 만들기
{: .notice}

```lua
local Part = script.Parent

local Debounce = false

local function ChangeColor(Material_1, Material_2, WaitTime)
	repeat
		task.wait(WaitTime)
		if Debounce == false then
			if Part.Material == Material_1 then Part.Material = Material_2
			else Part.Material = Material_1
			end
		end
	until Part == true and Part ~= nil
end

task.spawn(ChangeColor, Enum.Material.Plastic, Enum.Material.Neon, 1)
```




# 📌 속성 값이 바뀌었을 때 사용하는 함수 Changed
{: .notice}

```lua
local Part = script.Parent

Part.Changed:Connect(function(Property)
	if Property == "Transparency" then
		print("Transparency Changed")
	end
end)
```




# 📌 Attachment Instance
{: .notice}

Part안에 필요한 Transform을 따로 잡는 용도, Unity의 Empty GameObject와 비슷한 용도




# 📌 ProximityPormpt
{: .notice}

오브젝트에 가까이 갔을 때 상호작용할 수 있게 해주는 Instance

<span class="color-keyword">ActionText</span>

<span class="color-keyword">HoldDuration</span>




# 📌 Module Script
{: .notice}

<span class="color-control">ServerScriptService</span> <br>
├─<span class="color-string">Module Script</span> <br>
└─<span class="color-string">Script</span>

<br>

<span class="color-string">Module Script</span>

```lua
local module = {
	
	["Apple"] = {
		["Cost"] = 10,
		["Health"] = 20,
	},
	
	["Banana"] = {
		["Cost"] = 5,
		["Health"] = 10,
	}
	
}

module.AddFunction = function(Number1, Number2)
	return Number1 + Number2
end

return module
```

<br>

<span class="color-string">Script</span>

```lua
local MyModule = require(game.ServerScriptService.ModuleScript)

print(MyModule.AddFunction(MyModule.Apple.Cost, MyModule.Banana.Cost))
```




# 📌 CFrame
{: .notice}

Script
```lua
local Part = game.Workspace.Part

Part.CFrame = CFrame.new(1, 2, 3)
Part.CFrame = Part.CFrame + Vector3.new(1, 2, 3)
Part.CFrame = CFrame.lookAt(Part.Position, game.Workspace.SpawnLocation.Position)
Part.CFrame = CFrame.new(1, 2, 3) * CFrame.Angles(math.rad(90), 0, 0) -- 회전

Part.CFrame = game.Workspace.Part0.CFrame:Lerp(game.Workspace.Part1.CFrame, 0.5) -- Part0과 Part1의 0.5 값을 구함

Part.CFrame = Part.CFrame + (Part.CFrame.LookVector * 10) -- 현재 바라보는 방향으로 10만큼 이동
```

Local Script
```lua
local Camera = game.Workspace.CurrentCamera -- 현재 게임에서 사용 중인 플레이어 카메라를 가져옴
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()

Camera.CameraType = Enum.CameraType.Scriptable -- 카메라를 스크립트로 직접 제어하도록 변경

Camera.Changed:Connect(function(Property)
	if Property == "CameraType" and Camera.CameraType ~= Enum.CameraType.Scriptable then 
		Camera.CameraType = Enum.CameraType.Scriptable -- 카메라 타입이 어떠한 이유로 바뀌어도 강제로 Scriptable로 고정
	end
end)

Camera.CFrame = CFrame.lookAt(Character.HumanoidRootPart.Position, game.Workspace.Part.Position)
```




# 📌 Raycast
{: .notice}

Script
```lua
local Part1 = game.Workspace.Part
local Part2 = game.Workspace.EndPart

local Origin = Part1.Position
local Direction = Part2.Position - Part1.Position

local RaycastParams = RaycastParams.new()
RaycastParams.FilterType = Enum.RaycastFilterType.Exclude -- 다음에서 지정할 Part들을 Raycast 검사에서 제외시킴
RaycastParams.FilterDescendantsInstances = {Part1, Part2} -- 검사에서 제외할 Part 지정

while true do

	local RaycastResult = workspace:Raycast(Origin, Direction, RaycastParams)

	if RaycastResult then
		print("Intersection" .. RaycastResult.Instance.Name)
	else
		print("No Intersection")
	end

	task.wait(0.1)
end
```




# 📌 os.time, os.clock
{: .notice}

Script
```lua
local Part1 = game.Workspace.Part
local Part2 = game.Workspace.EndPart

local StartTime = nil

Part1.Touched:Connect(function(Hit)
	if Hit.Parent:FindFirstChild("Humanoid") then
		StartTime = os.clock() -- Part1에 닿는 순간의 시각을 기록해 스톱워치 시작
		print(os.clock()) -- os.clock()의 경우 소수점까지 기록, os.time()의 경우 초단위까지만 기록
	end
end)

Part2.Touched:Connect(function(Hit)
	if Hit.Parent:FindFirstChild("Humanoid") then
		print(os.clock() - StartTime) -- Part2에 닿는 순간 현재 시각에서 기록한 시간을 빼 기록 계산
	end
end)
```