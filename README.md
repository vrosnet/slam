SLAM (non-invasive version)
===========================
.. is the **Simple [LOVE] Audio Manager**.

[![GitHub release](https://img.shields.io/github/release/Guard13007/slam.svg?maxAge=2592000)](https://github.com/Guard13007/slam/releases/latest)
[![GitHub downloads](https://img.shields.io/github/downloads/Guard13007/slam/latest/total.svg?maxAge=2592000)](https://github.com/Guard13007/slam/releases/latest)
[![GitHub issues](https://img.shields.io/github/issues-raw/Guard13007/slam.svg?maxAge=2592000)](https://github.com/Guard13007/slam/issues)
![GitHub license](https://img.shields.io/badge/license-MIT%20License-blue.svg?maxAge=2592000)

This version returns a table of functions you can use, instead of overwriting LOVE's functions.

In contrast to sources that can only have one playing instance, SLAM sources create instances
when played, so that you can play one source multiple times at once. Each instance will inherit
the settings (volume, speed, looping, etc) of it's SLAM source, but can override them.

SLAM also features tags, which can be used to modify a number of sources at the
same time.

Example
-------

    local slam = require 'slam'
    function love.load()
        music = slam.audio.newSource('music.ogg', 'stream') -- creates a new SLAM source
        music:setLooping(true)                              -- all instances will be looping
        music:setVolume(.3)                                 -- set volume for all instances
        slam.audio.play(music)                              -- play music
        
        woosh = slam.audio.newSource({'woosh1.ogg', 'woosh2.ogg'}, 'static')
    end
    
    function love.keypressed()
        local instance = woosh:play()                       -- creates a new instance
        instance:setPitch(.5 + math.random() * .5)          -- set pitch for this instance only
    end


Reference
---------

### Operations on Sources

    source = slam.audio.newSource(what, how)

Returns a new SLAM source. Accepts the same parameters as
[love.audio.newSource](http://love2d.org/wiki/love.audio.newSource), with one
major difference: `what` can be a table, in which case each new playing
instance will pick an item of that table at random.


    instance = slam.audio.play(source)
    instance = source:play()

Plays a source, removes all paused instances and returns a handle to the player
instance. Instances will inherit the settings (looping, pitch, volume) of
`source`.


    slam.audio.stop(source)
    source:stop()

Stops all playing instances of a source.


    slam.audio.stop()

Stops all playing instances.


    source:pause()

Pauses all playing instances of a source.


    source:resume()

Resumes all paused instances of a source. **Note:** source:play() clears paused
instances from a paused source.


    source:isStatic()

Returns `true` if the source is static, `false` otherwise.


    looping = source:isLooping()
    source:setLooping(looping)
    pitch = source:getPitch()
    source:setPitch(pitch)
    volume = source:getVolume()
    source:setVolume(volume)

Sets properties for all instances. Affects playing instances immediately. For
details on the parameters, see the [LOVE wiki](http://love2d.org/wiki/Source).


### Instances

All functions that affect LOVE Sources can be applied to SLAM instances. These
are:

    slam.audio.pause(instance)
    instance:pause()
    instance:isPaused()
    
    slam.audio.play(instance)
    instance:play()
    
    slam.audio.resume(instance)
    instance:resume()
    
    slam.audio.rewind(instance)
    instance:rewind()
    
    instance:getDirection()
    instance:setDirection()
    
    instance:getPitch()
    instance:setPitch()
    
    instance:getPosition()
    instance:setPosition()
    
    instance:getVelocity()
    instance:setVelocity()
    
    instance:getVolume()
    instance:setVolume()
    
    instance:isLooping()
    instance:setLooping()

See the [LOVE wiki](http://love2d.org/wiki/Source) for details.


### Tags

With tags you can group several sources together and call functions upon them.
A simple example:

    -- add some stuff to the background tag
    drums:addTags('music')
    baseline:addTags('background', 'music') -- a source can have multiple tags
    muttering:addTags('background')
    noise:addTags('background')
    cars:addTags('background')
    
    (...)
    
    slam.audio.tags.background.setVolume(0) -- mute all background sounds
    slam.audio.tags.music.setVolume(.1)     -- ... but keep the music alive


#### Functions

    source:addTags(tag, ...)

Adds one or more tags to a source. By default, all sources are member of the
tag `all`.


    source:removeTags(tag, ...)

Remove one or more tags from a source.


    slam.audio.tags.TAG.FUNCTION(...)
    slam.audio.tags[TAG].FUNCTION(...)

Calls `FUNCTION` on all sources tagged with `TAG`.


[LOVE]: http://love2d.org
