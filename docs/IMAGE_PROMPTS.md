# 이미지 교체 계획과 GPT 프롬프트

작성일: 2026-10-22
목적: 지금 이모지·CSS로 그린 요소를 아이들이 좋아할 일관된 그림으로 바꾸기. GPT 이미지 생성으로 만들고 `assets/` 폴더에 넣어 게임에 연결한다.

## 1. 지금 게임이 쓰는 그림 요소와 교체 여부

| 화면 | 현재 | 교체 | 이유 |
|---|---|---|---|
| 캐릭터 4종 (선택·지도·방) | 이모지 🦊🐧🐱🦖 | **교체** | 몰입도에 가장 큰 영향. 표정(정답·오답)까지 넣으면 더 좋음 |
| 타이틀 던전 입구 | CSS 아치 + 🔥 | **교체** | 첫 화면 인상. 그림 한 장으로 분위기가 확 바뀜 |
| 보물상자 (층 클리어) | CSS | **교체** | 보상 연출의 핵심 |
| 트로피 (결과) | 🏆 + 색 필터 | **교체** | 금·은·동 구분이 그림으로 더 확실 |
| 별 (점수) | ⭐ | **교체** | 게임 전체에서 가장 자주 보이는 아이콘 |
| 던전 문 (지도) | CSS | **교체** | 잠김·현재·클리어 3상태를 그림으로 |
| 층 배경 3종 | CSS 그라데이션 | **교체** | 이끼·얼음·용암 동굴 분위기 |
| 로봇·보석·장애물 | 🤖💎🧱 | **교체** | 로봇의 "앞쪽"이 분명한 그림이 필요 |
| 과일 6종 (스도쿠) | 이모지 | **교체** | 기기마다 이모지 모양이 달라서 통일 필요 |
| 횃불, 힌트 전구, 퍼즐 안내 아이콘 | 🔥💡🔮 | 교체(후순위) | 있으면 좋지만 급하지 않음 |
| 패턴 찾기 도형 | CSS 도형 | **유지** | 규칙 찾기용이라 단순한 도형이 더 좋음 |
| 불빛 기억 패드 | CSS 색판 | **유지** | 색 구분이 목적. 나중에 보석 모양으로 바꿀 수 있음 |
| 명령 블록 ⬆️↩️↪️, 소리 🔊 | 이모지 | 유지(선택) | 뜻이 명확해 그대로 둬도 됨 |

## 2. 우선순위

- **1순위 (11장)**: 캐릭터 기본 4, 타이틀 입구 1, 보물상자 2, 트로피 3, 별 1
- **2순위 (15장)**: 문 3, 층 배경 3, 로봇 세트 3, 과일 6
- **3순위 (11장)**: 캐릭터 표정(기쁨·놀람) 8, 횃불 1, 힌트 전구 1, 수정구슬 1

## 3. GPT에서 만들 때 요령

1. **스타일 고정**: 모든 프롬프트 맨 앞에 아래 `[STYLE]` 문장을 똑같이 붙인다. 같은 대화 안에서 이어서 만들면 "위와 같은 스타일로"라고 덧붙이면 더 일관된다.
2. **세트는 한 장에 모아서**: 캐릭터 4종, 문 3종, 트로피 3종, 과일 6종처럼 짝이 되는 것은 한 장에 나란히 그리게 하면 스타일이 맞는다. 나중에 잘라서 쓰면 된다(잘라 쓰는 작업은 제가 할 수 있음).
3. **투명 배경**: 스프라이트는 반드시 "fully transparent background (real alpha channel), not a drawn checkerboard"라고 쓴다. 체크무늬를 그림으로 그려 놓는 경우가 있어서 결과를 확인한다.
4. **글자 금지**: GPT가 그림에 이름표·글자를 넣는 경우가 많다. "no text, no letters"를 꼭 넣는다.
5. **여백**: 둥근 원 안에 넣을 캐릭터·아이콘은 사방 10% 여백을 두고 중앙에 그리게 한다.
6. **크기**: 스프라이트는 1024×1024 정사각, 배경은 1536×1024 가로. 게임에 넣을 때는 제가 256~512px로 줄여서 용량을 맞춘다(디벗 로딩 속도 때문).
7. **로봇은 "앞"이 분명해야** 한다. 게임이 로봇을 회전시키므로 위에서 내려다본 모습에 눈·헤드라이트가 위쪽에 있어야 한다.

## 4. 공통 문장

`[STYLE]` 모든 프롬프트 앞에 붙이는 문장:

```
Cute kids' mobile puzzle game art. Chunky flat vector cartoon style with thick rounded dark-brown outlines, bright saturated candy colors, soft simple cel shading, glossy highlights, friendly playful mood, clean readable silhouette. No text, no letters, no logo, no watermark.
```

`[SPRITE]` 투명 배경 스프라이트 뒤에 붙이는 문장:

