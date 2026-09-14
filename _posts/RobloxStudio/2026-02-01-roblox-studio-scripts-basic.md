---
title: "로블록스 스튜디오 스크립트: 기본"
categories: RobloxStudio
# excerpt: ""
---




# 📌 for
{: .notice}

```lua
for i = 1, 3 do -- Start, End
	print(i) -- 1, 2, 3
end

for i = 1, 10, 2 do -- Start, End, Step
	print(i) -- 1, 3, 5, 7, 9
end
```




# 📌 repeat
{: .notice}

```lua
local JoinTime = time()
local CurrentPlayTime = 0
repeat
	task.wait(1)
	CurrentPlayTime = time() - JoinTime
	print("PlayTime: ", math.floor(CurrentPlayTime))
until false
```




# 📌 문자열 연결
{: .notice}

```lua
local StringA = "CozyCozy"
local StringB = 4
local StringC = StringA .. StringB .. "Tato" -- 문자열 연결
print(StringC) -- CozyCozy4Tato -- 숫자도 문자열로 변환하여 연결
```




# 📌 function
{: .notice}

```lua
local function Add(A, B)
	return A + B
end

print(Add(1000, 24)) -- 1024
```




# 📌 Instance 접근 방법
{: .notice}

```lua
print(game.ServerScriptService.ApproachInstance) -- ApproachInstance(현재 스크립트) -- game부터 접근
print(script.Parent.Parent) -- game -- 현재 스크립트부터 접근
```