---
title: "로블록스 스튜디오 스크립트: Table"
categories: RobloxStudio
excerpt: "Array Table, ipairs, Dictionary Table, pairs"
---




# 📌 Table?
{: .notice}

luau에서 Array와 Dictionary 형태로 사용




# 📌 Array Table, ipairs
{: .notice}

<br>

<span class="RSST-VALUE">Array Table</span>

```lua
-- 인덱스는 1부터 시작, [Index]를 Key로 사용하는 딕셔너리 형태
local Array = {} -- 테이블 선언, 딕셔너리 형태이기 때문에 배열의 크기를 처음에 지정하지 않아도 됨
Array = {"A", "B", "C", 100, [6]=200, "Hello", [7]="World"}
print(Array[0]) -- nil -- 인덱스는 1부터 시작해 0은 nil
print(Array[1]) -- A
print(Array[2]) -- B
print(Array[3]) -- C
print(Array[4]) -- 100 -- 다른 DataType도 가능
print(Array[5]) -- Hello -- 6번 인덱스를 지정해서 썼기 때문에 알아서 5번 인덱스로 지정(6번 인덱스를 지정했지만 다음 값은 알아서 7번 인덱스를 사용하지 않음)
print(Array[6]) -- 200 -- [6]=200으로 인덱스를 지정할 수도 있음, [Index]를 Key로 사용하고 있다는 의미
print(Array[7]) -- World
table.insert(Array, "Inserted") -- 배열의 마지막에 값 추가
print(#Array) -- 8 -- 배열의 길이를 구하는 방법
print(Array[8]) -- Inserted
table.remove(Array, 7) -- 배열의 인덱스 7의 값 제거, 뒤의 값을 앞으로 당김
print(#Array) -- 7
print(Array[8]) -- nil
table.insert(Array, 2, "Temp") -- 배열의 인덱스 2에 값 추가, 기존 값은 뒤로 밀림
Array[1] = nil -- 인덱스 1을 제거했지만 배열에서 remove를 사용하지 않고 nil로 제거했기 때문에 뒤의 Key들이 앞으로 당겨지지 않음
Array[9] = {Name="Potato", Price=20000} -- Table안에 이중 Table도 가능
print(Array)
--{
--	[1] = nil,
--	[2] = "Temp",
--	[3] = "B",
--	[4] = "C",
--	[5] = 100,
--	[6] = "Hello",
--	[7] = 200,
--	[8] = "Inserted",
--	[9] = {
--		["Name"] = "Potato",
--		["Price"] = 20000
--	}
--}
```

<br>

<span class="RSST-VALUE">ipairs</span>

```lua
-- ipairs : 배열을 순차적으로 순회할 때 사용
-- 배열 형태여야 함 : 키 값이 [1], [2], [3] ... 인 딕셔너리
-- 중간에 remove가 아닌 nil로 Key, Value를 삭제했다면 그 다음으로 넘어가지 않고 순회가 끝남
local Fruits = {"Apple", "Banana", "Coconut", [4]="Durian"}
Fruits[3] = nil
for i, Fruit in ipairs(Fruits) do
	print(i, Fruit) -- 1 Apple, 2 Banana -- 3번째 Key, Value를 nil로 제거해 순회가 끝남
end
print(Fruits[4]) -- Durian -- 4번째 Key와 Value는 남아있음
```




# 📌 Dictionary Table, pairs
{: .notice}

<br>

<span class="RSST-VALUE">Dictionary Table</span>

```lua
local Parts = {} -- 테이블 선언

for i=1, 4 do
	local Part = Instance.new("Part")
	Part.Parent = workspace
	Part.Name = "Part" .. i
	Part.Position = Vector3.new(math.random(10), math.random(10), math.random(10))
	if i~=4 then Parts[Part] = Part.Position -- Dictionary Table에 Key와 Value를 넣는 방법, Instance도 Key로 넣을 수 있음
	else Parts[Part] = {Position = Part.Position, Name = Part.Name} -- 이중 Table
	end
end

local Length = 0
for Key, Value in pairs(Parts) do
	print(Key.Name) -- 현재 Key인 Intance에 바로 접근 -- Part2, Part3, Part1, Part4
	print("Key:", Key, "Value:", Value)
	-- Key: Part2 Value: 7, 3, 3
	-- Key: Part3 Value: 5, 5, 4
	-- Key: Part1 Value: 3, 8, 9
	-- Key: Part4 Value: {["Name"] = "Part4", ["Position"] = 6, 2, 4}
	Length += 1 -- Dictionary Table에서 Length를 구하는 방법, luau에선 ++연산자 사용 불가
end
print("Parts.Length:", Length) -- Parts.Length: 4

local Index = 0
for Key, Value in pairs(Parts) do
	Index += 1
	if Index == 3 then Parts[Key] = nil end -- Value를 nil로 넣으면 해당 Key까지 제거됨, Key와 Value를 삭제하는 방법
	if Index == 4 then Parts[Key] = false end -- Value를 false나 "" 빈 문자열로 넣으면 해당 Key는 유지됨
end

Length = 0
for Key, Value in pairs(Parts) do
	print("Key:", Key, "Value:", Value)
	-- Key: Part2 Value: 7, 3, 3
	-- Key: Part3 Value: 5, 5, 4
	-- Key: Part4 Value: false
	Length += 1
end
print("Parts.Length:", Length) -- Parts.Length: 3
```

<br>

<span class="RSST-VALUE">pairs</span>

```lua
-- pairs : Table의 Key, Value를 순회할 때 사용
-- 순회 순서는 보장되지 않음
-- 배열 Table에서도 사용할 수 있지만, 배열은 일반적으로 ipairs()를 사용, table.insert()와 table.remove()를 이용해 Index를 연속적으로 관리하기 때문
-- 배열 Table에서 사용 시, 순서를 신경쓰지 않기 때문에 nil로 Key, Value를 제거해도 순회가 끝나지 않음
local PlayerData = {Name="James", Level=10, Cash=500, Speed=32}
PlayerData.Cash = nil -- Key, Value 삭제
for Key, Value in pairs(PlayerData) do
	print(Key, Value) -- Speed 32, Level 10, Name James -- 순서가 보장되지 않음
end
print(PlayerData.Name) -- James, .으로 접근하는 방법으로 Key가 문자열일 때 가능함
print(PlayerData["Name"]) -- James, []으로 접근하는 방법으로 Key 자체를 넣어야 함
print(PlayerData.Cash) -- nil, Key가 존재하지 않아 nil 반환
```