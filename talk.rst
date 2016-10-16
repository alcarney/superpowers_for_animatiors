:title: Queues, Queueing and Ciw: Superpowers for Animators!
:css: talk.css

----

Queues, Queueing and Ciw: Superpowers for Animators!
====================================================

- `@alcarney <https://twitter.com/alcarneyorg>`_
- http://alcarney.org

----

:id: animation

What is Animation?
==================

.. image :: animation.jpg

.

    Animation is creating the illusion of motion, using a rapid
    succession of still images.


----

:id: tradition

Traditional Animation
=====================

- Snow White, Cinderella, Lion King etc.

- Lead animator would draw 'keyframes' depicting important poses

.. image :: keyframes.jpg

- Junior animators would draw 'inbetweeners' creating the motion
  between the keyframes

----

:id: blender

What is Blender?
================

.. image:: blender.png
    :height: 400px
    :width: 400px

----


I'll show you!
==============

.. note::

    - Introduce Blender, model a car quickly.
    - Show how to animate car - draw parallels to traditional animation
    - Now bring up animating a character - time consuming.
    - How are we going to get around this?


----

:id: queue

What is a Queue?
================

.. image:: queue.png
   :height: 380px
   :width: 680px

----

What is Ciw?
============

----

Duplicating an Object
=====================

.. code:: python

    # How many actors do we have?
    num_actors = len(bpy.data.groups['Actors'].objects)

    # Pick a random object to duplicate
    obj = bpy.data.groups['Actors'].objects[randint(0, num_actors - 1)]

    # Instacne it
    mesh = obj.data
    actor = bpy.data.objects.new(obj.name, mesh)
    actor.location = (0, 0, -10)

    # Link it to the scene
    bpy.context.scene.objects.link(actor)

    return actor

----

:id: constraint
:data-x: r1800

The Follow Path Constraint
==========================

.. image :: constraint.png
    :height: 400px
    :width: 350px

.. code:: python

    constraint = actor.constraints.new(type='FOLLOW_PATH')
    constraint.target = bpy.data.objects['Path' + str(record['Class'])]
    constraint.use_curve_follow = True
    constraint.forward_axis = 'FORWARD_Y'

----

Offset Keyframes
================

.. code:: python

  def insert_offset_keyframe(obj, time, offset):

      # Set the new offset
      obj.constraints['Follow Path'].offset = offset

      # Record the keyframe
      obj.keyframe_insert(
           data_path='constraints["Follow Path"].offset', frame=time)

----

Location Keyframes
==================

.. code:: python

  def insert_loc_keyframe(obj, time, loc):

      # Set the object's location
      obj.location = loc

      # Record the keyframe
      obj.keyframe_insert(data_path='location', frame=time)

----
