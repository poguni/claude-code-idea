# 먹이그물 탐정단 그림 생성 프롬프트 (image-prompts.md)

| 항목 | 내용 |
|---|---|
| 문서 버전 | v0.1 |
| 작성일 | 2026-10-19 |
| 대상 PRD | `../prd-ecosystem-foodweb.md` (v0.3) 5.5절 |
| 권장 도구 | Google Gemini 이미지 생성(나노바나나). 다른 도구도 같은 프롬프트로 쓸 수 있습니다. |
| 필요한 그림 | 생물 39장 + 비생물 요소 4장 + 사건 18장 + 관리인 행동 6장 + 기타 6장 = **73장** |

> 이 개발 환경에서는 이미지를 직접 만들 수 없어서, 생성 도구에 그대로 붙여 넣을 수 있는 프롬프트를 정리했습니다. 프롬프트는 영어로 쓰면 결과가 더 안정적이라 영어로 쓰고, 확인할 점은 한국어로 적었습니다.

---

## 1. 작업 순서

1. **기준 그림 만들기**: 2장의 "공통 스타일"과 `forest-frog`(개구리) 프롬프트로 먼저 1장을 만듭니다. 마음에 들 때까지 다시 생성합니다.
2. **기준 그림을 함께 넣어 생성**: 나머지 그림을 만들 때마다 기준 그림을 첨부하고, 프롬프트 맨 앞에 아래 문장을 붙입니다.
   > `Use the attached image only as a style reference (same watercolor and colored-pencil technique, line weight, lighting and level of detail). Do not copy its subject.`
3. **한 대화에서 한 테마씩**: 숲 16장을 한 대화에서 이어서 만들면 화풍이 잘 유지됩니다. 테마가 바뀌면 새 대화를 열고 기준 그림을 다시 첨부합니다.
4. **검수**: 그림마다 5장의 점검표로 확인하고, 틀린 부분은 "고쳐 줘" 프롬프트(4장)로 수정합니다.
5. **후처리**: 배경 제거 → 정사각형 자르기 → WebP 변환(512px, 256px 두 크기) → 파일 이름 규칙에 맞게 저장(6장).

---

## 2. 공통 스타일 (모든 생물·비생물 카드)

모든 생물 카드 프롬프트 뒤에 아래 문단을 **그대로 붙입니다.**

```
STYLE: Scientific nature illustration for a Korean 4th-grade science card game.
Soft watercolor with fine colored-pencil linework, like a children's field guide
or a Korean elementary science textbook. Anatomically accurate shape, proportions,
markings and natural colors. Friendly and gentle mood, but NOT cartoonish, NOT chibi,
no big cute eyes, no human expressions, no clothes.
COMPOSITION: One single subject, centered, whole body visible, 3/4 side view,
filling about 75% of the square frame, with even margins.
BACKGROUND: Plain pure white background (#FFFFFF). Only a very faint soft shadow
under the subject. No scenery, no ground texture.
DO NOT include any text, letters, numbers, labels, logos, borders, frames or watermarks.
FORMAT: Square 1:1, 1024 x 1024.
```

### 사건·행동 카드 공통 스타일
사건 카드와 관리인 행동 카드는 장면 그림이므로 아래 문단을 붙입니다.

```
STYLE: Same soft watercolor and colored-pencil technique as a Korean elementary
science textbook. Simple, calm scene that a 10-year-old can understand at a glance.
Child-safe: no blood, no injured or dead animals shown, no scary faces, no people's
faces in close-up. Show "disappearance" as an empty habitat or a faded silhouette.
COMPOSITION: One clear focal point in the center, uncluttered background,
soft natural light.
DO NOT include any text, letters, numbers, labels, logos, borders or watermarks.
FORMAT: Landscape 4:3, 1600 x 1200.
```

---

## 3. 그림 목록과 프롬프트

표의 **프롬프트(주제)** 뒤에 2장의 공통 스타일을 붙여서 넣습니다. **확인할 점**은 생성 뒤 검수할 때 봅니다.

### 3.1 숲 — 교과서 49쪽 먹이그물 (16장)

