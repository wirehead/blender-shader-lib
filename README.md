# My blender shader library

I was like "Hey, I should set up a GitHub repo to store bits for Blender, so I can keep track of everything."

Then I was like "Hey, the only repos full of shaders worth keeping secret are the repos at the major effects houses, so why not just keep this all public"

Since I didn't really find myself liking any of the tools for managing workflows., I'm just providing these as Blender files that you can append or link as you desire, with verbose and hopefully useful descriptions as necessary.

This is all a little bit half-baked, but on purpose.  You should be appending these shaders to your project and individually customizing them to the known properties of the object.

## Disney's Principled BSDF: A Brief history

In the mid-to-late 1990s, 3D computer graphics was a *REAL* thing, after some awkward periods in the aftermath of Tron.  Pixar was making some amazing movies, all computer-rendered.  You could even render a few minutes of video that looked pretty good on a common desktop PC.

Now, at this point in time, Pixar was doing amazing stuff with their own custom shaders using the RenderMan language.  I'm not totally sure how complex their shaders were, but most everyone else was just using a preset BSDF that had somewhere around 6 terms based on the Lambert and Phong reflectance models.  Like, I wrote some plugins for Carrara 1.x and there was not even a way to plug anything other than the 6-term Lambert+Phong model into the rendering engine, that's how far baked into the rendering system was.

Now, we the graphics programmers of the time all kinda already knew about PBR stuff.  Heck, Cook-Torrance came out in the 80s, and Oren-Nayar had just recently generalized the same microfacet idea to diffuse reflectance.  Oh, and Blender was this weird free thing that was kinda neat that some crazy Dutchmen had cooked up and handed out free CD-ROMs of at SigGraph 2000.

After I stopped paying attention to computer graphics, because I was primarily doing physical art, the whole Physically-Based-Render thing became popular.  It's something that I would have been a huge fan of.  So much of how we read the world is based on subtle cues, not gross details, something that I came to appreciate all the more when I actually started working with the real materials.

Enter a hard drive corruption, a new machine where I realized that the built-in Intel GPU is 600% faster than the fanless GeForce I was using and decided I might want to do something that actually required a fast-ish GPU.  So I downloaded the 2.79 release of Blender, discovered this new Principled BSDF function, and realized that nobody's got a good pile of shaders for it as yet, so I might as well do some digging.

If you compared to where we were (Lambert+Phong) to Principled BSDF, you'll note that both of them have a small number of knobs that you tweak to express a wide variety of material properties.  It's just that Principled BSDF has the benefit of a few decades of careful observation, math, and experience making scenes.

So far, Principled has had me twiddling fewer knobs than Lambert+Phong.

## Cycles Principled Metals

### Polished Pickled Metals

In this category, there is:
- Aluminum
- Iron/Steel
- Copper
- Brass
- Chrome
- Copper
- Gold
- Lead
- Nickel
- Platinum

For simplicity's sake, I'm going to start off with the polished pickled metals.  This is where you fabricated something out of the metal, then you polished it, then you threw it in an acid bath to get off any greases or oxides or contaminates, and then you looked at it before it had a chance to rust / corrode / etc.  These are pretty much the base case for the Metallicity=1.0 slider.

Even so, Aluminum and Titanium have a small amount of clear coat turned on because they instantly develop a layer of oxides.

Obviously, this is an unnatural state to observe a metal in.  Mostly, I wanted to get a set of presets for the right color of metals before I started altering them.

If you are looking at a 'chromed' surface, then you are looking at a layer of chrome and can, for the purposes of rendering, ignore what is underneath.  If you are looking at painted or powder coated materials, you aren't looking at a metal.

### Surface-treated pickled metals

This is going to end up being a bit of PBR-meets-metal-fabrication.

From the how-it-looks-later perspective, there's a few things you can do to shape an object.

First, you can scrape it with some sort of tool.  Softer materials you can fabricate however and then texture it with a brush.  Harder materials, you just get the marks from a lathe or a mill.  All of these are going to be along an axis.  If you fabricate metal in a lathe, the texturing is going to resemble tiny circles around the lathed axis.  If you use a mill, you get straight lines down the cutting axis.

Second, you can roll it.  This also tends to leave roller mark down the axis of rolling.  Then you can cut, bend, and otherwise fabricate it.

Third, you can cast or 3D print it.  This tends to leave a rough crystalized texture.

Fourth, you can hammer it.

Aluminum is soft, as far as metals go.  It's really easy to get a nice texture out of it with a stainless steel brush, so that's fairly common.  You probably want to make sure that the direction of the shader (the tangent vector) is pointing intelligently.

Steel is a very flexible material, overall.  Right now, I've just added cast steel, which is going to inevitably have some bumpyness as it crystallizes as well as some bumpyness from potentially being cast in a refractory sand.

Copper is frequently hammered to shape it.

Finally, you can powder-coat the metal.

There is:
- Brushed Pickled Aluminum
- Cast Pickled Steel
- Hammered Pickled Copper
- Hammered Lightly Worn Copper

## Cycles Principled Transparent Materials

### Glass

