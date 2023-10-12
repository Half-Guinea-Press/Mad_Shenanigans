The Basics
The simplest part of DPR is the character’s attack and damage rolls, and they’re the most important part of the equation. If a character’s attack bonus is terrible, their DPR will suffer greatly because they can’t hit anything. If the character’s attack bonus is great but their damage is poor, their DPR will suffer. This establishes a crucial balance between attack bonus and damage which is central to the DPR score.

Attack Bonus
We will define our attack bonus as “A”, for “Attack”. Nice and simple.

Target AC
We will define the AC of a hypothetical enemy as “M”, for “Monster”. Determine this value by check the AC of a creature of the character’s level according to the table “Monster Statistics by Challenge Rating” found on page 274 of the Dungeon Master’s Guide.

Base Damage
We will define the value of our damage as “D”, for “Damage”, where D is the total of the value of the average roll of each damage die dealt by the character’s attacks. Static damage bonuses like that from Strength added to a weapon attack are omitted because those bonuses are not multiplied on a critical hit. This can include many sources of bonus damage: Hunter’s Mark, Hex, Sneak Attack, Divine Smite, etc. are all multiplied. For clarification, see this Sage Advice twitter thread from Jeremy Crawford.

For convenience, I’ve included a table of the average values of each die.

Die	Damage
d4	2.5
d6	3.5
d8	4.5
d10	5.5
d12	6.5
For Example: Craig the 1st-level Warlock casts Hex on a creature, then attacks it with a Glaive. Craig normally deals 1d10+3 damage with his Glaive, and adds +1d6 for the bonus damage from Hex. 1d10 averages to 5.5 damage, and 1d6 averages to 3.5 damage. Adding both dice (5.5 + 3.5) gives us a total of 9, which is our value for D. Remember that static bonus (Craig’s +3 bonus) are not added to the value of D because they are not multiplied on a critical hit.

Additional Damage
Any damage not multiplied on a critical hit becomes “B”, for “Bonus”. This includes things like Strength damage to weapon attacks, bonus damage from the Fighting Style (Dueling), etc.

Critical Hits
Critical hits complicate our damage calculations, and this is further complicated by the availability of items and class features which allow characters to score critical hits on rolls other than a natural 20. However, since we separate our damage dice and our other damage bonuses, we can easily include critical hits in our equation.

We will define a variable for our critical hits, C, for “Critical Hit”, which is taken from the table below:

Range	C
20	0.05
19-20	0.1
18-20	0.15
Hit Chance
We will define our hit chance as “H”, for “Hit”. Hit will be a value ranging from 0.5 to 0.95, which is the probability of a hit represented as a decimal. Probabilities typically range from 0 (impossible) to 1 (guaranteed), but natural 1’s always miss and natural 20’s always hit.

H = 1 – ((M – A)/20)

Advantage / Disadvantage
Advantage and Disadvantage are a crucially important mechanic in 5e. Some characters can easily gain Advantage (Barbarians, Kobolds with Pack Tactics, anyone with a familiar, etc.) on one or more attacks, potentially leading to a significant increase in DPR.

We’ll define our hit change with Advantage as HA and our hit chance with Disadvantage as HD. Like our regular Hit Chance, natural 1’s and 20’s factor into the equation, giving both HA and HD a minimum of 0.0025 and a maximum of 0.9975.

H = 1 – ((M – A – 1)/20)
HA = 1 – ((M – A)/20)^2
HD = ((20 – M + A)/20)^2

We also need to consider how Advantage and Disadvantage affect critical hits and their effect on our DPR. We’ll define our critical chance with Advantage as CA and our critical chance with Disadvantage as CD.

CA = 1 – (1-C)^2
CD = C^2

The Equation
DPR = C * D + H * (D + B)
DPRA = CA * D + HA * (D + B)
DPRD = CD * D + HD * (D + B)

A = Attack bonus
M = Monster AC
D = Damage Die
B = Damage Bonus
C = Critical Hit
H = Hit Chance