| 파일 이름 | 생물 | 프롬프트(주제) | 확인할 점 |
|---|---|---|---|
| `forest-grass` | 풀 | A clump of common wild green grass with thin long leaf blades and a few foxtail-like seed heads, as found at a Korean forest edge. | 꽃이 화려하지 않은 평범한 풀 |
| `forest-clover` | 토끼풀 | White clover (Trifolium repens) plant: several three-part round leaflets with pale V-shaped marks, and two round white flower heads on thin stems. | 잎이 3장, 흰 공 모양 꽃 |
| `forest-cornus` | 산수유 | A branch of Korean cornelian cherry (Cornus officinalis) with glossy oblong bright red berries hanging in small clusters and oval leaves with curved veins. | 빨간 열매가 길쭉한 타원형 |
| `forest-acorn` | 도토리(상수리나무) | Three acorns of sawtooth oak (Quercus acutissima) with large cups covered in long curly spiky scales, beside a long narrow leaf with bristle-tipped saw-teeth. | 깍정이(모자)에 털 같은 비늘, 잎 가장자리 톱니 끝이 바늘처럼 뾰족 |
| `forest-grasshopper` | 메뚜기 | A rice grasshopper (Oxya chinensis): slender bright green body with a brown stripe from eye to wing, short antennae, six legs with long jumping hind legs. | 다리 6개, 더듬이 짧음, 뒷다리 굵고 김 |
| `forest-caterpillar` | 애벌레 | A smooth bright green butterfly caterpillar on a small leaf, segmented body, three pairs of true legs at the front and fleshy prolegs at the rear, small head. | 털 없는 초록 애벌레, 마디 구분 |
| `forest-rabbit` | 토끼 | A wild Korean hare (Lepus coreanus) sitting: grayish-brown fur, long ears with dark tips, large dark eye, white belly. | 집토끼처럼 흰색·통통하지 않게, 야생 갈색 |
| `forest-squirrel` | 다람쥐 | A Siberian chipmunk (Tamias sibiricus) sitting upright and holding an acorn: orange-brown fur with five dark stripes along the back, striped face, bushy tail. | 등에 검은 줄 5개(청설모가 아님) |
| `forest-frog` | 개구리 | A black-spotted pond frog (Pelophylax nigromaculatus), Korean "참개구리": green to olive back with irregular black spots, a thin pale yellow-green stripe down the middle of the back, long striped hind legs. | 등 가운데 연한 줄, 검은 점무늬, 앞발가락 4개·뒷발가락 5개 |
| `forest-bulbul` | 직박구리 | A brown-eared bulbul (Hypsipetes amaurotis) perched on a twig: gray body, distinct chestnut-brown patch on the cheek behind the eye, slightly shaggy crown, long tail, dark slender bill. | 뺨의 밤색 무늬, 꼬리 김 |
| `forest-duck` | 오리 | An eastern spot-billed duck (Anas zonorhyncha), Korean "흰뺨검둥오리", standing: dark brown scaly body, pale buff face with a dark eye stripe, black bill with a bright yellow tip, orange legs. | 부리 끝만 노랑, 흰 얼굴에 검은 눈선 |
| `forest-snake` | 뱀 | A Korean steppe ratsnake (Elaphe dione), Korean "누룩뱀", in a relaxed S-curve: light gray-brown body with dark-edged brown blotches along the back, round pupils, calm pose, mouth closed. | 입 다물고 위협적이지 않게, 무늬 |
| `forest-weasel` | 족제비 | A Siberian weasel (Mustela sibirica) standing on all fours: long slender body, bright orange-brown fur, dark brown mask around the eyes, white muzzle and chin, short legs, long bushy tail. | 눈 주위 짙은 갈색, 흰 턱, 몸이 길쭉 |
| `forest-falcon` | 매 | A peregrine falcon (Falco peregrinus) perched: slate blue-gray back and wings, white chest with fine dark horizontal bars, dark "moustache" stripe below the eye, yellow feet and eye ring, hooked bill. | 눈 아래 검은 수염 무늬, 가슴 가로 줄무늬 |
| `forest-mushroom` | 버섯 | A small group of brown wild mushrooms growing from a mossy piece of fallen, decaying log, with visible gills under the caps. | 썩은 나무에서 자라는 모습(분해자) |
| `forest-mold` | 곰팡이 | A fallen brown oak leaf partly covered with soft gray-green fuzzy mold, and a circular magnifying-lens inset at the upper right showing thin branching mold threads with round spore heads. | 확대 원 안에 실 모양 균사 |

