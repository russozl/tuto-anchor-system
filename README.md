# OBS Chase Cam: Anchor System (TV Cameras)

How to place fixed cameras around the track that auto activate when a car drives through their zone. The output goes to your OBS mirror texture, so what OBS captures changes automatically based on where the car is on track.

## Video Tutorial

Watch the full walkthrough:

https://github.com/russozl/tuto-anchor-system/releases/download/v1.0/anchor-tutorial.mp4

## The Concept

Think of it like a real TV broadcast. You paint invisible zones on the track and place a camera at each one. When the car enters a zone, that camera takes over the OBS feed. When the car leaves, it falls back to the default chase camera. No manual switching, it's all automatic.

```
Car enters zone A  >  Camera A activates in OBS
Car leaves zone A  >  Falls back to chase cam
Car enters zone B  >  Camera B takes over
```

## Setup

### 1. Enable the system

Open the OBS Chase Cam app in AC. Go to the Anchors tab.

Check "Enable TV Cameras". This does two things:
it activates the anchor detection system and auto switches your active cameras to anchor mode so you don't have to do it manually.

Check "Auto Switch" (should already be on). This makes cameras switch automatically when the car enters or leaves zones.

### 2. Position your camera

Drive to the spot where you want a camera. Think about the angle. A good TV camera is usually elevated (2 to 5 meters above the road), outside the track (not in the racing line), and angled to see both the approach and exit of a corner.

Use F7 (free camera) to fly to the exact position you want. The camera captures your current viewpoint, including position, direction, and FOV.

### 3. Create a new anchor

In the Anchors tab, click "+ New Camera". The create panel opens:

Step 1: Your camera position is captured automatically from your current view.

Step 2: Give it a name (like "Turn 1 Outside" or "Bridge Overhead") and choose the type. Tracking means the camera follows the car smoothly like a real cameraman panning. Static means a fixed angle that adds shake when the car passes close, like a mounted camera.

Step 3: Click "Start Drawing Zone" to define where this camera should activate.

### 4. Draw the activation zone

After clicking "Start Drawing Zone", you enter waypoint placement mode.

Click on the track to place waypoints. The first waypoint is where the zone starts (entry point) and the last waypoint is where the zone ends (exit point). The system creates an OBB (Oriented Bounding Box) between these points, a rectangular volume that covers the road.

Place at least 2 waypoints:
1. Click where you want the camera to START activating
2. Click where you want it to STOP

Press ENTER to confirm, or ESC to cancel.

The zone is visualized as a colored overlay on the track (visible when Debug is enabled).

### 5. Fine tune the zone

After creating, select the anchor in the list and adjust:

OBB Settings (Camera Capture Volume):
Width is how wide the detection zone is (should cover the full road width plus some margin). Length is how long the zone extends along the track. Height is the vertical extent (usually leave default). Distance Offset moves the zone closer or further from the camera. Lateral Offset shifts the zone left or right.

Camera Settings:
FOV is the field of view. Human Error adds subtle breathing and tremor to tracking (makes it look like a real cameraman). Shake makes the camera shake when the car passes close (intensity based on proximity and speed).

### 6. Test it

Drive through the zone. If everything is set up correctly:
1. You see the car approaching in your OBS capture
2. The camera smoothly tracks (or stays fixed with shake)
3. When the car exits the zone, it transitions back to the chase cam

Enable the "Debug" checkbox to see the zones visualized on track, which makes it easy to verify coverage.

## Zone Types

### Zone (default)
Fixed camera position. The car triggers it by entering the OBB volume. Best for corners, chicanes, specific spots.

### Rail
Camera moves along a path (multiple waypoints forming a rail). The camera slides along the rail to follow the car. Best for long straights, sweeping corners, or pit lane shots.

For rails, place multiple waypoints along the path the camera should travel. The camera interpolates between them based on the car's position.

## How It Looks in OBS

```
+----------------------------------+
|                                  |
|   OBS Source: "OBS Chase Cam"    |
|                                  |
|   Car in zone > TV camera view   |
|   Car outside > Chase cam view   |
|                                  |
+----------------------------------+
```

The transition between TV camera and chase cam includes a configurable delay (default 0.5s) to prevent flickering at zone edges.

## Multiple Cameras

You can create as many anchors as you want. When zones overlap, the system picks the best candidate based on proximity. A typical setup:

```
Track layout with 5 cameras:

  [Cam 1: Turn 1]     [Cam 2: Straight]     [Cam 3: Chicane]

                  [Cam 4: Hairpin]     [Cam 5: Exit]
```

Each camera has its own zone painted on the track. As the car races through, the OBS feed switches between them, just like watching a real broadcast.

## Saving and Loading

Camera setups are saved per track. When you load the same track again, your anchors are automatically loaded. You can also export and import anchor presets to share them.

## Tips

Start with 3 or 4 cameras at the most interesting corners. Make zones slightly larger than you think, better to have overlap than gaps. Use the Debug visualization to check zone coverage. Tracking cameras at corners plus static cameras at straights gives good variety. The Human Error setting at 20 to 30 percent makes tracking cameras look way more natural. Test your setup by driving a full lap and watching the OBS preview.

