This is a Unity script to give clean mosaics.

**[Live WebGL demo](https://s3.amazonaws.com/unity-mosaix/unity-chan-demo/index.html)**

[Demo video](https://s3.amazonaws.com/unity-mosaix/mosaix-demo.mp4)

This code is in the public domain.

No bleeding with the scene
--------------------------

This mosaic doesn't bleed mosaiced objects with the scene around them.
On the left, the cylinder has bled with the yellow background, making the outline
muddy.  On the right, the cylinder is only mosaiced with itself.  It's blurred,
but the outline is still easy to see.

![](Images/no_mosaic_bleeding.png)

No bleeding between mosaics
---------------------------

Multiple objects can be mosaiced separately, without bleeding between them.
This blurs each object while keeping them distinguishable.

On the left, an old mosaic has blurred both objects together.  On the right, both
objects are mosaiced, but the outline is still visible.

![](Images/separate_mosaics.png)

Texture masks
-------------

A texture can be used to define which parts of an object should be mosaiced.
This allows precise control over what gets blurred.

Below, the spout of the teapot has been painted white in the mask texture, so
only that part of the model is blurred.

![](Images/texture_masked.png)

Spherical mosaic masks
----------------------

Alternatively, a cylinder can be used to mask which part of the object you want to mosaic.
For example, this can be used to mosaic only a game character's eyes.

The left side shows the mask cylinder.  The right shows the result: just the handle
of the teapot is blurred.  The cylinder can be scaled to fit the desired shape,
and parented to the object or a character's skeleton so it follows the object.

![](Images/sphere_masked.png)

Mosaic anchoring
----------------

The mosaic can be anchored to an object.  When the object moves, the mosaic will shift
with it, making the motion of the object clearer.  Optionally, the mosaic can also scale
with the object, so the mosaic gets finer as the object gets further away.

[Anchoring demo](https://s3.amazonaws.com/unity-mosaix/anchoring-demo.mp4)

Integrating with cartoon outline shaders
----------------------------------------

Existing cartoon outline shaders can be drawn as a separate pass.  This allows mosaicing an
object, but retaining clean outlines.

(Since everyone has their own shaders and each shader works differently, integration
with external shaders like toon outlines requires shader editing.)

![](Images/external_shaders.png)

Usage
-----

See "Test scene\Demo scene.unity" for an example.

- In the Inspector, Layer -> Add Layer, and create a layer to mosaic.  
![](Images/Setup_AddLayer.png)
- Put objects to mosaic in the new layer.  
![](Images/Setup_PutMeshInLayer.png)
- Select your camera and add Component -> Effects -> Mosaix.
- Set "Mosaic Layer" to the layer you created above.
- Play the scene.

Settings
--------

**Mosaic Blocks** The number of mosaic blocks.  Higher numbers give smaller mosaic blocks.  
**Shadows Cast On Mosaic** Whether other objects cast shadows on the mosaic.  Turning this
off may be faster, but may look wrong depending on your scene's lighting.  
**High Resolution Render** A high-quality mode is used to render the mosaic.
This results in less flicker as objects move, more accurate lighting, and is required
for masking and alpha.  
**Alpha** Fade out the mosaic.  This can be animated to transition the mosaic on and off
smoothly.

Texture masking
---------------

To mask with a texture, select "Texture" as the masking mode, and connect a texture
to Masking Texture.  White areas in the mask will be blurred and black ones won't.

This only works well if the masking layer only contains a single object, since this
only uses a single texture, but allows fine control over blurring.

Sphere masking
--------------

To mask the mosaic, select "Sphere" as the masking mode, create a 3D sphere and connect
it to Masking Sphere.  Place the sphere, and parent it correctly so it follows the object
to be mosaiced.  Once the sphere is placed, disable its Mesh Renderer so the sphere isn't
visible.

The object must be a sphere, but it can be scaled and rotated into oblong shapes to cover
non-spherical areas.

The "Sphere Collider" component should be deleted from the sphere, so objects don't collide
with it.

Mask Fade can be set to fade away the mosaic.  At 0, the mosaic cuts off sharply at the
edge of the sphere.

Multiple mosaics
----------------

To mosaic two objects separately, add a second Mosaix script to your camera, and use it
with a separate layer.  The objects won't bleed together, and can use different settings,
such as a different number of mosaic blocks.

Integrating with cartoon shaders
--------------------------------

See **Test scene\Materials\MosaicWithOutline.mat** and **Test scene\Materials\MosaicWithOutline.shader**
for an example.  By creating your own shader that calls the mosaic shader with UsePass,
you can integrate other effects on top of the mosaic, such as cartoon outlines.  Assign
your alternate shader to a material, and set that material as the Mosaic Material inside
the Shaders section of the script.

Anchoring
---------

To anchor the mosaic so it locks to an object, connect a transform to Anchor.

[A demo for this effect can be found here.](https://s3.amazonaws.com/unity-mosaix/anchoring-demo.mp4)

This causes the mosaic lines to align themselves to the object, which can make motion underneath
the mosaic easier to see.

If **Scale mosaic size** is turned on, the mosaic will also scale itself as the anchor gets
further and closer to the camera.  This allows the mosaic to be smaller when the object is
far from the camera and finer as the object is closer to the camera.

Note that **Scale mosaic size** can cause additional flicker as the size of the mosaic changes,
especially with bright specular highlights.  Anchoring to something that doesn't move too much
can reduce this.  For example, if you're mosaicing a character's eyes and want the mosaic to
zoom as the character comes closer to the character, putting the anchor on the character's root
joint instead of on his face may cause less flicker, but still give reasonable mosaic scaling.

