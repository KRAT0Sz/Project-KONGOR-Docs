# คู่มือการสร้างไฟล์ Effect (Effect Creation Guide)

*เรียบเรียงและจัดหมวดหมู่ใหม่จากต้นฉบับของ ElementUser โดยแยก **ค่าพื้นฐานที่ใช้ร่วมกัน** ออกจาก **ค่าเฉพาะตัวของแต่ละแท็ก** เพื่อให้อ่านและค้นหาได้ง่ายขึ้น*

## 1. โครงสร้างหลัก (Core Tags)

### `<effect>` (แท็กครอบสูงสุดของไฟล์)

* **`useentityeffectscale`** (`true/false` หรือ `0/1`): ใช้กับ Effect ส่วนกลางที่แชร์ให้หลายฮีโร่ (เช่น `state.effect`, `impact.effect`)
* **`deferred`** (`true/false` หรือ `0/1`): ทำให้ Sprite ค่อยๆ จางหาย (Fade) ได้อย่างถูกต้อง
* **`persistent`** (`true/false` หรือ `0/1`): ปล่อยให้ Emitter ทำงานจนจบแม้ว่า Gadget หรือ Entity แม่จะถูกทำลายไปแล้ว *(ข้อควรระวัง: หากเปิดเป็น `true` ต้องกำหนด `life` หรือ `count` ให้ทุก Emitter เสมอ)*

### `<particlesystem>` (กลุ่มระบบอนุภาค)

* **`name`**: ตั้งชื่อระบบ (เช่น `system0`, `system1`) เพื่อใช้อ้างอิงในแท็ก `spawnparticlesystem`
* **`space`** (`world` หรือ `entity`): ตั้งเป็น `entity` เพื่อให้ติดตัวฮีโร่ หรือ `world` เพื่อให้ทิ้งร่องรอยไว้ตามพื้น/อากาศเมื่อเคลื่อนที่
* **`scale`**: กำหนดขนาดรวมของระบบ (เช่น `1`, `50`, `.2`) แต่แนะนำให้ตั้งเป็น `1` เพื่อความเป็นระเบียบ

### `<particle>` (คุณสมบัติของตัวอนุภาค/Sprite)

> **💡 เคล็ดลับ:** ค่าส่วนใหญ่ใน `<particle>` สามารถใช้ร่วมกับ `start`, `mid`, `end` ได้ (เช่น `startcolor`, `midcolor`, `endcolor`) และคุมจังหวะช่วงกลางได้ด้วย `midcolorpos` (ค่าเริ่มต้นคือ `.5`)

| หมวดหมู่ | Attribute | คำอธิบายและตัวอย่างการใช้งาน |
| :--- | :--- | :--- |
| **สีและความโปร่งใส** | `color` | ค่าสี RGB 3 ตัวเลข (เช่น `"1 1 1"`) |
| | `alpha` | ความโปร่งใส `0` - `1` (เช่น `".5"` คือโปร่งใส 50%) |
| **ขนาดและสัดส่วน** | `size` | ขนาดของ Sprite (นิยมใช้ `startsize`, `midsize`, `endsize`) |
| | `width` / `height` | ปรับความกว้างหรือความสูงแยกกันอย่างอิสระ |
| | `scale` | ตัวคูณขนาด (นิยมใช้ `startscale`, `midscale`, `endscale` ควบคู่กับ `size`) |
| | `widthdistort` / `heightdistort` | บิดเบือนสัดส่วนความกว้างหรือความสูง |
| **การหมุน (Rotation)** | `pitch` / `yaw` / `roll` | หมุนตามแกน X / Z / Y ตามลำดับ |
| | `pitchspeed` / `yawspeed` / `rollspeed` | ความเร็วในการหมุนรอบแกนนั้นๆ (เช่น `yawspeed="360"` จะหมุนครบ 1 รอบตลอดอายุขัย) |
| **การยึดเกาะและล็อกมุม** | `anchor` | ยึด Sprite ไว้กับ Bone (ใช้แทน `stickiness` ที่เก่ากว่า) เช่น `startanchor="1"` `endanchor="0"` |
| | `lockup` / `lockright` | ล็อกแกนตั้งและแกนขวา (`true/false`) หากเปิดทั้งคู่และตั้ง `pitch="90"` จะได้ Sprite ที่แบนราบขนานกับพื้น |
| | `turn` | (`true/false`) ให้ Sprite หันตามทิศทางของ Entity (นิยมใช้กับโมเดลที่ฝังในกระสุน/Projectile) |
| **แอนิเมชันและเอฟเฟกต์พิเศษ** | `frame` / `framespeed` | ใช้กับ Animated Sprite (เช่น `startframe="0"` `endframe="1"`) และคุมความเร็วเฟรม |
| | `param` | ความแรงของการหักเหแสงสำหรับ Refraction Sprite (ปกติอยู่ระหว่าง `".1"` ถึง `"1"`) |
| | `flare` | (`true/false`) เปิดใช้งานเอฟเฟกต์แสงแฟลร์ |
| | `emitter` | เรียกชื่อ Emitter อื่นมาฝังซ้อนไว้ข้างใน (Embedding) |
| **การจัดการ UV** | `scaleu` / `scalev` | ขยายสเกลตามแกน U หรือ V ของ Texture |
| | `offsetu` / `offsetv` / `weight` | *(ผู้เขียนระบุว่าไม่เคยใช้และไม่แนะนำให้ใช้)* |

