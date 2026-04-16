

Each tuple contains its own 23-byte (minimum) header before the actual column data. This header is the mechanism that drives PostgreSQL's MVCC.

- `xmin`: The Transaction ID (TXID) of the transaction that inserted this tuple.
    
- `xmax`: The TXID of the transaction that deleted or updated this tuple. If 0, the tuple is live.
    
- `t_ctid`: A pointer `(page_number, line_pointer_index)` pointing to the current, valid version of this row. If the row is updated, the old tuple's `t_ctid` points to the new tuple.
- `t_cid` (CommandId, 4 B): Command ID within transaction (technical).
- `t_xvac` (TransactionId, 4 B): Used by VACUUM when moving rows.
- `t_infomask`: A bitmask storing various boolean states, such as whether the tuple has null values (triggering a Null Bitmap), or "hint bits" indicating if the `xmin`/`xmax` transactions have committed.
- `t_hoff` (uint8, 1 B): Offset from tuple start to the beginning of actual column data (accounts for header, null bitmap, alignment).