```
Single isolated subject, centered, fully visible with about 10% empty margin on all sides, front view, lit from the top-left. Fully transparent PNG background (real alpha channel, not a drawn checkerboard). Square 1024x1024.
```

`[BG]` 배경 그림 뒤에 붙이는 문장:

```
Wide 1536x1024 landscape illustration used as a dimmed game background behind UI panels. Keep the middle area simple and dark with low contrast, put the detailed elements near the edges, soft blur on far elements. No characters, no text.
```

## 5. 프롬프트

### 5-1. 캐릭터 (1순위) → `assets/char/`

캐릭터는 "던전 탐험가" 콘셉트로 통일한다. 공통: 2.5등신 치비 체형, 큰 반짝이는 눈, 작은 갈색 탐험 배낭.

한 장에 4명 (추천, 스타일 맞추기용):

```
[STYLE] Character lineup sheet of four chibi animal explorer characters standing side by side in one row, same height, same proportions (about 2.5 heads tall), all facing front with big sparkling eyes and cheerful open-mouth smiles, each wearing a small brown explorer backpack. From left to right: (1) an orange fox with a white muzzle and chest, wearing a green safari explorer hat; (2) a black-and-white penguin with rosy cheeks, wearing a red knitted scarf and a tiny yellow headlamp; (3) a golden-yellow tabby cat with a yellow bandana around the neck; (4) a mint-green baby T-rex dinosaur with a tiny explorer hat and a cream belly. Equal spacing between characters, nothing overlapping. [SPRITE] (but use a wide 1536x1024 canvas)
```

개별로 만들 때 (파일명: `fox.png`, `penguin.png`, `cat.png`, `dino.png`):

```
[STYLE] A chibi orange fox explorer character, about 2.5 heads tall, standing and facing front, big sparkling eyes, cheerful open-mouth smile, one paw raised in a friendly wave. White muzzle and chest, fluffy tail with a white tip, wearing a green safari explorer hat and a small brown explorer backpack. [SPRITE]
```

```
[STYLE] A chibi black-and-white penguin explorer character, about 2.5 heads tall, standing and facing front, big sparkling eyes, rosy cheeks, cheerful smile, one flipper raised in a friendly wave. Wearing a red knitted scarf and a tiny yellow headlamp on the head, and a small brown explorer backpack. [SPRITE]
```

```
[STYLE] A chibi golden-yellow tabby cat explorer character, about 2.5 heads tall, standing and facing front, big sparkling eyes, cheerful smile, one paw raised in a friendly wave. Darker orange stripes, a yellow bandana tied around the neck, and a small brown explorer backpack. [SPRITE]
```

```
[STYLE] A chibi mint-green baby T-rex dinosaur explorer character, about 2.5 heads tall, standing and facing front, big sparkling eyes, cheerful open-mouth smile showing tiny rounded teeth, tiny arms raised in a friendly wave. Cream-colored belly, small round spikes on the back, wearing a tiny explorer hat and a small brown explorer backpack. [SPRITE]
```

표정 2종 (3순위, 파일명: `fox-happy.png`, `fox-oops.png` 식으로 4종 각각). 같은 대화에서 기본 캐릭터를 만든 직후에 쓴다:

```
Same character, same outfit, same colors and proportions as the previous image. Pose: jumping up with both arms raised in celebration, eyes closed in a big happy smile, a few small sparkles and stars around. [SPRITE]
```

```
Same character, same outfit, same colors and proportions as the previous image. Pose: surprised, leaning back slightly, wide round eyes, mouth open in a small "oh", paws up near the cheeks, one small sweat drop near the head. Still cute and funny, not sad or scared. [SPRITE]
```

### 5-2. 타이틀 던전 입구 (1순위) → `assets/bg/title.png`

```
[STYLE] A magical dungeon entrance seen straight from the front at night: a big rounded stone archway built from chunky cobblestones covered in soft green moss, a dark tunnel inside with warm golden light glowing from deep within and a faint glint of treasure, two burning wall torches with big cartoon flames on either side of the arch, a few tiny glowing teal mushrooms and fireflies at the base, a deep blue night sky with soft round stars above. Composition: the archway sits in the upper-middle of the picture, the lower third is simple dark ground with little detail so buttons can be placed over it. [BG]
```

### 5-3. 보물상자 (1순위) → `assets/ui/chest-closed.png`, `chest-open.png`

```
[STYLE] A chunky rounded wooden treasure chest, closed, dark oak planks with golden metal corner guards and bands, a big golden padlock-shaped clasp in the front center, slightly tilted toward the viewer so the top is visible. [SPRITE]
```

```
Same treasure chest as the previous image, now wide open: the lid swung back, the inside overflowing with gold coins, colorful gems and small golden stars bursting upward with sparkles and a soft golden glow. [SPRITE]
```

### 5-4. 트로피 (1순위) → `assets/ui/trophy-gold.png`, `trophy-silver.png`, `trophy-bronze.png`

한 장에 3개 (추천):

