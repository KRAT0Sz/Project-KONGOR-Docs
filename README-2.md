Thread previously made by ElementUser

The purpose of this thread is to help people understand how to properly make effects for those who are learning how to create their own effects.

 

 

<effect>
useentityeffectscale --true,false 0,1 (should be used for global effects used on multiple heroes... i.e. state.effect impact.effect etc.)
deferred --true, false 0,1 (allows the sprites to fade correctly)
persistent --true, false 0,1 (allows emitters to finish emitting even after gadget/entity etc. is terminated. note: if persistent is set to true make sure all emitters have a life or count.)

<particlesystem>
name --used to name your particle systems. typically they are named system0 or system1 but you can use any name you want, just make sure that you call it correctly in the spawnparticlesystem tag.
space --set to world or entity... if set to entity the emitter stays with the hero. set it to world if u want the particles to drag or be left behind.
scale --can set scale to whatever you want ie: 1, 50 or even .2 but keep in mind it's cleaner to have it set to 1.

<particle>
color --should have three numbers ie: color="1 1 1". the order of the numbers is for red green blue (rgb).
alpha --can range from 0-1... ie: alpha=".5" means transparency is at 50%.
size --used to bring the sprite to a certain size. good to use startsize midsize and endsize to get more variety out of the same sprite.
width --will change the width of the sprite... will scale only the width of the sprite leaving the height alone.
height --will change the height of the sprite... will scale only the height of the sprite leaving the width alone.
scale --you can set scale to whatever you want ie: 1, 50, .5, 10.5 etc. good to use startscale midscale and endscale to get more variety out of the same sprite. scale and size are closely related they are there to give more control to the user.
pitch --is used to rotate sprite along the x axis... typically, this one is used with lockup and lockright set to true, and pitch set to 90 to make the sprite flat or parallel to the ground.
yaw --is used to rotate along the z axis.
roll --is used to rotate along the y axis. * you can add speed to yaw, pitch and roll to get the sprite to spin along said axis. for instance yawspeed="360" will make the sprite spin 360 degrees along the z axis according to how long the life is.
frame --used for animated sprites typically set using startframe="0" endframe="1". framespeed can be used to make the animated sequence faster or slower too.
param --used for refraction sprites. basically sets the strength of the refraction. is usually set between param=".1" to param="1".
stickiness --acts almost the same as anchor in fact you'll typically wanna use anchor... it's the newer setting.
anchor --will keep the sprite anchored to the bone it is called on... can get some interesting results with startanchor="1" and endanchor="0", or whatever u wanna set endanchor at.
widthdistort --exactly what it says... will distort the width only
heightdistort --will distort the height only
scaleu --is used to scale along the u in uv...
scalev --is used to scale along the v in uv... has some interesting results when used simultaneously with startscale and endscale.
offsetu --never use these settings but i'm assuming that this offsets the sprite along said direction... in this case the u in uv.
offsetv --never use these settings but i'm assuming that this offsets the sprite along said direction... in this case the u in uv.
weight --never use this... not sure if it even works.
lockup --true, false 0,1 locks the sprite up.
lockright --true, false 0,1 locks the sprite right. both lock settings are used to isolate or control the sprite. when both are set to true and pitch is set to 90 the sprite is locked parallel to the ground.
turn --true, false 0,1 allows sprites to turn with entity. used mostly with embedded models inside simpleemitters to turn correctly with projectile.
flare --true, false 0,1
emitter --can be used to call an emitter inside another emitter by name... so, it is typically used for embedding emitters.

** keep in mind that most settings in the particle tag can use start, mid, and end settings too. for example startcolor="1 1 1" midcolor="1 0 1" endcolor="1 0 0".
you can control when the mid setting hits with midcolorpos. midcolorpos=".25" will happen sooner, and midcolorpos=".85" will happen later. if midcolorpos is not being
used the default is set to ".5"

