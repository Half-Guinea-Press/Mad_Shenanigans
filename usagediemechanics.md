https://anydice.com/program/21a73

Since the usage die was discussed recently, I figured I might as well post the AnyDice script I had lying around. The x-axis is how many uses you get out of the usage die before running out.

If anybody isn't familiar with the usage die, it's a concept from The Black Hack:

USAGE DIE

Any item listed in the equipment section that has a Usage die is considered a consumable, limited item. When that item is used the next Minute (turn) its Usage die is rolled. If the roll is 1-2 then the usage die is downgraded to the next lower die in the following chain:

d20 > d12 > d10 > d8 > d6 > d4

When you roll a 1-2 on a d4 the item is expended and the character has no more of it left.

<code>
set "explode depth" to 50
set "maximum function depth" to 10

function: usagedie D:d keep on TN:n {
    SIZE: [maximum of D]
    if SIZE <= 4 {
        result: 1 + [explode D>=TN]
        }
    else {
        result: 1 + [explode D>=TN] + [usagedie d(SIZE-2) keep on TN]
        }
    }

output [lowest of [usagedie d4 keep on 3] and 50] named "d4"
output [lowest of [usagedie d6 keep on 3] and 50] named "d6"
output [lowest of [usagedie d8 keep on 3] and 50] named "d8"
output [lowest of [usagedie d10 keep on 3] and 50] named "d10"
output [lowest of [usagedie d12 keep on 3] and 50] named "d12"
</code>
