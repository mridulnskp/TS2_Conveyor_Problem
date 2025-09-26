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

## Transfer Logic

State machine FBs for different paths:

- FB_DirectTransferGreen → e.g. 12→6, 6→12

- FB_DirectTransferOrange → e.g. 3→9, 9→3

- FB_TransferCorner_LowerToUpper → e.g. 12→3, 6→9

- FB_TransferCorner_UpperToLower → e.g. 3→6, 9→12

- FB_TransferChain_LowerToUpper → e.g. 12→9, 6→3

- FB_TransferChain_UpperToLower → e.g. 9→6, 3→12

Each FB implements a clear state machine (IDLE → ALIGN → RELEASE → WAIT_ENTER → LIFT → PUSH → COMPLETE → ERROR).

## Scheduler

Implemented as FB_Scheduler_Queue:

Collects requests from all Readers.

Stores them in a FIFO queue.

Dispatches one job at a time.

Uses JobID to ensure even identical routes run back-to-back.

Releases Busy flag when transfer Done edge is detected.

## State Machines

Each transfer logic was modeled first on paper as a state diagram.
Examples:

Direct Transfer (Green):
IDLE → ALIGN → RELEASE → WAIT_ENTER → WAIT_LEAVE → COMPLETE

Corner Transfer (12→9):
passes through 1 LTUs + 1 Purple stoppers before completion.

Multi-Hop Transfer (12→9):
passes through 3 LTUs + 3 Purple stoppers before completion.