<simpleemitter>
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrate is. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
particledirectionspace {global, local} --local space will use the bones local axis, and global will use the world axis. typically this is used in local to get a sprite pointing down a specific axis of a specific bone.
gravity --is what it is. you can set the strength of gravity. minus gravity goes up and positive gravity goes down.
speed --the speed at which the particles move in a certain direction. when no direction is specified the sprites go out from the center.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. good to use with other settings such as speed to get more variety.
minangle--hard to explain but minangle and maxangle are used to create an area or window for emitting. for instance if minangle="0" and maxangle="45" the emitter will only emit in that range of 45 degrees.
maxangle --refer to minangle
inheritvelocity --the emitter can inherit the velocity of an object, entity, bone, projectile etc. this setting gives a very dynamic feel to effects but dont set it too high, because the sprites start goin crazy.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --direction is setup in x,y,z. so, to set this correctly use -1 1 or 0. an example would be direction="0 0 1". this will send the sprites in the positive z direction, or in our engine up. -1 would send the sprites in a negative z direction, or in our engine down.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --the best way for me to describe drag i guess is to think of it like molasses. it causes the sprites to slow down as if they are fighting through something thick. it doesn't take much to create lots of drag. its typically set around .1 or so.
friction --basically the opposite of acceleration. over time friction will decelerate the sprites to a complete stop if its strong enough.
bone --is used to emit from a specific bone. make sure the bone name is correct or the emitter will not work. the names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
offsetsphere --creates a sphere that the emitter can emit from. if the offsetsphere="100 100 100" for instance, the sprites will emit from anywhere within this 100x100x100 sphere.
offsetcube --creates a cube that the emitter emits from. same as offsetsphere but in a cube. both are set using x,y,z coordinates.
offsetdirection --can be used with offsetradial to give the emitter some depth. like emitting from a donut.
offsetradial --creates a circle. the emitter then emits along the circumference of the circle.
offsetradialangle --another setting to go with offsetradial. this one can be set anywhere from 0-180 basically makes a cone shape from the point of origin. works almost like a spotlight in max.
collide --true, false 0,1 when set to true the sprites will collide with the ground... very cool effect but can suck up processing power. use wisely, and keep the life and count low.
subframepose -- true, false 0,1 used with trailemitters and twopointtrailemitters. should always be set to true.

<orbiteremitter>
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrate is. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
gravity --does not work with orbiteremitters.
speed --basically speeds up the orbit setting. so if the orbit="1" and speed="2" the orbiting particles will now move at a speed of 2. if orbit is set to 0 speed will have no effect.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. if orbit is set to 0 acceleration will have no effect.
minangle --does not work with orbiteremitter
maxangle --does not work with orbiteremitter
inheritvelocity --does not work with orbiteremitter
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --direction is setup in x,y,z. it is important to set a direction so that the orbiteremitter knows which axis to work from. for example direction="0 0 1".
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --does not work with orbiteremitter
friction --once the sprite, or sprites are emitted they will slow down according to the friction setting. if orbit is set to 0, the friction setting will have no effect.
bone --this setting is used to emit from a specific bone. make sure the bone name is correct or the emitter will not work. the names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
origin --the origin gives control over placement of the emitter, and could be looked at as a distance from center setting. for example if direction="0 0 1" and origin="0 50 250" the orbiteremitter will have a diameter of 250 and will move up 50 from the center in the z axis.
cylindrical --when set to true makes a cylindrical shape that the particles emit from. by default is shaped like a disc.
offset --the offset gives more control to the user in shaping the cylinder. hard to describe but let me give an example. offset="10 250 500". the number 250 gives the cylinder your emitting from an offset depth of 250. the number 500 gives the particles an offset of 500 widthwise from the outside edge or circumference of the cylinder. and the 10 starts the sprites going around the cylinder's circumference. if this number is too low the sprites wont emit from all the way around the cylinder.
orbit --this is the speed of the turn/orbiting sprites. a positive number will rotate the sprites counter clockwise and a negative number will rotate them around the origin in a clockwise manner.
orbitangle --i think this is an old setting or it just doesn't work.

