# My blender shader library

I was like "Hey, I should set up a GitHub repo to store bits for Blender, so I can keep track of everything."

Then I was like "Hey, the only repos full of shaders worth keeping secret are the repos at the major effects houses, so why not just keep this all public"

## Disney's Principled BSDF: A Brief history

In the mid-to-late 1990s, 3D computer graphics was a *REAL* thing, after some awkward periods in the aftermath of Tron.  Pixar was making some amazing movies, all computer-rendered.  You could even render a few minutes of video that looked pretty good on a common desktop PC.

Now, at this point in time, Pixar was doing amazing stuff with their own custom shaders using the RenderMan language.  I'm not totally sure how complex their shaders were, but most everyone else was just using a preset BSDF that had somewhere around 6 terms based on the Lambert and Phong reflectance models.  Like, I wrote some plugins for Carrara 1.x and there was not even a way to plug anything other than the 6-term Lambert+Phong model into the rendering engine, that's how far baked into the rendering system was.

Now, we the graphics programmers of the time all kinda already knew about PBR stuff.  Heck, Cook-Torrance came out in the 80s, and Oren-Nayar had just recently generalized the same microfacet idea to diffuse reflectance.  Oh, and Blender was this weird free thing that was kinda neat that some crazy Dutchmen had cooked up and handed out free CD-ROMs of at SigGraph 2000.

After I stopped paying attention to computer graphics, because I was primarily doing physical art, the whole Physically-Based-Render thing became popular.  It's something that I would have been a huge fan of.  So much of how we read the world is based on subtle cues, not gross details, something that I came to appreciate all the more when I actually started working with the real materials.

Enter a hard drive corruption, a new machine where I realized that the built-in Intel GPU is 600% faster than the fanless GeForce I was using and decided I might want to do something that actually required a fast-ish GPU.  So I downloaded the 2.79 release of Blender, discovered this new Principled BSDF function, and realized that nobody's got a good pile of shaders for it as yet, so I might as well do some digging.

If you compared to where we were (Lambert+Phong) to Principled BSDF, you'll note that both of them have a small number of knobs that you tweak to express a wide variety of material properties.  It's just that Principled BSDF has the benefit of a few decades of careful observation, math, and experience making scenes.

## Cycles Principled Metals

### Polished Pickled Metals

For simplicity's sake, I'm going to start off with the polished pickled metals.  This is where you fabricated something out of the metal, then you polished it, then you threw it in an acid bath to get off any greases or oxides or contaminates, and then you looked at it before it had a chance to rust / corrode / etc.  These are pretty much the base case for the Metallicity=1.0 slider.

Even so, Aluminum and Titanium have a small amount of clear coat turned on because they instantly develop a layer of oxides.

Obviously, this is an unnatural state to observe a metal in.  Mostly, I wanted to get a set of presets for the right color of metals before I started altering them.


### Surface-treated pickled metals

This is going to end up being a bit of PBR-meets-metal-fabrication.

Aluminum is soft, as far as metals go.  It's really easy to get a nice texture out of it with a stainless steel brush.



## References
- http://www.robinwood.com/Catalog/Technical/Gen3DTuts/Gen3DPages/RefractionIndexList.html
- https://docs.google.com/document/d/1Fb9_KgCo0noxROKN4iT8ntTbx913e-t4Wc2nMRWPzNk/edit
- https://seblagarde.wordpress.com/2014/04/14/dontnod-physically-based-rendering-chart-for-unreal-engine-4/
- https://www.filmetrics.com/refractive-index-database
- https://blenderartists.org/forum/showthread.php?278285-Yet-Another-Thread-about-Cycles-Materials&p=2288101&viewfull=1#post2288101