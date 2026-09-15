# RetailEdge — Database Migration Cutover Plan

## Phase 1 — Full Load
DMS copies all existing data (8 years of customer and order records)
from the current on-premises MySQL database to the new RDS instance.
Estimated time: 4–8 hours, depending on final data volume.

## Phase 2 — CDC (Change Data Capture)
Once the full load completes, DMS begins tracking and replicating
any new changes (inserts, updates, deletes) happening on the old
database in near real-time. This phase continues until the team is
confident in stability, potentially several days.

## Phase 3 — Cutover
When replication lag drops below 5 seconds:
1. Briefly pause writes on the old database (short maintenance window)
2. Wait for the last changes to fully sync
3. Repoint the application's database connection to the new RDS
   instance
4. Verify the application is functioning correctly against the new
   database

Target duration: under 30 minutes, since all data is already
synced — this step only switches the connection target.

## Phase 4 — Rollback (if an issue is found within 48 hours)
1. Immediately stop writes to the new RDS instance
2. Repoint the application back to the old database, which remains
   available (not deleted) during this window
3. Manually reconcile any data written to RDS during the rollback
   window, if needed
4. Investigate the root cause before attempting cutover again

**Note:** the old database is kept running (read-only) for 48 hours
after cutover as a safety net before being decommissioned.
