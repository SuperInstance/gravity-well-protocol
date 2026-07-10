# Gravity Well Protocol 🛸

⚠️ This is a design/concept document — no working software exists yet.
   The repository contains only this README and a license.
   Implementation is planned for the future.

## Overview

✅ **Concept** — A coordination layer for the Cocapn Fleet where agents
   broadcast state updates only within a defined local region—like a
   latency radius—instead of flooding the entire network. This edge-native
   protocol reduces unnecessary global message traffic.

🔮 The broader Cocapn fleet can be seen at
   [the-fleet.casey-digennaro.workers.dev](https://the-fleet.casey-digennaro.workers.dev).
   Note: That site showcases the fleet but does **not** demonstrate this
   protocol (no region‑scoped broadcast or gossip algorithm is visible).

## Why This Exists

✅ In many distributed agent systems, a significant portion of messages are
   used for presence announcements ("heartbeats") to the entire network,
   not just to relevant neighbors. This protocol addresses that by limiting
   communication to a configurable radius, ensuring agents only talk to
   nearby peers that need to know.

## Quick Start (not yet implementable)

🔮 The following steps describe the intended workflow once implementation
   is complete. No deployment is possible today.

1. Fork this repository.
2. Deploy it with one click to Cloudflare Workers (requires a
   `wrangler.toml` and JavaScript source, neither of which exist yet).
3. Connect any agent. It will automatically discover and communicate only
   with nodes within your defined radius.

## Architecture

✅ The system is designed as a single, stateless Cloudflare Worker.
   It will process peer messages and propagate state using an Eigenvector
   Gossip algorithm, which favors paths based on observed traffic patterns.
   There is no central coordinator.

**Core Operations (design goals):**
*   ✅ **Region-Limited Broadcasts:** Communication is scoped by
    configurable parameters like latency, geography, or trust boundaries.
*   ✅ **Traffic-Informed Gossip:** State propagation follows paths of
    actual message flow, not random peer selection.
*   ✅ **Implicit Liveness Detection:** Silent nodes are identified through
    gossip patterns, eliminating dedicated heartbeat messages.
*   🔮 **Zero Dependencies:** The intended implementation is a single
    ~32 KB JavaScript file (not yet written).
*   🔮 **Fork-First:** The intended workflow is to run your own private
    fleet first; contributing back is optional.

## What Makes This Different

✅ 1. It guarantees eventual consistency only *within* your defined working
      radius, not across the entire global network.
✅ 2. Nodes discover neighbors organically from received messages; no
      hardcoded bootstrap node list is needed.
✅ 3. No single observer—not even the operator—can see the full network
      topology.

## One Honest Limitation

✅ The protocol's efficiency gains are most pronounced when agent
   interactions have inherent locality. In a simulated, completely random
   peer network where every agent must communicate with every other agent
   globally, the reduction in message traffic can fall below 50%.

---

MIT License

Superinstance and Lucineer (DiGennaro et al.).

<div style="text-align:center;padding:16px;color:#64748b;font-size:.8rem"><a href="https://the-fleet.casey-digennaro.workers.dev" style="color:#64748b">The Fleet</a> &middot; <a href="https://cocapn.ai" style="color:#64748b">Cocapn</a></div>
