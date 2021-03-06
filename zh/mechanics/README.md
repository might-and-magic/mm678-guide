# 魔法门678游戏机制与算法

针对魔法门6、7、8和整合版的游戏机制与算法。

* [属性](#属性)
  + [七属性](#七属性)
  + [生命值和魔法值](#生命值和魔法值)
  + [属性修正值表格](#属性修正值表格)
* [等级](#等级)
* [攻击](#攻击)
  + [杀伤力XDY](#杀伤力xdy)
  + [近战攻击](#近战攻击)
    - [攻击点数](#攻击点数)
    - [攻击杀伤力](#攻击杀伤力)
    - [举例](#举例)
  + [远程攻击](#远程攻击)
    - [射击点数](#射击点数)
    - [射击杀伤力](#射击杀伤力)
  + [其他](#其他)
    - [匕首特技](#匕首特技)
    - [吸血类武器](#吸血类武器)
    - [手杖](#手杖)
* [防御和攻击概率](#防御和攻击概率)
  + [防御等级](#防御等级)
  + [攻击打中概率](#攻击打中概率)
  + [魔法](#魔法)
  + [怪物的特效](#怪物的特效)
  + [抗性](#抗性)
* [恢复时间](#恢复时间)
  + [公式](#公式)
  + [时间单位BTU](#时间单位btu)
  + [武器基础恢复时间](#武器基础恢复时间)
  + [攻击加速和武器词缀](#攻击加速和武器词缀)
  + [防具惩罚](#防具惩罚)
  + [其他注意](#其他注意)
  + [双持](#双持)
  + [恢复时间和杀伤力的总结](#恢复时间和杀伤力的总结)
* [知名度](#知名度)
* [参考资料](#参考资料)


## 属性

### 七属性

下文中`修正值(某某属性)`指的是对于某某属性（七项属性之一），查询“属性修正值表格”，找到这个属性值所对应的修正值。

| 属性                | 修正值添加到？之中     | 对修正值的注释     |
|--------------------|-----------------------|--------------|
| 力量                | 近战（肉搏）攻击杀伤力 | 最小为0       |
| 精确（又译命中率） | 肉搏和射击的攻击点数   |               |
| 速度                | 防御等级             | 最小为0        |
| 速度                | 攻击恢复时间         | 减去修正值，单位为秒（即BTU） |
| 运气                | 不为0的抗性          | 将会加到不为0的抗性中，不过并不显示在角色属性栏里 |
| 耐力                | 受击恢复时间         | 20 - 修正值。所以耐力在350或以上时遭受攻击后角色能立即恢复行动 |
| 耐力                | 生命值               | 见下           |
| 个性/智力           | 魔法值               | 见下           |

### 生命值和魔法值

生命值即HP；魔法值即SP

```
增加的HP = HP系数 × 修正值(耐力)
```

```
增加的SP = SP系数 × 修正值(个性 或 智力)
```

总计算公式为（不计算直接加HP或SP的物品词缀等）：

```
HP = 基础HP + HP系数 × (等级 + 修正值(耐力) + 健身术技能点（专家×2，大师×3）)
```

```
SP = 基础SP + SP系数 × (等级 + 修正值(个性) + 修正值(智力)（注：个性与智力是否修正SP与职业有关） + 冥想术技能点（专家×2，大师×3）)
```

例如：某魔法师，23级，冥想术6级专家，智力41，那么其魔法值为（不计算直接加HP或SP的物品词缀等）：

```
SP = 魔法师基础SP + 魔法师SP系数 × (等级 + 修正值(智力) + 冥想术技能点 × 2（专家）)
= 10 + 4 × (23 + 8 + 6 × 2)
= 182
```

魔法门6中的职业基础HP、SP及其系数一览表：

|               | 基础HP | HP系数 | 基础SP | SP系数 |
|---------------|------|--------|------|--------|
| 剑客（骑士）   | 30   | 4      | 0    | 0      |
| 豪侠          | 30   | 6      | 0    | 0      |
| 勇士          | 30   | 8      | 0    | 0      |
| 牧师          | 20   | 2      | 10   | 3      |
| 神父          | 20   | 3      | 10   | 4      |
| 大主教        | 20   | 4      | 10   | 5      |
| 巫师          | 20   | 2      | 10   | 3      |
| 魔法师        | 20   | 3      | 10   | 4      |
| 大魔法师      | 20   | 4      | 10   | 5      |
| 游侠（圣武士） | 25   | 3      | 5    | 1      |
| 十字军        | 25   | 4      | 5    | 2      |
| 大英雄        | 25   | 5      | 5    | 3      |
| 弓箭手        | 25   | 3      | 5    | 1      |
| 魔箭手        | 25   | 4      | 5    | 2      |
| 神箭手        | 25   | 5      | 5    | 3      |
| 僧侣（德鲁伊） | 20   | 2      | 10   | 3      |
| 祭司          | 20   | 3      | 10   | 4      |
| 大祭司        | 20   | 4      | 10   | 5      |

魔法门6中，牧师、游侠及其升级职业靠个性加成SP；巫师、弓箭手及其升级职业靠智力加成SP；僧侣及其升级职业靠个性和智力同时加成SP

### 属性修正值表格

属性修正，也称突破点、突破奖励点、奖励值、奖励修正、加权等，英文中通常使用“breakpoint”。

| 属性数值 | 修正值 |
|-----|----|
| 500 | 30 |
| 400 | 25 |
| 350 | 20 |
| 300 | 19 |
| 275 | 18 |
| 250 | 17 |
| 225 | 16 |
| 200 | 15 |
| 175 | 14 |
| 150 | 13 |
| 125 | 12 |
| 100 | 11 |
| 75  | 10 |
| 50  | 9  |
| 40  | 8  |
| 35  | 7  |
| 30  | 6  |
| 25  | 5  |
| 21  | 4  |
| 19  | 3  |
| 17  | 2  |
| 15  | 1  |
| 13  | 0  |
| 11  | -1 |
| 9   | -2 |
| 7   | -3 |
| 5   | -4 |
| 3   | -5 |
| 0   | -6 |

## 等级

升级经验公式如下：

```
角色从n级升级到第n+1级所需要增加的经验值 = n × 1000
```

```
角色升级到第n+1级所需要的总经验值 = n × (n + 1) × 500
```

## 攻击

### 杀伤力XDY

武器物品的杀伤力显示为XDY，意思是，攻击时，掷一个Y面的骰子，掷X次，计算掷出的点数的总和，即为对敌人的杀伤。显然最少能掷出X点，最多X × Y点（如4D5就是掷一个5面的骰子，掷4次，计算掷出的点数的总和，显然最少4点，最多4*5=20点）

杀伤力在角色属性界面中显示为多少到多少的区间，如持一把4D5的武器，不考虑其他因素，就是“4 - 20”。

### 近战攻击

近战类武器，包括榴弹枪

#### 攻击点数

攻击点数：attack bonus。角色属性界面上简称为“攻击”（attack），右键看介绍时可以看到叫做“攻击点数”。

```
攻击点数 = 修正值(精确) + 武器的攻击点数 + 武器（或空手）的技能等级加成 + 武器使用术的加成（如可用）
```

如果双持（双手各拿一把武器）：

```
攻击点数 = 修正值(精确) + 右手武器的攻击点数 + 左手武器的攻击点数 + 左手武器的技能等级加成 + 武器使用术的加成（如可用）
```

MM6-8所有武器，从初级阶段开始，技能等级×1就会加到攻击点数里。特殊情况（乘以的不是1，以及空手、武器使用术等）如下表（对于MM6-8均使用，当然，MM6无空手搏斗术、武器使用术和榴弹枪宗师）：

|    | 普通 | 专家 | 大师 | 宗师 |
|----|-----|-----|-----|-----|
| 榴弹枪 | ×1 | ×2 | ×3 | ×4 |
| 空手搏斗术（对于空手） | ×1 | ×1 | ×2 | ×2 |
| 武器使用术（对于任何近战武器） | 无 | ×1 | ×1 | ×2 |

#### 攻击杀伤力

攻击杀伤力：attack damage。又译“攻击破坏力”、“攻击伤害”。

```
攻击杀伤力 = 修正值(力量) + 武器的杀伤力 + 武器（或空手）的技能等级加成（如可用） + 武器使用术的加成（如可用）
```

如果双持：

```
攻击杀伤力 = 修正值(力量) + 右手武器的杀伤力 + 左手武器的杀伤力 + 左手武器的技能等级加成（如可用）（MM6为右手武器的技能等级） + 武器使用术的加成（如可用）
```

参见表格《武器技能从什么阶段开始，技能等级可以增加杀伤力》：

|    | MM6 | MM7 | MM8 |
|----|-----|-----|-----|
| 手杖 | 无   | 无   | 宗师  |
| 长剑 | 无   | 无   | 无   |
| 匕首 | 无   | 宗师  | 宗师  |
| 战斧 | 大师  | 大师  | 大师  |
| 长矛 | 大师  | 专家  | 专家  |
| 弓箭 | 无   | 宗师  | 宗师  |
| 钉锤 | 专家  | 专家  | 专家  |
| 榴弹枪 | 无   | 无   | 无   |
| 空手搏斗术（对于空手） | 无此技能 | 宗师  | 宗师  |
| 武器使用术（对于任何近战武器） | 无此技能 | 大师（宗师×2） | 大师（宗师×2） |

除标注×2之外者均×1。

#### 举例

例子1：MM7和MM8中，如果右手持矛左手持剑，长矛技能只有宗师级加防御等级的特性有效，攻击杀伤力（当然剑没有杀伤力加成）、攻击点数和恢复时间应该由长剑技能决定。

例子2：MM6中，如果右手持矛左手持剑，长矛技能只影响攻击杀伤力，攻击点数和恢复时间由长剑技能决定。

### 远程攻击

远程类武器，即弓箭和榴弹枪。

#### 射击点数

射击点数：shoot bonus。角色属性界面上简称为“射击”（shoot），右键看介绍时可以看到叫做“射击点数”。

```
射击点数 = 修正值(精确) + 武器的攻击点数 + 武器的技能等级加成
```

#### 射击杀伤力

射击杀伤力：shoot damage。又译“射击破坏力”。

```
射击杀伤力 = 武器的杀伤力 + 武器的技能等级加成（如可用）
```

见之前的两个有关技能等级加成到攻击点数和攻击杀伤力的表格中的榴弹枪和弓箭。

即便在修正了双持恢复时间bug的版本中（MM7和修复了bug的MM6），榴弹枪双持时（榴弹枪在右手），近战攻击当做普通武器计算。其近战和远程攻击的点数和恢复时间都会被左手武器拖累，故通常不建议这样做。

### 其他
#### 匕首特技

匕首造成三倍伤害的概率恒定是10%，不管技能等级是多少。只有匕首本身的伤害造成三倍。在MM7和MM8里，哪怕你只是匕首专家，左手的匕首也有恒定10%的概率造成三倍伤害。

以上两个bug都在grayface补丁里得到修复。

（匕首附带的魔法伤害不参与三倍运算，但是种族特效则参与三倍运算，种族特效本身是翻倍伤害，如果再加上特效触发三倍，那么总共就是六倍）

#### 吸血类武器

吸血类（Vampiric）武器会把对怪物所造成的`所有物理伤害/5`转化为自己的HP，两把武器的物理伤害部分都算，还包括天赐神力和技能加成（还有力量修正值加成。当然前提是怪物没有物理抗性或者掷骰子判定失败，如果判定成功伤害是要打折的）。如果你有两把吸血类武器，那两把吸血都有效。需要注意的是如果一下就把怪秒了是吸不了血的，还有致命那一击也是吸不了血的，只有非致命的攻击才能够吸血。

在MM8里面，你可以吸取`最小值(所有物理伤害/5，剩余HP/5)`。剩余HP是指怪物在受到你的攻击后所剩下的HP。也就是说，当怪物满血（拟100HP），而你又不能一下打掉它半管血的时候（拟对怪物造成40dmg），你可以吸取物理总伤害/5的HP（也就是40/5=8HP），但是如果你一下打掉它半管血以上（拟70dmg），你就只能吸取怪物所剩HP的1/5了（也就是30/5=6HP）。

#### 手杖

手杖（棍棒、棍术；Staff）。

MM7中，手杖宗师可以获得空手搏斗术技能的加成，但失去武器使用术技能的伤害加成。GrayFace补丁中已修复此问题。

MM8中，手杖宗师的技能等级可以增加攻击杀伤力，而不仅仅是攻击点数。武器使用术的加成正常。

## 防御和攻击概率

### 防御等级

防御等级影响着怪物物理攻击击中角色的概率。见下面的章节。

### 攻击打中概率

所有攻击都仅有一定的概率打中。玩家的角色对怪物发起普通物理攻击时，公式是：

```
击中概率 = (15 + 攻击点数 × 2)/(30 + 攻击点数 × 2 + 怪物的防御等级)
```

怪物用普通物理攻击角色命中的概率公式是：

```
怪物物理击中概率 = (5 + 怪物等级 × 2)/(10 + 怪物等级 × 2 + 角色防御等级)
```

怪物用对角色施加特殊状态、角色用冰电火毒魔系攻击打怪物、怪物用这些攻击来打角色的公式也都各不相同，这里不一一提及。

实际杀伤则就是前面说的掷骰的总点数，介于角色属性界面中显示出来的最小和最大值之间。

（当然别忘了有些武器的附加效果能够增加最高达18点的冰电火毒系杀伤力）

### 魔法

在MM7和MM8中，诸如定身大法（麻痹；Paralyze）和迟缓大法（Slow）魔法的成功概率是：

```
魔法成功概率 = 30 / (30 + MonsterResistance + MonsterLevel / 4)
```

但在MM6中是：

```
魔法成功概率 = 30 / (30 + MonsterResistance + MonsterLevel)
```

### 怪物的特效

有些怪物的攻击会附加某些特效，可能会让人物瞬间昏迷甚至死亡，而人物也有对应的属性去抵抗这些负面效果。

怪物攻击会让人物中其附加特效的概率是：

```
怪物特效攻击成功概率 = 30 / (30 + 修正值(运气) + 其他加成)
```

在MM7和MM8里面，其他加成见表：

| 怪物特效              | 对应的加成                   |
|-------------------|-------------------------|
| 虚弱、沉睡、喝醉、患病、昏迷、老化 | 修正值(耐力)                 |
| 诅咒                | 修正值(个性)                 |
| 吸光法力、驱散           | ( 修正值(个性)+修正值(智力) ) / 2 |
| 痴狂、麻痹、害怕          | 心智抗性                    |
| 石化                | 土系抗性                    |
| 中毒、死亡、消灭          | 肢体抗性                    |
| 损坏物品、偷窃物品         | 物品强度                    |

MM6里的其他加成：

| 怪物特效              | 对应的加成                   |
|-------------------|-------------------------|
| 虚弱、沉睡、喝醉、患病、昏迷、害怕   | 修正值(耐力) |
| 诅咒                  | 修正值(个性) |
| 痴狂                  | 修正值(智力) |
| 中毒                  | 中毒抗性    |
| 麻痹、石化、死亡、消灭、吸光法力、老化 | 魔法抗性    |
| 损坏物品、偷窃物品           | 物品强度    |

### 抗性

当人物遭受魔法攻击时，若对应类型的抗性不为0，那么就要“掷骰子”进行判定，而每次“掷骰子”都有一定的概率减少一定的伤害。概率的公式为：

```
概率 = 1 - 30 / (30 + 抗性 + 修正值(运气))
```

这个公式的意思就是掷一个30+面的骰子，若掷出30或以下的数字就是判定失败，并且不再进入后面的判定，若掷出30以上的数字就是这次判定成功，并且进入下一次判定，每一点抗性和运气修正值都会增加一面骰子。

第一次“掷骰子”判定，如果不够幸运，你将受到100%的伤害。

如果够幸运，那么继续“掷骰子”判定，如果这次不够幸运，你将受到1/2（即50%）的伤害。

如果够幸运，那么继续“掷骰子”判定，如果这次不够幸运，你将受到1/4（即25%）的伤害。

如果够幸运，那么继续“掷骰子”判定，如果这次不够幸运，你将受到1/8（即12.5%）的伤害。

如果够幸运，你将只受到1/16（6.25%）伤害。结束。

下表是由于上述的掷骰，在常见运气值下，在很多次攻击中，抗性对应的大概的平均伤害：

| 抗性 |平均伤害|
|------|-------|
|  0   |  100% |
|  20  |  75%  |
|  40  |  60%  |
|  60  |  50%  |
|  100 |  39%  |
|  150 |  31%  |
|  200 |  26%  |
|  300 |  20%  |

当角色击中敌人时，它有相同的机会减少伤害，不过它没有运气属性的修正，即：

```
概率 = 1 - 30 / (30 + 抗性)
```

与角色不同，怪物还具有物理抗性。

## 恢复时间

### 公式

武器的攻击恢复时间的计算公式为：

```
武器（或空手）实际恢复时间 =
武器（或空手）基础恢复时间
- 特定武器技能等级（如为剑斧弓专家）
- 武器使用术技能等级（如为近战武器）
- 修正值(速度)
- 25（如有攻击加速魔法的话）
- 20（如有武器词缀加成的话）
+ 防具惩罚值（如装备有防具且未到大师的话）
```

举例：锤棍，盾牌，皮甲，15速度。所有技能都是普通级别，无武器使用术，恢复时间将是`80+10+10-1=99`

### 时间单位BTU

BTU（Basic Time Unit(s)）：意思是基础时间单位。一般来说，`1 BTU = 1游戏秒`。魔法门6-8中文玩家广泛使用“BTU”作为攻击恢复时间的单位，以至于该英文缩写现在已经成为了攻击恢复时间的代名词。但英文魔法门论坛中几乎看不到“BTU”的说法。

### 武器基础恢复时间

|     |      | 基础恢复时间 | 武器词缀加成 | 技能等级加成  | 注释                                                     |
|-----|------|--------|--------|---------|--------------------------------------------------------|
| 榴弹枪       | 单手   | 30     |        |         | 近身/远程                                                  |
| 匕首         | 单手   | 60     | 有      |         | 近身；专家以上可左手                                             |
| 长矛         | 单/双手 | 80     | 有      |         | 近身；所有长矛类武器都可单/双手使用；在左手没有装备盾牌或其他武器时，长矛被强制以双手使用，恢复时间算法一样 |
| 钉锤（锤棍）  | 单手   | 80     | 有      |         | 近身                                                     |
| 长剑         | 单/双手 | 90     | 有      | 专家以上有 | 近身；大师可左手；专家以上方有技能等级加成；双手剑需双手使用，计算和单手剑相同；                |
| 战斧         | 单/双手 | 100    | 有      | 专家以上有 | 近身；专家以上方有技能等级加成；双手斧需双手使用，计算和单手斧相同                       |
| 手杖（棍棒）  | 双手   | 100    | 有      |         | 近身                                                     |
| 粗头棍       | 单手   | 100    | 有      |         | 近身。MM6中没问题，但MM7和8中基础恢复时间被搞错成了0和6 BTU，由于30 BTU的下限，就变成了总是30 BTU |
| 弓箭         | 身后   | 100    | 有      | 专家以上有 | 远程；专家以上方有技能等级加成 |
| 空手         |        | 100，空手搏斗术者60 |        |         | 近身。MM78中有空手搏斗术。无空手搏斗术者空手战斗的基础恢复时间为100，有空手搏斗术者为60 |
| 武器使用术（对于所有近战武器） |        |        |        | 有，宗师×2 | MM78中的该技能普通级即可对所有近战武器减恢复时间，宗师减的恢复时间×2 |

物品对剑斧弓的技能等级的加成被错误地忽略了，该bug在GrayFace补丁中已经修复。武器使用类词缀的物品则没有问题。

### 攻击加速和武器词缀

* 攻击加速魔法：-25 BTU（MM7和MM8中无效，该bug在Mok和GrayFace的补丁中修复）

* 迅捷类/敏捷类（of Swiftness/Swift）：-20 BTU
* 黑暗类（或译暗黑类；of Darkness）（同时带有“迅捷类”、“吸血鬼”词缀属性）：-20 BTU
* 神器帕西佛（带“迅捷类”词缀），以及其他带有上述词缀的神器：-20 BTU
* MM8神器最终之剑（Finality）独有的“缓慢”（Slow）词缀：-20 BTU

另外，MM8前锋之靴（Herald's Boots）迅捷词缀无效的bug在GrayFace的补丁中修复。但同样有迅捷词缀的超凡铠甲没有修复。

如果你带词缀武器拿在左手上时，不一定会进行武器词缀奖励。请稍候看关于左右手武器的章节。

### 防具惩罚

防具对于恢复时间的惩罚如下表：

|    | 普通 | MM6专家 | MM6大师 | MM78专家 | MM78大师 | MM78宗师 |
|----|----|-------|-------|--------|--------|--------|
| 盾牌 | 10 | 5     | 0     | 0      | 0      | 0      |
| 皮甲 | 10 | 5     | 0     | 0      | 0      | 0      |
| 锁甲 | 20 | 10    | 0     | 10     | 0      | 0      |
| 钢甲 | 30 | 15    | 0     | 15     | 15     | 0      |

### 其他注意

近身攻击的恢复时间限制：所有近身攻击的武器，实际恢复时间不会小于30 BTU，按公式计算小于30 BTU的，一律以30 BTU计。两个远程武器——榴弹枪和弓箭则没有限制，恢复时间可以达到0 BTU。

奔跑或加速飞行无惩罚：有小道消息说，武器攻击的同时奔跑、加速飞行或者后退，会使武器的恢复时间乘以1.5。魔法门6的说明书里也说有惩罚。实际上并没有。

恢复类/康复类（of recovery）词缀属性，游戏文件中注释的意思看起来是“受击恢复时间减10 BTU”，不过GrayFace在他的游戏机制一文中好像是说，该词缀实际上使恢复速率乘以1.5，但因bug而无效，GrayFace的MM6补丁修复了bug，使恢复速率增加10%。

### 双持

双持即双手各拿一把武器（而不是拿一把双手武器）。

双持时恢复时间是算双手拿着的两把武器之中，技能基础恢复时间较大的那把武器。如果两把武器的基础恢复时间相等，计算右手武器的基础恢复时间。

用于计算恢复时间的这把武器上的技能等级加成和词缀属性（如迅捷类）都有效。不用于计算恢复时间的那把武器是完全不影响恢复时间的。

举例1：如果右手持矛左手持剑，那么恢复时间是由剑技能决定。

举例2：如果双持两把剑，其中一把带有迅捷词缀属性，务必把它放到右手，不然迅捷属性会被忽略掉。

MM6原版bug：原版简体（相当于v1.0）、繁体（相当于v1.1）和英文官方补丁v1.2或GOG英文版（相当于v1.3）的魔法门6，都千万别双持，因为系统会将恢复时间简单相加，极其缓慢。这个应该是一个bug。bug算法中具体的技能等级和词缀加成算法此处不赘述。2018年版中文“完美”补丁从1.4版开始的和GrayFace（灰脸）补丁都修正了这个bug（GrayFace补丁可用回原设定，在mm6.ini里把FixDualWeaponsRecovery=1改为0即可）。MM7和MM8里无bug。正确的算法在上文中已经叙述。

### 恢复时间和杀伤力的总结

恢复时间和杀伤力一样重要（由于恢复时间有时比较容易降低，所以可能更重要），因为玩家应该关注的是“单位时间的杀伤”，显然：

```
单位时间的杀伤

= 单位时间的出手次数 × 击中概率 × 攻击平均杀伤

= (1/攻击恢复时间) × ((15 + 攻击点数 × 2) / (30 + 攻击点数 × 2 + 怪物的防御等级)) × 攻击杀伤力的平均数

= ( (15 + 攻击点数 × 2) × 攻击杀伤力的平均数 ) / ( (30 + 攻击点数 × 2 + 怪物的防御等级) × 攻击恢复时间 )
```

## 知名度

MM6和MM7中，知名度（Fame）的算法为：

```
知名度 = 向下取整(四名队员经验值总和 / 1000)
```

MM8中：

```
知名度 = 向下取整(主角经验值 / 250)
```

在MM6和MM7里面，每个队员出生都有200多到300多经验值，四名队员经验值总和总是会略高于1000，于是游戏一开始知名度就有1，今后会通过做任务、杀怪得到经验值从而提高知名度，如果知名度为0，NPC将不会加入玩家。

在MM8里，主角出生经验值为0，于是知名度为0，不过由于NPC雇佣系统已经不同于MM6和MM7了，所以并没有什么影响。

## 参考资料

* [Might and Magic Mechanics by GrayFace](https://grayface.github.io/mm/mechanics/)
* [魔法门678的各种运算公式 by nickzqz](https://tieba.baidu.com/p/4221521428)
* [Bones' Combat Guide](http://www.angelfire.com/nt/bones/)
* 基础数学 by Tom CHEN
* 恢复时间学 by Tom CHEN
* [mm6攻速测试 by gladius](http://gamerhome.com/bbs/forum.php?mod=viewthread&tid=163855)
* [关于MM7的攻速再讨论 by 阿基巴德](http://gamerhome.com/bbs/forum.php?mod=viewthread&tid=196437)
