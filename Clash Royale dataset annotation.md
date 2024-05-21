# Clash Royale Dataset

This dataset is specially designed to extract the image feature in game Clash Royale, including object detection (visual object classes, VOC), optical character recognition (OCR) and card classification.

## Dataset struct

### Video

- `CR/videos/deck_name/video_name.mp4`: Download from You Tube, video resolution is `590x1280` (720p)
- `CR/videos/deck_name/video_name_episodes/{1,2,...}.mp4`: Crop the origin videos by episodes, using `cut_episodes.py` to complete.

```bash
CR/videos/deck_name/video_name.mp4  # Origin game video
CR/videos/deck_name/video_name_episodes/{1,2,...}.mp4  # Cropped by episodes
```

### Images

- `CR/images/part1`: Crop the upper right corner of the origin image for the current **time**.
- `CR/images/part2`: Crop the middle main part of the origin image for the **Arena**.
- `CR/images/part3`: Crop the bottom of the origin image for the current **deck**.
- `CR/images/part4`: Crop by OCR for the number in the battle field, like **text, bar, tower bar**.

```bash
CR/images/part{1,2,3,4}/video_name/episode/frame.jpg
```

### VOC

```bash
# Associate with `CR/images/part2`
CR/voc/part2/video_name/episode/frame.txt
```

### OCR

```bash
# Associate with `CR/images/part{1,4}`, respectively
CR/ocr/part{1,4}/video_name/episode/frame.txt
```

### Classification

```bash
# Associate with `CR/images/part3`
CR/card_labels/part3/video_name/episode/frame.txt
```

## Bounding box label definition

### Cards & Units

Until 2023/10/30, there are 116 different cards in Royale (include evolution cards). Card names can be found at [`katacv/constants/card_list.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/constants/card_list.py).

But the card name is not completely, because the unit for different card could be same. So, we use unit name to distinguish different units, they can be found at [`katacv/constants/unit_list.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/constants/unit_list.py). There are $127$ different units in Royale (include card unit, `queen-tower`, `king-tower`, 13 spells and 3 objects, see detail state).

#### Detail state

We add 9 types of states for each detection object，7 are predicted by YOLO, 2 are constants.

```vim
# Need predict:
blong:				0/1  # firend/enermy after card name
movement:			norm(walk/wait)/attack/deploy/freeze/dash(destory)
shield or charge:	bare(charge)/shield(over)
visible:			visible/invisible
rage:				norm/rage
slow:				norm/slow
heal or clone:		norm/heal/clone

# Constants:
height:				ground/air
object:				unit/object
```

- `heal`: `heal-spirit`, `battle-healer`
- `freeze`: `deploy`, `electro-spirit`, `zap`, `freeze`, `lightning`, `electro-wizard`
- `rage`: `barbarian-evolution`, `rage`
- `shield`: `royal-recruit`, `royal-recruit-evolution`, `gurad`, `dark-prince`, `cannon-cart`, `monk`
- `dash`: `royal-recruit-evolution`, `bandit`, `battle-ram`, `dark-prince`, `golden-knight`, `prince`, `ram-rider`, `mega-knight`
- `clone`: `clone`
- `invisible`: `royal-ghost`, `archer-queen`
- `slow`: `giant-snowball`, `poison`, `ice-golem`
- `object`: 
  - Spells (13): `zap`, `giant-snowball`, `rage`, `the-log`, `arrows`, `earthquake`, `clone`, `tornado`, `fireball`, `freeze`, `poison`, `rocket`, `lightning`
  - Others: `dirt`(`miner`, `goblin-drill`, `mighty-miner`), `bowl`(`bowler`), `axe`(`executioner`)

#### Note

For `king-tower` and `queen-tower`, the states have different meaning:

```vim
king-tower_freeze  # means king tower hasn't been activated
queen-tower_destory  # means the queen tower has been destoryed
```

#### Examples

```vim
archer-queen0_freeze  # our archer queen, deploy state
archer-queen0_attack_invisible_rage_slow_heal  # our archer queen with 5 buffers
```

### Bar

There are two types of bar in Royale:

- `tower-bar`: The health bar of queen tower and king tower.
- `bar`: The bar of card unit.

### Others

- `selected`: The card is selected. (translucent state)
- `clock`: The little clock when card is deployed.
- `emote`: Just emotes were sent by players.
- `text`: All the text, such as `Good luck!`, `FIGHT!`, etc.
- `elixir`: When deploy a unit, there will appear a elixir with the cost.
