# 03. Frame compositing

In this route the guest renders its own world, and the host blends the guest's colour and depth into its own frame. The failures that keep coming back are a pose from the wrong frame, host depth you can't read, lighting estimated from an image, and a stale picture left on screen.

You'll usually have a ReShade add-on or a present/swapchain hook on the host side and a frame capture in the guest. The examples below come from the [Minecraft × GTA V example](../case-studies/universal-modder.md), [NewVegasCraft](../case-studies/new-vegascraft.md) and the two [CrossOver bridges](../case-studies/minecraft-crossover-bridge.md) (Elden Ring and Monster Hunter: World).

## The transport is a CPU copy

Each of these bridges reads guest pixels back from the GPU, copies them into shared memory, and uploads them into host textures. It isn't GPU texture sharing. The CrossOver bridges use a file-backed mapping so a native macOS Java process and a CrossOver process can both see it. Plan for the cost:

- **Triple slots, double readback.** The CrossOver writers alternate two sets of pixel buffers, use three frame slots, and throttle capture to the host's frame rate.
- **Cap the size.** NewVegasCraft scales the guest to a 1920×1080 pixel budget. The GTA V example keeps the host's aspect ratio and scales by area to roughly 1080p. The CrossOver bridges cap frames at 1920×1200 and fall back to a depthless overlay window above that.
- **Upload with dynamic textures.** NewVegasCraft's creator reports guest uploads dropping from 18.7 ms to about 5 ms per frame after switching to dynamic write-discard textures.
- **Send layers separately.** World colour and depth go in one capture, the hand and HUD in another, so the HUD stays still while the world is reprojected. The Elden Ring bridge splits hand and HUD so it can relight the hand and leave the HUD alone.

## Match each image to the pose it was rendered with

- Publish a pose only after its pixels exist. Keep a short pose history on the host (the CrossOver hosts keep eight) and pick in this order: exact match, then an older slot, then the last uploaded image. Count each outcome and log it; they log every 300 presents.
- Read the host camera when the frame is presented, not in the main game loop. NewVegasCraft's shake went away when it moved the pose read to the present callback.
- Reproject when the poses differ. The GTA V example turns each host-camera ray into the guest's pose and marches 24 log-spaced depths, refining three times after it crosses a guest surface, and stops at 400 m or at nearer host geometry. That bounds what it can recover. It can't restore pixels the guest frame never had.

## Lighting is an estimate

Monster Hunter: World multiplies the guest world by blurred host brightness. Elden Ring adds ambient light and haze from two downsampled passes plus a colour tint. The GTA V example relights from a blurred host image and adds screen-space contact shadows from ten nearby samples. None of these make guest blocks receive real host shadows. If you need that, you need [geometry transfer](04-geometry-transfer.md).

## Build the diagnostic tools first

NewVegasCraft is the model to copy:

- One key cycles the composite, host depth, guest depth and a difference view.
- One key drops a small marker pillar where the host crosshair ray lands.
- One key dumps the native projection matrix and pose.
- A capture burst saves frames at a fixed host-frame interval.
- A pose ring lets you try lag 0, 1 and 2.

A fake host that renders known geometry against each exported frame and the pose recorded with it catches misalignment before the real game is involved. The GTA V example has one in Python and a synthetic reversed-depth D3D11 fixture. These are visual checks with no numeric tolerance, so they tell you about alignment, not that the real game lines up.

## Problems and their real causes

1. **The image slides or shakes when you turn.** The host pose was sampled at a different time from the displayed image. Read it at present time and tag poses and images with a frame counter.
2. **Host depth reads empty.** In NewVegasCraft the scene depth was a 4× MSAA surface. An earlier theory about a special depth texture format was wrong and the patch was removed. Depth also has to be copied before the host clears it. Instrument the actual depth resource.
3. **The guest disappears behind glass and water.** Host depth was sampled after transparent passes. [LibertyCraft](../case-studies/libertycraft.md) (a geometry-transfer bridge with the same depth problem) snapshots opaque depth before transparency, at the cost of host glass not drawing over the guest.
4. **"Drift" that isn't drift.** NewVegasCraft's last apparent slide was a real Minecraft block intersecting a sign. A field-of-view control added during the hunt was removed after it threw calibration off. Keep a list of hypotheses and delete the ones you disprove.
5. **A stale guest image over the pause menu.** The host keeps drawing the last upload when the guest stops sending. The GTA V example's author cut around it in the demo video and hadn't built the fix. Timeouts alone don't clear an uploaded image. Hide the guest layer while host menus are open, as NewVegasCraft does, and drop images older than a set age.
6. **Torn or mixed layers.** The reader accepted a slot mid-write, or uploaded to GPU textures before its final sequence check. One CrossOver host uploads first; the other validates the CPU copy first and only then schedules uploads. Validate, then upload, and clear "valid" flags when a buffer map fails.
7. **Shaders don't compile under Proton.** NewVegasCraft's setup needed a native 32-bit shader compiler DLL. This is version-specific, so check current setup notes.
8. **ReShade doesn't load through its usual proxy DLL.** The GTA V example installs ReShade as an ASI plugin because the usual DXGI proxy didn't load for its author.
9. **The docs and the code disagree.** The GTA V example's field note describes a one-frame pose lag; the compositor defaults to zero and a code comment says zero measured best. Record the setting you actually used with every capture.
10. **Compiler ABI mismatches.** NewVegasCraft hit a hidden-return-pointer difference between compilers for a struct-returning virtual method. 32-bit hosts also need an address-space budget; a whole-map allocation can matter.
