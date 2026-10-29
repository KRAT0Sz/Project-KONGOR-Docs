EnginePK Engine# Heroes of Newerth (K2 Engine) — XML Scripting: A Primer

> **แหล่งข้อมูลอ้างอิง:** HoN Official Developer Forums (Author: ElementUser - S2/Frostburn Staff)
> **Archive URL:** https://web.archive.org/web/20211207092852/https://forums.heroesofnewerth.com/index.php?/topic/1546-xml-scripting-a-primer/
> **จัดเก็บและเรียบเรียง:** สำหรับ (Project KONGOR)

---

## ส่วนที่ 1: การตั้งค่าพื้นฐาน และโครงสร้างไฟล์ Entity

If you want to script your own abilities & heroes or make changes to them, this is the place to look! I will be slowly updating this thread over time.

Setup

Console Commands to run in HoN

In the HoN Client, press Ctrl+F8 to open the console and type in the following commands & then press Enter after each one:

> Quote

SetSave "ui_showQuickStart" "true"

SetSave "ui_forceEnableTesting" "true"

host_dynamicResReload true

ui_showQuickStart (when set to true) makes a nice new button show up in the top bar near the center-right of it: http://i.imgur.com/0Rftyjs.jpg

ui_forceEnableTesting (when set to true) will enable Advanced Mode for the Test+++ menu in Practice Mode. This will come in handy later if you do more advanced scripting/testing.

host_dynamicResReload (when set to true) will make sure that file changes will be hot-reloaded upon saving the file. Note that this does not necessarily apply to stringtables or art assets.

Text Editor

Feel free to use any text editor you want to use. I personally use Notepad++: https://notepad-plus-plus.org/downloads/

Other popular text editors include (but are not limited to):

Visual Studio Code: https://code.visualstudio.com/download

Sublime Text: https://www.sublimetext.com/

Atom: https://atom.io/

Vim (just kidding, don't use this unless you're a Vim enthusiast/Vim master or a masochist)

Extracting from the resourcesX.s2z files

