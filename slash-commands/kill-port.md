---
description: Kill processes running on a specific port
argument-hint: PORT=<port-number>
---

Kill processes on port(s): $PORT (default: 3000)

Steps:
1. Identify process: lsof -ti:$PORT (macOS/Linux) or netstat -ano | findstr :$PORT (Windows)
2. Kill gracefully: kill $(lsof -ti:$PORT) then force: kill -9 $(lsof -ti:$PORT)
3. Windows: taskkill /PID $PID /F
4. Verify: lsof -ti:$PORT (should return nothing)
5. Warn about system processes, suggest service stops for critical services

macOS/Linux: lsof -ti:$PORT | xargs kill -9
Windows: netstat -ano | findstr :$PORT then taskkill /PID $PID /F
Alternative: sudo fuser -k $PORT/tcp (Linux)