There are a few kinds of glass to know about.

First, there's soda lime glass.  For the present purposes, I'm calling them "crown" glass.  The majority of the glass you experience in the real world is some formulation of soda lime glass -- windows, drinking glasses, etc.

Second, there's leaded glass.  This is often called "Crystal".  It's one of the cheapest fake-gemstones you can find.  It's got a very different refractive index, which makes it sparkle.

And there's borosilicate or "Pyrex" that is much harder to work with, but very sturdy. Note that most of the things you go into the store and buy that are branded as "Pyrex" are not actually borosilicate anymore.  Lab glassware is often Pyrex.

Finally, there's fused silica or fused quartz, which is absurdly annoying to work with and mostly seen in labs and spacecraft windows.

Cycles can't do real dispersion (meaning, that rainbow effect you get out of properly cut crystal) so there's fake dispersion provided for a few materials.  There is also a reduced complexity crown glass for use with windows.

### Plastics

You can look at a piece of plastic and tease out some of the properties.

I have PET (#1) and Polystyrine (#6) plastics, both of which you should see the number of in the recycling symbol.  The majority of plastic drink bottles are PET.

Acrylic is the most common non-glass window material, with Polycarbonate the second most popular one.

### Gems

There's Ruby and White Sapphire and Diamond, with some fake dispersion available.

### Liquids

Since the TSA counts ice as a "liquid", I'll file it under this category.  There's also water and whisky.

### Fully fake glass

Sometimes you want a material that looks kinda like glass, but not really.

That's where the Fully Fake Glass material comes in.  It's not using Principled, it's instead using a node group.  The whole point is to have a fresnel reflection that looks more-or-less like real glass, but not actually refract.

## Cycles Principled Plastics

- Powdercoated metal - If you look at any sort of powdercoated metal, regardless of the material underneath, you are going to end up seeing the same top material.
- Gray pebbled powdercoat - This is the more pebbly sort of powdercoat.  I made it for a stage lighting instrument, because that's where I see it the most.
- White Melamine - This is the sort of thing that white IKEA furniture is covered with.  Change the white to something else and you've got every sort of cheap melamine countertop in the world.

There's probably more plastics to be made, but also understand that in 3D, plastic is the default mode and the default criticism.  :D

## Cycles Principled Construction Materials

### Wall paint

Wall paint is usually a little bit bumpy and the way the paint dries creates a bit of an impression of a clearcoat, even if there isn't one.  I've provided a white one and a gray one.  You probably want to adjust the particular texture and roughness depending on what sort of wall you want.

### Other materials

 - Stucco - This is all about the bump map.
 - Soundproofing hard material - This is a rumply cellular sort of material, for the hard sort of soundproofing baffles
 - Marble - You can get whatever color you want by tweaking the color ramps.

## Cycles Principled Fabric

There's a lot of fabrics here.  Most of them are going to require you to have UV mapping set up properly.

All of these will require some tweaking.  You'll want to change the scale, the colors, etc.

### Nylons

All of these materials are designed such that they look good with a fully three-dimensional solid mesh with a little bit of thickness, say the solidify modifier set very thin.

I included "Sheer Nylon Fake" as part of my experimentation into sheer fabrics.  It renders faster, but it doesn't quite look the same.

The rest of the materials, in a variety of colors, are as if Cycles was rendering tiny threads.  I think it looks more realistic, especially up close.  Sampling means that they tend to look like the usual sheer fabrics from afar, but resolve into textures up close.

At a wider mesh, there's also black fishnets.

### Cottons

I've got oxford cotton, checkered cotton, t-shirt material with a bit of a streakyness, striped cotton, and there's probably a bunch to be had by cloning an existing material and tinkering with it.

### Leathers

I supplied new black leather and worn black leather.  Worn black leather has less glossy bits and some fake-wear on it.

### Wool

Most wool has some patterning to it, so I've got two different shades.  There's probably more to be had with more intricate striped patterns.

### Other fabrics

- B&W Felt
- Carbon fiber
- Two tone satin
- Silk
- Satin
- Crushed Velvet

## CC0: Share and enjoy

This is covered by the CC0 license:

https://creativecommons.org/publicdomain/zero/1.0/

## References
- http://www.robinwood.com/Catalog/Technical/Gen3DTuts/Gen3DPages/RefractionIndexList.html
- https://docs.google.com/document/d/1Fb9_KgCo0noxROKN4iT8ntTbx913e-t4Wc2nMRWPzNk/edit
- https://seblagarde.wordpress.com/2014/04/14/dontnod-physically-based-rendering-chart-for-unreal-engine-4/
- https://www.filmetrics.com/refractive-index-database
- https://blenderartists.org/forum/showthread.php?278285-Yet-Another-Thread-about-Cycles-Materials&p=2288101&viewfull=1
- https://blender.stackexchange.com/questions/40920/how-can-i-make-a-procedural-polished-granite-material
- https://blender.stackexchange.com/questions/18348/how-to-make-a-bronze-material-in-cycles
- https://www.reddit.com/r/blender/comments/5gr7it/this_is_the_best_node_setup_i_can_think_of_for/