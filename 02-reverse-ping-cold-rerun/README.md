# Lab 02 — Reverse Ping & Cold Re-Run

**Goal:** Return to Lab 01 after time away and re-run it from memory (no notes) to confirm it stuck, then prove the network path works in BOTH directions.

## Part 1 — Cold re-run (retrieval practice)
Came back to the connectivity check from Lab 01 after a gap of several days and repeated it without looking at notes:
- Pulled both machine IPs from memory (`ip a` on Kali, `ifconfig` on Metasploitable2).
- Confirmed both on the same `10.0.2.0/24` network.
- **Recall gap:** reached for `ip -c 4 <ip>` to ping — wrong tool. Needed a reminder that the command is `ping`, not `ip`. Once corrected, the rest (`-c 4`, the target) came back from memory.

**The fix that stuck:**
- `ip` = view/configure *my own* machine's networking (looks inward).
- `ping` = test reachability to *another* host (looks outward).

## Part 2 — Reverse ping (new step)
- Lab 01 proved **Kali → Meta**. A one-way ping does NOT guarantee the reverse works (firewall rules, bad routes, or half-configured interfaces can block one direction).
- Ran `ping -c 4 <kali-ip>` from Metasploitable2 → got clean replies.
- This confirms **bidirectional connectivity**: traffic flows both ways, so the link is trustworthy.

## Commands used
| Command | What it does |
|---------|--------------|
| `ip a` | View my own machine's interfaces/IPs (Kali) |
| `ifconfig` | Same, on Metasploitable2 |
| `ping -c 4 <ip>` | Send 4 packets to another host to test reachability |

## What I learned
- `-c` = **count** — how many packets to send before stopping (without it, ping runs forever on Linux).
- "It pinged" isn't the same as "the path is clean" — one-way success can hide a blocked return path. Proving both directions = bidirectional connectivity.
- Returning to a lab after several days and reproducing it (with only a small nudge) is a stronger sign it's genuinely stored than repeating it the next day.

## Still to do
- Bring Windows 10 onto the same network and repeat connectivity checks (three-way).