## 2. ค่าพื้นฐานที่ใช้ร่วมกันในกลุ่ม Emitter (Common Emitter Attributes)

แท็กตระกูล Emitter เกือบทั้งหมดจะใช้ค่าพื้นฐานเหล่านี้ร่วมกัน:

* **การอ้างอิงและตำแหน่ง:**
  * `material`: พาธของไฟล์ Sprite (เช่น `"/shared/effects/materials/mySprite.material"`)
  * `owner`: ชื่อโมเดลเป้าหมาย (ใช้น้อยลงตั้งแต่มีระบบฝัง Emitter)
  * `bone`: ชื่อกระดูก (Bone) ที่ให้กำเนิดอนุภาค *(ต้องสะกดตัวพิมพ์เล็ก-ใหญ่ให้ตรงเป๊ะ)*
  * `position`: ตำแหน่งชดเชยในแกน `"x y z"` (เช่น `"0 0 50"` คือสูงขึ้นไป 50 หน่วย)
* **เวลาและจำนวน:**
  * `life`: อายุการทำงานของ Emitter หน่วยเป็นมิลลิวินาที (เช่น `"5000"` = 5 วินาที)
  * `count`: บังคับจำนวน Sprite ที่ปล่อยออกมาทั้งหมดแบบตายตัว โดยไม่สน `life` หรือ `spawnrate`
  * `spawnrate`: อัตราการปล่อยอนุภาคต่อวินาที (เช่น `"25"` = 25 ชิ้น/วินาที)
  * `particlelife`: อายุของอนุภาคแต่ละชิ้น (มิลลิวินาที) สามารถใช้ `minparticlelife` และ `maxparticlelife` เพื่อสุ่มความหลากหลายได้
  * `delay`: หน่วงเวลาก่อนเริ่มปล่อยอนุภาค (มิลลิวินาที)
  * `timenudge`: การเลื่อนเวลาแบบเก่า (แนะนำให้ใช้ `delay` แทน ยกเว้นต้องการให้เริ่มทำงานล่วงหน้าก่อนแสดงผลให้ใส่ค่าติดลบ เช่น `"-2000"`)
  * `loop`: (`true/false` หรือ `0/1`) วนลูปการทำงาน (ใช้บ่อยใน `state.effects`)
