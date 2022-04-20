# VisAreas

This is a very simple approach to culling:
VisAreas and Portals (reusing CryEngine terminology).

Advantages:
 - Very simple to implement
 - Does not require gpu feedback, muliple render passes, compute shaders...
 - Works everywhere, even on very limited hardware
 - Very well suited for classic first-person games set in indoor spaces

Disadvantages:
 - Very coarse-grained; works at "room granularity"
 - Requires the game developer to manually place and adjust the VisAreas and Portals
   - Only practical with tooling like a GUI editor

This should be considered orthogonal to any more advanced and fine-grained
culling algorithms. There is merit in discussing it even if we are also
going to have other stuff.

## Example

As an example, imagine a dungeon with many rooms and corridors.

While you are in one room, only that room should be rendered.
No point in rendering other rooms.

When multiple rooms are connected, like when you are looking
through a doorway, you need to render both the current room
and the other room (so you can see through the doorway).

## VisAreas

These are the "rooms".

A simple box encapsulating a self-contained "region".

All meshes are tracked to see if their bounding boxes intersect the VisArea
or not.

During any given frame, each VisArea in the world can be "active" or
"inactive".

If there is at least one active VisArea, then only stuff inside active
VisAreas is rendered, and all other stuff (in inactive VisAreas, or not in
any VisArea) is not rendered.

If there are no active VisAreas (such as when the camera is in an outdoor
scene), everything is rendered.

When the camera is inside of a VisArea, that VisArea becomes active. The
camera may be inside multiple VisAreas at once.

## Portals

These are the "doorways".

A box that overlaps any number of VisAreas (typically 2, in the case of
a doorway).

If the portal is in-view (inside the camera frustum), all the VisAreas
connected to the portal are activated.

The effect is that we only render the room on the other side of the doorway,
if the camera can see the doorway.

A portal is only to be used if at least one of the VisAreas it connects is
currently active. Otherwise, the portal is to be ignored.

The effect is only the portals connecting the room we are currently in
(and any we can see through it), can enable additional VisAreas, and we will
not accidentally render some super far away room just because some far away
portal happens to be in the frustum.