Acs by level.
1-3: 13
4: 14
5-7: 15
8-9: 16
10-12: 17
13-16: 18
17-20: 19.


Level	Proficiency Bonus
1	+2
2	+2
3	+2
4	+2
5	+3
6	+3
7	+3
8	+3
9	+4
10	+4
11	+4
12	+4
13	+5
14	+5
15	+5
16	+5
17	+6
18	+6
19	+6
20	+6

MISS: 0
HIT: 1
CRITICALHIT: 2

function: attack ROLL:n vs DEFENSE:n {
 if ROLL = 1 { result: MISS }
 if ROLL = 20 {
  if ROLL + ATTACK >= DEFENSE { result: CRITICALHIT }
  result: HIT
 }
 if ROLL + ATTACK >= DEFENSE { result: HIT }
 result: MISS
}

ATTACK: 5 + 3
output [attack d20 vs 15]

ATTACK: 5 + 3
loop AC over {5..30} {
 output [attack d20 vs AC] named "[AC]"
}

ATTACK: 5 + 3
output [attack d20 vs d{5..30}]

function: damage for ATTACKRESULT:n {
 if ATTACKRESULT = CRITICALHIT {
  result: CRITICALDAMAGE
 }
 if ATTACKRESULT = HIT {
  result: HITDAMAGE
 }
 result: MISSDAMAGE
}

ATTACK: 5 + 3
HITDAMAGE: d10 + 4
CRITICALDAMAGE: 14
MISSDAMAGE: 0
loop AC over {10, 20, 30} {
 output [damage for [attack d20 vs AC]] named "[AC]"
}

function: attack {
 result: [damage for [attack d20 vs DEFENSE]]
}

DEFENSE: d{12..22}

ATTACK: 5 + 2
HITDAMAGE: d10 + 4
CRITICALDAMAGE: 14
MISSDAMAGE: 0
output [attack] named "Battleaxe"

ATTACK: 5 + 2
HITDAMAGE: d8 + 4
CRITICALDAMAGE: 12 + d8
output [attack] named "Scimitar"

ATTACK: 5 + 3
HITDAMAGE: d8 + 4
CRITICALDAMAGE: 12
output [attack] named "Longsword"

				CR	Prof	AC	AVE HP	ATK B	AVE DPR	SAVE DC
				0	2	13	4	3	0.5	13
				0.125	2	13	21	3	2.5	13
				0.25	2	13	43	3	4.5	13
level	proficiency	ability	atk bonus	0.5	2	13	60	3	7	13
1	2	3	5	1	2	13	78	3	11.5	13
2	2	3	5	2	2	13	93	3	17.5	13
3	2	3	5	3	2	13	108	4	23.5	13
4	2	4	6	4	2	14	123	5	29.5	14
5	3	4	7	5	3	15	138	6	35.5	15
6	3	4	7	6	3	15	153	6	41.5	15
7	3	4	7	7	3	15	168	6	47.5	15
8	3	5	8	8	3	16	183	7	53.5	16
9	4	5	9	9	4	16	198	7	59.5	16
10	4	5	9	10	4	17	213	7	65.5	16
11	4	5	9	11	4	17	228	8	71.5	17
12	4	5	9	12	4	17	243	8	77.5	17
13	5	5	10	13	5	18	258	8	83.5	18
14	5	5	10	14	5	18	273	8	89.5	18
15	5	5	10	15	5	18	288	8	95.5	18
16	5	5	10	16	5	18	303	9	101.5	18
17	6	5	11	17	6	19	318	10	107.5	19
18	6	5	11	18	6	19	333	10	113.5	19
19	6	5	11	19	6	19	348	10	119.5	19
20	6	5	11	20	6	19	378	10	131.5	19
				21	7	20	423	11	149.5	20
				22	7	20	468	11	167.5	20
				23	7	20	513	11	185.5	20
				24	7	20	558	12	203.5	21
				25	8	21	603	12	221	21

