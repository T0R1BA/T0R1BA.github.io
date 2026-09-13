---
title: "로블록스 스튜디오 데브로그: Keycaps"
categories: RobloxStudio
# excerpt: ""
---




# 📌 Keycaps
{: .notice}

<span class="color-control">Workspace</span> <br>
└─<span class="color-string">Keycaps</span> : 키캡들을 모아둘 모델 <br>
　 　├─<span class="color-function">Script</span> : 키캡 생성 스크립트 <br>
　 　└─<span class="color-function">Part </span> : 기즈모 파트

<br>

<span class="color-control">ServerScriptService</span> <br>
└─<span class="color-string">Script</span> : 플레이어에 감지 파트를 추가할 스크립트

<br>

<span class="color-control">ServerStorage</span> <br>
└─<span class="color-string">Keycap</span> : 키캡 원본 메시파트 <br>
　 　├─<span class="color-function">Sound</span> : 키캡이 밟혔을 때 낼 사운드 <br>
　 　└─<span class="color-function">SurfaceGui - Frame - TextLabel</span> : 키캡 표면에 알파벳 적용할 텍스트레이블

<br>

<span class="color-control">StarterPlayer</span> <br>
└─<span class="color-string">StarterPlayerScripts</span> <br>
　 　└─<span class="color-function">LocalScript</span> : 키캡의 사운드, 움직임, 충돌 감지 로컬 스크립트

<br>

<span class="RSST-VALUE">Workspace - Keycaps - Script</span>

```lua
local Keycaps = script.Parent
local Keycap = game.ServerStorage.Keycap -- 원본은 Storage에 넣어 원본 유지

local ALPHABET = {"A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"}
local BRICK_COLORS = {"Reddish brown", "Br. yellowish orange", "Cork"}

local OriginalPivot = Keycaps:GetPivot()
Keycaps:PivotTo(CFrame.new(OriginalPivot.Position)) -- 현재 Pivot의 Position은 가져오고 Rotation은 0으로 초기화
local ResetPivot = Keycaps:GetPivot()

local Width = 10
local Depth = 5
local Height = 1
local Spacing = 3

for x = 0, Width - 1 do
	for z = 0, Depth - 1 do
		local Part = Keycap:Clone()
		Part.Parent = Keycaps
		Part.SurfaceGui.Frame.TextLabel.Text = ALPHABET[math.random(#ALPHABET)] -- luau에서 #는 length를 구하는 방법
		Part.BrickColor = BrickColor.new(BRICK_COLORS[math.random(#BRICK_COLORS)]) -- 인자값을 하나만 넣을경우 최소값은 1로 고정 시작(luau에서 배열은 0이 아닌 1부터 시작)
		Part.Position = ResetPivot * Vector3.new(x * Spacing, Height, z * Spacing) -- Position과 CFrame의 차이: CFrame을 사용하면 회전값까지 따라감
	end
end

Keycaps:PivotTo(OriginalPivot) -- Keycaps에 기존 Rotation을 적용하기 위해 원래 Pivot 적용

-- Model에 Transform Gizmo 용도의 넣어둔 Part 비활성화
local Gizmo = Keycaps.Part
Gizmo.Transparency = 1
Gizmo.CanCollide = false
Gizmo.CanTouch = false
Gizmo.CanQuery = false
```

<br>

<span class="RSST-VALUE">ServerScriptService - Script</span>

```lua
local Players = game:GetService("Players")

Players.PlayerAdded:Connect(function(Player)
	Player.CharacterAdded:Connect(function(Character)
		local RootPart = Character:WaitForChild("HumanoidRootPart")

		local Detector = Instance.new("Part")
		Detector.Name = "Detector"
		Detector.Size = Vector3.new(2, 1, 2)
		Detector.Transparency = 1
		Detector.CanCollide = false
		Detector.CanTouch = true
		Detector.CanQuery = false
		Detector.Massless = true
		Detector.Anchored = false

		Detector.CFrame = RootPart.CFrame * CFrame.new(0, -2, 0)
		Detector.Parent = Character

		local Weld = Instance.new("WeldConstraint") -- RootPart와 Detector를 Weld로 연결
		Weld.Part0 = RootPart
		Weld.Part1 = Detector
		Weld.Parent = Detector
	end)
end)
```

<br>

<span class="RSST-VALUE">StarterPlayer - StarterPlayerScripts - LocalScript</span>

```lua
-- 해당 코드를 LocalScript로 만들어 플레이어 개별로 키캡과 상호작용하게 만들고, 서버를 거치지 않아 키캡이 반응하는 속도를 빠르게 함

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService") -- 키캡 움직임을 부드럽게 처리하기 위해 사용

local Player = Players.LocalPlayer -- 현재 LocalScript를 실행하고 있는 플레이어
local Keycaps = workspace:WaitForChild("Keycaps")

local CLICK_SOUND_IDS = {
	"rbxassetid://113108830240353",
	"rbxassetid://88838553648526",
	"rbxassetid://96591611478915",
}

local OriginalPositions = {} -- 키캡의 원래 위치를 기록해 놓을 테이블
local Pressed = {} -- 현재 눌려있는 키캡을 기록해 놓을 테이블

local PressTweenInfo = TweenInfo.new(0.05, Enum.EasingStyle.Quad, Enum.EasingDirection.Out) -- 0.05초 동안 Quad+Out(빠르게 움직여 천천히 멈춤)
local ReleaseTweenInfo = TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

local function GetOriginalPosition(Keycap)
	if not OriginalPositions[Keycap] then OriginalPositions[Keycap] = Keycap.Position end

	return OriginalPositions[Keycap]
end

local function Press(Keycap)
	if Pressed[Keycap] then return end

	Pressed[Keycap] = true
	local OriginalPosition = GetOriginalPosition(Keycap)
	local PressTween = TweenService:Create(Keycap, PressTweenInfo, {Position = OriginalPosition - Vector3.new(0, 1, 0)})
	PressTween:Play()

	local Sound = Keycap:FindFirstChild("Sound")
	if Sound then
		Sound.SoundId = CLICK_SOUND_IDS[math.random(#CLICK_SOUND_IDS)]
		Sound:Play()
	end
end

local function Release(Keycap)
	if not Pressed[Keycap] then return end

	Pressed[Keycap] = nil
	local OriginalPosition = GetOriginalPosition(Keycap)
	local ReleaseTween = TweenService:Create(Keycap, ReleaseTweenInfo, {Position = OriginalPosition})
	ReleaseTween:Play()
end

local Character = Player.Character or Player.CharacterAdded:Wait()
local Detector = Character:WaitForChild("Detector")

Detector.Touched:Connect(function(Hit)
	if Hit.Parent == Keycaps then Press(Hit) end
end)

Detector.TouchEnded:Connect(function(Hit)
	if Hit.Parent == Keycaps then Release(Hit) end
end)
```