<trackeremitter>
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrate is. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
gravity --is what it is. you can set the strength of gravity. minus gravity goes up and positive gravity goes down. this will effect the tracking of particles so be careful when setting gravity.
speed --the speed at which the particles move toward the target.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. good to use with other settings such as speed to get more variety.
minangle --hard to explain but minangle and maxangle are used to create an area or window for emitting. for instance if minangle="0" and maxangle="45" the emitter will only emit in that range of 45 degrees.
maxangle --refer to minangle... minangle and maxangle can create some neat effects when used with different track types.
inheritvelocity --the emitter can inherit the velocity of an object, entity, bone, projectile etc. this setting gives a very dynamic feel to effects but dont set it too high. if the object is moving to fast the sprites start goin crazy, and it can look like a mistake.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --can be used to change the direction of sprites. especially useful when used with minangle and maxangle.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --the best way for me to describe drag i guess is to think of it like molasses. it causes the sprites to slow down as if they are fighting through something thick. it doesn't take much to create lots of drag. its typically set around .1 or so.
friction --once the sprite, or sprites are emitted they will slow down according to friction setting. over time friction will decelerate the sprites to a complete stop if its strong enough.
bone --this is the bone the trackeremitter starts at.
position --this is the position the trackeremitter will start at. can be used with or without bones.
targetbone --this is the bone that the emitter tracks to.
targetposition --this is the position that the emitter tracks to. can be used with or without bones.
offsetsphere --creates a sphere that the emitter can emit from. if the offsetsphere="100 100 100" for instance, the sprites will emit from anywhere within this 100x100x100 sphere.
offsetcube --creates a cube that the emitter emits from. same as offsetsphere but in a cube. both are set using x,y,z coordinates.
offsetdirection --can be used with offsetradial to give the emitter some depth. like emitting from a donut.
offsetradial --creates a circle. the emitter then emits along the circumference of the circle.
offsetradialangle --another setting to go with offsetradial. this one can be set anywhere from 0-180 basically makes a cone shape from the point of origin. works almost like a spotlight in max.
tracktype {distance, angular, gravity, cgravity, target, lerp}
trackspeed will be set to 1 typically. but for some of the tracktypes it can be used to modify the look at the beginning of the tracker. for instance the higher trackspeed with angular will cause the sprites to stay closer together at the origin. however, like with lerp for instance trackspeed can only be set to one. then speed or acceleration can be used to get the look you want.
distancelife --true, false 0,1 when set to true this setting will cause the particles to die once they have reached the targetbone or position.

<meshemitter>
mesh --this is the name of the mesh that the emitter will emit from... without the mesh name the emitter will not emit correctly.
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrate is. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
particledirectionspace {global, local} --local space will use the bones local axis, and global will use the world axis. typically this is used in local to get a sprite pointing down a specific axis of a specific bone.
gravity --is what it is. you can set the strength of gravity. minus gravity goes up and positive gravity goes down.
speed --the speed at which the particles move in a certain direction. when no direction is specified the sprites go out from the center.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. good to use with other settings such as speed to get more variety.
minangle --hard to explain but minangle and maxangle are used to create an area or window for emitting. for instance if minangle="0" and maxangle="45" the emitter will only emit in that range of 45 degrees.
maxangle --refer to minangle
inheritvelocity --the emitter can inherit the velocity of an object, entity, bone, projectile etc. this setting gives a very dynamic feel to effects but dont set it too high, because the sprites start goin crazy.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --direction is setup in x,y,z. so, to set this correctly use -1 1 or 0. an example would be direction="0 0 1". this will send the sprites in the positive z direction, or in our engine up. -1 would send the sprites in a negative z direction, or in our engine down.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --the best way for me to describe drag i guess is to think of it like molasses. it causes the sprites to slow down as if they are fighting through something thick. it doesn't take much to create lots of drag. its typically set around .1 or so.
friction --basically the opposite of acceleration. over time friction will decelerate the sprites to a complete stop if its strong enough.
bone --is used to emit from a specific bone. make sure the bone name is correct or the emitter will not work. the names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
offsetsphere --creates a sphere that the emitter can emit from. if the offsetsphere="100 100 100" for instance, the sprites will emit from anywhere within this 100x100x100 sphere.
offsetcube --creates a cube that the emitter emits from. same as offsetsphere but in a cube. both are set using x,y,z coordinates.
offsetdirection --can be used with offsetradial to give the emitter some depth. like emitting from a donut.
offsetradial --creates a circle. the emitter then emits along the circumference of the circle.
offsetradialangle --another setting to go with offsetradial. this one can be set anywhere from 0-180 basically makes a cone shape from the point of origin. works almost like a spotlight in max.
collide --true, false 0,1 when set to true the sprites will collide with the ground... very cool effect but can suck up processing power. use wisely, and keep the life and count low.