### 3.2 논 (11장 + 외래종 1장)

| 파일 이름 | 생물 | 프롬프트(주제) | 확인할 점 |
|---|---|---|---|
| `paddy-rice` | 벼 | A few rice plants (Oryza sativa) standing in shallow paddy water: long narrow green leaves and drooping golden-green grain panicles. | 이삭이 고개 숙임 |
| `paddy-weed` | 논 잡초(피) | Barnyard grass (Echinochloa crus-galli), Korean "피", growing in a rice paddy: similar to rice but with a purplish-green bristly seed head. | 벼와 비슷하지만 이삭이 다름 |
| `paddy-waterplant` | 물풀 | Small floating water plants (duckweed) and soft green filamentous algae on the surface of clear shallow paddy water, seen slightly from above. | 물에 떠 있는 작은 식물 |
| `paddy-grasshopper` | 벼메뚜기 | A rice grasshopper (Oxya chinensis) clinging to a rice leaf: slender bright green body, brown side stripe, six legs, long hind legs. | 숲 메뚜기와 같은 종, 벼 잎 위 |
| `paddy-snail` | 우렁이 | A Chinese mystery snail (Cipangopaludina chinensis), Korean "논우렁이": round dark olive-brown spiral shell, body and short tentacles out, crawling on mud. | 껍데기가 둥글고 짙은 올리브색 |
| `paddy-loach` | 미꾸라지 | A pond loach (Misgurnus anguillicaudatus): slender eel-like brownish body with small dark spots, small mouth with whiskers (barbels), rounded tail fin. | 입수염, 가늘고 긴 몸 |
| `paddy-spider` | 거미 | A wolf spider standing on a rice leaf: brown and gray striped body, eight legs, several small eyes. | 다리 8개(곤충 아님) |
| `paddy-frog` | 개구리 | (숲 `forest-frog` 그림을 그대로 써도 됩니다.) Black-spotted pond frog sitting on the edge of a rice paddy. | 숲 개구리와 같은 종 |
| `paddy-snake` | 뱀 | A tiger keelback (Rhabdophis lateralis), Korean "유혈목이": olive-green body with black bars and a bright red-orange patch on the neck, round pupils, calm pose, mouth closed. | 목의 붉은 무늬, 위협하지 않는 자세 |
| `paddy-egret` | 백로 | A little egret (Egretta garzetta) standing in shallow water: pure white feathers, long black bill, long black legs with yellow feet, slender S-shaped neck. | 검은 부리·다리, 노란 발 |
| `paddy-bacteria` | 세균·곰팡이 | Rice straw lying on wet soil, with a circular magnifying-lens inset showing many tiny rod-shaped and round bacteria and thin mold threads. | 확대 원 안의 작은 세균 |
| `paddy-bullfrog` | 황소개구리(외래종) | An American bullfrog (Lithobates catesbeianus): very large heavy frog, green to olive-brown, large round eardrum behind the eye bigger than the eye, no ridges along the back. | 눈보다 큰 고막, 참개구리보다 훨씬 큼 |

### 3.3 갯벌 (11장)

