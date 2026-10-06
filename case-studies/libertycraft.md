# LibertyCraft: SkyCraft ported to GTA IV

**Repository:** [mrborghini/libertycraft@0fe0f2c](https://github.com/mrborghini/libertycraft/tree/0fe0f2c9aae2ba974cb077e282b091a38f1ca48f) (default branch, 6 Oct 2026)  
**Route:** live state bridge plus geometry transfer (a fork of SkyCraft)  
**What we read:** source and the 43-commit history available when we looked. Nothing was run.

LibertyCraft forks SkyCraft's Fabric mod and protocol header and points them at GTA IV (through a community scripting SDK, D3D9), running under Wine on Linux. The transport was split into Windows and POSIX versions.

**What we learned**

- **Reuse the representation, rewrite the acquisition.** The guest still receives triangles plus eighth-block voxels. Only the way host geometry is gathered changed, to native line probes and nearby-object sampling. That's why the guest side could be kept.
- **Keep the layout, change the magic.** It kept SkyCraft's v11 byte layout but changed the magic number and added GTA-specific events and flags, so an old SkyCraft build is rejected rather than misread.
- **Hand control to the host for its own content.** GTA takes over for missions, cutscenes, minigames and cars. The guest follows a non-physical mount at the seat and re-syncs with a teleport handshake.
- **Combat keeps native attribution.** Explicit crime attribution, ragdoll before death, safe death in cars and a large native health buffer that feeds guest-side damage.
- **Opaque depth before transparency.** Guest blocks no longer vanish behind glass and water, at the cost of host glass not drawing over the guest.
- **Stand-ins for both ends of the bridge.** Useful for development; passing them still needs both real games.
- **Keep corrections in the history.** An early commit "fixed" a login loop; a later one names a different add-on as the cause and removes it.

**Limits:** sampled city geometry can refill dug holes when new collision updates arrive, and thin objects can vanish. The first commit has no parent, so the exact SkyCraft commit it forked from can't be recovered from history.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.
