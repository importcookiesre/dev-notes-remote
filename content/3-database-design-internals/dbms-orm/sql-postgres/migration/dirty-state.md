
A dirty database means a migration started failing partway through and the dirty flag stayed set, which blocks future migrations until you fix the situation and force the version you know the database is actually at. 

How to detect: you run `migrate ... up` and get something like `Dirty database version X. Fix and force version`. You can also inspect the version table or use the `version` command. The docs recommend using `force VERSION` once you know what the database’s real state is.

How to fix safely:

1. Inspect the partially applied schema.
2. Decide whether the migration partially succeeded or failed before changing anything.
3. Repair the database manually if needed.
4. Run `force` to the real version.
5. Re-run the corrected migration.

