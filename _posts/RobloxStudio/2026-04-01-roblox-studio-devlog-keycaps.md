---
title: "로블록스 스튜디오 데브로그: Keycaps"
categories: RobloxStudio
# excerpt: ""
---




# 📌 Keycaps
{: .notice}

<span class="color-control">Workspace</span> <br>
└─<span class="color-string">Keycaps</span> <br>
　 　├─<span class="color-function">Script</span> <br>
　 　└─<span class="color-function">Part</span>

<span class="color-control">ServerScriptService</span> <br>
└─<span class="color-string">Script</span>

<span class="color-control">ServerStorage</span> <br>
└─<span class="color-string">Keycap</span> <br>
　 　├─<span class="color-function">Sound</span> <br>
　 　├─<span class="color-function">Script</span> <br>
　 　└─<span class="color-function">SurfaceGui - Frame - TextLabel</span>

Workspace Script
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

ServerScriptService Script
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

ServerStorage Script
```lua
local Part = script.Parent
local OriginalPosition = Part.Position
local Sound = Part.Sound

local CLICK_SOUND_IDS = {
	"rbxassetid://113108830240353",
	"rbxassetid://88838553648526",
	"rbxassetid://96591611478915",
}

Part.Touched:Connect(function(Hit)
	if Hit.Name == "Detector" then
		Part.Position = OriginalPosition - Vector3.new(0, 1, 0)
		Sound.SoundId = CLICK_SOUND_IDS[math.random(#CLICK_SOUND_IDS)]
		Sound:Play()
	end
end)

Part.TouchEnded:Connect(function(Hit)
	if Hit.Name == "Detector" then
		Part.Position = OriginalPosition
	end
end)
```