<skeletonemitter>
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrate is. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
particledirectionspace {global, local} --local space will use the bones local axis, and global will use the world axis. typically this is used in local to get a sprite pointing down a specific axis of a specific bone.
gravity --is what it is. you can set the strength of gravity. minus gravity goes up and positive gravity goes down.
speed --the speed at which the particles move in a certain direction. when no direction is specified the sprites go out from the center.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. good to use with other settings such as speed to get more variety.
minangle --hard to explain but minangle and maxangle are used to create an area or window for emitting. for instance if minangle="0" and maxangle="45" the emitter will only emit in that range of 45 degrees.
maxangle --refer to minangle
inheritvelocity --the emitter can inherit the velocity of an object, entity, bone, projectile etc. this setting gives a very dynamic feel to effects but dont set it too high, because the sprites start goin crazy.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --direction is setup in x,y,z. so, to set this correctly use -1 1 or 0. an example would be direction="0 0 1". this will send the sprites in the positive z direction, or in our engine up. -1 would send the sprites in a negative z direction, or in our engine down.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --the best way for me to describe drag i guess is to think of it like molasses. it causes the sprites to slow down as if they are fighting through something thick. it doesn't take much to create lots of drag. its typically set around .1 or so.
friction --basically the opposite of acceleration. over time friction will decelerate the sprites to a complete stop if its strong enough.
bone --is used to emit from a specific bone. make sure the bone name is correct or the emitter will not work. the names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
offsetsphere --creates a sphere that the emitter can emit from. if the offsetsphere="100 100 100" for instance, the sprites will emit from anywhere within this 100x100x100 sphere.
offsetcube --creates a cube that the emitter emits from. same as offsetsphere but in a cube. both are set using x,y,z coordinates.
offsetdirection --can be used with offsetradial to give the emitter some depth. like emitting from a donut.
offsetradial --creates a circle. the emitter then emits along the circumference of the circle.
offsetradialangle --another setting to go with offsetradial. this one can be set anywhere from 0-180 basically makes a cone shape from the point of origin. works almost like a spotlight in max.
collide --true, false 0,1 when set to true the sprites will collide with the ground... very cool effect but can suck up processing power. use wisely, and keep the life and count low.

<twopointtrailemitter>
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrate is. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
particledirectionspace {global, local} --local space will use the bones local axis, and global will use the world axis. typically this is used in local to get a sprite pointing down a specific axis of a specific bone.
gravity --is what it is. you can set the strength of gravity. minus gravity goes up and positive gravity goes down.
speed --the speed at which the particles move in a certain direction. when no direction is specified the sprites go out from the center.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. good to use with other settings such as speed to get more variety.
minangle --hard to explain but minangle and maxangle are used to create an area or window for emitting. for instance if minangle="0" and maxangle="45" the emitter will only emit in that range of 45 degrees.
maxangle --refer to minangle
inheritvelocity --the emitter can inherit the velocity of an object, entity, bone, projectile etc. this setting gives a very dynamic feel to effects but dont set it too high, because the sprites start goin crazy.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --direction is setup in x,y,z. so, to set this correctly use -1 1 or 0. an example would be direction="0 0 1". this will send the sprites in the positive z direction, or in our engine up. -1 would send the sprites in a negative z direction, or in our engine down.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --the best way for me to describe drag i guess is to think of it like molasses. it causes the sprites to slow down as if they are fighting through something thick. it doesn't take much to create lots of drag. its typically set around .1 or so.
friction --basically the opposite of acceleration. over time friction will decelerate the sprites to a complete stop if its strong enough.
bone_a --allows you to name a bone as one of the two points in the twopointtrailemitter.
bone_b --allows you to name an additional bone as the second point in the twopointtrailemitter.
position_a --since this is a twopointtrailemitter there is a position_a and position_b in case there are no bones to use. this allows you to manually set the point in space. this setting can be used in addition to, or without bone_a.
position_b --since this is a twopointtrailemitter there is a position_a and position_b in case there are no bones to use. this allows you to manually set the point in space. this setting can be used in addition to, or without bone_b.
offsetsphere --creates a sphere that the emitter can emit from. if the offsetsphere="100 100 100" for instance, the sprites will emit from anywhere within this 100x100x100 sphere.
offsetcube --creates a cube that the emitter emits from. same as offsetsphere but in a cube. both are set using x,y,z coordinates.
offsetdirection --can be used with offsetradial to give the emitter some depth. like emitting from a donut.
offsetradial --creates a circle. the emitter then emits along the circumference of the circle.
offsetradialangle --another setting to go with offsetradial. this one can be set anywhere from 0-180 basically makes a cone shape from the point of origin. works almost like a spotlight in max.
texpostime --this should be set to same amount of time as the particlelife. so if your particlelife="2000" set textpostime="2000"
texposscale --this should be set to 0
texstretchscale --this should be set to 1 typically.
subframepose --true, false 0,1 this should be set to true.

