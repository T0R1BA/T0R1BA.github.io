---
title: "로블록스 스튜디오 스크립트: 테이블"
categories: RobloxStudio
# excerpt: ""
---




# 📌 Table
{: .notice}

```lua
local <Table Name> = {
	game.Workspace.<Instance Name>,
	game.Workspace.<Instance Name>,
	game.Workspace.<Instance Name>
}

table.insert(<Table Name>, <Value>)
table.remove(<Table Name>, <Index>) -- Index 번째 Value를 지우고 뒤의 값들을 앞당김
<Table Name>[Index] = nil -- Index 번째 Value를 nil값(null)으로 둠
```