* **การเคลื่อนที่และฟิสิกส์:**
  * `direction`: ทิศทางการพุ่งในแกน `"x y z"` ใช้ค่า `-1`, `0`, `1` (เช่น `"0 0 1"` คือพุ่งขึ้นฟ้า)
  * `directionalspace` / `particledirectionspace`: กำหนดแกนอ้างอิงเป็น `global`/`world` (แกนโลก) หรือ `local` (แกนของ Bone นั้นๆ)
  * `speed` / `acceleration`: ความเร็วเริ่มต้น และอัตราเร่งหลังจากปล่อยออกไปแล้ว
  * `gravity`: แรงโน้มถ่วง (ค่าลบ = ลอยขึ้น, ค่าบวก = ตกลงพื้น)
  * `drag` / `friction`: แรงต้านอากาศคล้ายเคลื่อนที่ในของเหลวหนืด (มักตั้งราว `.1`) และแรงเสียดทานที่ทำให้ช้าลงจนหยุด
  * `minangle` / `maxangle`: กำหนดขอบเขตมุมองศาในการกระจายตัว (เช่น `0` ถึง `45` องศา)
  * `inheritvelocity`: รับความเร็วมาจากวัตถุแม่ (ห้ามตั้งสูงเกินไปเพราะอนุภาคจะกระเจิงผิดปกติ)
  * `collide`: (`true/false`) ให้อนุภาคชนกับพื้นได้ *(กินสเปกเครื่องสูง ควรจำกัด `life` และ `count` ให้น้อย)*
* **รูปทรงพื้นที่ปล่อยอนุภาค (Offset Shapes):**
  * `offsetsphere` / `offsetcube`: สุ่มจุดเกิดภายในพื้นที่ทรงกลมหรือลูกบาศก์ `"x y z"` (เช่น `"100 100 100"`)
  * `offsetradial`: ปล่อยอนุภาคตามเส้นรอบวงของวงกลม
  * `offsetdirection` / `offsetradialangle`: ใช้คู่กับ `offsetradial` เพื่อสร้างความหนาแบบโดนัท หรือบีบมุมเป็นทรงกรวย (`0` - `180` องศา)
* **ค่าที่ไม่ทำงานแล้ว:** `particletimenudge` (ใช้งานไม่ได้แล้ว)

## 3. ประเภทของ Emitter (Emitter Types)

### `<simpleemitter>` (ตัวปล่อยอนุภาคมาตรฐาน)

* ใช้ค่าพื้นฐานทั้งหมดจากหมวดที่ 2
* **`subframepose`** (`true/false`): ใช้ร่วมกับ Trail Emitter (ควรตั้งเป็น `true` เสมอ)

### `<orbiteremitter>` (ตัวปล่อยอนุภาคแบบหมุนวนรอบจุดศูนย์กลาง)

* **ค่าเฉพาะตัว:**
  * `orbit`: ความเร็วในการหมุนรอบจุดศูนย์กลาง (ค่าบวก = ทวนเข็มนาฬิกา, ค่าลบ = ตามเข็มนาฬิกา)
  * `origin`: ระยะห่างและตำแหน่งจากจุดศูนย์กลาง (เช่น `direction="0 0 1"` และ `origin="0 50 250"` จะได้วงโคจรเส้นผ่านศูนย์กลาง 250 และยกสูงขึ้น 50)
  * `cylindrical`: (`true/false`) เปลี่ยนรูปทรงการปล่อยจากแผ่นจานแบน (ค่าเริ่มต้น) ให้เป็นทรงกระบอก
  * `offset`: ปรับรูปทรงกระบอก (เช่น `"10 250 500"` คือ จุดเริ่มต้นรอบวง 10, ความลึกทรงกระบอก 250, ความกว้างจากขอบ 500)
* **ข้อจำกัด:** `gravity`, `minangle`, `maxangle`, `inheritvelocity`, `drag` **ไม่ทำงาน** ในแท็กนี้ ส่วน `speed`, `acceleration`, `friction` จะทำงานก็ต่อเมื่อ `orbit` ไม่ใช่ `0` เท่านั้น (และ `orbitangle` ใช้งานไม่ได้)

### `<trackeremitter>` (ตัวปล่อยอนุภาคแบบวิ่งติดตามเป้าหมาย)