<trailemitter>
owner --was used a lot more before embedding but, is still used to add emitters to models by name.
life --gives the emitter a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
count --gives the user control of how many sprites to use no matter what the life and spawnrates are. count="5" will spit out 5 sprites no more no less.
timenudge --timenudge is setup in milliseconds, and is an older setting. for the most part delay is better to use. however, in some cases you'll want an emitter to get a head start before the sprites start showing up. in this case you can set timenudge to a minus number and get the emitter goin early. i.e. timenudge="-2000"
delay --used to delay the emitting of sprites. delay is setup in milleseconds as well. so, to delay the emitter for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used to loop any emitter with a life.
spawnrate --is the amount of particles emitting from the emitter by the second. if spawnrate="25" then 25 sprites will be spit out per second. for trailemitters higher spawnrates look smoother but at a cost in performance.
particlelife --is the amount of time the particle will live in milliseconds. minparticlelife and maxparticlelife can be used to get more variety.
particletimenudge --i think this is an old setting and pretty sure itsa no worky.
particledirectionspace {global, local} --local space will use the bones local axis, and global will use the world axis. typically this is used in local to get a sprite pointing down a specific axis of a specific bone.
gravity --is what it is. you can set the strength of gravity. minus gravity goes up and positive gravity goes down.
speed --the speed at which the particles move in a certain direction. when no direction is specified the sprites go out from the center.
acceleration --once the sprite, or sprites are emitted they will pick up speed according to acceleration setting. good to use with other settings such as speed to get more variety.
minangle --hard to explain but minangle and maxangle are used to create an area or window for emitting. for instance if minangle="0" and maxangle="45" the emitter will only emit in that range of 45 degrees.
maxangle --refer to minangle
inheritvelocity --the emitter can inherit the velocity of an object, entity, bone, projectile etc. this setting gives a very dynamic feel to effects but dont set it too high, because the sprites start goin crazy.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
direction --direction is setup in x,y,z. so, to set this correctly use -1 1 or 0. an example would be direction="0 0 1". this will send the sprites in the positive z direction, or in our engine up. -1 would send the sprites in a negative z direction, or in our engine down.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
drag --the best way for me to describe drag i guess is to think of it like molasses. it causes the sprites to slow down as if they are fighting through something thick. it doesn't take much to create lots of drag. its typically set around .1 or so.
friction --basically the opposite of acceleration. over time friction will decelerate the sprites to a complete stop if its strong enough.
bone --is used to emit from a specific bone. make sure the bone name is correct or the emitter will not work. the names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
offsetsphere --creates a sphere that the emitter can emit from. if the offsetsphere="100 100 100" for instance, the sprites will emit from anywhere within this 100x100x100 sphere.
offsetcube --creates a cube that the emitter emits from. same as offsetsphere but in a cube. both are set using x,y,z coordinates.
offsetdirection --can be used with offsetradial to give the emitter some depth. like emitting from a donut.
offsetradial --creates a circle. the emitter then emits along the circumference of the circle.
offsetradialangle --another setting to go with offsetradial. this one can be set anywhere from 0-180 basically makes a cone shape from the point of origin. works almost like a spotlight in max.
texpostime --this should be set to same amount of time as the particlelife. so if your particlelife="2000" set textpostime="2000"
texposscale --this should be set to 0
texstretchscale --this should be set to 1 typically.
subframepose --true, false 0,1 this should be set to true.

