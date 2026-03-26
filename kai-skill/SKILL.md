---
name: ken-ops
description: KEN OPS task management - read, add, update, complete, delete tasks in Firebase
version: 1.0.0
---

# KEN OPS Task Manager

You can manage Ken's tasks stored in Firebase Realtime Database.

## Firebase URL
`https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app`

## Available Operations

### List all tasks
```bash
curl -s "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/tasks.json" | python3 -m json.tool
```

### Get game state (points, streak)
```bash
curl -s "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/gs.json"
```

### Add a new task
Use the next available index. First check how many tasks exist, then add at that index:
```bash
# Check task count
curl -s "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/tasks.json" | python3 -c "import json,sys; print(len(json.load(sys.stdin)))"

# Add task at index N (replace N with the count)
curl -s -X PUT "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/tasks/N.json" \
  -H "Content-Type: application/json" \
  -d '{"id":"TIMESTAMP","title":"TASK_TITLE","project":"PROJECT_ID","priority":"PRIORITY","deadline":"YYYY-MM-DD","status":"To Do","note":"","est_minutes":30,"preferred_time":"any"}'
```

### Update a task (e.g., complete it)
Find the task index first, then update:
```bash
# Find task by title (shows index)
curl -s "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/tasks.json" | python3 -c "
import json,sys
tasks=json.load(sys.stdin)
for i,t in enumerate(tasks):
  if 'SEARCH_TERM' in t.get('title','').lower():
    print(f'Index {i}: {t[\"title\"]} [{t[\"status\"]}]')
"

# Update status to Done
curl -s -X PATCH "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/tasks/INDEX.json" \
  -H "Content-Type: application/json" \
  -d '{"status":"Done"}'
```

### Delete a task
```bash
curl -s -X DELETE "https://ken-ops-default-rtdb.asia-southeast1.firebasedatabase.app/data/tasks/INDEX.json"
```

## Project IDs
- kai: Kai商品化
- golfin: GOLFIN
- funding: 資金繰り
- vexum: VEXUM
- club361: club361°
- sake_nft: 日本酒NFT
- pokedao: ポケダオ
- ai_agent: AIエージェント
- ss_kaikei: SS会計
- self: 自分自身
- company: 全社
- shinkawa: 新川問題

## Priority IDs
- urgent: 緊急 (40pt)
- high: 高 (25pt)
- medium: 中 (15pt)
- low: 低 (10pt)

## Preferred Time
- morning: 午前（集中系）
- afternoon: 午後（軽め）
- evening: 夜（クリエイティブ）
- any: いつでも

## Important
- When completing a task, update its status to "Done"
- KEN OPS dashboard auto-refreshes from Firebase, so changes appear immediately
- Always confirm with Ken before deleting tasks
- Use Japanese when talking to Ken