* **ค่าเฉพาะตัว:**
  * `targetbone` / `targetposition`: ชื่อกระดูกหรือพิกัดเป้าหมายที่ต้องการให้อนุภาควิ่งเข้าหา (ใช้คู่กับ `bone` และ `position` ที่เป็นจุดเริ่มต้น)
  * `tracktype`: รูปแบบการวิ่งติดตาม ได้แก่ `distance`, `angular`, `gravity`, `cgravity`, `target`, `lerp`
  * `trackspeed`: ความเร็วในการติดตาม (ปกติตั้งเป็น `1` และห้ามเปลี่ยนหากใช้ `lerp`)
  * `distancelife`: (`true/false`) หากเป็น `true` อนุภาคจะหายไปทันทีเมื่อวิ่งถึงเป้าหมาย

### `<meshemitter>` และ `<skeletonemitter>`

* ใช้ค่าพื้นฐานเหมือน `<simpleemitter>` (ยกเว้นไม่มี `subframepose`)
* **`<meshemitter>`** ต้องระบุ **`mesh`** (ชื่อ Mesh ที่ต้องการให้อนุภาคเกาะ/ปล่อยออกมา) มิฉะนั้นจะไม่ทำงาน
* **`<skeletonemitter>`** ปล่อยอนุภาคออกมาตามโครงกระดูกของโมเดล

### `<trailemitter>` และ `<twopointtrailemitter>` (ตัวปล่อยเส้นแสง/หางอนุภาค)

* **ค่าเฉพาะตัวของ `<twopointtrailemitter>`:**
  * `bone_a` / `bone_b`: กำหนดกระดูก 2 จุดเพื่อสร้างเส้นลากเชื่อมระหว่างกัน
  * `position_a` / `position_b`: กำหนดพิกัด 2 จุดในอากาศ (ใช้ร่วมกับหรือแทนที่ `bone_a`/`bone_b` ได้)
* **ค่าเฉพาะตัวของทั้ง 2 แท็ก:**
  * `texpostime`: ควรตั้งเวลาให้เท่ากับ `particlelife` (เช่น `"2000"`)
  * `texposscale`: ควรตั้งเป็น `0`
  * `texstretchscale`: ปกติตั้งเป็น `1`
  * `subframepose`: ควรตั้งเป็น `true` เสมอ เพื่อให้เส้นโค้งเนียนไม่หักเหลี่ยม
  * **การไล่สี/ความโปร่งใส:** ใช้ `startparticlecolor`, `midparticlecolor`, `endparticlecolor` (สำหรับแบบ Additive) หรือ `startparticlealpha`, `midparticlealpha`, `endparticlealpha` (สำหรับแบบ Blended)

## 4. องค์ประกอบแสง เส้นลำแสง สไปรต์ และโมเดล

### `<light>` (แหล่งกำเนิดแสง)

> **⚠️ ข้อควรระวัง:** ในหน้าจอเกมสามารถแสดงผล `<light>` ได้สูงสุดเพียง **4 ดวงพร้อมกัน** เท่านั้น ควรออกแบบ Effect ให้ดูดีแม้ไม่มีแสงสำรองไว้ด้วย

* **ค่าพื้นฐาน:** `owner`, `life`, `timenudge`, `delay`, `loop`, `bone`, `position`
* **`color`**: ค่าสี RGB สามารถใส่เกิน `1` ได้เพื่อเร่งความสว่าง (เช่น `"10 5 5"`)
* **`falloffstart` / `falloffend`**: ระยะเริ่มจางและระยะสิ้นสุดของแสง (เช่น `0` ถึง `75` จะนุ่มนวล แต่ `74` ถึง `75` จะขอบคม) *ค่า start ต้องน้อยกว่า end เสมอ*

### `<beam>` (ลำแสงเชื่อมระหว่าง 2 จุด)