**there are some other settings for the trailemitters and twopointtrailemitters. if you want to get a nice particle fade you can use startparticlecolor, midparticlecolor, endparticlecolor if the sprites are additive.
if the sprites are blended then you will want to use startparticlealpha, midparticlealpha, endparticlealpha.**


<light>
owner --was used a lot more before embedding but, still can be used to add lights to models by name.
life --gives the light a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
timenudge --timenudge is setup in milliseconds, and is an older setting. for lights your probably better off just using delay instead.
delay --used to delay the light from showing up. delay is setup in milleseconds as well. so, to delay the light for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used anytime to loop any light with a life.
bone --is used to emit from a specific bone. make sure the bone name is correct or the emitter will not work. the names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
color --should have three numbers ie: color="1 1 1". the order of the numbers is for red green blue (rgb). with lights you can exceed 1 1 1 and do something like 10 5 5 to make the light even brighter.
falloffstart --falloffstart works with falloffend. lets say for instance falloffend="75" and falloffstart="74" this will create a sharp light with barely any falloff. if you want a smoother falloff you'll need more range between the two settings. for example set the falloffstart="0" and falloffend="75", and this will create a much smoother falloff. objects close to the light will be brighter then objects further away. hence the name falloff.
falloffend --refer to falloffstart... just another thing to note. falloffstart should always be less than falloffend, unless for some reason you want to invert the falloff.

**just a note... when using lights remember that there can only be 4 lights at any giving time playing on the screen at once. so be careful when using lights, and make sure the effects look good without them just in case.

<beam>
owner_a -- owner_a is one end of the beam and owner_b is the other... they can be set to source, target, etc... they can also be set to model names if you have a beam effect between two different models in your effect file.
owner_b -- refer to owner_a...
bone_a --can be used to have one end of the beam start at a specific bone. make sure the bone name is correct or the beam will not work. the names are case sensitive.
bone_b --can be used for the other end of the beam effect, also a specific bone. make sure the bone name is correct or the beam will not work. the names are case sensitive.
position_a --if there is no bone to reference you can use the position_a and position_b settings. position_a being one end and position_b being the other. these settings can be used with or without the bone_a or bone_b settings. so, here is an example using position_a with bone_a... say for instance bone_a="headBone" and position_a="0 0 50" then the beam will start 50 units up in the the z axis from the headBone.
position_b --refer to position_a...
life --gives the beam a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
timenudge --timenudge is setup in milliseconds, and is an older setting. for beams your probably better off just using delay instead.
delay --will delay the beam from showing up. delay is setup in milleseconds as well. so, to delay the beam for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used anytime to loop any beam with a life.
color --should have three numbers ie: color="1 1 1". the order of the numbers is for red green blue (rgb).
alpha --can range from 0-1... ie: alpha=".5" means transparency is at 50%.
size --this setting controls the size of the beam. when used with the tile setting size can be very handy in shaping the beam and the sprite its using.
tile --is used to repeat the sprite along the beam effect. this setting can be used to create some cool repeating parts. an example of when you might wanna use this setting would be when you want to create chain with several links. the higher you set the number, the more repetition you'll get within the beam effect.
frame --used for animated sprites typically set using startframe="0" endframe="1". framespeed is another setting that can be used to make the animated sequence faster or slower. beam effects with animated lighting sprites look pretty cool. 
param --can be used to create a refraction sprite beam effect. param is used specifically for refraction sprites. basically it sets the strength of the refraction. param is usually set between param=".1" to param="1", but can definately be set higher if you want. keep in mind that when using param and refraction sprites with beams the tile setting doesn't work properly.
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.