```
[STYLE] Three identical cartoon trophy cups side by side in one row, same size and shape: a tall cup with two rounded handles on a small dark wooden base, a big five-point star emblem on the front of each cup. From left to right the materials are shiny gold, polished silver, and warm bronze, each with glossy highlights and a few sparkles. Equal spacing, nothing overlapping. [SPRITE] (but use a wide 1536x1024 canvas)
```

### 5-5. 별 (1순위) → `assets/ui/star.png`

```
[STYLE] A glossy golden five-point star with softly rounded tips, a bright white highlight on the upper-left, a slightly darker orange-gold bottom edge, and three tiny white sparkle dots around it. [SPRITE]
```

### 5-6. 던전 문 (2순위) → `assets/ui/door-closed.png`, `door-locked.png`, `door-open.png`

한 장에 3개 (추천):

```
[STYLE] Three dungeon doors side by side in one row, same size, each set inside a rounded stone archway made of chunky mossy cobblestones. Left: a closed chunky wooden door of dark oak planks with iron bands, rivets and a round iron ring handle. Middle: the same closed door with a big golden padlock on a short chain hanging across the center. Right: the same archway with the door swung open inward, bright warm golden light and small sparkles pouring out of the doorway. Equal spacing, nothing overlapping. [SPRITE] (but use a wide 1536x1024 canvas)
```

### 5-7. 층 배경 3종 (2순위) → `assets/bg/floor-moss.png`, `floor-ice.png`, `floor-lava.png`

```
[STYLE] Interior of a mossy green underground cave: rounded green-grey stone walls covered with soft moss, hanging vines, clusters of glowing teal mushrooms near the floor, a few drifting fireflies, cool green ambient light. [BG]
```

```
[STYLE] Interior of a frozen ice cave: pale blue ice walls, big glowing light-blue crystals growing from the floor and ceiling, rounded icicles hanging from above, gentle floating snow sparkles, cool blue ambient light. [BG]
```

```
[STYLE] Interior of a volcanic lava cave: dark rounded volcanic rock walls, glowing orange-red lava rivers and small lava pools along the left and right edges, floating embers, warm red-orange ambient light. The middle stays dark. [BG]
```

### 5-8. 로봇 길 찾기 세트 (2순위) → `assets/robot/robot.png`, `gem.png`, `rock.png`

```
[STYLE] A cute round explorer robot seen from directly above (top-down view), clearly facing upward: two big round glowing cyan eyes and a small yellow headlight on the top edge of the body, a tiny antenna with a red ball, light-blue rounded body with yellow accents, small dark tank treads on the left and right sides. The front (top) of the robot must be obvious at a glance. [SPRITE]
```

```
[STYLE] A big sparkling blue diamond gem with simple clean facets, a bright white highlight, and small sparkle stars around it, seen from a slight top-down angle. [SPRITE]
```

```
[STYLE] A chunky rounded grey-brown boulder with a little patch of moss on top, seen from a slight top-down angle, simple and solid looking, clearly an obstacle. [SPRITE]
```

### 5-9. 과일 6종 (2순위) → `assets/fruit/apple.png`, `banana.png`, `grape.png`, `orange.png`, `strawberry.png`, `kiwi.png`

한 장에 6개 (추천):

```
[STYLE] Six cute fruit icons arranged in a 3 by 2 grid, all the same size, equal spacing, nothing overlapping: top row a red apple with a small green leaf, a yellow banana, a bunch of purple grapes; bottom row an orange, a red strawberry with a green top, and a kiwi cut in half showing the green inside. Each fruit has a glossy white highlight. Simple, bold, easy to tell apart. [SPRITE] (but use a wide 1536x1024 canvas)
```

### 5-10. 작은 아이콘 (3순위) → `assets/ui/torch.png`, `bulb.png`, `crystal.png`

```
[STYLE] A wall torch: a short wooden handle with an iron band, topped by a big bright cartoon flame in yellow and orange with a soft warm glow. [SPRITE]
```

```
[STYLE] A glowing cartoon light bulb with a happy golden-yellow glow, a few small sparkle lines around it, simple rounded shape. [SPRITE]
```

```
[STYLE] A magical crystal ball on a small golden stand, with a few tiny colorful floating shapes (a red circle, a blue triangle, a yellow star) glowing inside the glass. [SPRITE]
```

## 6. 완성 후 전달 방법

- 파일명은 위의 이름대로 `assets/` 아래 폴더에 넣어 주시거나, 그림 파일만 보내 주시면 제가 이름을 맞춰 넣겠습니다.
- 한 장에 여러 개를 그린 세트(캐릭터·문·트로피·과일)는 잘라서 쓰는 작업까지 제가 합니다.
- 그림이 들어가면 `index.html`은 이모지 대신 `<img>`와 CSS 배경으로 바뀌고, 게임은 `index.html` + `assets/` 폴더 구성이 됩니다. GitHub Pages에는 그대로 올라갑니다.
