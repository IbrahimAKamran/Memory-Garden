Voiceprint Garden AR — your voice, made visible
Each person's voice becomes a voiceprint: a list of numbers describing their pitch, timbre, breathiness and dynamics. That list is boiled down to 10 genes, and the genes grow a unique tree that looks the same every time for the same voice. Talk and it grows. Laugh and it blooms.

In AR, your tree grows next to your face. Point the webcam at someone and their tree appears beside their head, with a card showing what you learned about them: name, where they're from, what they're building. A garden of everyone you met, overlaid on the room.

It's a single HTML file. There's no build step, no server code and no API keys.

Run it
python3 -m http.server 8000     # in this folder (or: npx serve .)
Open http://localhost:8000 in Chrome or Edge. Or use your GitHub Pages link, which is HTTPS and works anywhere.

It must be localhost or https, not file://, or the browser blocks the camera and mic. You need internet: Three.js and the MediaPipe face model load from CDNs, and Chrome's speech-to-text runs in the cloud.

Three ways to see the garden
Mode	Device	What you get
🪞 AR mirror (recommended for demos)	Laptop + webcam	The live camera feed with trees anchored beside (or on top of) each person's head, plus floating cards of facts. Tracks up to 4 faces.
📱 AR headset/phone	Quest 3/3S/Pro (passthrough) or Android Chrome	Trees standing on your real floor. Point at the floor and pull the trigger to move the selected tree.
🌳 Garden / 🥽 VR	Laptop screen or any WebXR headset	A night garden of everyone you met: walk around, click a tree to examine it.
The 📱 and 🥽 buttons only appear when the browser detects a compatible device.

Demo flow (AR mirror)
Click 🎤 Start listening (allow the mic), then 🪞 AR mirror (allow the camera).
A visitor steps in front of the laptop. A "👤 New face" card appears above their head.
Click 🌱 Plant their voice on that card. They talk for about 6–8 seconds ("Hi, I'm Maya, I'm from Chicago, we're building…"). A tree sprouts beside their head, their name gets picked up from what they said, and facts fill the card.
Keep chatting. Whoever's mouth is moving while there's sound counts as the speaker, so their tree grows, their card glows, and their words float up from their tree. If they smile and laugh, the tree blooms.
The visitor leaves and comes back later. They're recognised by face (geometry of their face landmarks) or by voice (once they speak), and their tree reappears.
Click a card to open the full editor.
Adding and editing trees
➕ Add tree (T): create a tree by hand, with no voice needed. Link it to a face from that face's card ("Link to an existing tree…").
Editor (click a card, or click a tree in the garden):
Name, notes and facts (add or remove)
All 10 shape genes as sliders that update the tree live, plus 🎲 Randomize and ↺ Shape from voice
Growth and bloom sliders
AR spot: beside the head (auto, left or right) or 👑 on top of the head
🎙 Re-record voice · 👤 Link face in view · Forget face · 📌 Attribute speech · Delete
Everything saves automatically in the browser. ⬇ Export / ⬆ Import moves the garden to another laptop.
Controls
Desktop: N plant voice · T add tree · B bloom · M toggle AR mirror · G (hold) fast-grow the selected tree · 1–9 send speech to person N, 0 back to auto · H hide the panel · Esc close · in Garden: drag to look, WASD to walk.

Headset: right trigger selects (and, in AR, places the selected tree on the floor point) · left trigger plants a voice · grip blooms · left stick walks (VR only) · right stick snap-turns (VR only). The left wrist shows a live status display.

Phone AR: tap a tree to select it, then tap the floor to move it there. On-screen buttons handle Plant voice / Bloom / Exit.

How it works
webcam ─► MediaPipe Face Landmarker (478 landmarks + 52 blendshapes per face, up to 4 faces)
           ├─ face tracks (frame-to-frame matching)
           ├─ jaw-motion variance + audio activity  ─► "who is talking"
           ├─ smile + open jaw + sound              ─► laughter → bloom
           ├─ 20 landmark-distance ratios           ─► face signature → re-identification
           └─ head position/size/roll               ─► tree anchored in 3D beside the head

mic ─► MFCC + pitch + timbre (40 frames/s) ─► voice ID  ─┐
                                └─► 14-d embedding ─► 10 genes ─► procedural tree
Web Speech API ─► transcript ─► attributed speaker ─► facts, topics, floating words
Who gets credit for speech: a pinned person if you've chosen one, otherwise the talking face (when it's linked to a tree), otherwise voice ID. When voice ID recognises someone while an unlinked face is talking, the two get linked automatically.

Gene	Driven mainly by	Story
height	pitch (inverse)	deep voices grow tall
spread	pitch variability	melodic speakers spread wide
branches	breathiness	breathy voices are bushy
depth / twist	timbre (MFCCs)	branching structure
curl	loudness dynamics	dynamic speakers grow gnarly
stem hue / leaf	timbre, brightness	warm voice → big soft leaves
flower hue	pitch	low → blue/violet, high → pink/gold
petals	MFCC 1	4–9 petals
Tuning on the day (open "Recognition tuning")
Voice match: watch the voice distance line while each person talks. Set the slider between the right-person and wrong-person numbers.
Face match: an unlinked face's card shows closest face: Name (0.0xx). Raise the slider if returning people aren't recognised; lower it if strangers get matched. Face geometry is a lightweight heuristic and works best looking straight at the camera in decent light. Good lighting on faces matters more than anything else.
Mouth motion: if the wrong face gets credit for speech, raise it. If nobody gets credit, lower it.
Framing: the face detector is built for faces within about 2 m of the camera. Ask people to step up close to the laptop.
Noisy hall: pin the speaker with keys 1–9, and press B to bloom on demand.
No camera or no mic: 🧪 Demo fills the garden, and ➕ Add tree works without either.
Limits to know
Speech-to-text needs Chrome or Edge (desktop or Android). The Quest browser has no Web Speech API, so in headset AR trees still grow from voice but facts aren't extracted. For a Quest demo, run voice on the laptop and show the garden in the headset, or just use the AR mirror.
WebXR passthrough doesn't give web pages access to the camera, so "tree next to your face" is only available in the laptop AR mirror. Headset AR places trees in the room.
Face and voice recognition here are lightweight demo heuristics, not security-grade biometrics.
Privacy
No audio or images are stored. Each person's numeric voiceprint, face-geometry signature, transcript snippets and facts stay in your browser's local storage, and in any export file you create. Speech-to-text goes through Chrome's cloud recognizer. Ask people before you plant their voice or link their face, and use Delete / Forget face whenever someone asks.


ds