* **จุดเริ่มต้นและปลายทาง:** `owner_a` / `owner_b` (เช่น `source`, `target` หรือชื่อโมเดล), `bone_a` / `bone_b`, `position_a` / `position_b`
* **เวลาและสี:** `life`, `timenudge`, `delay`, `loop`, `color`, `alpha`, `material`
* **รูปทรงและลวดลาย:**
  * `size`: ความหนา/ขนาดของลำแสง
  * `tile`: จำนวนการทำซ้ำลวดลายของ Sprite ตามความยาวเส้น (เช่น ใช้ทำโซ่หลายๆ ข้อ)
  * `frame` / `framespeed`: คุมแอนิเมชันบนลำแสง (เหมาะกับลำแสงสายฟ้า)
  * `param`: ความแรงของการหักเหแสง *(หากใช้ `param` ค่า `tile` จะทำงานไม่ถูกต้อง)*

### `<groundsprite>` (สไปรต์แปะบนพื้น)

> **⚠️ ข้อควรระวัง:** กินทรัพยากรเครื่องสูงมาก โดยเฉพาะเมื่อวางซ้อนกันหลายชั้นหรือมีขนาดใหญ่ (ตรวจสอบ FPS ได้ด้วยคำสั่ง `host_drawFPS 1` ใน Console)

* **ค่าที่ใช้งาน:** `owner`, `bone`, `position` *(ค่าแกน Z จะไม่มีผล เพราะติดพื้นเสมอ)*, `life`, `timenudge`, `delay`, `loop`, `color`, `alpha`, `size`, `width`, `height`, `scale`, `frame`, `framespeed`, `param`, `material`

### `<billboard>` (แผ่นภาพหันเข้าหากล้อง)

* **ค่าที่ใช้งาน:** เหมือน `<groundsprite>` แต่รองรับแกน Z ใน `position` และเพิ่มคุณสมบัติของ `<particle>` เข้ามา ได้แก่ `pitch`, `yaw`, `roll` (พร้อม speed), `lockup`, `lockright`, `turn`, `flare`, `directionalspace`
* **`depthbias`**: ดึง Sprite เข้าหากล้อง (ค่าติดลบ) หรือผลักออกจากกล้อง (ค่าบวก) เพื่อแก้ปัญหาภาพซ้อนทับกัน

### `<model>` (โมเดล 3 มิติ)

* **ค่าที่ใช้งาน:** `name`, `owner`, `life`, `timenudge`, `delay`, `loop`, `directionalspace`, `bone`, `position`, `color`, `alpha`, `pitch`, `yaw`, `roll` (พร้อม speed), `scale`
* **ค่าเฉพาะตัว:** `model` (พาธไฟล์ `.mdf`), `skin`, `material`, `anim`, `parentmodel`, `parentskeleton`

## 5. แท็กอื่นๆ ที่ระบุรายชื่อไว้ (ยังไม่มีคำอธิบายในต้นฉบับ)

* **`<tracer>`**: `owner_a`, `owner_b`, `bone_a`, `bone_b`, `position_a`, `position_b`, `life`, `timenudge`, `delay`, `loop`, `color`, `alpha`, `width`, `length`, `speed`, `acceleration`, `taper`, `tile`, `frame`, `param`, `material`
* **`<sound>`**: `owner`, `life`, `timenudge`, `delay`, `loop`, `bone`, `position`, `falloff`, `volume`, `pitch`, `sound2d`, `soundloop`, `fadein`, `fadeoutstarttime`, `fadeout`, `speed1`, `speed2`, `speeduptime`, `slowdowntime`
* **`<precipemitter>`**: `owner`, `life`, `count`, `timenudge`, `delay`, `loop`, `spawnrate`, `particlelife`, `particletimenudge`, `gravity`, `speed`, `acceleration`, `minangle`, `maxangle`, `inheritvelocity`, `material`, `direction`, `directionalspace`, `drag`, `friction`, `drawdistance`, `collide`
* **`<terrainemitter>`**: `owner`, `life`, `count`, `timenudge`, `delay`, `loop`, `spawnrate`, `particlelife`, `particletimenudge`, `gravity`, `speed`, `acceleration`, `minangle`, `maxangle`, `inheritvelocity`, `material`, `direction`, `directionalspace`, `drag`, `friction`, `range`, `collide`
* **`<modifier>`**: `name`, `life`, `timenudge`, `delay`, `loop`, `color`, `alpha`, `material`