| 파일 이름 | 생물 | 프롬프트(주제) | 확인할 점 |
|---|---|---|---|
| `mudflat-plankton` | 식물 플랑크톤 | A circular magnifying-lens view of marine phytoplankton: several transparent golden-brown diatoms of different shapes (round, boat-shaped, chain-like) floating in blue-green water. | 현미경으로 본 모습임이 드러나게 |
| `mudflat-sealettuce` | 파래 | Bright green thin filamentous seaweed (Ulva, Korean "파래") spread over a wet rock on a muddy tidal flat. | 밝은 초록 실 모양 해조류 |
| `mudflat-clam` | 바지락 | A Manila clam (Ruditapes philippinarum) half buried in mud with two short siphons out; oval shell with fine crossing ridges and varied brown-gray patterns. | 껍데기 무늬, 수관 2개 |
| `mudflat-crab` | 칠게 | A Japanese sentinel crab (Macrophthalmus japonicus), Korean "칠게", on mud: wide rectangular grayish-brown shell, two long eye stalks, one pair of claws, eight walking legs. | 눈자루가 긺, 다리 8개 + 집게 2개 |
| `mudflat-worm` | 갯지렁이 | A clam worm (Hediste/Nereis), Korean "갯지렁이": long reddish-brown segmented worm with many small paddle-like side appendages, partly coming out of a hole in the mud. | 옆에 다리 같은 돌기가 많음 |
| `mudflat-snail` | 고둥 | Two small mud creeper snails (Batillaria), Korean "갯고둥": tall cone-shaped dark gray-brown spiral shells crawling on wet mud, leaving faint trails. | 길쭉한 원뿔 모양 껍데기 |
| `mudflat-goby` | 망둑어 | A finetail goby (Acanthogobius hasta), Korean "풀망둑": long tapered pale brown body with dark blotches, large head, two dorsal fins, resting on mud. | 길쭉한 몸, 등지느러미 2개 |
| `mudflat-octopus` | 낙지 | A long-arm octopus (Octopus minor), Korean "낙지": small reddish-gray body, eight very long thin arms with suckers, coming out of a mud burrow. | 팔 8개, 가늘고 김 |
| `mudflat-sandpiper` | 도요새 | A dunlin (Calidris alpina) walking on wet mud: small shorebird, gray-brown back, white belly, black legs, long slightly down-curved black bill. | 부리가 길고 끝이 살짝 휨 |
| `mudflat-spoonbill` | 저어새 | A black-faced spoonbill (Platalea minor) standing in shallow water: white body, long flat black bill with a wide spoon-shaped tip, black bare skin on the face around the eye, black legs. | 주걱 모양 부리, 얼굴의 검은 피부 |
| `mudflat-bacteria` | 세균 | Wet gray tidal-flat mud with a circular magnifying-lens inset showing many tiny rod-shaped and round bacteria among small bits of dead plants. | 확대 원 안의 세균 |

### 3.4 비생물 요소 (4장)
분류 놀이(교과서 42쪽)에 씁니다. 생물 카드와 같은 공통 스타일을 씁니다.

| 파일 이름 | 요소 | 프롬프트(주제) |
|---|---|---|
| `abiotic-sun` | 햇빛 | Warm sunlight: a soft glowing sun with gentle rays shining down on a single small green sprout. |
| `abiotic-water` | 물 | Clear water: a few large clean water drops and a small ripple, light blue. |
| `abiotic-air` | 공기 | Air: soft swirling light-blue wind lines around a few floating dandelion seeds. |
| `abiotic-soil` | 흙 | A small mound of dark brown forest soil with crumbly texture, small pebbles and a few dry leaves. |

### 3.5 사건 카드 (18장)
2장의 "사건·행동 카드 공통 스타일"을 붙입니다.