resources0.s2z, resources1.s2z, ..., are really just zip files. Use any zip file program to open them (I recommend 7-Zip: https://www.7-zip.org/).

Never modify files in the resources files directly! Doing so may cause undesired crashes & problems for your client!

Assuming that you installed HoN in the default path:

> Quote

C:\Program Files (x86)\Heroes of Newerth\game

, you should find those resource files in that location. Once you open them, feel free to extract whatever you need, so long as the folder directory structure matches that of the resources file.

Example: if you want to extract the hero folder for Magebane, the folder hero alias is called "javaras", found in the resources2.s2z file. You would then make a folder path like this (relative path to the HoN default path):

> Quote

heroes/javaras/

The extracted folder structure for the "javaras" folder should look something like this (left-side). The right side shows what resources1.s2z has.

You can do this for any file/folder in HoN, and can even add new files and use them (so long as they are referenced properly & placed in the right directory).

Modifying Files for HoN

Generally speaking, any changes in the resources files you have extracted will have their changes taking place in your local client in Practice Mode. Any cosmetic changes will also apply to your local client even in real games, but any non-cosmetic changes will not be used by the server (for obvious reasons).

Although this behaviour is the default one, you should not attempt to play a real HoN game on an official server if you are modding actual functional/mechanical behaviour. Doing so has the risk of crashing your game, or causing undesired issues to your local client only.

XML Files

First off, XML stands for "Extensible Markup Language": https://www.w3schools.com/xml/xml_whatis.asp

In terms of syntax & format, XML has 2 main concepts: an element (or a tag) and attributes (fields). A brief example using Magebane's ability.entity file for his Mana Combustion ability:

The single-line comments without open or close angled brackets (< and >) are the "attributes", while the content that are enclosed by the angled brackets are considered "elements" or "tags".

Entity System

For many of HoN's files, we use XML as the format to encapsulate attributes for an entity. An entity is considered the "base" type for anything that affects HoN mechanically/functionally. Entities can be broken down to more specific entity types, such as (but not limited to): hero entities, gadget entities, projectile entities, affector entities, pet entities, and state entities.

Using Magebane as an example (remember the hero alias is "javaras"), I recommend just changing some numbers to start out, and making them obvious changes to make sure that your changes work locally.

For example, you can change Magebane's base Agility to 1000. To do this, open `heroes/javaras/hero.entity`, then find the "agility" attribute:

Then change that to any value you want (in this case, 1000). Make sure to save the file to save the change so that you can see the change in the HoN client!

To test if the change worked, open up your HoN Client and go to Practice Mode (if you have not done that already), then select Magebane. If you see the attribute change to 1000, then your change worked!

This is what you should see for the above example if you did the change correctly:

Entity Type Breakdown

Like I mentioned, there are several types of entities. This is not an exhaustive list, but here are the main entity types that are used:

hero

ability

gadget

pet

neutral

projectile

affector

state

statenetaccum

item

building

Hero Entities

Contains the base information about a hero, including details like model size, animation references, model references, starting attributes/damage, ability entity references, and so forth.

Ability Entities

This is a tool that is used by entities that are allowed to have ability slots. These include hero, building, gadget, pet, heropet and neutral entities. You can apply passive attributes (e.g. extra Attack Damage, Agility, Armor, Attack Speed, etc.) to an ability entity, as well as combat-related events.

Technically, because of HoN's polymorphism for XML entities, you can apply combat-related events to most entity types.

Gadget & Pet Entities

Gadgets and pets are very similar & normally act as a regular unit. However, there are some differences:

- A Gadget entity is usually immobile (this is by convention - you can in fact make a gadget move if you wanted to)

	- A Gadget entity typically has GadgetImmunity, which renders it immune to Magic & certain target schemes. Again, this is by convention, they can just not have this feature if you wish.

	- A Gadget (rather than a pet) is conventionally used to serve some auxiliary purpose: acting as a timer, granting vision of an area by binding it to a projectile or a unit, storing information, acting as a reference point for the location of certain abilities.

	- A Pet is usually mobile & can attack and perform actions more or less like a regular unit.

Neutral Entities

These are just regular units, except they belong to the computer & act as NPCs if they are not controlled by a player. They are normally defined this way to have certain behaviours apply to them (such as the algorithm to move the neutrals back to the camp & respawn the neutrals, and to prevent certain abilties from targeting them, etc.).

Projectile Entities

Anything that physically flies from 1 place to another is normally done with a projectile entity. You can bind other units (gadgets, heroes, etc.) to a projectile as well, making abilities like leaps, dashes and throws doable.

Affector Entities

Affector entities are entities that target all valid units within a predefined area (usually a circle or a cone). In other words, they are essentially "area of effect" entities that do something in said area.

State & Statenetaccum Entities

State entities are by far some of the most versatile entity types in HoN. As their name implies, they keep track of some kind of state. These states are essentially timers that do something during the time they are active, at certain time intervals, upon their infliction/re-application/expiration, etc. States are also typically used with some kind of combat-related event (e.g. upon taking damage, you could heal the damage inflictor - something like what Ichor's Life Leech ability does: https://hon.gamepedia.com/Ichor ).

States are also typically used when you want to modify the behaviour of certain abilities (e.g. a state for Bubbles' Shell Surf is used to keep track of when he can teleport to his Shell after he uses the ability: https://hon.gamepedia.com/Bubbles).

Statenetaccum entity files are just a specialized state entity typically meant for shield states (to display the visual shield bar).

Item Entities

This is a tool that is used by entities that are allowed to have inventory slots. WIth some small differences (e.g. Perplex will disable items from being activated, whereas items are unaffected by Silence, and the fact that you can only right-click items to move them), they are more or less the  same thing as an ability.

Building Entities

This entity type is used for structures - structures are typically immobile with their own unique properties (such as being able to be targeted by certain abilities & being affected by the Glyph of Fortification).

Edited July 29 by ElementUser

4

2

### Link to post

### Share on other sites

ElementUser

793

Posted October 15, 2020

ElementUser

Staff

793

2899 posts

Author

Share

Posted October 15, 2020 

(edited)

---

## ส่วนที่ 2: แท็ก XML สำหรับการดีบัก (Debug Tags)

Helpful XML tags to use

> Quote

This will be one of your best friends when scripting, as you get a general breakdown of the commonly used variables when this is used.

> Quote

This will tell the result on the stack. You can replace "result" with any variable that exists.

Edited November 1, 2020 by ElementUser

### Link to post

### Share on other sites

ElementUser

793

Posted October 15, 2020

ElementUser

Staff

793

2899 posts

Author

Share

Posted October 15, 2020 

(edited)

---

## ส่วนที่ 3: ตาราง C++ Declarations สำหรับ Combat Events และ Action Processors ทั้งหมด

XML - Combat Events & Tags

The following code excerpt is directly taken from a source .cpp file. Essentially, it maps some actual elements and attributes from XML to actual behaviour/attributes in C++.

This list is not exhaustive & only covers combat-related XML events.

`csharp

//=============================================================================
// Declarations
//=============================================================================
DECLARE_XML_PROCESSOR(chance)
DECLARE_XML_PROCESSOR(condition)
DECLARE_XML_PROCESSOR(elsecondition)
DECLARE_XML_PROCESSOR(else)
DECLARE_XML_PROCESSOR(compare)
DECLARE_XML_PROCESSOR(upgradeselected)
DECLARE_XML_PROCESSOR(comparestrings)
DECLARE_XML_PROCESSOR(cantarget)
DECLARE_XML_PROCESSOR(canactivate)
DECLARE_XML_PROCESSOR(isready)
DECLARE_XML_PROCESSOR(isstealthed)
DECLARE_XML_PROCESSOR(testactivate)
DECLARE_XML_PROCESSOR(canattack)
DECLARE_XML_PROCESSOR(caneffect)
DECLARE_XML_PROCESSOR(combatsupertype)
DECLARE_XML_PROCESSOR(combateffecttype)
DECLARE_XML_PROCESSOR(damagesupertype)
DECLARE_XML_PROCESSOR(damageeffecttype)
DECLARE_XML_PROCESSOR(currentdamagesupertype)
DECLARE_XML_PROCESSOR(currentdamageeffecttype)
DECLARE_XML_PROCESSOR(casteffecttype)
DECLARE_XML_PROCESSOR(consume)
DECLARE_XML_PROCESSOR(negate)
DECLARE_XML_PROCESSOR(absorbdamage)
DECLARE_XML_PROCESSOR(addattackpreimpactactions)
DECLARE_XML_PROCESSOR(addattackpredamageactions)
DECLARE_XML_PROCESSOR(addattackdamageeventactions)
DECLARE_XML_PROCESSOR(addattackimpactactions)
DECLARE_XML_PROCESSOR(addattackimpactinvalidactions)
DECLARE_XML_PROCESSOR(hasmetteamscoregoal)
DECLARE_XML_PROCESSOR(hasmetplayerscoregoal)
DECLARE_XML_PROCESSOR(isiteminbackpack)
DECLARE_XML_PROCESSOR(isiteminstash)
DECLARE_XML_PROCESSOR(hasmodifier)
DECLARE_XML_PROCESSOR(hasavatarkey)
DECLARE_XML_PROCESSOR(comparetiledefinition)
DECLARE_XML_PROCESSOR(areaofeffect)
DECLARE_XML_PROCESSOR(foreachplayer)
DECLARE_XML_PROCESSOR(foreachproxy)
DECLARE_XML_PROCESSOR(while)
DECLARE_XML_PROCESSOR(whilescriptcondition)
DECLARE_XML_PROCESSOR(loop)
DECLARE_XML_PROCESSOR(scriptcondition)
DECLARE_XML_PROCESSOR(foreachitem)
DECLARE_XML_PROCESSOR(foreachstate)
DECLARE_XML_PROCESSOR(entitytype)
DECLARE_XML_PROCESSOR(targettype)
DECLARE_XML_PROCESSOR(combatevent)
DECLARE_XML_PROCESSOR(lineofeffect)
DECLARE_XML_PROCESSOR(hasmodel)
DECLARE_XML_PROCESSOR(isitem)
DECLARE_XML_PROCESSOR(isability)
DECLARE_XML_PROCESSOR(issplitprojectile)
DECLARE_XML_PROCESSOR(ownsproduct)
DECLARE_XML_PROCESSOR(comparecvar)
DECLARE_XML_PROCESSOR(setcvar)
DECLARE_XML_PROCESSOR(checkcamerapos)
DECLARE_XML_PROCESSOR(isterminated)
//=============================================================================

//=============================================================================
// Definitions
//=============================================================================
// REGISTER_COMBAT_EVENT_XML_PROCESSOR
#define REGISTER_COMBAT_EVENT_XML_PROCESSOR(parent) \
CXMLProcessor_##parent::RegisterProcessor(this); \
CombatEvent::CXMLProcessor_##parent::RegisterProcessor(this);

// REGISTER_XML_COMBAT_ACTION
#define REGISTER_XML_COMBAT_ACTION(name) \
BEGIN_XML_REGISTRATION(name) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onframe) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onframeimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oninterval) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onbegin) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onprecost) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onaction) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onpreimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onpredamage) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondamageevent) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondoubleactivate) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onimpactinvalid) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oncomplete) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oncancel) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onactivatestart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onactivateprecost) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onactivatepreimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onactivateimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onactivateend) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onabilitystart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onabilityimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onabilityfinish) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onabilityend) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontoggleon) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontoggleoff) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelstart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelframe) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelbreak) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelend) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelingstart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelingframe) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelingbreak) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchannelingend) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackstart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattack) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackpreimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackpredamage) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackdamageevent) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackimpactinvalid) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackend) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackedstart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackedpreimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackedpredamage) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackeddamageevent) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackedpostimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackingstart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackingpreimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackingpredamage) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackingdamageevent) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onattackingpostimpact) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondamage) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondamaged) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onheal) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onhealed) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstunned) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onkilled) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onsmackdowned) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onexpired) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onunitexpired) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onmanareduction) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondisjointed) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onorderdisjointed) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onexperienceadded) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondeath) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onkill) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onunkillabledeath) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onassist) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onindirectkill) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onspawn) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onrespawn) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onfailedrespawn) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onlevelup) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oninflict) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onrefresh) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oninflicted) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onownerrespawn) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onrelease) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontouch) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontouched) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oncollide	) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onthink) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontargetacquired) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onlearn) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onupgrade) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onremove) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oncreate) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onpurchase) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onprepurchase) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontimer) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onpickup) \
    REGISTER_COMBAT_EVENT_XML_PROCESSOR(ondropped) \
    REGISTER_COMBAT_EVENT_XML_PROCESSOR(onsold) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onready) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onleash) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onlobbystart) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onaddplayer) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onentergame) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onexitgame) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstartprematch) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstartmatch) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstartpostmatch) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(oncourierdelivered) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstartpreround) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstartround) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstartpostround) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onconcede) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onendround) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onteammetscoregoal) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onplayermetscoregoal) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontimer0expire) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontimer1expire) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontimer2expire) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(ontimer3expire) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onmapenter) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onmapexit) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onevade) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onevaded) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onprestoleitem) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onstoleitem) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onchangehealth) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onsubscriberunitdeath) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onsubscriberunitkill) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onsubscriberitemacquisition) \
	REGISTER_COMBAT_EVENT_XML_PROCESSOR(onpremove) \
	REGISTER_XML_PROCESSOR(checkcost) \
	REGISTER_XML_PROCESSOR(checktriggeredcost) \
	REGISTER_XML_PROCESSOR(activatecost) \
	REGISTER_XML_PROCESSOR(getthreatlevel) \
	REGISTER_XML_PROCESSOR(chance) \
	REGISTER_XML_PROCESSOR(condition) \
	REGISTER_XML_PROCESSOR(elsecondition) \
	REGISTER_XML_PROCESSOR(else) \
	REGISTER_XML_PROCESSOR(compare) \
	REGISTER_XML_PROCESSOR(upgradeselected) \
	REGISTER_XML_PROCESSOR(comparestrings) \
	REGISTER_XML_PROCESSOR(cantarget) \
	REGISTER_XML_PROCESSOR(combatsupertype) \
	REGISTER_XML_PROCESSOR(combateffecttype) \
	REGISTER_XML_PROCESSOR(damagesupertype) \
	REGISTER_XML_PROCESSOR(damageeffecttype) \
	REGISTER_XML_PROCESSOR(currentdamagesupertype) \
	REGISTER_XML_PROCESSOR(currentdamageeffecttype) \
	REGISTER_XML_PROCESSOR(casteffecttype) \
	REGISTER_XML_PROCESSOR(consume) \
	REGISTER_XML_PROCESSOR(canactivate) \
	REGISTER_XML_PROCESSOR(isready) \
	REGISTER_XML_PROCESSOR(isstealthed) \
	REGISTER_XML_PROCESSOR(testactivate) \
	REGISTER_XML_PROCESSOR(canattack) \
	REGISTER_XML_PROCESSOR(negate) \
	REGISTER_XML_PROCESSOR(absorbdamage) \
	REGISTER_XML_PROCESSOR(addattackpreimpactactions) \
	REGISTER_XML_PROCESSOR(addattackpredamageactions) \
	REGISTER_XML_PROCESSOR(addattackdamageeventactions) \
	REGISTER_XML_PROCESSOR(addattackimpactactions) \
	REGISTER_XML_PROCESSOR(addattackimpactinvalidactions) \
	REGISTER_XML_PROCESSOR(hasmetteamscoregoal) \
	REGISTER_XML_PROCESSOR(hasmetplayerscoregoal) \
	REGISTER_XML_PROCESSOR(isiteminbackpack) \
	REGISTER_XML_PROCESSOR(isiteminstash) \
	REGISTER_XML_PROCESSOR(hasmodifier) \
	REGISTER_XML_PROCESSOR(hasavatarkey) \
	REGISTER_XML_PROCESSOR(comparetiledefinition) \
	REGISTER_XML_PROCESSOR(areaofeffect) \
	REGISTER_XML_PROCESSOR(foreachplayer) \
	REGISTER_XML_PROCESSOR(foreachproxy) \
	REGISTER_XML_PROCESSOR(scriptthread) \
	REGISTER_XML_PROCESSOR(while) \
	REGISTER_XML_PROCESSOR(whilescriptcondition) \
	REGISTER_XML_PROCESSOR(loop) \
	REGISTER_XML_PROCESSOR(scriptcondition) \
	REGISTER_XML_PROCESSOR(foreachitem) \
	REGISTER_XML_PROCESSOR(foreachstate) \
	REGISTER_XML_PROCESSOR(entitytype) \
	REGISTER_XML_PROCESSOR(targettype) \
	REGISTER_XML_PROCESSOR(lineofeffect) \
	REGISTER_XML_PROCESSOR(hasmodel) \
	REGISTER_XML_PROCESSOR(isitem) \
	REGISTER_XML_PROCESSOR(isability) \
	REGISTER_XML_PROCESSOR(issplitprojectile) \
	REGISTER_XML_PROCESSOR(ownsproduct) \
	REGISTER_XML_PROCESSOR(comparecvar) \
	REGISTER_XML_PROCESSOR(setcvar) \
	REGISTER_XML_PROCESSOR(checkcamerapos) \
	REGISTER_XML_PROCESSOR(isterminated) \
END_XML_REGISTRATION

// BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR
#define BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(name, object) \
DECLARE_XML_PROCESSOR(name) \
REGISTER_XML_COMBAT_ACTION(name) \
BEGIN_XML_PROCESSOR(name, IActionScript) \
	object* pAction(K2_NEW(ctx_GameActionCombat, object)); \
	pAction->SetSource(node.GetProperty(_T("source"), _T("source_entity"))); \
	pAction->SetTarget(node.GetProperty(_T("target"), _T("target_entity")));

// END_COMBAT_ACTION_LEAF_XML_PROCESSOR
#define END_COMBAT_ACTION_LEAF_XML_PROCESSOR \
	pObject->AddAction(pAction); \
END_XML_PROCESSOR_NO_CHILDREN

// BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR
#define BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(name, object) \
REGISTER_XML_COMBAT_ACTION(name) \
BEGIN_XML_PROCESSOR(name, IActionScript) \
	object* pAction(K2_NEW(ctx_GameActionCombat, object)); \
	pAction->SetSource(node.GetProperty(_T("source"), _T("source_entity"))); \
	pAction->SetTarget(node.GetProperty(_T("target"), _T("target_entity")));

// END_COMBAT_ACTION_BRANCH_XML_PROCESSOR
#define END_COMBAT_ACTION_BRANCH_XML_PROCESSOR \
	pObject->AddAction(pAction); \
END_XML_PROCESSOR(pAction->GetActionScript())

// BEGIN_COMBAT_ACTION_EVENT_XML_PROCESSOR
#define BEGIN_COMBAT_ACTION_EVENT_XML_PROCESSOR(name, object) \
REGISTER_XML_COMBAT_ACTION(name) \
BEGIN_XML_PROCESSOR(name, IActionScript) \
	object* pAction(K2_NEW(ctx_GameActionCombat) object); \
	pAction->SetSource(node.GetProperty(_T("source"), _T("source_entity"))); \
	pAction->SetTarget(node.GetProperty(_T("target"), _T("target_entity")));

// END_COMBAT_ACTION_EVENT_XML_PROCESSOR
#define END_COMBAT_ACTION_EVENT_XML_PROCESSOR \
	pObject->AddAction(pAction); \
END_XML_PROCESSOR_NO_CHILDREN

// COMBAT_EVENT_XML_PROCESSOR
#define COMBAT_EVENT_XML_PROCESSOR(name, script) \
DECLARE_XML_PROCESSOR(name) \
BEGIN_XML_REGISTRATION(name) \
	REGISTER_WITH_ENTITY_PROCESSORS \
END_XML_REGISTRATION \
BEGIN_XML_PROCESSOR(name, IEntityDefinition) \
	CCombatActionScript* pScript(pObject->NewActionScript(script, node.GetPropertyInt(_T("priority")), node.GetPropertyBool(_T("propagatetoillusions"), true), /*node.GetPropertyBool(_T("propagatetosplitattacks"), true),*/ \
		node.GetPropertyBool(_T("activateonbounces")), Game.LookupResponseType(node.GetProperty(_T("responsetype"))), Game.LookupResponseType(node.GetProperty(_T("ignoreresponsetype"))))); \
END_XML_PROCESSOR(pScript) \
namespace CombatEvent \
{ \
DECLARE_XML_PROCESSOR(name) \
BEGIN_XML_REGISTRATION(name) \
	REGISTER_XML_PROCESSOR(combatevent) \
END_XML_REGISTRATION \
BEGIN_XML_PROCESSOR(name, CCombatActionCombatEvent) \
	CCombatActionScript* pScript(pObject->NewActionScript(script)); \
END_XML_PROCESSOR(pScript) \
}

// COMBAT_EVENT_XML_PROCESSOR
DECLARE_XML_PROCESSOR(scriptthread)
BEGIN_XML_REGISTRATION(scriptthread)
	REGISTER_XML_PROCESSOR_EX(XMLGameInfo, game)
	REGISTER_XML_PROCESSOR_EX(XMLGameLogic, gamelogic)
END_XML_REGISTRATION
BEGIN_XML_PROCESSOR(scriptthread, IEntityDefinition)
	CScriptThread* pScript(EntityRegistry.NewScriptThread(node.GetProperty(_T("name")))); \
END_XML_PROCESSOR(pScript)
//=============================================================================

//=============================================================================
// Auras
//=============================================================================
// 
DECLARE_XML_PROCESSOR(aura)
BEGIN_XML_REGISTRATION(aura)
	REGISTER_WITH_ENTITY_PROCESSORS
END_XML_REGISTRATION
BEGIN_XML_PROCESSOR(aura, IEntityDefinition)
	const tstring& sName(node.GetProperty(_CTS("name")));
	const tstring& sStateName(node.GetProperty(_CTS("state")));
	const tstring& sGadgetName(node.GetProperty(_CTS("gadget")));
	
	// Remove any duplicate auras so that you can override auras in modifiers
	pObject->RemoveAura(sName);

	if (sStateName.empty() && sGadgetName.empty())
		return false;

	// Note that aura has seperated priority and it's derived from host definition on loading.
	// This guideline is similar with process of normal properties of entity definitions  

	pObject->AddAura( K2_NEW( ctx_GameActionCombat, CAuraDefinition )(
		pObject->GetPriority(),	
		sName,
		sStateName,
		sGadgetName,
		node.GetProperty(_CTS("radius")),
		node.GetProperty(_CTS("duration")),
		node.GetProperty(_CTS("targetscheme")),
		node.GetProperty(_CTS("effecttype")),
		node.GetProperty(_CTS("ignoreinvulnerable")),
		node.GetProperty(_CTS("condition")),
		node.GetProperty(_CTS("reflexivestate")),
		node.GetProperty(_CTS("propagatecondition")),
		node.GetProperty(_CTS("propagatesource")),
		node.GetProperty(_CTS("propagatetarget")),
		node.GetProperty(_CTS("proxy")),
		node.GetProperty(_CTS("stack")),
		node.GetProperty(_CTS("gadgetowner")),
		node.GetPropertyBool(_CTS("notooltip")),
		node.GetPropertyBool(_CTS("disablenegativeeffectpopuponrefresh")),
		node.GetPropertyBool(_CTS("towersignore")),
		node.GetPropertyBool(_CTS("propagatetoillusions"), true )
	), false );
END_XML_PROCESSOR_NO_CHILDREN 
//=============================================================================

//=============================================================================
// Events
//=============================================================================
COMBAT_EVENT_XML_PROCESSOR(onframe, ACTION_SCRIPT_FRAME)
COMBAT_EVENT_XML_PROCESSOR(onframeimpact, ACTION_SCRIPT_FRAME_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(oninterval, ACTION_SCRIPT_INTERVAL)

COMBAT_EVENT_XML_PROCESSOR(onbegin, ACTION_SCRIPT_BEGIN)
COMBAT_EVENT_XML_PROCESSOR(onstart, ACTION_SCRIPT_START)
COMBAT_EVENT_XML_PROCESSOR(onprecost, ACTION_SCRIPT_PRE_COST)
COMBAT_EVENT_XML_PROCESSOR(onaction, ACTION_SCRIPT_ACTION)
COMBAT_EVENT_XML_PROCESSOR(onpreimpact, ACTION_SCRIPT_PRE_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onpredamage, ACTION_SCRIPT_PRE_DAMAGE)
COMBAT_EVENT_XML_PROCESSOR(ondamageevent, ACTION_SCRIPT_DAMAGE_EVENT)
COMBAT_EVENT_XML_PROCESSOR(ondoubleactivate, ACTION_SCRIPT_DOUBLE_ACTIVATE)
COMBAT_EVENT_XML_PROCESSOR(onimpact, ACTION_SCRIPT_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onimpactinvalid, ACTION_SCRIPT_IMPACT_INVALID)
COMBAT_EVENT_XML_PROCESSOR(oncomplete, ACTION_SCRIPT_COMPLETE)
COMBAT_EVENT_XML_PROCESSOR(oncancel, ACTION_SCRIPT_CANCEL)

COMBAT_EVENT_XML_PROCESSOR(onactivatestart, ACTION_SCRIPT_ACTIVATE_START)
COMBAT_EVENT_XML_PROCESSOR(onactivateprecost, ACTION_SCRIPT_ACTIVATE_PRE_COST)
COMBAT_EVENT_XML_PROCESSOR(onactivatepreimpact, ACTION_SCRIPT_ACTIVATE_PRE_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onactivateimpact, ACTION_SCRIPT_ACTIVATE_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onactivateend, ACTION_SCRIPT_ACTIVATE_END)

COMBAT_EVENT_XML_PROCESSOR(onabilitystart, ACTION_SCRIPT_ABILITY_START)
COMBAT_EVENT_XML_PROCESSOR(onabilityimpact, ACTION_SCRIPT_ABILITY_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onabilityfinish, ACTION_SCRIPT_ABILITY_FINISH)
COMBAT_EVENT_XML_PROCESSOR(onabilityend, ACTION_SCRIPT_ABILITY_END)

COMBAT_EVENT_XML_PROCESSOR(ontoggleon, ACTION_SCRIPT_TOGGLE_ON)
COMBAT_EVENT_XML_PROCESSOR(ontoggleoff, ACTION_SCRIPT_TOGGLE_OFF)

COMBAT_EVENT_XML_PROCESSOR(onchannelstart, ACTION_SCRIPT_CHANNEL_START)
COMBAT_EVENT_XML_PROCESSOR(onchannelframe, ACTION_SCRIPT_CHANNEL_FRAME)
COMBAT_EVENT_XML_PROCESSOR(onchannelbreak, ACTION_SCRIPT_CHANNEL_BROKEN)
COMBAT_EVENT_XML_PROCESSOR(onchannelend, ACTION_SCRIPT_CHANNEL_END)

COMBAT_EVENT_XML_PROCESSOR(onchannelingstart, ACTION_SCRIPT_CHANNELING_START)
COMBAT_EVENT_XML_PROCESSOR(onchannelingframe, ACTION_SCRIPT_CHANNELING_FRAME)
COMBAT_EVENT_XML_PROCESSOR(onchannelingbreak, ACTION_SCRIPT_CHANNELING_BROKEN)
COMBAT_EVENT_XML_PROCESSOR(onchannelingend, ACTION_SCRIPT_CHANNELING_END)

COMBAT_EVENT_XML_PROCESSOR(onattackstart, ACTION_SCRIPT_ATTACK_START)
COMBAT_EVENT_XML_PROCESSOR(onattack, ACTION_SCRIPT_ATTACK)
COMBAT_EVENT_XML_PROCESSOR(onattackpreimpact, ACTION_SCRIPT_ATTACK_PRE_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onattackpredamage, ACTION_SCRIPT_ATTACK_PRE_DAMAGE)
COMBAT_EVENT_XML_PROCESSOR(onattackdamageevent, ACTION_SCRIPT_ATTACK_DAMAGE_EVENT)
COMBAT_EVENT_XML_PROCESSOR(onattackimpact, ACTION_SCRIPT_ATTACK_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onattackimpactinvalid, ACTION_SCRIPT_ATTACK_IMPACT_INVALID)
COMBAT_EVENT_XML_PROCESSOR(onattackend, ACTION_SCRIPT_ATTACK_END)

COMBAT_EVENT_XML_PROCESSOR(onattackedstart, ACTION_SCRIPT_ATTACKED_START)
COMBAT_EVENT_XML_PROCESSOR(onattackedpreimpact, ACTION_SCRIPT_ATTACKED_PRE_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onattackedpredamage, ACTION_SCRIPT_ATTACKED_PRE_DAMAGE)
COMBAT_EVENT_XML_PROCESSOR(onattackeddamageevent, ACTION_SCRIPT_ATTACKED_DAMAGE_EVENT)
COMBAT_EVENT_XML_PROCESSOR(onattackedpostimpact, ACTION_SCRIPT_ATTACKED_POST_IMPACT)

COMBAT_EVENT_XML_PROCESSOR(onattackingstart, ACTION_SCRIPT_ATTACKING_START)
COMBAT_EVENT_XML_PROCESSOR(onattackingpreimpact, ACTION_SCRIPT_ATTACKING_PRE_IMPACT)
COMBAT_EVENT_XML_PROCESSOR(onattackingpredamage, ACTION_SCRIPT_ATTACKING_PRE_DAMAGE)
COMBAT_EVENT_XML_PROCESSOR(onattackingdamageevent, ACTION_SCRIPT_ATTACKING_DAMAGE_EVENT)
COMBAT_EVENT_XML_PROCESSOR(onattackingpostimpact, ACTION_SCRIPT_ATTACKING_POST_IMPACT)

COMBAT_EVENT_XML_PROCESSOR(ondamage, ACTION_SCRIPT_DAMAGE)
COMBAT_EVENT_XML_PROCESSOR(ondamaged, ACTION_SCRIPT_DAMAGED)
COMBAT_EVENT_XML_PROCESSOR(onheal, ACTION_SCRIPT_HEAL)
COMBAT_EVENT_XML_PROCESSOR(onhealed, ACTION_SCRIPT_HEALED)
COMBAT_EVENT_XML_PROCESSOR(onstunned, ACTION_SCRIPT_STUNNED)
COMBAT_EVENT_XML_PROCESSOR(onkilled, ACTION_SCRIPT_KILLED)
COMBAT_EVENT_XML_PROCESSOR(onsmackdowned, ACTION_SCRIPT_SMACKDOWNED)
COMBAT_EVENT_XML_PROCESSOR(onexpired, ACTION_SCRIPT_EXPIRED)
COMBAT_EVENT_XML_PROCESSOR(ondeath, ACTION_SCRIPT_DEATH)
COMBAT_EVENT_XML_PROCESSOR(onkill, ACTION_SCRIPT_KILL)
COMBAT_EVENT_XML_PROCESSOR(onindirectkill, ACTION_SCRIPT_INDIRECT_KILL)
COMBAT_EVENT_XML_PROCESSOR(onunkillabledeath, ACTION_SCRIPT_UNKILLABLE_DEATH)
COMBAT_EVENT_XML_PROCESSOR(onassist, ACTION_SCRIPT_ASSIST)
COMBAT_EVENT_XML_PROCESSOR(ondisjointed, ACTION_SCRIPT_DISJOINTED)
COMBAT_EVENT_XML_PROCESSOR(onorderdisjointed, ACTION_SCRIPT_ORDER_DISJOINTED)
COMBAT_EVENT_XML_PROCESSOR(onexperienceadded, ACTION_SCRIPT_EXPERIENCE_ADDED)

COMBAT_EVENT_XML_PROCESSOR(onspawn, ACTION_SCRIPT_SPAWN)
COMBAT_EVENT_XML_PROCESSOR(onrespawn, ACTION_SCRIPT_RESPAWN)
COMBAT_EVENT_XML_PROCESSOR(onfailedrespawn, ACTION_SCRIPT_FAILED_RESPAWN)
COMBAT_EVENT_XML_PROCESSOR(onlevelup, ACTION_SCRIPT_LEVELUP)

COMBAT_EVENT_XML_PROCESSOR(oninflict, ACTION_SCRIPT_INFLICT)
COMBAT_EVENT_XML_PROCESSOR(onrefresh, ACTION_SCRIPT_REFRESH)
COMBAT_EVENT_XML_PROCESSOR(oninflicted, ACTION_SCRIPT_INFLICTED)

COMBAT_EVENT_XML_PROCESSOR(onownerrespawn, ACTION_SCRIPT_OWNER_RESPAWN)

COMBAT_EVENT_XML_PROCESSOR(onrelease, ACTION_SCRIPT_RELEASE)

COMBAT_EVENT_XML_PROCESSOR(ontouch, ACTION_SCRIPT_TOUCH)
COMBAT_EVENT_XML_PROCESSOR(ontouched, ACTION_SCRIPT_TOUCHED)

COMBAT_EVENT_XML_PROCESSOR(oncollide, ACTION_SCRIPT_COLLIDE)

COMBAT_EVENT_XML_PROCESSOR(onthink, ACTION_SCRIPT_THINK)
COMBAT_EVENT_XML_PROCESSOR(ontargetacquired, ACTION_SCRIPT_TARGET_ACQUIRED)
COMBAT_EVENT_XML_PROCESSOR(onlearn, ACTION_SCRIPT_LEARN)
COMBAT_EVENT_XML_PROCESSOR(onupgrade, ACTION_SCRIPT_UPGRADE)
COMBAT_EVENT_XML_PROCESSOR(onremove, ACTION_SCRIPT_REMOVE)
COMBAT_EVENT_XML_PROCESSOR(oncreate, ACTION_SCRIPT_CREATE)
COMBAT_EVENT_XML_PROCESSOR(onprepurchase, ACTION_SCRIPT_PRE_PURCHASE)
COMBAT_EVENT_XML_PROCESSOR(onpurchase, ACTION_SCRIPT_PURCHASED)
COMBAT_EVENT_XML_PROCESSOR(ontimer, ACTION_SCRIPT_TIMER)
COMBAT_EVENT_XML_PROCESSOR(onpickup, ACTION_SCRIPT_PICKUP)
COMBAT_EVENT_XML_PROCESSOR(ondropped, ACTION_SCRIPT_DROPPED)
COMBAT_EVENT_XML_PROCESSOR(onsold, ACTION_SCRIPT_SOLD)
COMBAT_EVENT_XML_PROCESSOR(onready, ACTION_SCRIPT_READY)
COMBAT_EVENT_XML_PROCESSOR(onleash, ACTION_SCRIPT_LEASH)

COMBAT_EVENT_XML_PROCESSOR(onlobbystart, ACTION_SCRIPT_LOBBY_START)
COMBAT_EVENT_XML_PROCESSOR(onaddplayer, ACTION_SCRIPT_ADD_PLAYER)
COMBAT_EVENT_XML_PROCESSOR(onentergame, ACTION_SCRIPT_ENTER_GAME)
COMBAT_EVENT_XML_PROCESSOR(onexitgame, ACTION_SCRIPT_EXIT_GAME)
COMBAT_EVENT_XML_PROCESSOR(onstartprematch, ACTION_SCRIPT_START_PREMATCH)
COMBAT_EVENT_XML_PROCESSOR(onstartmatch, ACTION_SCRIPT_START_MATCH)
COMBAT_EVENT_XML_PROCESSOR(onstartpostmatch, ACTION_SCRIPT_START_POSTMATCH)
COMBAT_EVENT_XML_PROCESSOR(onconcede, ACTION_SCRIPT_CONCEDE)
COMBAT_EVENT_XML_PROCESSOR(oncourierdelivered, ACTION_SCRIPT_COURIER_DELIVERED)

COMBAT_EVENT_XML_PROCESSOR(onstartpreround, ACTION_SCRIPT_START_PRE_ROUND)
COMBAT_EVENT_XML_PROCESSOR(onstartround, ACTION_SCRIPT_START_ROUND)
COMBAT_EVENT_XML_PROCESSOR(onstartpostround, ACTION_SCRIPT_START_POST_ROUND)
COMBAT_EVENT_XML_PROCESSOR(onendround, ACTION_SCRIPT_END_ROUND)

COMBAT_EVENT_XML_PROCESSOR(onteammetscoregoal, ACTION_SCRIPT_TEAM_MET_SCORE_GOAL)
COMBAT_EVENT_XML_PROCESSOR(onplayermetscoregoal, ACTION_SCRIPT_PLAYER_MET_SCORE_GOAL)

COMBAT_EVENT_XML_PROCESSOR(ontimer0expire, ACTION_SCRIPT_TIMER_0_EXPIRE)
COMBAT_EVENT_XML_PROCESSOR(ontimer1expire, ACTION_SCRIPT_TIMER_1_EXPIRE)
COMBAT_EVENT_XML_PROCESSOR(ontimer2expire, ACTION_SCRIPT_TIMER_2_EXPIRE)
COMBAT_EVENT_XML_PROCESSOR(ontimer3expire, ACTION_SCRIPT_TIMER_3_EXPIRE)

COMBAT_EVENT_XML_PROCESSOR(onmapenter, ACTION_SCRIPT_MAP_ENTER)
COMBAT_EVENT_XML_PROCESSOR(onmapexit, ACTION_SCRIPT_MAP_EXIT)

COMBAT_EVENT_XML_PROCESSOR(onevade, ACTION_SCRIPT_EVADE)
COMBAT_EVENT_XML_PROCESSOR(onevaded, ACTION_SCRIPT_EVADED)

COMBAT_EVENT_XML_PROCESSOR(onprestoleitem, ACTION_SCRIPT_PRE_STOLE_ITEM)
COMBAT_EVENT_XML_PROCESSOR(onstoleitem, ACTION_SCRIPT_STOLE_ITEM)

COMBAT_EVENT_XML_PROCESSOR(onchangehealth, ACTION_SCRIPT_HEALTH_CHANGED)

COMBAT_EVENT_XML_PROCESSOR(onpremove, ACTION_SCRIPT_PREMOVE)
COMBAT_EVENT_XML_PROCESSOR(onunitexpired, ACTION_SCRIPT_UNIT_EXPIRED)
COMBAT_EVENT_XML_PROCESSOR(onmanareduction, ACTION_SCRIPT_MANA_REDUCTION)
//=============================================================================

//=============================================================================
// Subscriber Events
//=============================================================================
COMBAT_EVENT_XML_PROCESSOR(onsubscriberunitdeath, SUBSCRIBER_ACTION_SCRIPT_UNIT_DEATH)
COMBAT_EVENT_XML_PROCESSOR(onsubscriberunitkill, SUBSCRIBER_ACTION_SCRIPT_UNIT_KILL)
COMBAT_EVENT_XML_PROCESSOR(onsubscriberitemacquisition, SUBSCRIBER_ACTION_SCRIPT_ITEM_ACQUISITION)
//=============================================================================

//=============================================================================
// Callbacks
//=============================================================================
COMBAT_EVENT_XML_PROCESSOR(checkcost, ACTION_SCRIPT_CHECK_COST)
COMBAT_EVENT_XML_PROCESSOR(checktriggeredcost, ACTION_SCRIPT_CHECK_TRIGGERED_COST)
COMBAT_EVENT_XML_PROCESSOR(activatecost, ACTION_SCRIPT_ACTIVATE_COST)
COMBAT_EVENT_XML_PROCESSOR(getthreatlevel, ACTION_SCRIPT_GET_THREAT_LEVEL)
//=============================================================================

//=============================================================================
// Combat event
//=============================================================================
// 
REGISTER_XML_COMBAT_ACTION(combatevent)
BEGIN_XML_PROCESSOR(combatevent, IActionScript)
	CCombatActionCombatEvent* pAction(K2_NEW(ctx_GameActionCombat, CCombatActionCombatEvent));

	READ_COMBAT_ACTION_PROPERTY_EX(Source, source, source_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Target, target, target_entity)

	READ_COMBAT_ACTION_PROPERTY_EX(SuperType, supertype, spell)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY_EX(Inflictor, inflictor, inflictor_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Proxy, proxy, proxy_entity)
	READ_COMBAT_ACTION_PROPERTY(NoResponse, noresponse)
	READ_COMBAT_ACTION_PROPERTY(ResponseType, responsetype)
	
	READ_COMBAT_ACTION_PROPERTY(DamageType, damagetype)
	READ_COMBAT_ACTION_PROPERTY(BaseDamage, basedamage)
	READ_COMBAT_ACTION_PROPERTY(BaseDamageB, basedamageb)
	READ_COMBAT_ACTION_PROPERTY(BaseDamageOp, basedamageop)
	READ_COMBAT_ACTION_PROPERTY(AdditionalDamage, additionaldamage)
	READ_COMBAT_ACTION_PROPERTY(AdditionalDamageB, additionaldamageb)
	READ_COMBAT_ACTION_PROPERTY(AdditionalDamageOp, additionaldamageop)
	READ_COMBAT_ACTION_PROPERTY(DamageMultiplier, damagemultiplier)
	READ_COMBAT_ACTION_PROPERTY(DamageMultiplierB, damagemultiplierb)
	READ_COMBAT_ACTION_PROPERTY(DamageMultiplierOp, damagemultiplierop)
	READ_COMBAT_ACTION_PROPERTY(BonusDamage, bonusdamage)
	READ_COMBAT_ACTION_PROPERTY(BonusDamageB, bonusdamageb)
	READ_COMBAT_ACTION_PROPERTY(BonusDamageOp, bonusdamageop)
	READ_COMBAT_ACTION_PROPERTY(BonusDamageMultiplier, bonusdamagemultiplier)
	READ_COMBAT_ACTION_PROPERTY(BonusDamageMultiplierB, bonusdamagemultiplierb)
	READ_COMBAT_ACTION_PROPERTY(BonusDamageMultiplierOp, bonusdamagemultiplierop)
	READ_COMBAT_ACTION_PROPERTY(LifeSteal, lifesteal)
	READ_COMBAT_ACTION_PROPERTY(NonLethal, nonlethal)
	READ_COMBAT_ACTION_PROPERTY(MissChance, misschance)
	READ_COMBAT_ACTION_PROPERTY(TrueStrike, truestrike)
	READ_COMBAT_ACTION_PROPERTY(AttackAbility, attackability)
	
	pObject->AddAction(pAction);
END_XML_PROCESSOR(pAction)
//=============================================================================

//=============================================================================
// Actions
//=============================================================================
// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(print, CCombatActionPrint)
	READ_COMBAT_ACTION_PROPERTY(Text, text)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settimeofday, CCombatActionSetTimeOfDay)
	READ_COMBAT_ACTION_PROPERTY(Daytime, daytime)
	READ_COMBAT_ACTION_PROPERTY(TimeOfDay, timeofday)
	READ_COMBAT_ACTION_PROPERTY(OffsetTimeOfDay, offsettimeofday)
	READ_COMBAT_ACTION_PROPERTY(MsTimeOfDay, mstimeofday)
	READ_COMBAT_ACTION_PROPERTY(MsOffsetTimeOfDay, msoffsettimeofday)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changeexperience, CCombatActionChangeExperience)
	READ_COMBAT_ACTION_PROPERTY(SetExp, setexp)
	READ_COMBAT_ACTION_PROPERTY(AddExp, addexp)
	READ_COMBAT_ACTION_PROPERTY(MulExp, mulexp)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(removeglobalmodifier, CCombatActionRemoveGlobalModifier)
	READ_COMBAT_ACTION_PROPERTY(Key, key)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(addglobalmodifier, CCombatActionAddGlobalModifier)
	READ_COMBAT_ACTION_PROPERTY(Key, key)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprematch, CCombatActionSetPreMatch)
	READ_COMBAT_ACTION_PROPERTY(Pause, pause)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprematchbuildinginvulnerability, CCombatActionSetPreMatchBuildingInvulnerability)
	READ_COMBAT_ACTION_PROPERTY(Vulnerable, vulnerable)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(enableneutralcontroller, CCombatActionEnableNeutralController)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Enable, enable)
	READ_COMBAT_ACTION_PROPERTY(ForceSpawn, forcespawn)
	READ_COMBAT_ACTION_PROPERTY(AttemptSpawn, attemptspawn)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

//enableneutralcontrollerinradius
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(enableneutralcontrollersinradius, CCombatActionEnableNeutralControllersInRadius)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Enable, enable)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(ForceSpawn, forcespawn)
	READ_COMBAT_ACTION_PROPERTY(AttemptSpawn, attemptspawn)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(printdebuginfo, CCombatActionPrintDebugInfo)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(printpersistentmodifierkeys, CCombatActionPrintPersistentModifierKeys)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(printvalue, CCombatActionPrintValue)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(Label, label, value, value)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(printpos, CCombatActionPrintPos)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(Label, label, value, value)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(chance, CCombatActionChance)
	READ_COMBAT_ACTION_PROPERTY(Threshold, threshold)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(condition, CCombatActionCondition)
	READ_COMBAT_ACTION_PROPERTY(Test, test)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(elsecondition, CCombatActionElseCondition)
	READ_COMBAT_ACTION_PROPERTY(Test, test)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(else, CCombatActionElse)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(compare, CCombatActionCompare)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(upgradeselected, CCombatActionUpgradeSelected)
	READ_COMBAT_ACTION_PROPERTY(Upgrade, upgrade)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(comparestrings, CCombatActionCompareStrings)
	READ_COMBAT_ACTION_PROPERTY(StrA, a)
	READ_COMBAT_ACTION_PROPERTY(StrB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(cantarget, CCombatActionCanTarget)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(IgnoreInvulnerable, ignoreinvulnerable)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(canactivate, CCombatActionCanActivate)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isready, CCombatActionIsReady)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isstealthed, CCombatActionIsStealthed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(testactivate, CCombatActionTestActivate)
	READ_COMBAT_ACTION_PROPERTY(Distance, distance)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(canattack, CCombatActionCanAttack)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(combatsupertype, CCombatActionCombatSuperType)
	READ_COMBAT_ACTION_PROPERTY(SuperType, supertype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(combateffecttype, CCombatActionCombatEffectType)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(damagesupertype, CCombatActionDamageSuperType)
	READ_COMBAT_ACTION_PROPERTY(SuperType, supertype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(damageeffecttype, CCombatActionDamageEffectType)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(currentdamagesupertype, CCombatActionCurrentDamageSuperType)
	READ_COMBAT_ACTION_PROPERTY(SuperType, supertype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(currentdamageeffecttype, CCombatActionCurrentDamageEffectType)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(casteffecttype, CCombatActionCastEffectType)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(consume, CCombatActionConsume)
	READ_COMBAT_ACTION_PROPERTY(Item, item)
	READ_COMBAT_ACTION_PROPERTY_EX(Count, count, 1)
	READ_COMBAT_ACTION_PROPERTY(UseRecipe, userecipe)
	READ_COMBAT_ACTION_PROPERTY(IgnoreCharges, ignorecharges)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(addattackpreimpactactions, CCombatActionAddAttackPreImpactActions)
	//READ_COMBAT_ACTION_PROPERTY_EX(PropagateToSplitAttack, propagatetosplitattack, true)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(addattackpredamageactions, CCombatActionAddAttackPreDamageActions)
	//READ_COMBAT_ACTION_PROPERTY_EX(PropagateToSplitAttack, propagatetosplitattack, true)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(addattackdamageeventactions, CCombatActionAddAttackDamageEventActions)
	//READ_COMBAT_ACTION_PROPERTY_EX(PropagateToSplitAttack, propagatetosplitattack, true)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(addattackimpactactions, CCombatActionAddAttackImpactActions)
	//READ_COMBAT_ACTION_PROPERTY_EX(PropagateToSplitAttack, propagatetosplitattack, true)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(addattackimpactinvalidactions, CCombatActionAddAttackImpactInvalidActions)
	//READ_COMBAT_ACTION_PROPERTY_EX(PropagateToSplitAttack, propagatetosplitattack, true)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(hasmetteamscoregoal, CCombatHasMetTeamScoreGoal)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(hasmetplayerscoregoal, CCombatHasMetPlayerScoreGoal)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isiteminbackpack, CCombatActionIsItemInBackpack)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_owner_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isiteminstash, CCombatActionIsItemInStash)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_owner_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(checkcamerapos, CCombatActionCheckCameraPosition)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionEntity, positionentity)
	READ_COMBAT_ACTION_PROPERTY(PositionName, positionname)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Tolerance, tolerance)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isterminated, CCombatActionIsTerminated)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(testnearby, CCombatActionTestNearby)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY_EX(IgnoreInvulnerable, ignoreinvulnerable, false)
	READ_COMBAT_ACTION_PROPERTY_EX(Origin, origin, source_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(damage, CCombatActionDamage)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(MinDamage, min, amount, 0)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(MaxDamage, max, amount, 0)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(IsNonLethal, nonlethal)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
	READ_COMBAT_ACTION_PROPERTY_EX(Inflictor, inflictor, this_entity)
	READ_COMBAT_ACTION_PROPERTY(SuperType, supertype)
	READ_COMBAT_ACTION_PROPERTY(ArmorPierce, armorpierce)
	READ_COMBAT_ACTION_PROPERTY(ArmorPiercePercent, armorpiercepercent)
	READ_COMBAT_ACTION_PROPERTY(MagicArmorPierce, magicarmorpierce)
	READ_COMBAT_ACTION_PROPERTY(MagicArmorPiercePercent, magicarmorpiercepercent)
	READ_COMBAT_ACTION_PROPERTY(TowersIgnore, towersignore)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(splashdamage, CCombatActionSplashDamage)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
	READ_COMBAT_ACTION_PROPERTY(IsNonLethal, nonlethal)
	READ_COMBAT_ACTION_PROPERTY(CenterOnTarget, centerontarget)
	READ_COMBAT_ACTION_PROPERTY(SuperType, supertype)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(heal, CCombatActionHeal)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changehealth, CCombatActionChangeHealth)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(chatmessage, CCombatActionChatMessage)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Message, message)
	READ_COMBAT_ACTION_PROPERTY(Param, param)
	READ_COMBAT_ACTION_PROPERTY(ParamValue, paramvalue)
	READ_COMBAT_ACTION_PROPERTY(ParamValuePrecision, paramvalueprecision)
	READ_COMBAT_ACTION_PROPERTY(ParamPlayer1, paramplayer1)
	READ_COMBAT_ACTION_PROPERTY(ParamPlayer2, paramplayer2)
	READ_COMBAT_ACTION_PROPERTY(Important, important)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
	READ_COMBAT_ACTION_PROPERTY(ExcludePlayer, excludeplayer)
	READ_COMBAT_ACTION_PROPERTY(Delay, delay)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(popup, CCombatActionPopup)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(ping, CCombatActionPing)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY_EX(Position, position, target_position)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(givegold, CCombatActionGiveGold)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
    READ_COMBAT_ACTION_PROPERTY(NotShowPopup, notshowpopup)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(takegold, CCombatActionTakeGold)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spendgold, CCombatActionSpendGold)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(giveexperience, CCombatActionGiveExperience)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(givemana, CCombatActionGiveMana)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(takemana, CCombatActionTakeMana)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
	READ_COMBAT_ACTION_PROPERTY(TriggerActionScript, triggeractionscript)
	READ_COMBAT_ACTION_PROPERTY(CostOverTime, costovertime)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(manaburn, CCombatActionManaBurn)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(AmountB, b)
	READ_COMBAT_ACTION_PROPERTY(AmountOp, op)
    READ_COMBAT_ACTION_PROPERTY(NoPopup, nopopup)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(applystate, CCombatActionApplyState)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
	READ_COMBAT_ACTION_PROPERTY(Charges, charges)
	READ_COMBAT_ACTION_PROPERTY_EX(ChargesMultiplier, chargesmult, 1.0)
	READ_COMBAT_ACTION_PROPERTY(IsChannel, ischannel)
	READ_COMBAT_ACTION_PROPERTY(IsToggle, istoggle)
	READ_COMBAT_ACTION_PROPERTY(Proxy, proxy)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY_EX(Stack, stack, none)
	READ_COMBAT_ACTION_PROPERTY(Continuous, continuous)
	READ_COMBAT_ACTION_PROPERTY(Timeout, timeout)
	READ_COMBAT_ACTION_PROPERTY_EX(Spawner, spawner, inflictor_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Inflictor, inflictor, source_entity)
	READ_COMBAT_ACTION_PROPERTY(StateLevel, statelevel)
	READ_COMBAT_ACTION_PROPERTY(Accumulator, accumulator)
	READ_COMBAT_ACTION_PROPERTY(DisableNegativeEffectPopupOnRefresh, disablenegativeeffectpopuponrefresh)
	READ_COMBAT_ACTION_PROPERTY(TowersIgnore, towersignore)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(expirestate, CCombatActionExpireState)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(copystate, CCombatActionCopyState)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Transfer, transfer)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(copystates, CCombatActionCopyStates)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(IgnoreEffectType, ignoreeffecttype)
	READ_COMBAT_ACTION_PROPERTY(Inflictor, inflictor)
	READ_COMBAT_ACTION_PROPERTY(Transfer, transfer)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(clearforcedorders, CCombatActionClearForcedOrders)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(teleport, CCombatActionTeleport)
	READ_COMBAT_ACTION_PROPERTY(Interpolate, interpolate)
	READ_COMBAT_ACTION_PROPERTY(FaceTarget, facetarget)
	READ_COMBAT_ACTION_PROPERTY(SpecifyAngle, specifyangle)
	READ_COMBAT_ACTION_PROPERTY(Angle, angle)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionOrigin, positionorigin, source_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY(PositionModifier, positionmodifier)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(NonInterrupting, noninterrupting)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
	READ_COMBAT_ACTION_PROPERTY(NoOffset, nooffset)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(lookat, CCombatActionLookAt)
	READ_COMBAT_ACTION_PROPERTY(Angle, angle)
	READ_COMBAT_ACTION_PROPERTY(Interpolate, interpolate)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(playanim, CCombatActionPlayAnim)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Variations, variations)
	READ_COMBAT_ACTION_PROPERTY(Channel, channel)
	READ_COMBAT_ACTION_PROPERTY_EX(Speed, speed, 1.0)
	READ_COMBAT_ACTION_PROPERTY(Length, length)
	READ_COMBAT_ACTION_PROPERTY(Seq, seq)
	READ_COMBAT_ACTION_PROPERTY(OverrideWalkAnimation, overridewalkanimation)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(playeffect, CCombatActionPlayEffect)
	READ_COMBAT_ACTION_PROPERTY(Effect, effect)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(Occlude, occlude)
	READ_COMBAT_ACTION_PROPERTY(IgnoreCulling, ignoreculling)
	READ_COMBAT_ACTION_PROPERTY(Visibility, visibility)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(playeffectdynamic, CCombatActionPlayEffectDynamic)
	READ_COMBAT_ACTION_PROPERTY(Effect, effect)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(Occlude, occlude)
	READ_COMBAT_ACTION_PROPERTY(IgnoreCulling, ignoreculling)
	READ_COMBAT_ACTION_PROPERTY(Visibility, visibility)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(stopeffectdynamic, CCombatActionStopEffectDynamic)
READ_COMBAT_ACTION_PROPERTY(Effect, effect)
READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(chain, CCombatActionChain)
	READ_COMBAT_ACTION_PROPERTY(Count, count)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(bounce, CCombatActionBounce)
	READ_COMBAT_ACTION_PROPERTY(Count, count)
	READ_COMBAT_ACTION_PROPERTY(Range, range)
	READ_COMBAT_ACTION_PROPERTY(DamageMult, damagemult)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(Seek, seek)
	READ_COMBAT_ACTION_PROPERTY(MaxBouncesPerTarget, maxbouncespertarget)
	READ_COMBAT_ACTION_PROPERTY(ApplyRealAttackEffects, applyrealattackeffects)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(split, CCombatActionSplit)
	READ_COMBAT_ACTION_PROPERTY(Count, count)
	READ_COMBAT_ACTION_PROPERTY(Range, range)
	READ_COMBAT_ACTION_PROPERTY_EX(Angle, angle, 180.0)
	READ_COMBAT_ACTION_PROPERTY(DamageMult, damagemult)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(FullDamageOnMainTarget, fulldamageonmaintarget)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(return, CCombatActionReturn)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(bind, CCombatActionBind)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Vertical, vertical)
	READ_COMBAT_ACTION_PROPERTY(TurnWithObject, turnwithobject)
	READ_COMBAT_ACTION_PROPERTY(UnbindOnDeath, unbindondeath)
	READ_COMBAT_ACTION_PROPERTY(NoPush, nopush)
	READ_COMBAT_ACTION_PROPERTY_EX(Position, position, true)
	READ_COMBAT_ACTION_PROPERTY(FreeTurn, freeturn)
	READ_COMBAT_ACTION_PROPERTY(VerticalOverride, verticaloverride)
	READ_COMBAT_ACTION_PROPERTY(PositionOverride, positionoverride)
	READ_COMBAT_ACTION_PROPERTY(NonInterrupting, noninterrupting)
	READ_COMBAT_ACTION_PROPERTY(AttachTo, attachto)
	READ_COMBAT_ACTION_PROPERTY_EX(AttachToBoneIndex, attachtoboneindex, 0)
	READ_COMBAT_ACTION_PROPERTY(NoInterpolate, nointerpolate)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(unbind, CCombatActionUnbind)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnunit, CCombatActionSpawnUnit)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Count, count)
	READ_COMBAT_ACTION_PROPERTY(Mount, mount)
	READ_COMBAT_ACTION_PROPERTY(Bind, bind)
	READ_COMBAT_ACTION_PROPERTY(FixedPosition, fixedposition)
	READ_COMBAT_ACTION_PROPERTY_EX(InheritModifiers, inheritmodifiers, true)
	READ_COMBAT_ACTION_PROPERTY(IsChannel, ischannel)
	READ_COMBAT_ACTION_PROPERTY(IsToggle, istoggle)
	READ_COMBAT_ACTION_PROPERTY(MaxActive, maxactive)
	READ_COMBAT_ACTION_PROPERTY(Facing, facing)
	READ_COMBAT_ACTION_PROPERTY(Angle, angle)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(LifetimeA, lifetime)
	READ_COMBAT_ACTION_PROPERTY(LifetimeB, lifetimeb)
	READ_COMBAT_ACTION_PROPERTY(LifetimeOp, lifetimeop)
	READ_COMBAT_ACTION_PROPERTY(Proxy, proxy)
	READ_COMBAT_ACTION_PROPERTY_EXVALUE(Team, team, XtoA(int(TEAM_INVALID)))
	READ_COMBAT_ACTION_PROPERTY(SnapTargetToGrid, snaptargettogrid)
	READ_COMBAT_ACTION_PROPERTY(TeamShare, teamshare)
	READ_COMBAT_ACTION_PROPERTY(AttachTo, attachto)
	READ_COMBAT_ACTION_PROPERTY_EX(AttachToBoneIndex, attachtoboneindex, 0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnaffector, CCombatActionSpawnAffector)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY_EX(Target, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Direction, direction, target_position)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(FirstTarget, firsttarget, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY(LevelProperty, level)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(IsChannel, ischannel)
	READ_COMBAT_ACTION_PROPERTY(IsToggle, istoggle)
	READ_COMBAT_ACTION_PROPERTY(Distance, distance)
	READ_COMBAT_ACTION_PROPERTY_EX(CountA, count, 1)
	READ_COMBAT_ACTION_PROPERTY(CountB, countb)
	READ_COMBAT_ACTION_PROPERTY(CountOperator, countop)
	READ_COMBAT_ACTION_PROPERTY(Distribute, distribute)
	READ_COMBAT_ACTION_PROPERTY(Proxy, proxy)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(Param, param)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionOrigin, positionorigin, source_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY(PositionModifier, positionmodifier)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(Ignore, ignore)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(InnerRadiusOffset, innerradiusoffset)
	READ_COMBAT_ACTION_PROPERTY(Lifetime, lifetime)
	READ_COMBAT_ACTION_PROPERTY(Speed, speed)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnlinearaffector, CCombatActionSpawnLinearAffector)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY_EX(Target, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Direction, direction, target_position)
	READ_COMBAT_ACTION_PROPERTY_EX(FirstTarget, firsttarget, target_entity)
	READ_COMBAT_ACTION_PROPERTY(LevelProperty, level)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(IsChannel, ischannel)
	READ_COMBAT_ACTION_PROPERTY(IsToggle, istoggle)
	READ_COMBAT_ACTION_PROPERTY(Proxy, proxy)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(Param, param)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(TargetOrigin, targetorigin, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY(TargetValue, targetvalue)
	READ_COMBAT_ACTION_PROPERTY(TargetModifier, targetmodifier)
	READ_COMBAT_ACTION_PROPERTY(TargetOffset, targetoffset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(DirectionOrigin, directionorigin, direction, target_position)
	READ_COMBAT_ACTION_PROPERTY(DirectionValue, directionvalue)
	READ_COMBAT_ACTION_PROPERTY(DirectionModifier, directionmodifier)
	READ_COMBAT_ACTION_PROPERTY(DirectionOffset, directionoffset)
	READ_COMBAT_ACTION_PROPERTY(TargetPositionMid, targetpositionmid)
	READ_COMBAT_ACTION_PROPERTY(DirectionPositionMid, directionpositionmid)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
	READ_COMBAT_ACTION_PROPERTY(MinLength, minlength)
	READ_COMBAT_ACTION_PROPERTY(MaxLength, maxlength)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawncapsuleaffector, CCombatActionSpawnCapsuleAffector)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY_EX(Target, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(SourceUnit, sourceunit, this_owner_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(TargetUnit, targetunit, target_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Direction, direction, target_position)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(FirstTarget, firsttarget, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY(LevelProperty, level)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(IsChannel, ischannel)
	READ_COMBAT_ACTION_PROPERTY(IsToggle, istoggle)
	READ_COMBAT_ACTION_PROPERTY(Distance, distance)
	READ_COMBAT_ACTION_PROPERTY_EX(CountA, count, 1)
	READ_COMBAT_ACTION_PROPERTY(CountB, countb)
	READ_COMBAT_ACTION_PROPERTY(CountOperator, countop)
	READ_COMBAT_ACTION_PROPERTY(Distribute, distribute)
	READ_COMBAT_ACTION_PROPERTY(Proxy, proxy)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(Param, param)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionOrigin, positionorigin, source_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY(PositionModifier, positionmodifier)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(Ignore, ignore)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
	READ_COMBAT_ACTION_PROPERTY(SourceRadius, sourceradius)
	READ_COMBAT_ACTION_PROPERTY(TargetRadius, targetradius)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnprojectile, CCombatActionSpawnProjectile)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY_EX(Source, source, source_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Target, target, target_entity)
	READ_COMBAT_ACTION_PROPERTY(Bind, bind)
	READ_COMBAT_ACTION_PROPERTY(BindTurn, bindturn)
	READ_COMBAT_ACTION_PROPERTY(BindNoPush, bindnopush)
	READ_COMBAT_ACTION_PROPERTY(UnbindOnDeath, unbindondeath)
	READ_COMBAT_ACTION_PROPERTY(IgnoreTargetOffset, ignoretargetoffset)
	READ_COMBAT_ACTION_PROPERTY(BindState, bindstate)
	READ_COMBAT_ACTION_PROPERTY(Proxy, proxy)
	READ_COMBAT_ACTION_PROPERTY(Proxy1, proxy1)
	READ_COMBAT_ACTION_PROPERTY(Proxy2, proxy2)
	READ_COMBAT_ACTION_PROPERTY(Proxy3, proxy3)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, source, source_entity)
	READ_COMBAT_ACTION_PROPERTY(LevelProperty, level)
	READ_COMBAT_ACTION_PROPERTY(IsChannel, ischannel)
	READ_COMBAT_ACTION_PROPERTY(IsToggle, istoggle)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(Param, param)
	READ_COMBAT_ACTION_PROPERTY(NoResponse, noresponse)
	READ_COMBAT_ACTION_PROPERTY(ResponseType, responsetype)
	READ_COMBAT_ACTION_PROPERTY_EX(SuperType, supertype, spell)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(Ignore, ignore)
	READ_COMBAT_ACTION_PROPERTY(IgnoreInvulnerable, ignoreinvulnerable)
	READ_COMBAT_ACTION_PROPERTY(Orbit, orbit)
	READ_COMBAT_ACTION_PROPERTY(OrbitClockwise, orbitclockwise)
	READ_COMBAT_ACTION_PROPERTY(OrbitRadius, orbitradius)
	READ_COMBAT_ACTION_PROPERTY(OrbitSpeed, orbitspeed)
	READ_COMBAT_ACTION_PROPERTY(OrbitAcceleration, orbitacceleration)
	READ_COMBAT_ACTION_PROPERTY(OrbitMinSpeed, orbitmainspeed)
	READ_COMBAT_ACTION_PROPERTY(OrbitMaxSpeed, orbitmaxspeed)
	READ_COMBAT_ACTION_PROPERTY(OrbitMaxTouches, orbitmaxtouches)
	READ_COMBAT_ACTION_PROPERTY(OrbitMaxTouchesPerTarget, orbitmaxtouchespertarget)
	READ_COMBAT_ACTION_PROPERTY(OrbitDieOnMaxTouches, orbitdieonmaxtouches)
	READ_COMBAT_ACTION_PROPERTY(OrbitDieOnMaxTouchesPerTarget, orbitdieonmaxtouchespertarget)
	READ_COMBAT_ACTION_PROPERTY(OrbitStartingTheta, orbitstartingtheta)
	READ_COMBAT_ACTION_PROPERTY(UseLocalTheta, uselocaltheta)
	READ_COMBAT_ACTION_PROPERTY_EX(OrbitStartingThetaTime, orbitstartingthetatime, -1)
	READ_COMBAT_ACTION_PROPERTY(Homing, homing)
	READ_COMBAT_ACTION_PROPERTY(HomingSeekRange, homingseekrange)
	READ_COMBAT_ACTION_PROPERTY(HomingSpeedMul, homingspeedmul)
	READ_COMBAT_ACTION_PROPERTY(HomingUnlockRange, homingunlockrange)
	READ_COMBAT_ACTION_PROPERTY(HomingLockTarget, hominglocktarget)
	READ_COMBAT_ACTION_PROPERTY(HomingSeekStealth, homingseekstealth)
	READ_COMBAT_ACTION_PROPERTY(NonInterrupting, noninterrupting)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getaffectorradius, CCombatActionGetAffectorRadius)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setaffectorradius, CCombatActionSetAffectorRadius)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
READ_COMBAT_ACTION_PROPERTY(Radius, radius)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getaffectorinnerradius, CCombatActionGetAffectorInnerRadius)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setaffectorinnerradius, CCombatActionSetAffectorInnerRadius)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
READ_COMBAT_ACTION_PROPERTY(Radius, radius)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setcapsuleaffectorradiuses, CCombatActionSetCapsuleAffectorRadiuses)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
READ_COMBAT_ACTION_PROPERTY_EX(Target, target, -1)
READ_COMBAT_ACTION_PROPERTY_EX(Source, source, -1)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getcapsuleaffectortargetradius, CCombatActionGetCapsuleAffectorTargetRadius)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getcapsuleaffectorsourceradius, CCombatActionGetCapsuleAffectorSourceRadius)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprojectilebounds, CCombatActionSetProjectileBounds)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY_EX(Height, height, 0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectiledistancetraveled, CCombatActionGetProjectileDistanceTraveled)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectiletheta, CCombatActionGetProjectileTheta)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectileorbitradius, CCombatActionGetProjectileOrbitRadius)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprojectileorbitradius, CCombatActionSetProjectileOrbitRadius)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectileorbitspeed, CCombatActionGetProjectileOrbitSpeed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprojectileorbitspeed, CCombatActionSetProjectileOrbitSpeed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectileorbitminspeed, CCombatActionGetProjectileOrbitMinSpeed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprojectileorbitminspeed, CCombatActionSetProjectileOrbitMinSpeed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectileorbitmaxspeed, CCombatActionGetProjectileOrbitMaxSpeed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprojectileorbitmaxspeed, CCombatActionSetProjectileOrbitMaxSpeed)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getprojectileorbitacceleration, CCombatActionGetProjectileOrbitAcceleration)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setprojectileorbitacceleration, CCombatActionSetProjectileOrbitAcceleration)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushprojectiletarget, CCombatActionPushProjectileTarget)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getyaw, CCombatActionGetYaw)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(attack, CCombatActionAttack)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(FaceTarget, facetarget)
	READ_COMBAT_ACTION_PROPERTY_EX(StartCooldown, startcooldown, true)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startattack, CCombatActionStartAttack)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(FaceTarget, facetarget)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(attackaction, CCombatActionAttackAction)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(bonusdamageadd, CCombatActionBonusDamageAdd)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(bonusdamagemult, CCombatActionBonusDamageMult)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(resetattackcooldown, CCombatActionResetAttackCooldown)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setignoreattackcooldown, CCombatActionSetIgnoreAttackCooldown)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startattackcooldown, CCombatActionStartAttackCooldown)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(DurationA, duration)
	READ_COMBAT_ACTION_PROPERTY(DurationB, durationb)
	READ_COMBAT_ACTION_PROPERTY(DurationOp, durationop)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(order, CCombatActionOrder)
	READ_COMBAT_ACTION_PROPERTY(Command, command)
	READ_COMBAT_ACTION_PROPERTY(Parameter, parameter)
	READ_COMBAT_ACTION_PROPERTY(Queue, queue)
	READ_COMBAT_ACTION_PROPERTY(Force, force)
	READ_COMBAT_ACTION_PROPERTY(ForceDuration, forceduration)
	READ_COMBAT_ACTION_PROPERTY(ForceUseTools, forceusetools)
	READ_COMBAT_ACTION_PROPERTY(OrderName, ordername)
	READ_COMBAT_ACTION_PROPERTY(Value0, value0)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
	READ_COMBAT_ACTION_PROPERTY(Block, block)
	READ_COMBAT_ACTION_PROPERTY(DirectPathing, directpathing)
	READ_COMBAT_ACTION_PROPERTY(IgnoreSight, ignoresight)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(useability, CCombatActionUseAbility)
	READ_COMBAT_ACTION_PROPERTY_EX(Slot, slot, -1)
	READ_COMBAT_ACTION_PROPERTY_EX(Queue, queue, front)
	READ_COMBAT_ACTION_PROPERTY(Force, force)
	READ_COMBAT_ACTION_PROPERTY(ForceDuration, forceduration)
	READ_COMBAT_ACTION_PROPERTY(ForceUseTools, forceusetools)
	READ_COMBAT_ACTION_PROPERTY(OrderName, ordername)
	READ_COMBAT_ACTION_PROPERTY(Value0, value0)
	READ_COMBAT_ACTION_PROPERTY(Block, block)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(useitem, CCombatActionUseItem)
	READ_COMBAT_ACTION_PROPERTY_EX(Queue, queue, front)
	READ_COMBAT_ACTION_PROPERTY(Force, force)
	READ_COMBAT_ACTION_PROPERTY(ForceDuration, forceduration)
	READ_COMBAT_ACTION_PROPERTY(ForceUseTools, forceusetools)
	READ_COMBAT_ACTION_PROPERTY(OrderName, ordername)
	READ_COMBAT_ACTION_PROPERTY(Value0, value0)
	READ_COMBAT_ACTION_PROPERTY(Block, block)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(selectentity, CCombatActionSelectEntity)
	READ_COMBAT_ACTION_PROPERTY(Request, request)
	READ_COMBAT_ACTION_PROPERTY(CenterCamera, centercamera)
	READ_COMBAT_ACTION_PROPERTY(Targets, targets)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(killillusions, CCombatActionKillIllusions)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(killtrees, CCombatActionKillTrees)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(UseAltDeathAnims, usealtdeathanims)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(revivetrees, CCombatActionReviveTrees)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setdeathanim, CCombatActionSetDeathAnim)
	READ_COMBAT_ACTION_PROPERTY(NoAnim, noanim)
	READ_COMBAT_ACTION_PROPERTY(NoCorpse, nocorpse)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(soullink, CCombatActionSoulLink)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(breaksoullink, CCombatActionBreakSoulLink)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(dispel, CCombatActionDispel)
	READ_COMBAT_ACTION_PROPERTY(Type, type)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(takecontrol, CCombatActionTakeControl)
	READ_COMBAT_ACTION_PROPERTY(MaxActive, maxactive)
	READ_COMBAT_ACTION_PROPERTY(InheritModifiers, inheritmodifiers)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(releasecontrol, CCombatActionReleaseControl)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setactivemodifierkey, CCombatActionSetActiveModifierKey)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(delete, CCombatActionDelete)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(kill, CCombatActionKill)
	READ_COMBAT_ACTION_PROPERTY_EX(ExperienceBountyMult, experiencebountymult, 1.0)
	READ_COMBAT_ACTION_PROPERTY_EX(GoldBountyMult, goldbountymult, 1.0)
	READ_COMBAT_ACTION_PROPERTY(NoCorpse, nocorpse)
	READ_COMBAT_ACTION_PROPERTY(NoDeathAnim, nodeathanim)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnillusion, CCombatActionSpawnIllusion)
	READ_COMBAT_ACTION_PROPERTY_EX(Count, count, 1)
	READ_COMBAT_ACTION_PROPERTY(FixedPosition, fixedposition)
	READ_COMBAT_ACTION_PROPERTY(Lifetime, lifetime)
	READ_COMBAT_ACTION_PROPERTY_EX(ReceiveDamageMultiplier, receivedamagemultiplier, 1.0)
	READ_COMBAT_ACTION_PROPERTY_EX(InflictDamageMultiplier, inflictdamagemultiplier, 1.0)
	READ_COMBAT_ACTION_PROPERTY(SpawnEffect, spawneffect)
	READ_COMBAT_ACTION_PROPERTY(DeathEffect, deatheffect)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(Uncontrollable, uncontrollable)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(PlayDeathAnim, playdeathanim)
	READ_COMBAT_ACTION_PROPERTY(InheritActions, inheritactions)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircular, spawncircular)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircularRadius, spawncircularradius)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircularNoShuffle, spawncircularnoshuffle)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircularRotate, spawncircularrotate)
	READ_COMBAT_ACTION_PROPERTY(DontCloneStates, dontclonestates)
	READ_COMBAT_ACTION_PROPERTY(DontCloneInventory, dontcloneinventory)
	READ_COMBAT_ACTION_PROPERTY(NoIllusionEffect, noillusioneffect)	
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnmimic, CCombatActionSpawnMimic)
	READ_COMBAT_ACTION_PROPERTY_EX(Count, count, 1)
	READ_COMBAT_ACTION_PROPERTY(FixedPosition, fixedposition)
	READ_COMBAT_ACTION_PROPERTY(Lifetime, lifetime)
	READ_COMBAT_ACTION_PROPERTY_EX(ReceiveDamageMultiplier, receivedamagemultiplier, 1.0)
	READ_COMBAT_ACTION_PROPERTY_EX(InflictDamageMultiplier, inflictdamagemultiplier, 1.0)
	READ_COMBAT_ACTION_PROPERTY(SpawnEffect, spawneffect)
	READ_COMBAT_ACTION_PROPERTY(DeathEffect, deatheffect)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, source_entity)
	READ_COMBAT_ACTION_PROPERTY(Uncontrollable, uncontrollable)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
	READ_COMBAT_ACTION_PROPERTY(PlayDeathAnim, playdeathanim)
	READ_COMBAT_ACTION_PROPERTY(InheritActions, inheritactions)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircular, spawncircular)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircularRadius, spawncircularradius)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircularNoShuffle, spawncircularnoshuffle)
	READ_COMBAT_ACTION_PROPERTY(SpawnCircularRotate, spawncircularrotate)
	READ_COMBAT_ACTION_PROPERTY(DontCloneStates, dontclonestates)
	READ_COMBAT_ACTION_PROPERTY(DontCloneInventory, dontcloneinventory)
	READ_COMBAT_ACTION_PROPERTY_EX(InheritItemModifier, inherititemmodifier, false)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(inheritactions, CCombatActionInheritActions)
	READ_COMBAT_ACTION_PROPERTY(IgnoreForcedOrders, ignoreforcedorders)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(clearteamtarget, CCombatActionClearTeamTarget)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(refreshabilities, CCombatActionRefreshAbilities)
	READ_COMBAT_ACTION_PROPERTY(Excluded, excluded)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(refreshinventoryitems, CCombatActionRefreshInventoryItems)
	READ_COMBAT_ACTION_PROPERTY(Excluded, excluded)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(negate, CCombatActionNegate)
	READ_COMBAT_ACTION_PROPERTY(ImmunityType, immunitytype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startcooldown, CCombatActionStartCooldown)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(DurationA, duration)
	READ_COMBAT_ACTION_PROPERTY(DurationB, durationb)
	READ_COMBAT_ACTION_PROPERTY(DurationOp, durationop)
	READ_COMBAT_ACTION_PROPERTY(ToolName, toolname)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(resetcooldown, CCombatActionResetCooldown)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(showchannelbar, CCombatActionShowChannelBar)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(DurationA, duration)
	READ_COMBAT_ACTION_PROPERTY(DurationB, durationb)
	READ_COMBAT_ACTION_PROPERTY(DurationOp, durationop)
	READ_COMBAT_ACTION_PROPERTY(ToolName, toolname)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(hidechannelbar, CCombatActionHideChannelBar)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ToolName, toolname)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(morph, CCombatActionMorph)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(disjoint, CCombatActionDisjoint)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(orderdisjoint, CCombatActionOrderDisjoint)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(forgetattacks, CCombatActionForgetAttacks)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(protecteddeath, CCombatActionProtectedDeath)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrespawntimemultiplier, CCombatActionSetRespawnTimeMultiplier)
	READ_COMBAT_ACTION_PROPERTY_EX(Value, value, 1.0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrespawntimebonus, CCombatActionSetRespawnTimeBonus)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrespawntime, CCombatActionSetRespawnTime)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrespawnhealthmultiplier, CCombatActionSetRespawnHealthMultiplier)
	READ_COMBAT_ACTION_PROPERTY_EX(Value, value, 1.0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrespawnmanamultiplier, CCombatActionSetRespawnManaMultiplier)
	READ_COMBAT_ACTION_PROPERTY_EX(Value, value, 1.0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrespawnposition, CCombatActionSetRespawnPosition)
	READ_COMBAT_ACTION_PROPERTY_EX(Position, position, target_position)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgoldlossmultiplier, CCombatActionSetGoldLossMultiplier)
	READ_COMBAT_ACTION_PROPERTY_EX(Value, value, 1.0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgoldlossbonus, CCombatActionSetGoldLossBonus)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(addcharges, CCombatActionAddCharges)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Count, count, 1)
	READ_COMBAT_ACTION_PROPERTY(Timed, timed)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(removecharge, CCombatActionRemoveCharge)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(multcharges, CCombatActionMultCharges)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(Value, value, 1.0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setcharges, CCombatActionSetCharges)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnitem, CCombatActionSpawnItem)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, target, target_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(dropitem, CCombatActionDropItem)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, this_owner_entity)
	READ_COMBAT_ACTION_PROPERTY(ItemName, itemname)
    READ_COMBAT_ACTION_PROPERTY_EX(LoseOwnership, loseownership, true)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(scaledamage, CCombatActionScaleDamage)
	READ_COMBAT_ACTION_PROPERTY(Scale, scale)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changedamage, CCombatActionChangeDamage)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setdamage, CCombatActionSetDamage)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changecurrentcombatdamage, CCombatActionChangeCurrentCombatDamage)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(accumulatedamage, CCombatActionAccumulateDamage)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Scale, scale)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setaccumulator, CCombatActionSetAccumulator)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueA, value)
	READ_COMBAT_ACTION_PROPERTY(ValueB, valueb)
	READ_COMBAT_ACTION_PROPERTY(ValueOp, valueop)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changeaccumulator, CCombatActionChangeAccumulator)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(ValueOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(savehealth, CCombatActionSaveHealth)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(expire, CCombatActionExpire)	
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(absorbdamage, CCombatActionAbsorbDamage)
	READ_COMBAT_ACTION_PROPERTY(Max, max)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(adjuststrength, CCombatActionAdjustStrength)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(adjustagility, CCombatActionAdjustAgility)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(adjustintelligence, CCombatActionAdjustIntelligence)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(retarget, CCombatActionRetarget)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(recallpets, CCombatActionRecallPets)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(levelpets, CCombatActionLevelPets)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(levelupability, CCombatActionLevelUpAbility)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Slot, slot)
	READ_COMBAT_ACTION_PROPERTY(Force, force)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(push, CCombatActionPush)
	READ_COMBAT_ACTION_PROPERTY(Force, force)
	READ_COMBAT_ACTION_PROPERTY(ForceB, forceb)
	READ_COMBAT_ACTION_PROPERTY(ForceOp, forceop)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
	READ_COMBAT_ACTION_PROPERTY(Perpendicular, perpendicular)
	READ_COMBAT_ACTION_PROPERTY(Frame, frame)
	READ_COMBAT_ACTION_PROPERTY(Cliffwalking, cliffwalking)
	READ_COMBAT_ACTION_PROPERTY(Unitwalking, unitwalking)
	READ_COMBAT_ACTION_PROPERTY(Treewalking, treewalking)
	READ_COMBAT_ACTION_PROPERTY(Buildingwalking, buildingwalking)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(defer, CCombatActionDefer)
	READ_COMBAT_ACTION_PROPERTY(Time, time)
	READ_COMBAT_ACTION_PROPERTY(Multiplier, mult)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(evaluate, CCombatActionEvaluate)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushstack, CCombatActionPushStack)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(popstack, CCombatActionPopStack)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(peekstack, CCombatActionPeekStack)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushentity, CCombatActionPushEntity)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY_EX(SearchForTarget, searchfortarget, false)
	READ_COMBAT_ACTION_PROPERTY(SearchOrigin, searchorigin)
	READ_COMBAT_ACTION_PROPERTY(SearchRadius, radius)
	READ_COMBAT_ACTION_PROPERTY(SearchTargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY_EX(SearchIgnoreInvulnerable, ignoreinvulnerable, false)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(popentity, CCombatActionPopEntity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushhero, CCombatActionPushHero)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changeduration, CCombatActionChangeDuration)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changetotalduration, CCombatActionChangeTotalDuration)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setproxy, CCombatActionSetProxy)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY_EX(SearchForTarget, searchfortarget, false)
	READ_COMBAT_ACTION_PROPERTY(SearchOrigin, searchorigin)
	READ_COMBAT_ACTION_PROPERTY(SearchRadius, radius)
	READ_COMBAT_ACTION_PROPERTY(SearchTargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY_EX(SearchIgnoreInvulnerable, ignoreinvulnerable, false)
	READ_COMBAT_ACTION_PROPERTY(Index, index)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(clearproxy, CCombatActionClearProxy)
	READ_COMBAT_ACTION_PROPERTY(Index, index)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(foreachproxy, CCombatActionForEachProxy)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setparam, CCombatActionSetParam)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startfade, CCombatActionStartFade)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushability, CCombatActionPushAbility)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Slot, slot)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushentitybyname, CCombatActionPushEntityByName)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setvar0, CCombatActionSetVar0)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setvar1, CCombatActionSetVar1)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setvar2, CCombatActionSetVar2)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setvar3, CCombatActionSetVar3)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setpos0, CCombatActionSetPos0)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, entity, target_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(PositionEnd, positionend)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionModifier, positionmodifier, start)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(WorldPos, worldpos)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setpos1, CCombatActionSetPos1)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, entity, target_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(PositionEnd, positionend)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionModifier, positionmodifier, start)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(WorldPos, worldpos)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setpos2, CCombatActionSetPos2)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, entity, target_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(PositionEnd, positionend)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionModifier, positionmodifier, start)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(WorldPos, worldpos)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setpos3, CCombatActionSetPos3)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Offset, offset)
	READ_COMBAT_ACTION_PROPERTY_INHERIT(OffsetSpace, offsetspace, entity, target_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionMid, positionmid)
	READ_COMBAT_ACTION_PROPERTY(PositionEnd, positionend)
	READ_COMBAT_ACTION_PROPERTY(PositionValue, positionvalue)
	READ_COMBAT_ACTION_PROPERTY_EX(PositionModifier, positionmodifier, start)
	READ_COMBAT_ACTION_PROPERTY(PositionOffset, positionoffset)
	READ_COMBAT_ACTION_PROPERTY(WorldPos, worldpos)
	READ_COMBAT_ACTION_PROPERTY(Position3D, position3d)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setent0, CCombatActionSetEnt0)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setent1, CCombatActionSetEnt1)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setent2, CCombatActionSetEnt2)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setent3, CCombatActionSetEnt3)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setstr0, CCombatActionSetStr0)
	READ_COMBAT_ACTION_PROPERTY(StrA, a)
	READ_COMBAT_ACTION_PROPERTY(StrB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setstr1, CCombatActionSetStr1)
	READ_COMBAT_ACTION_PROPERTY(StrA, a)
	READ_COMBAT_ACTION_PROPERTY(StrB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setstr2, CCombatActionSetStr2)
	READ_COMBAT_ACTION_PROPERTY(StrA, a)
	READ_COMBAT_ACTION_PROPERTY(StrB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setstr3, CCombatActionSetStr3)
	READ_COMBAT_ACTION_PROPERTY(StrA, a)
	READ_COMBAT_ACTION_PROPERTY(StrB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setattackprojectile, CCombatActionSetAttackProjectile)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setattackactioneffect, CCombatActionSetAttackActionEffect)
	READ_COMBAT_ACTION_PROPERTY(Effect, effect)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setattackselfactioneffect, CCombatActionSetAttackSelfActionEffect)
	READ_COMBAT_ACTION_PROPERTY(Effect, effect)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setattackimpacteffect, CCombatActionSetAttackImpactEffect)
	READ_COMBAT_ACTION_PROPERTY(Effect, effect)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(resettouches, CCombatActionResetTouches)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(toggleoff, CCombatActionToggleOff)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(breakchannel, CCombatActionBreakChannel)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(aggression, CCombatActionAggression)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(playtauntsound, CCombatActionPlayTauntSound)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(broadcastmessage, CCombatActionBroadcastMessage)
	READ_COMBAT_ACTION_PROPERTY(Type, type)
	READ_COMBAT_ACTION_PROPERTY(TargetType, targettype)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setvalue, CCombatActionSetValue)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(seteffecttype, CCombatActionSetEffectType)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(hasmodifier, CCombatActionHasModifier)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(hasavatarkey, CCombatActionHasAvatarKey)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(comparetiledefinition, CCombatActionCompareTileDefinition)
	READ_COMBAT_ACTION_PROPERTY_EX(Position, position, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(areaofeffect, CCombatActionAreaOfEffect)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(IgnoreInvulnerable, ignoreinvulnerable)
	READ_COMBAT_ACTION_PROPERTY_EX(Center, center, target_position)
	READ_COMBAT_ACTION_PROPERTY(TargetSelection, targetselection)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(InnerRadiusOffset, innerradiusoffset)
	READ_COMBAT_ACTION_PROPERTY(MaxTotalImpacts, maxtotalimpacts)
	READ_COMBAT_ACTION_PROPERTY_EX(MaxImpactsPerTarget, maximpactspertarget, 1)
	READ_COMBAT_ACTION_PROPERTY(FirstTarget, firsttarget)
	READ_COMBAT_ACTION_PROPERTY(Ignore, ignore)
	READ_COMBAT_ACTION_PROPERTY(Global, global)
	READ_COMBAT_ACTION_PROPERTY(IncludeTrees, includetrees)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(calculatedamage, CCombatActionCalculateDamage)
	READ_COMBAT_ACTION_PROPERTY(Amount, amount)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(SuperType, supertype)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(transferitemstohero, CCombatActionTransferItemsToHero)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setitemsasoncourier, CCombatActionSetItemsAsOnCourier)
	READ_COMBAT_ACTION_PROPERTY(TargetInventoryRegions, targetregions)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(ordercourieritemtransfer, CCombatActionOrderCourierItemTransfer)
	READ_COMBAT_ACTION_PROPERTY(TargetInventoryRegion, targetregion)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(countslaves, CCombatActionCountSlaves)
READ_COMBAT_ACTION_PROPERTY(TargetInventoryRegions, targetregions)
READ_COMBAT_ACTION_PROPERTY(SlaveNames, slavenames)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushissuinghero, CCombatActionPushIssuingHero)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushentitysearch, CCombatActionPushEntitySearch)
	READ_COMBAT_ACTION_PROPERTY_EX(Origin, origin, source_position)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY_EX(IgnoreInvulnerable, ignoreinvulnerable, false)
	READ_COMBAT_ACTION_PROPERTY(Global, global)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(starttimer, CCombatActionStartTimer)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(DurationA, duration)
	READ_COMBAT_ACTION_PROPERTY(DurationB, durationb)
	READ_COMBAT_ACTION_PROPERTY(DurationOp, durationop)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pausetimer, CCombatActionPauseTimer)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Paused, paused)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(resettimer, CCombatActionResetTimer)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changetimer, CCombatActionChangeTimer)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(ValueOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(invalidate, CCombatActionInvalidate)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startexpire, CCombatActionStartExpire)	
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pushentityproxy, CCombatActionPushEntityProxy)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Index, index)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(complete, CCombatActionComplete)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(cancel, CCombatActionCancel)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(distance, CCombatActionDistance)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(addcritical, CCombatActionAddCritical)
	READ_COMBAT_ACTION_PROPERTY_EX(Chance, chance, 1.0)
	READ_COMBAT_ACTION_PROPERTY_EX(Multiplier, multiplier, 1.0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(clearcriticals, CCombatActionClearCriticals)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(clonebackpack, CCombatActionCloneBackpack)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(lockbackpack, CCombatActionLockBackpack)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(CanUseItems, canuseitems)
	READ_COMBAT_ACTION_PROPERTY(CanPurchase, canpurchase)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(unlockbackpack, CCombatActionUnlockBackpack)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(reducecooldown, CCombatActionReduceCooldown)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getcooldown, CCombatActionGetCooldown)
	READ_COMBAT_ACTION_PROPERTY(Item, item)
	READ_COMBAT_ACTION_PROPERTY(Ability, ability)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getremainingcooldown, CCombatActionGetRemainingCooldown)
	READ_COMBAT_ACTION_PROPERTY(Item, item)
	READ_COMBAT_ACTION_PROPERTY(Ability, ability)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(activatemodifierkey, CCombatActionActivateModifierKey)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(deactivatemodifierkey, CCombatActionDeactivateModifierKey)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setselectedupgrademodifierkey, CCombatActionSetSelectedUpgradeModifierKey)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Type, type)
	READ_COMBAT_ACTION_PROPERTY(IsActive, isactive)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(foreachplayer, CCombatActionForEachPlayer)
	READ_COMBAT_ACTION_PROPERTY(IncludeSpectators, includespectators)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setteamsize, CCombatActionSetTeamSize)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
	READ_COMBAT_ACTION_PROPERTY(Size, size)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changeteam, CCombatActionChangeTeam)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
	READ_COMBAT_ACTION_PROPERTY_EX(Slot, slot, -1)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setinterface, CCombatActionSetInterface)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setoverlayinterface, CCombatActionSetOverlayInterface)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnthread, CCombatActionSpawnThread)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(while, CCombatActionWhile)
	READ_COMBAT_ACTION_PROPERTY(Test, test)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(loop, CCombatActionLoop)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(whilescriptcondition, CCombatActionWhileScriptCondition)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(wait, CCombatActionWait)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(yield, CCombatActionYield)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(terminate, CCombatActionTerminate)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(waituntilcompare, CCombatActionWaitUntilCompare)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(waituntilcondition, CCombatActionWaitUntilCondition)
	READ_COMBAT_ACTION_PROPERTY(Test, test)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(waituntilmessage, CCombatActionWaitUntilMessage)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setscriptvalue, CCombatActionSetScriptValue)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(createcamera, CCombatActionCreateCamera)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(PositionEntity, positionentity)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(createcamerafromplayer, CCombatActionCreateCameraFromPlayer)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY_EX(AutoAssign, autoassign, true)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setcamera, CCombatActionSetCamera)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Camera, camera)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(movecamera, CCombatActionMoveCamera)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(PositionEntity, positionentity)
	READ_COMBAT_ACTION_PROPERTY(PositionName, positionname)
	READ_COMBAT_ACTION_PROPERTY(Position, position)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
	READ_COMBAT_ACTION_PROPERTY(Smooth, smooth)
	READ_COMBAT_ACTION_PROPERTY(Interpolate, interpolate)
	READ_COMBAT_ACTION_PROPERTY(Block, block)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(zoomcamera, CCombatActionZoomCamera)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
	READ_COMBAT_ACTION_PROPERTY(Height, height)
	READ_COMBAT_ACTION_PROPERTY(Smooth, smooth)
	READ_COMBAT_ACTION_PROPERTY(Interpolate, interpolate)
	READ_COMBAT_ACTION_PROPERTY(Block, block)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(rotatecamera, CCombatActionRotateCamera)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Angles, angles)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
	READ_COMBAT_ACTION_PROPERTY(Interpolate, interpolate)
	READ_COMBAT_ACTION_PROPERTY(Block, block)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawn, CCombatActionSpawn)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnclientthread, CCombatActionSpawnClientThread)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(clientuitrigger, CCombatActionClientUITrigger)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Param, param)
	READ_COMBAT_ACTION_PROPERTY(ParamValue, paramvalue)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setowner, CCombatActionSetOwner)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Player, player)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setownerunit, CCombatActionSetOwnerUnit)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(transferallownership, CCombatActionTransferAllOwnership)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
	READ_COMBAT_ACTION_PROPERTY(Excluded, excluded)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(sethero, CCombatActionSetHero)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Hero, hero)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setlevel, CCombatActionSetLevel)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setexperience, CCombatActionSetExperience)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgold, CCombatActionSetGold)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setplayershopmode, CCombatActionSetPlayerShopMode)
READ_COMBAT_ACTION_PROPERTY(Mode, mode)
READ_COMBAT_ACTION_PROPERTY_EX(HasItems, hasitems, false)
READ_COMBAT_ACTION_PROPERTY_EX(ClearPreviousItems, clearpreviousitems, true)
READ_COMBAT_ACTION_PROPERTY_EX(Items, items, TSNULL)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(changeplayershopmodeitems, CCombatActionChangePlayerShopModeItems)
READ_COMBAT_ACTION_PROPERTY_EX(IsRemove, isremove, true)
READ_COMBAT_ACTION_PROPERTY(Items, items)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(openshop, CCombatActionOpenShop)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(closeshop, CCombatActionCloseShop)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(lockshop, CCombatActionLockShop)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(unlockshop, CCombatActionUnlockShop)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setactiveshop, CCombatActionSetActiveShop)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setactiverecipe, CCombatActionSetActiveRecipe)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(scriptcondition, CCombatActionScriptCondition)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Owner, owner)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getscriptvalue, CCombatActionGetScriptValue)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Entity, entity)
	READ_COMBAT_ACTION_PROPERTY(DestVar, destvar)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgamephase, CCombatActionSetGamePhase)
	READ_COMBAT_ACTION_PROPERTY(Phase, phase)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startmatch, CCombatActionStartMatch)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnneutrals, CCombatActionSpawnNeutrals)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(giveitem, CCombatActionGiveItem)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
	READ_COMBAT_ACTION_PROPERTY(Recipe, recipe)
	READ_COMBAT_ACTION_PROPERTY(Stash, stash)
	READ_COMBAT_ACTION_PROPERTY(PushEntity, pushentity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(foreachitem, CCombatActionForEachItem)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(foreachstate, CCombatActionForEachState)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(IgnoreEffectType, ignoreeffecttype)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(lockitem, CCombatActionLockItem)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(unlockitem, CCombatActionUnlockItem)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(lockcontrol, CCombatActionLockControl)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(unlockcontrol, CCombatActionUnlockControl)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setaicontroller, CCombatActionSetAIController)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Controller, controller)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(entitytype, CCombatActionEntityType)
	READ_COMBAT_ACTION_PROPERTY(Type, type)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(targettype, CCombatActionTargetType)
	READ_COMBAT_ACTION_PROPERTY(Type, type)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(forceimpact, CCombatActionForceImpact)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Immediate, immediate)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pointonpath0, CCombatActionPointOnPath0)
	READ_COMBAT_ACTION_PROPERTY(Width, width)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
	READ_COMBAT_ACTION_PROPERTY(IgnoreUnits, ignoreunits)
	READ_COMBAT_ACTION_PROPERTY(IgnoreTrees, ignoretrees)
	READ_COMBAT_ACTION_PROPERTY(IgnoreCliffs, ignorecliffs)
	READ_COMBAT_ACTION_PROPERTY(IgnoreBuildings, ignorebuildings)
	READ_COMBAT_ACTION_PROPERTY(Distance, distance)
	READ_COMBAT_ACTION_PROPERTY(EndDistance, enddistance)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(lineofeffect, CCombatActionLineOfEffect)
	READ_COMBAT_ACTION_PROPERTY_EX(Start, start, source_position)
	READ_COMBAT_ACTION_PROPERTY_EX(End, end, target_position)
	READ_COMBAT_ACTION_PROPERTY(TargetScheme, targetscheme)
	READ_COMBAT_ACTION_PROPERTY(EffectType, effecttype)
	READ_COMBAT_ACTION_PROPERTY(IgnoreInvulnerable, ignoreinvulnerable)
	READ_COMBAT_ACTION_PROPERTY(TargetSelection, targetselection)
	READ_COMBAT_ACTION_PROPERTY(Radius, radius)
	READ_COMBAT_ACTION_PROPERTY(MaxTotalImpacts, maxtotalimpacts)
	READ_COMBAT_ACTION_PROPERTY_EX(MaxImpactsPerTarget, maximpactspertarget, 1)
	READ_COMBAT_ACTION_PROPERTY(Ignore, ignore)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(hasmodel, CCombatActionHasModel)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(Model, model)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isitem, CCombatActionIsItem)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(isability, CCombatActionIsAbility)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(issplitprojectile, CCombatActionIsSplitProjectile)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(adjustplayerscore, CCombatActionAdjustPlayerScore)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setplayerscore, CCombatActionSetPlayerScore)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getplayerscore, CCombatActionGetPlayerScore)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setplayerscoregoal, CCombatActionSetPlayerScoreGoal)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(DontEndMatch, dontendmatch)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(adjustteamscore, CCombatActionAdjustTeamScore)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setteamscore, CCombatActionSetTeamScore)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getteamscore, CCombatActionGetTeamScore)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setteamscoregoal, CCombatActionSetTeamScoreGoal)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(DontEndMatch, dontendmatch)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(random, CCombatActionRandom)
	READ_COMBAT_ACTION_PROPERTY(Min, min)
	READ_COMBAT_ACTION_PROPERTY(Max, max)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(randomfloat, CCombatActionRandomFloat)
	READ_COMBAT_ACTION_PROPERTY(Min, min)
	READ_COMBAT_ACTION_PROPERTY(Max, max)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(ownsproduct, CCombatActionOwnsProduct)
	READ_COMBAT_ACTION_PROPERTY(Product, product)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(spawnbot, CCombatActionSpawnBot)
	READ_COMBAT_ACTION_PROPERTY(PlayerName, playername)
	READ_COMBAT_ACTION_PROPERTY(DefinitionName, definitionname)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(comparecvar, CCombatActionCompareCvar)
	READ_COMBAT_ACTION_PROPERTY(Cvar, cvar)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_BRANCH_XML_PROCESSOR(setcvar, CCombatActionSetCvar)
READ_COMBAT_ACTION_PROPERTY(Cvar, cvar)
READ_COMBAT_ACTION_PROPERTY(Value, value)
READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_BRANCH_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(shuffleabilities, CCombatActionShuffleAbilities)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(preventrespawn, CCombatActionPreventRespawn)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(replaceinselectionsets, CCombatActionReplaceInSelections)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(endmatch, CCombatActionEndMatch)
	READ_COMBAT_ACTION_PROPERTY(LosingTeam, losingteam)
	READ_COMBAT_ACTION_PROPERTY(DontKillBase, dontkillbase)
	READ_COMBAT_ACTION_PROPERTY(SkipToEndMatch, skiptoendmatch)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(giveability, CCombatActionGiveAbility)
	READ_COMBAT_ACTION_PROPERTY(Name, name)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgamehaspostmatch, CCombatActionSetGameHasPostMatch)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setrange, CCombatActionSetRange)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
	READ_COMBAT_ACTION_PROPERTY(ValueA, value)
	READ_COMBAT_ACTION_PROPERTY(ValueB, valueb)
	READ_COMBAT_ACTION_PROPERTY(ValueOp, valueop)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getrange, CCombatActionGetRange)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(resetrange, CCombatActionResetRange)
	READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setachievementvalue, CCombatActionSetAchievementValue)
	READ_COMBAT_ACTION_PROPERTY(Trigger, trigger)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
	READ_COMBAT_ACTION_PROPERTY(Parameter, parameter)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(startround, CCombatActionStartRound)
	READ_COMBAT_ACTION_PROPERTY(RoundLength, roundlength)
	READ_COMBAT_ACTION_PROPERTY(HasPreRound, haspreround)
	READ_COMBAT_ACTION_PROPERTY(PreRoundLength, preroundlength)
	READ_COMBAT_ACTION_PROPERTY(HasPostRound, haspostround)
	READ_COMBAT_ACTION_PROPERTY(PostRoundLength, postroundlength)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(advanceround, CCombatActionAdvanceRound)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pausevoiceovers, CCombatActionPauseVoiceOvers)
READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(delayannouncer, CCombatActionDelayAnnouncer)
READ_COMBAT_ACTION_PROPERTY(Duration, duration)
READ_COMBAT_ACTION_PROPERTY(SourceOnly, sourceonly)
READ_COMBAT_ACTION_PROPERTY(TeamOnly, teamonly)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(play2dsound, CCombatActionPlaySound2D)
	READ_COMBAT_ACTION_PROPERTY(Sample, sample)
	READ_COMBAT_ACTION_PROPERTY_EX(Channel,channel, -1)
	READ_COMBAT_ACTION_PROPERTY_EX(Volume, volume, 1.0f)
	READ_COMBAT_ACTION_PROPERTY_EX(Dampen, dampen, 1.0f)
	READ_COMBAT_ACTION_PROPERTY(LabelID, waitingfor)
	READ_COMBAT_ACTION_PROPERTY_EX(SourceOnly, sourceonly, false)
	READ_COMBAT_ACTION_PROPERTY_EX(TargetOnly, targetonly, false)
	READ_COMBAT_ACTION_PROPERTY_EX(Localize, localize, false)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(transferitem, CCombatActionTransferItem)
	READ_COMBAT_ACTION_PROPERTY_EX(Slot, slot, -1)
	READ_COMBAT_ACTION_PROPERTY_EX(DropIfFull, dropiffull, false)
	READ_COMBAT_ACTION_PROPERTY(OwnerEntity, ownerentity)
	READ_COMBAT_ACTION_PROPERTY(ItemEntity, item)
	READ_COMBAT_ACTION_PROPERTY_EX(StripOwner, stripowner, false)
	READ_COMBAT_ACTION_PROPERTY(Unsellable, unsellable)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settimer0expire, CCombatActionSetTimer0Expire)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settimer1expire, CCombatActionSetTimer1Expire)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settimer2expire, CCombatActionSetTimer2Expire)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settimer3expire, CCombatActionSetTimer3Expire)
	READ_COMBAT_ACTION_PROPERTY(Duration, duration)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setglobalvar0, CCombatActionSetGlobalVar0)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setglobalvar1, CCombatActionSetGlobalVar1)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setglobalvar2, CCombatActionSetGlobalVar2)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setglobalvar3, CCombatActionSetGlobalVar3)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setchannelvolume, CCombatActionSetAudioChannelVolume)
	READ_COMBAT_ACTION_PROPERTY(ChannelID,channelid)
	READ_COMBAT_ACTION_PROPERTY_EX(Volume, volume, 1.0f)
	READ_COMBAT_ACTION_PROPERTY_EX(Fade, fade, 0)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgameplaystat, CCombatActionSetGameplayStat)
	READ_COMBAT_ACTION_PROPERTY(Index, index)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(transmitgameplaystat, CCombatActionTransmitGameplayStat)
	READ_COMBAT_ACTION_PROPERTY(Index, index)
	READ_COMBAT_ACTION_PROPERTY(Transmit, transmit)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setgameplaystate, CCombatActionSetGameplayState)
	READ_COMBAT_ACTION_PROPERTY(Index, index)
	READ_COMBAT_ACTION_PROPERTY(ValueA, a)
	READ_COMBAT_ACTION_PROPERTY(ValueB, b)
	READ_COMBAT_ACTION_PROPERTY(Operator, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(markheroattack, CCombatActionMarkHeroAttack)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// Begin Subscriber combat action events

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(subscribeunitdeath, CCombatActionSubscribeUnitDeath)
READ_COMBAT_ACTION_PROPERTY_EX(Subscribe, subscribe, true)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(subscribeunitkill, CCombatActionSubscribeUnitKill)
READ_COMBAT_ACTION_PROPERTY_EX(Subscribe, subscribe, true)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(subscribeitemacquisition, CCombatActionSubscribeItemAcquisition)
READ_COMBAT_ACTION_PROPERTY_EX(Subscribe, subscribe, true)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// End Subscriber combat action events

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getlanestart0, CCombatActionGetLaneStart0)
	READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, this_owner_entity)
	READ_COMBAT_ACTION_PROPERTY(Lane, lane)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(assigntolane, CCombatActionAssignToLane)
READ_COMBAT_ACTION_PROPERTY_EX(Owner, owner, this_owner_entity)
READ_COMBAT_ACTION_PROPERTY(Lane, lane)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pauselanefollowing, CCombatActionPauseLaneFollowing)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(resumelanefollowing, CCombatActionResumeLaneFollowing)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(couriermessage, CCombatActionCourierMessage)
READ_COMBAT_ACTION_PROPERTY(Message, message)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getplayercursorterrainposition, CCombatActionGetPlayerCursorTerrainPosition)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(morphabilityslot, CCombatActionMorphAbilitySlot)
READ_COMBAT_ACTION_PROPERTY(SourceSlot, sourceslot)
READ_COMBAT_ACTION_PROPERTY(TargetSlot, targetslot)
READ_COMBAT_ACTION_PROPERTY(State, state)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getlevel, CCombatActionGetLevel)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(transfereffects, CCombatActionTransferEffects)
READ_COMBAT_ACTION_PROPERTY_EX(Transfer, transfer, true)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(drawdebugline, CCombatActionDrawDebugLine)
READ_COMBAT_ACTION_PROPERTY(Start, start)
READ_COMBAT_ACTION_PROPERTY(End, end)
READ_COMBAT_ACTION_PROPERTY_EX(Arrow, arrow, false)
READ_COMBAT_ACTION_PROPERTY_EX(Color, color, blue)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(drawdebugx, CCombatActionDrawDebugX)
READ_COMBAT_ACTION_PROPERTY(Pos, pos)
READ_COMBAT_ACTION_PROPERTY_EX(Color, color, blue)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setstealvalue, CCombatActionSetStealValue)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settooltipnumparam, CCombatActionSetTooltipNumParam)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
READ_COMBAT_ACTION_PROPERTY_EX(Param, param, 1)
READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setuseportraiticon, CCombatActionSetUsePortraitIcon)
READ_COMBAT_ACTION_PROPERTY(UsePortraitIcon, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(updatemana, CCombatActionUpdateMana)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(updatehealth, CCombatActionUpdateHealth)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(validpath, CCombatActionValidPath)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
READ_COMBAT_ACTION_PROPERTY_EX(Source, source, source_position)
READ_COMBAT_ACTION_PROPERTY_EX(Target, target, target_position)
READ_COMBAT_ACTION_PROPERTY_EX(Srcestimate, srcestimate, false)
READ_COMBAT_ACTION_PROPERTY_EX(Unitwalking, unitwalking, false)
READ_COMBAT_ACTION_PROPERTY_EX(Treewalking, treewalking, false)
READ_COMBAT_ACTION_PROPERTY_EX(Cliffwalking, cliffwalking, false)
READ_COMBAT_ACTION_PROPERTY_EX(Buildingwalking, buildingwalking, false)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(toggledaynight, CCombatActionToggleDayNight)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getheroxpm, CCombatActionGetHeroXPM)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getheroexp, CCombatActionGetHeroEXP)
READ_COMBAT_ACTION_PROPERTY_EX(Entity, entity, this_entity)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getaveragegamexpm, CCombatActionGetAverageGameXPM)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getaveragegameexp, CCombatActionGetAverageGameEXP)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getaverageteamxpm, CCombatActionGetAverageTeamXPM)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getaverageteamexp, CCombatActionGetAverageTeamEXP)
	READ_COMBAT_ACTION_PROPERTY(Team, team)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(gettimeofdawn, CCombatActionGetTimeOfDawn)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(settimeofdawn, CCombatActionSetTimeOfDawn)
	READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(getlengthofday, CCombatActionGetLengthOfDay)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(setlengthofday, CCombatActionSetLengthOfDay)
READ_COMBAT_ACTION_PROPERTY(Value, value)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(pausedaynightcycle, CCombatActionPauseDayNightCycle)
	READ_COMBAT_ACTION_PROPERTY_EX(Pause, pause, false)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

// 
BEGIN_COMBAT_ACTION_LEAF_XML_PROCESSOR(sethealthaccumulator, CCombatActionSetHealthAccumulator)
READ_COMBAT_ACTION_PROPERTY(ValueA, a)
READ_COMBAT_ACTION_PROPERTY(ValueB, b)
READ_COMBAT_ACTION_PROPERTY(ValueOp, op)
END_COMBAT_ACTION_LEAF_XML_PROCESSOR

//=============================================================================
`

Edited October 15, 2020 by ElementUser

### Link to post

### Share on other sites

ElementUser

793

Posted October 15, 2020

ElementUser

Staff

793

2899 posts

Author

Share

Posted October 15, 2020


---

## ส่วนที่ 4: PK Engine / Custom Combat Actions & Scripting Extensions

> **จัดทำและเรียบเรียง:** สำหรับ (Project KONGOR)  
> **โมดูลเป้าหมาย:** `game_shared_x64.dll` (Section Extension)  
> **วันที่บันทึก:** ตุลาคม 2026

เอกสารส่วนนี้บันทึก Action Processors และส่วนขยายระบบ XML Scripting ใหม่ที่ถูกสร้างขึ้นในระดับ Native Engine (C++ / PE Section Injection) เพื่อรองรับเมคานิกขั้นสูง เช่น การขโมยสกิล (Spell Steal), การสลับช่องสกิลแบบไดนามิก, การควบคุมคูลดาวน์รายช่อง, และการลดคูลดาวน์เฉพาะสกิลที่ขโมยมา

---

### 1. ความเป็นมาและหน้าที่ของ Combat Action ใน K2 Engine

ใน K2 Engine (Heroes of Newerth) แท็กที่อยู่ภายใน Event Handlers ต่างๆ เช่น `<onimpact>`, `<onaction>`, `<onframe>`, `<onlearn>`, หรือ `<onexpired>` ทั้งหมดจะถูกประมวลผลผ่านระบบที่เรียกว่า **Combat Actions** 

เมื่อ Engine โหลดไฟล์ XML (`*.entity`) แท็กแต่ละตัวจะถูกจับคู่กับ **Action Processor** ใน C++ เพื่อสร้าง Object ของ Action บรรจุลงใน `CCombatActionScript` เมื่อ Event นั้นๆ ถูกกระตุ้นในเกม ฟังก์ชัน `Execute()` ของ Action จะทำงานตามลำดับ

เดิมที HoN ไม่มี Action สำหรับการสลับหรือขโมยสกิลข้ามฮีโร่แบบเรียลไทม์ (มักใช้วิธี Morph ทั้งตัวฮีโร่ หรือสลับผ่าน Sub-slot ที่ฮาร์ดโค้ดไว้ล่วงหน้า) เราจึงได้เพิ่ม Action Processors ชุดใหม่เข้าไปยัง C++ Engine โดยตรง

---

### 2. รายการ Action Processors ใหม่ใน PK Engine

```csharp
//=============================================================================
// PK Engine Custom XML Processors (Native Injection)
//=============================================================================
DECLARE_XML_PROCESSOR(copyslot)       // ขโมย/คัดลอกสกิลข้ามฮีโร่ (Spell Steal)
DECLARE_XML_PROCESSOR(stealability)   // Alias ของ copyslot
DECLARE_XML_PROCESSOR(setability)     // กำหนด/ใส่สกิลลงช่อง Slot เจาะจง
DECLARE_XML_PROCESSOR(clearability)   // ล้าง/ถอดสกิลออกจากช่อง Slot อย่างปลอดภัย
DECLARE_XML_PROCESSOR(swapability)    // สลับสกิลและคูลดาวน์ระหว่าง 2 ช่อง Slot
DECLARE_XML_PROCESSOR(setcooldown)    // สั่งเริ่มนับคูลดาวน์เจาะจงราย Slot
DECLARE_XML_PROCESSOR(resetcooldown)  // รีเซ็ตคูลดาวน์เจาะจงราย Slot (ขยายจากเดิม)
```

---

### 3. รายละเอียดและไวยากรณ์การใช้งาน (Syntax & Attributes)

#### 3.1 `<copyslot>` หรือ `<stealability>`
**หน้าที่:** ดึงข้อมูลสกิลจากเป้าหมาย (ศัตรู) มาใส่ในช่อง Inventory Slot ของผู้ร่าย โดยจำลองเมคานิก Rubick Spell Steal ของ Dota 2 อย่างสมบูรณ์

**Attributes ที่รองรับ:**
- `slot` *(integer, ค่าเริ่มต้น: 5)*: ช่อง Slot ปลายทางของผู้ขโมย (ช่อง D = 5, ช่อง F = 6)
- `targetslot` *(integer, ค่าเริ่มต้น: 0)*: ช่อง Slot ของศัตรูที่ต้องการขโมย (0=Q, 1=W, 2=E, 3=R)
- `source` *(target scheme, ค่าเริ่มต้น: source_entity)*: ยูนิตที่เป็นผู้ขโมยสกิล (ฮีโร่เรา)
- `target` *(target scheme, ค่าเริ่มต้น: target_entity)*: ยูนิตเป้าหมายที่ถูกขโมย (ฮีโร่ศัตรู)
- `level` *(integer, ค่าเริ่มต้น: 0)*: เลเวลสกิลที่ต้องการกำหนด (หากเว้นว่างหรือใส่ 0 ระบบจะดึงระดับเลเวลที่ศัตรูอัปไว้จริงมาใส่ให้อัตโนมัติ)
- `sotm` *(boolean 0/1, ค่าเริ่มต้น: 0)*: ทำงานในโหมด Staff of the Master หรือไม่ (1 = อนุญาตให้ถือสกิลขโมยได้ 2 สกิลพร้อมกัน)

**ฟังก์ชันการทำงานอัจฉริยะในระดับ C++:**
1. **Re-steal Same Spell (ขโมยตัวเดิมซ้ำใน Slot 5):** หากขโมยสกิลเดิมซ้ำ ระบบจะไม่รีเซ็ตสกิลใหม่จนทำให้คูลดาวน์หาย แต่จะรีเฟรชระยะเวลาบัฟ (240 วินาที) และหากศัตรูเพิ่งอัปเลเวลสกิลเพิ่ม ระบบจะ Level Up สกิลของเราให้เท่ากับศัตรูทันที
2. **Re-steal Slot 6 Spell (ขโมยสกิลที่กำลังอยู่ใน Slot 6 ซ้ำ):** สลับสกิลจาก Slot 6 กลับมา Slot 5 และย้ายสกิลใน Slot 5 ไป Slot 6 พร้อมสลับเวลาคูลดาวน์ที่กำลังนับอยู่ และรีเฟรชระยะเวลาของสกิลที่ขโมยซ้ำเป็น 240 วินาที
3. **New Spell Shifting (ขโมยสกิลใหม่เมื่อมี SOTM):** ย้ายสกิลเก่าใน Slot 5 ไปยัง Slot 6 (คงเวลาคูลดาวน์และระยะเวลาบัฟที่เหลืออยู่) ล้างสกิลเดิมใน Slot 6 ออก และนำสกิลใหม่ใส่เข้า Slot 5
4. **Cooldown Preservation:** เมื่อสกิลถูกสลับหรือเลื่อนช่อง เวลาคูลดาวน์ที่เหลืออยู่จะถูกโอนย้ายตามตัวสกิลอย่างต่อเนื่อง ไม่มีการโกงรีเซ็ตคูลดาวน์ฟรี

**ตัวอย่างการใช้งานใน XML:**
```xml
<!-- ขโมยสกิลล่าสุดจากศัตรู (ที่บันทึกไว้ใน State Tracker) มาใส่ Slot 5 แบบมี SOTM -->
<condition target="target_entity" test="hasstate State_Kenisis_LastCast_R">
    <copyslot slot="5" targetslot="3" source="source_entity" target="target_entity" sotm="1" />
</condition>
<else>
    <condition target="target_entity" test="hasstate State_Kenisis_LastCast_Q">
        <copyslot slot="5" targetslot="0" source="source_entity" target="target_entity" sotm="1" />
    </condition>
</else>
```

---

#### 3.2 `<setability>`
**หน้าที่:** มอบหมายสกิลใหม่ลงในช่อง Inventory Slot ที่ระบุโดยตรง เหมาะสำหรับการสลับโหมด, การให้ Sub-ability, หรือการเปลี่ยนสกิลชั่วคราว

**Attributes ที่รองรับ:**
- `name` *(string, บังคับ)*: ชื่อ Entity ของสกิล (เช่น `"Ability_Kenisis1"`, `"Ability_Devourer1"`)
- `slot` *(integer, ค่าเริ่มต้น: 5)*: ช่อง Inventory Slot (0-3: สกิลหลัก, 5-6: สกิลเสริม)
- `target` *(target scheme, ค่าเริ่มต้น: target_entity)*: ยูนิตที่จะได้รับสกิล (`source_entity`, `target_entity`, `this_owner_entity`)
- `level` *(integer, ค่าเริ่มต้น: 1)*: เลเวลเริ่มต้นของสกิลที่มอบให้

**ตัวอย่างการใช้งานใน XML:**
```xml
<!-- ใส่สกิลระเบิดหินลงในช่อง Slot 5 ของผู้ร่ายที่เลเวล 3 -->
<setability name="Ability_Kenisis_RockExplode" slot="5" target="source_entity" level="3" />
```

---

#### 3.3 `<clearability>`
**หน้าที่:** ถอดสกิลออกจากช่อง Slot 

**Attributes ที่รองรับ:**
- `slot` *(integer, ค่าเริ่มต้น: 5)*: ช่อง Slot ที่ต้องการล้างออก
- `target` *(target scheme, ค่าเริ่มต้น: target_entity)*: ยูนิตเป้าหมาย

**ตัวอย่างการใช้งานใน XML:**
```xml
<!-- ล้างสกิลใน Slot 5 และ Slot 6 เมื่อบัฟขโมยหมดเวลา -->
<clearability slot="5" target="this_owner_entity" />
<clearability slot="6" target="this_owner_entity" />
```

---

#### 3.4 `<swapability>`
**หน้าที่:** สลับสกิลระหว่างสองช่อง Slot ทันที พร้อมสลับสถานะคูลดาวน์ของทั้งสองช่องตามตำแหน่งที่ย้าย

**Attributes ที่รองรับ:**
- `slot_a` *(integer, ค่าเริ่มต้น: 5)*: ช่องแรก
- `slot_b` *(integer, ค่าเริ่มต้น: 6)*: ช่องที่สอง
- `target` *(target scheme, ค่าเริ่มต้น: target_entity)*: ยูนิตเป้าหมาย

**ตัวอย่างการใช้งานใน XML:**
```xml
<!-- สลับตำแหน่งสกิลขโมยระหว่างช่อง D (Slot 5) และ F (Slot 6) -->
<swapability slot_a="5" slot_b="6" target="source_entity" />
```

---

#### 3.5 `<setcooldown>`
**หน้าที่:** บังคับเริ่มนับเวลาคูลดาวน์บนช่อง Slot ที่ระบุโดยตรง (เป็นมิลลิวินาที ms)

**Attributes ที่รองรับ:**
- `slot` *(integer, ค่าเริ่มต้น: 5)*: ช่อง Slot ที่ต้องการเริ่มคูลดาวน์
- `duration` *(integer ms, ค่าเริ่มต้น: 0)*: ระยะเวลาคูลดาวน์ (เช่น 15000 = 15 วินาที)
- `target` *(target scheme, ค่าเริ่มต้น: target_entity)*: ยูนิตเป้าหมาย

**ตัวอย่างการใช้งานใน XML:**
```xml
<!-- สั่งคูลดาวน์ช่อง Slot 6 เป็นเวลา 12 วินาที -->
<setcooldown slot="6" duration="12000" target="source_entity" />
```

---

#### 3.6 `<resetcooldown>` (Extended Version)
**หน้าที่:** ล้างเวลาคูลดาวน์ของช่อง Slot ที่ระบุให้เป็น 0 ทำให้สกิลในช่องนั้นพร้อมใช้งานทันที

**Attributes ที่รองรับ:**
- `slot` *(integer, ค่าเริ่มต้น: 5)*: ช่อง Slot ที่ต้องการรีเซ็ตคูลดาวน์ (0-79)
- `target` *(target scheme, ค่าเริ่มต้น: target_entity)*: ยูนิตเป้าหมาย

**ตัวอย่างการใช้งานใน XML:**
```xml
<!-- รีเซ็ตคูลดาวน์ช่อง Slot 5 ให้พร้อมกดใช้ทันที -->
<resetcooldown slot="5" target="this_owner_entity" />
```

---

### 4. ระบบ Engine Hooks พิเศษ (Low-Level Architecture)

นอกเหนือจาก XML Action Processors ทางเราได้ติดตั้ง Assembly Hooks ระดับ Native ใน `game_shared_x64.dll` 2 จุดสำคัญ:

#### 4.1 ลดคูลดาวน์สกิลขโมย 20% อัตโนมัติ (`Dota2_HookToolStartCooldown`)
- **Hook Location:** Entry ของฟังก์ชัน `IEntityTool::StartCooldown`
- **การทำงาน:** ตรวจสอบ Attribute `m_ySlot` ของ Tool หากตรวจพบว่าเป็น **Slot 5** หรือ **Slot 6** ระบบจะลดระยะเวลาคูลดาวน์ลง 20% อัตโนมัติทันที:
  $$	ext{New Duration} = rac{	ext{Duration} 	imes 4}{5}$$
- **ความแม่นยำ:** ทำงานเฉพาะกับสกิลขโมยใน Slot 5 และ 6 เท่านั้น สกิลหลัก (Q, W, E, R) และช่องไอเทมจะไม่ได้รับผลกระทบ

#### 4.2 การอัปเดต HUD และเครือข่ายสำหรับ Slot 5 และ 6
- **Hook Location:** ในลูปของ `IUnitEntity::UpdateInventory`
- **การทำงาน:** กระตุ้นการส่ง Snapshot เครือข่ายและการซิงโครไนซ์สถานะสกิลใน Slot 5 และ 6 ไปยัง UI ทันทีที่มีการเปลี่ยนแปลง ป้องกันปัญหาไอคอนสกิลไม่ขึ้นบนหน้าจอ (HUD Desync)

---

### 5. ผังโครงสร้างช่อง Inventory Slots ใน PK

| ช่วง Slot | หน้าที่และการใช้งานใน Engine |
|---|---|
| **0 – 3** | สกิลหลักของฮีโร่ (Q, W, E, R) |
| **4** | Attribute Boost (สกิลบวกสเตตัส) |
| **5 – 6** | **Sub-Abilities / Stolen Abilities (Slot D & Slot F)** — ควบคุมด้วย PK Extensions |
| **7** | Extra Sub-Ability (Slot 8) |
| **8 – 11** | ช่องสำหรับ Taunt, Dance, Courier และคำสั่งพื้นฐาน |
| **12 – 17** | ช่องกระเป๋าไอเทมหลัก (Inventory Slots 1–6) |
| **18 – 23** | ช่องคลังเก็บของ (Stash Slots 1–6) |
| **24 – 39** | ช่องเก็บ Active States และ Modifiers ภายในตัวยูนิต |
