# TS2_Conveyor_Problem
TwinCAT PLC project for Bosch LTU transfer with queue-based scheduler
## Features
- Modular FBs for Stopper, LTU, Reader, Conveyor
- State machine logic for each transfer path:
  - Direct (e.g., 12 → 6, 6 → 12)
  - Corner (e.g., 12 → 3, 3 → 6)
  - Orange (e.g., 3 → 9, 9 → 3)
  - Multi-hop (e.g., 12 → 9, 9 → 6)
- Scheduler (single-shot and queue-based)