| 파일 이름 | 사건 | 프롬프트(주제) |
|---|---|---|
| `event-forest-frog-gone` | 개구리가 모두 사라졌어요 | A quiet forest stream bank with grass and stones but no frogs; one faint gray see-through silhouette of a frog on a stone to suggest it is gone. |
| `event-forest-bulbul-gone` | 직박구리가 사라졌어요 | A Korean cornelian cherry branch full of red berries with an empty perch; a faint gray silhouette of a brown-eared bulbul where the bird used to sit. |
| `event-forest-decomposer-gone` | 버섯과 곰팡이가 사라졌어요 | A forest floor piled high with fallen leaves and old logs that are not rotting, looking untidy and dry, with no mushrooms anywhere. |
| `event-forest-drought` | 가뭄 | A dry forest edge under a hot sun: cracked dry ground, yellowed drooping grass and clover, a dried-up small stream bed. |
| `event-forest-longrain` | 오랜 장마 | A forest under heavy gray rain clouds with steady rain for many days, dim light, grass bent under water drops. |
| `event-forest-road` | 숲에 길을 냈어요 | A new paved road cutting through the middle of a green forest, with trees on both sides, seen from a slight height. |
| `event-forest-snake-poaching` | 뱀을 마구 잡아갔어요 | A forest floor with an empty wire trap and a cloth sack left behind on the ground, no animals in view. |
| `event-forest-rabbit-boom` | 토끼가 크게 늘었어요 | A forest meadow with many wild brown hares eating grass, the grass looking short and thin. |
| `event-paddy-pesticide` | 농약을 뿌렸어요 | A rice paddy with a fine white mist being sprayed from a backpack sprayer seen from behind at a distance, no person's face visible. |
| `event-paddy-bullfrog` | 황소개구리가 들어왔어요 | A large American bullfrog sitting at the edge of a Korean rice paddy, much bigger than the small native frogs nearby that keep their distance. |
| `event-paddy-dry` | 논에 물이 말랐어요 | A rice paddy with dried, cracked mud and thirsty rice plants, a dry irrigation channel. |
| `event-paddy-frog-gone` | 논 개구리가 사라졌어요 | A calm rice paddy at the water edge with no frogs; a faint gray silhouette of a frog on a rice-field ridge. |
| `event-paddy-egret-fewer` | 백로가 줄었어요 | A wide rice paddy landscape with only one little egret far away and many empty spots in the water. |
| `event-mudflat-overharvest` | 바지락을 너무 많이 캤어요 | A tidal flat with many dug holes and several full baskets of clams lined up, digging tools left on the mud, no people's faces. |
| `event-mudflat-oil` | 기름이 흘렀어요 | A tidal flat shoreline with a dark shiny oil slick spreading over the mud and water, cleanup buoys in the distance, no animals shown. |
| `event-mudflat-reclaim` | 갯벌을 메웠어요 | A tidal flat being filled with sand and soil behind a long concrete seawall, one side mud and the other side flat new land. |
| `event-mudflat-trash` | 갯벌에 쓰레기가 쌓였어요 | A tidal flat with scattered plastic bottles, fishing nets and bags stuck in the mud. |
| `event-mudflat-spoonbill-fewer` | 저어새가 줄었어요 | A shallow tidal channel with only two black-faced spoonbills feeding, the rest of the wide flat empty. |

### 3.6 관리인 행동 카드 (6장)

| 파일 이름 | 행동 | 프롬프트(주제) |
|---|---|---|
| `action-reintroduce` | 되살리기 | A small open wooden box on the grass by a stream, with a few frogs hopping out into the habitat. |
| `action-remove-invasive` | 외래종 잡기 | A large wire live-trap containing an American bullfrog at the edge of a rice paddy, ready to be taken away. |
| `action-protect` | 보호 구역 정하기 | A wooden signpost with a green leaf emblem in a forest, a low rope fence around a protected green area. |
| `action-water` | 물 대 주기 | Clear water flowing from an irrigation channel into dry ground, green sprouts recovering. |
| `action-less-pesticide` | 농약 줄이기 | A healthy rice paddy with spiders' webs on rice plants, dragonflies flying, and ducks in the water as natural pest control. |
| `action-clean-mudflat` | 갯벌 청소 | Collected bags of trash and absorbent pads lined up on a clean wet tidal flat, a crab coming back out of its hole. |

### 3.7 기타 (6장)