<groundsprite>
owner --was used a lot more before embedding... but, still can be used to add groundsprites to models and such by name.
bone --adds the groundsprite to a specific bone which it will follow if it's not embedded in another emitter. make sure the bone name is correct or the groundsprite will not show up. the bone names are case sensitive.
position --position is setup in x,y,z. however in this case since its a groundsprite the z axis value is insignificant. so, position will be used for x and y values only.
life --gives the beam a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
timenudge --timenudge is setup in milliseconds, and is an older setting. for groundsprites your probably better off just using delay instead.
delay --will delay the groundsprite from showing up. delay is setup in milleseconds as well. so, to delay the groundsprite for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used anytime to loop any groundsprite with a life.
color --should have three numbers ie: color="1 1 1". the order of the numbers is for red green blue (rgb).
alpha --can range from 0-1... ie: alpha=".5" means transparency is at 50%.size --this setting controls the size of the groundsprite. size="25", size="1" etc.
width --will change the width of the groundsprite... will scale only the width of the groundsprite leaving the height alone.
height --will change the height of the groundsprite... will scale only the height of the groundsprite leaving the width alone.
scale --you can set scale to whatever you want ie: 1, 50, .5, 10.5 etc. good to use startscale midscale and endscale to get more variety out of the same sprite. scale and size are closely related they are both there to give more control to the user.
frame --used for animated groundsprites typically set using startframe="0" endframe="1". framespeed is another setting that can be used to make the animated sequence faster or slower. keep in mind though that groundsprites are expensive on their own, so don't go all crazy with a bunch of animated groundsprites.
param --can be used to create a refraction sprite beam effect. param is used specifically for refraction sprites. basically it sets the strength of the refraction. param is usually set between param=".1" to param="1", but can definately be set higher if you want. keep in mind that when using param and refraction sprites with beams the tile setting doesn't work properly.
material --this is where you call your sprite. for example material="/shared/effects/materials/myGroundSprite.material" the path is typically this one but can be specific to any directory.

**another bit of info... groundsprites are very expensive especially when stacked on top of eachother and large in size, so be careful... if you think it might be an issue you can check your frame rate drops by typing host_drawFPS 1 into the console.

<billboard>
owner --was used a lot more before embedding... but, still can be used to add billboards to models and such by name.
life --gives the billboard a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
timenudge --timenudge is setup in milliseconds, and is an older setting. for billboards your probably better off just using delay instead.
delay --will delay the billboard from showing up. delay is setup in milleseconds as well. so, to delay the billboard for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used anytime to loop any billboard with a life.
bone --adds the billboard to a specific bone which it will follow if it's not embedded in another emitter. make sure the bone name is correct or the billboard will not show up. the bone names are case sensitive.
position --position is setup in x,y,z. if the emitter for example has a bone="headbone" and the position="0 0 50", then the emitter will be positioned 50 above the headbone in the z axis.
color --should have three numbers ie: color="1 1 1". the order of the numbers is for red green blue (rgb).
alpha --can range from 0-1... ie: alpha=".5" means transparency is at 50%.size --this setting controls the size of the groundsprite. size="25", size="1" etc.
pitch --is used to rotate sprite along the x axis... typically, this one is used with lockup and lockright set to true, and pitch set to 90 to make the sprite flat or parallel to the ground.
yaw --is used to rotate along the z axis.
roll --is used to rotate along the y axis. * you can add speed to yaw, pitch and roll to get the sprite to spin along said axis. for instance yawspeed="360" will make the sprite spin 360 degrees along the z axis according to how long the life is.
size --this setting controls the size of the beam. when used with the tile setting size can be very handy in shaping the beam and the sprite its using.
width --will change the width of the groundsprite... will scale only the width of the groundsprite leaving the height alone.
height --will change the height of the groundsprite... will scale only the height of the groundsprite leaving the width alone.
scale --you can set scale to whatever you want ie: 1, 50, .5, 10.5 etc. good to use startscale midscale and endscale to get more variety out of the same sprite. scale and size are closely related they are both there to give more control to the user.
frame --used for animated billboards typically set using startframe="0" endframe="1". framespeed is another setting that can be used to make the animated sequence faster or slower.
param --can be used to create a refraction sprite. param is used specifically for refraction sprites. basically it sets the strength of the refraction. param is usually set between param=".1" to param="1", but can definately be set higher if you want.
depthbias --a minus depthbias will bring the sprite closer to the camera, and a positive depthbias will move the sprite away from the camera.
lockup --true, false 0,1 locks the sprite up.
lockright --true, false 0,1 locks the sprite right. both lock settings are used to isolate or control the sprite. when both are set to true and pitch is set to 90 the sprite is locked parallel to the ground.
turn --true, false 0,1 allows sprites to turn with entity. used mostly with embedded models inside simpleemitters to turn correctly with projectile.
flare --true, false 0,1
material --this is where you call your sprite. for example material="/shared/effects/materials/mySprite.material" the path is typically this one but can be specific to any directory.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.