| 파일 이름 | 용도 | 프롬프트(주제) |
|---|---|---|
| `ui-mascot` | 탐정 배지(마스코트) | A friendly gender-neutral child detective of about 10 years old wearing a navy blue coat and a small hat, holding a big golden magnifying glass, half-body, smiling. Same watercolor style, plain white background, square. |
| `ui-app-icon` | 앱 아이콘 | A golden magnifying glass over a simple web of thin lines connecting five small dots (leaf, grasshopper, frog, snake, falcon silhouettes), on a navy blue rounded square. Flat, bold, readable at 48 px, no text. Square. |
| `ui-bg-forest` | 숲 테마 배경 | A wide soft watercolor landscape of a Korean forest edge with a small stream, oak trees and a grassy bank, light and airy colors, lots of empty space at the top, no animals. Landscape 16:9. |
| `ui-bg-paddy` | 논 테마 배경 | A wide soft watercolor landscape of Korean rice paddies with water reflections and low hills, light colors, empty space at the top, no animals. Landscape 16:9. |
| `ui-bg-mudflat` | 갯벌 테마 배경 | A wide soft watercolor landscape of a Korean west-coast tidal flat at low tide with water channels and a distant island, light colors, empty space at the top, no animals. Landscape 16:9. |
| `ui-card-back` | 카드 뒷면 | A repeating pattern of small leaves, footprints and mushrooms in soft cream and navy, with a golden magnifying glass in the center. Square, no text. |

---

## 4. 고쳐 달라고 할 때 쓰는 프롬프트

생성된 그림이 틀렸을 때, 그림을 그대로 둔 채 아래처럼 고쳐 달라고 합니다.

| 문제 | 프롬프트 예시 |
|---|---|
| 글자가 들어감 | `Remove all text and letters from the image. Keep everything else exactly the same.` |
| 다리 수가 틀림 | `The insect must have exactly six legs: three on each side. Fix the legs only.` |
| 너무 귀엽게 나옴 | `Make it more realistic like a field guide illustration: smaller natural eyes, accurate proportions. Keep the watercolor style.` |
| 배경이 들어감 | `Replace the background with plain pure white. Keep the subject unchanged.` |
| 무늬가 틀림 | `The Siberian chipmunk must have five dark stripes along its back. Fix the stripes only.` |
| 화풍이 다름 | `Redraw this in the exact style of the attached reference image.` (기준 그림 첨부) |

---

## 5. 그림 검수 점검표

Claude가 개발 중 1차 검수를 돕고, 최종 확인은 선생님이 합니다(PRD 5.6).

- [ ] 생물의 종이 맞는가(예: 다람쥐 ≠ 청설모, 참개구리 ≠ 황소개구리)
- [ ] 다리·날개·더듬이·지느러미 수가 맞는가(곤충 다리 6개, 거미 다리 8개, 게 다리 8개 + 집게 2개, 낙지 팔 8개)
- [ ] 특징 무늬가 보이는가(위 표의 "확인할 점")
- [ ] 글자, 숫자, 로고, 워터마크가 없는가
- [ ] 배경이 흰색이고 생물 하나만 있는가(생물 카드)
- [ ] 무섭거나 잔인한 표현이 없는가(사건 카드)
- [ ] 같은 테마의 그림끼리 화풍·밝기·선 굵기가 비슷한가
- [ ] 크롬북 화면에서 64px로 줄였을 때도 무엇인지 알아볼 수 있는가

---

## 6. 후처리와 파일 규칙

| 단계 | 방법 |
|---|---|
| 배경 제거 | 이미지 편집 도구의 배경 지우기 기능으로 투명 PNG를 만듭니다(생물·비생물 카드만). |
| 크기 | 생물·비생물 카드: 정사각형 512×512, 256×256 / 사건·행동 카드: 800×600 / 배경: 1920×1080 |
| 형식 | WebP(품질 80 안팎). 크롬·엣지 모두 지원하고 용량이 작아 오프라인 저장에 유리합니다. |
| 파일 이름 | `파일이름-512.webp`, `파일이름-256.webp` (예: `forest-frog-512.webp`) |
| 저장 위치 | `public/img/forest/`, `public/img/paddy/`, `public/img/mudflat/`, `public/img/abiotic/`, `public/img/event/`, `public/img/action/`, `public/img/ui/` |
| 기록 | 그림마다 생성 도구, 날짜, 검수자를 `public/img/credits.json`에 남깁니다. |

- Gemini로 만든 그림에는 눈에 보이지 않는 디지털 워터마크(SynthID)가 들어갑니다. 수업 자료로 쓰는 데는 문제가 없지만, 학교나 교육청에 AI 생성 그림 사용 지침이 있으면 그에 따릅니다.