<model>
name --allows you to name the model... this was used a lot before we were able to embed effects. but, still can be used with the owner setting on emitters to add particles, billboards, etc. to the model by name.
owner --was used a lot more before embedding... but, still can be used to add models to other models and such by name.
life --gives the model a lifetime. life is setup in milliseconds. so, if u need it to last 5 seconds set the life="5000".
timenudge --timenudge is setup in milliseconds, and is an older setting. for models your probably better off just using delay instead.
delay --will delay the model from showing up. delay is setup in milleseconds as well. so, to delay the model for 2 seconds set delay="2000".
loop --true, false 0,1 is often used in state.effects. but, can be used anytime to loop any model with a life.
directionalspace --{global, local, and i think world works too} world and global will emit according to world axis. local will emit from a specific bone and its local axis which can be very different from the world depending on the animation of said bone.
bone --adds the model to a specific bone. make sure the bone name is correct or the model will not show up. the bone names are case sensitive.
position --position is setup in x,y,z. if the model for example has a bone="headbone" and the position="0 0 50", then the model will be positioned 50 above the headbone in the z axis.
color --should have three numbers ie: color="1 1 1". the order of the numbers is for red green blue (rgb).
alpha --can range from 0-1... ie: alpha=".5" means transparency is at 50%.size --this setting controls the size of the groundsprite. size="25", size="1" etc.
pitch --is used to rotate sprite along the x axis... typically, this one is used with lockup and lockright set to true, and pitch set to 90 to make the sprite flat or parallel to the ground.
yaw --is used to rotate along the z axis.
roll --is used to rotate along the y axis. * you can add speed to yaw, pitch and roll to get the sprite to spin along said axis. for instance yawspeed="360" will make the sprite spin 360 degrees along the z axis according to how long the life is.
scale --you can set scale to whatever you want ie: 1, 50, .5, 10.5 etc. good to use startscale midscale and endscale to get more variety out of the model. scale and size are closely related they are both there to give more control to the user.
model --this is where you'll call the model... for example model="model/model.mdf". the path can be specific to any directory.
skin
material
anim
parentmodel
parentskeleton

<tracer>
owner_a
owner_b
bone_a
bone_b
position_a
position_b
life
timenudge
delay
loop
color
alpha
width
length
speed
acceleration
taper
tile
frame
param
material

<sound>
*owner
life
timenudge
delay
loop
bone
position
falloff
volume
pitch
sound2d
soundloop
fadein
fadeoutstarttime
fadeout
speed1
speed2
speeduptime
slowdowntime

<precipemitter>
owner
life
count
timenudge
delay
loop
spawnrate
particlelife
particletimenudge
gravity
speed
acceleration
minangle
maxangle
inheritvelocity
material
direction
directionalspace
drag
friction
drawdistance
collide

<terrainemitter>
owner
life
count
timenudge
delay
loop
spawnrate
particlelife
particletimenudge
gravity
speed
acceleration
minangle
maxangle
inheritvelocity
material
direction
directionalspace
drag
friction
range
collide

<modifier>
name
life
timenudge
delay
loop
color
alpha
material
