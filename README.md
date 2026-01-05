# Stationeers IC10 MIPS Instructions Cheat Sheet

## Basic Info
- **Registers**: r0-r15 (16 registers for variables)
- **Device Pins**: d0-d5 (6 device connections), db (housing itself)
- **Special Registers**: ra (return address), sp (stack pointer)
- **Limits**: 128 lines max, 90 characters per line, 128 lines execution per tick
- **Tick**: 0.5 seconds in game time

## Utility

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **alias** | `alias str r?/d?` | Label a register or device with a name |
| **define** | `define str num` | Create a constant label |
| **yield** | `yield` | Pause execution for 1 tick |
| **sleep** | `sleep a` | Pause execution for a seconds |
| **hcf** | `hcf` | Halt and catch fire |

**Example:**
```
alias sensor d0
alias temp r0
define minTemp 293.15
```

## Mathematical

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **move** | `move r? a` | r = a |
| **add** | `add r? a b` | r = a + b |
| **sub** | `sub r? a b` | r = a - b |
| **mul** | `mul r? a b` | r = a × b |
| **div** | `div r? a b` | r = a / b |
| **mod** | `mod r? a b` | r = a mod b |
| **pow** | `pow r? a b` | r = a ^ b |
| **sqrt** | `sqrt r? a` | r = √a |
| **exp** | `exp r? a` | r = e ^ a |
| **log** | `log r? a` | r = ln(a) |
| **abs** | `abs r? a` | r = \|a\| |
| **ceil** | `ceil r? a` | r = ceiling of a |
| **floor** | `floor r? a` | r = floor of a |
| **round** | `round r? a` | r = a rounded |
| **trunc** | `trunc r? a` | r = a with fractional part removed |
| **max** | `max r? a b` | r = max(a, b) |
| **min** | `min r? a b` | r = min(a, b) |
| **rand** | `rand r?` | r = random value [0, 1) |
| **lerp** | `lerp r? a b c` | r = linear interpolation between a and b by ratio c |

## Trigonometric

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **sin** | `sin r? a` | r = sin(a radians) |
| **cos** | `cos r? a` | r = cos(a radians) |
| **tan** | `tan r? a` | r = tan(a radians) |
| **asin** | `asin r? a` | r = arcsin(a) in radians |
| **acos** | `acos r? a` | r = arccos(a) in radians |
| **atan** | `atan r? a` | r = arctan(a) in radians |
| **atan2** | `atan2 r? a b` | r = arctan(a/b) considering quadrant |

## Device I/O

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **l** | `l r? device logicType` | Load device logicType to register |
| **s** | `s device logicType r?` | Store register to device logicType |
| **ls** | `ls r? device slot logicSlotType` | Load slot logicSlotType from device |
| **ss** | `ss device slot logicSlotType r?` | Store register to device slot |
| **lr** | `lr r? device reagentMode hash` | Load reagent info (0=Contents, 1=Required, 2=Recipe) |

**Example:**
```
l r0 d0 Temperature    # Read temperature
s d1 On 1              # Turn device on
ls r1 d0 0 Occupied    # Check if slot 0 occupied
```

## Batch Operations (Network-wide)

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **lb** | `lb r? hash logicType mode` | Load from all devices with hash (0=Avg, 1=Sum, 2=Min, 3=Max) |
| **sb** | `sb hash logicType r?` | Store to all devices with hash |
| **lbn** | `lbn r? hash nameHash logicType mode` | Load from devices by hash and name |
| **sbn** | `sbn hash nameHash logicType r?` | Store to devices by hash and name |
| **lbs** | `lbs r? hash slot logicSlotType mode` | Load slot from all devices with hash |
| **sbs** | `sbs hash slot logicSlotType r?` | Store to slot on all devices with hash |
| **lbns** | `lbns r? hash nameHash slot logicSlotType mode` | Load slot from devices by hash and name |

**Example:**
```
lb r0 HASH("StructureBattery") Ratio Average
sb HASH("StructureWallLight") On 1
```

## Stack Operations

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **push** | `push a` | Push value to stack |
| **pop** | `pop r?` | Pop value from stack to register |
| **peek** | `peek r?` | Read top of stack without removing |
| **poke** | `poke addr value` | Write value at stack address |
| **get** | `get r? device addr` | Read from device stack at address |
| **put** | `put device addr value` | Write to device stack at address |
| **getd** | `getd r? id addr` | Read from device by ID |
| **putd** | `putd id addr value` | Write to device by ID |
| **clr** | `clr d?` | Clear device stack |
| **clrd** | `clrd id` | Clear device stack by ID |

## Comparison (Store 1 if true, 0 if false)

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **seq** | `seq r? a b` | r = (a == b) |
| **sne** | `sne r? a b` | r = (a != b) |
| **sgt** | `sgt r? a b` | r = (a > b) |
| **sge** | `sge r? a b` | r = (a >= b) |
| **slt** | `slt r? a b` | r = (a < b) |
| **sle** | `sle r? a b` | r = (a <= b) |
| **seqz** | `seqz r? a` | r = (a == 0) |
| **snez** | `snez r? a` | r = (a != 0) |
| **sgtz** | `sgtz r? a` | r = (a > 0) |
| **sgez** | `sgez r? a` | r = (a >= 0) |
| **sltz** | `sltz r? a` | r = (a < 0) |
| **slez** | `slez r? a` | r = (a <= 0) |
| **snan** | `snan r? a` | r = (a is NaN) |
| **snanz** | `snanz r? a` | r = (a is not NaN) |
| **sap** | `sap r? a b c` | r = 1 if \|a-b\| <= c×max(\|a\|,\|b\|) (approx equal) |
| **sna** | `sna r? a b c` | r = 1 if not approximately equal |
| **sapz** | `sapz r? a b` | r = 1 if \|a\| <= b×\|a\| |
| **snaz** | `snaz r? a b` | r = 1 if \|a\| > b×\|a\| |
| **sdse** | `sdse r? device` | r = 1 if device is set |
| **sdns** | `sdns r? device` | r = 1 if device is not set |
| **select** | `select r? a b c` | r = b if a != 0, else c (ternary) |

## Branching (Unconditional)

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **j** | `j line` | Jump to line/label |
| **jr** | `jr offset` | Relative jump by offset |
| **jal** | `jal line` | Jump and store return address in ra |

**Example:**
```
j start          # Jump to label
jal function     # Call function
j ra             # Return from function
```

## Branching (Conditional)

### Device Pin Branches
| Instruction | Syntax | Description |
|------------|--------|-------------|
| **bdse** | `bdse device line` | Branch if device is set |
| **bdns** | `bdns device line` | Branch if device not set |
| **bdseal** | `bdseal device line` | Branch if set and store return address |
| **bdnsal** | `bdnsal device line` | Branch if not set and store return address |
| **brdse** | `brdse device offset` | Relative branch if device set |
| **brdns** | `brdns device offset` | Relative branch if device not set |

### Value Comparison Branches
| Instruction | Syntax | Description |
|------------|--------|-------------|
| **beq** | `beq a b line` | Branch if a == b |
| **bne** | `bne a b line` | Branch if a != b |
| **bgt** | `bgt a b line` | Branch if a > b |
| **bge** | `bge a b line` | Branch if a >= b |
| **blt** | `blt a b line` | Branch if a < b |
| **ble** | `ble a b line` | Branch if a <= b |
| **beqz** | `beqz a line` | Branch if a == 0 |
| **bnez** | `bnez a line` | Branch if a != 0 |
| **bgtz** | `bgtz a line` | Branch if a > 0 |
| **bgez** | `bgez a line` | Branch if a >= 0 |
| **bltz** | `bltz a line` | Branch if a < 0 |
| **blez** | `blez a line` | Branch if a <= 0 |
| **bnan** | `bnan a line` | Branch if a is NaN |
| **bap** | `bap a b c line` | Branch if approximately equal |
| **bna** | `bna a b c line` | Branch if not approximately equal |
| **bapz** | `bapz a b line` | Branch if approximately zero |
| **bnaz** | `bnaz a b line` | Branch if not approximately zero |

**All conditional branches have:**
- **br** variants (relative): `breq`, `brne`, `brgt`, etc.
- **al** variants (and link): `beqal`, `bneal`, `bgtal`, etc. - stores return address in ra

## Bitwise Operations

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **and** | `and r? a b` | r = a AND b |
| **or** | `or r? a b` | r = a OR b |
| **xor** | `xor r? a b` | r = a XOR b |
| **nor** | `nor r? a b` | r = a NOR b |
| **not** | `not r? a` | r = NOT a (bitwise complement) |
| **sll** | `sll r? a b` | r = a << b (logical left shift) |
| **sla** | `sla r? a b` | r = a << b (arithmetic left shift) |
| **srl** | `srl r? a b` | r = a >> b (logical right shift, zero fill) |
| **sra** | `sra r? a b` | r = a >> b (arithmetic right shift, sign extend) |
| **ext** | `ext r? a b c` | Extract c bits from a starting at bit b |
| **ins** | `ins r? a b c` | Insert b bits of c into r starting at bit a |

**Example:**
```
and r0 3 6         # 3 & 6 = 2
sll r1 1 3         # 1 << 3 = 8
not r2 0           # ~0 = -1
```

## Special Instructions

| Instruction | Syntax | Description |
|------------|--------|-------------|
| **rmap** | `rmap r? device reagentHash` | Get prefab hash for reagent requirement |
| **bdnvl** | `bdnvl device logicType line` | Branch if device invalid for load |
| **bdnvs** | `bdnvs device logicType line` | Branch if device invalid for store |

## Common Patterns

### Basic Loop
```
start:
  yield
  # your code here
  j start
```

### Temperature Control (Schmitt Trigger)
```
alias sensor d0
alias heater d1
define minTemp 293.15
define maxTemp 298.15

loop:
  yield
  l r0 sensor Temperature
  blt r0 minTemp turnOn
  bgt r0 maxTemp turnOff
  j loop

turnOn:
  s heater On 1
  j loop

turnOff:
  s heater On 0
  j loop
```

### Function Call
```
main:
  move r0 100
  move r1 50
  jal average
  s db Setting r0
  j main

average:
  add r0 r0 r1
  div r0 r0 2
  j ra  # return
```

### Slot Iteration
```
alias device d0
define maxSlots 6
move r15 0

checkSlot:
  ls r0 device r15 Occupied
  beqz r0 nextSlot
  # slot is occupied, do something
  ls r1 device r15 Quantity

nextSlot:
  add r15 r15 1
  blt r15 maxSlots checkSlot
```

## Tips

1. **Use yield** at the start of main loops to prevent wasting CPU
2. **Alias everything** - makes code readable and easier to maintain
3. **Use defines** for magic numbers and constants
4. **Watch out for NOT** - it's bitwise, use `seqz` for logical NOT
5. **Batch operations** are powerful for network-wide control
6. **Stack has 512 slots** - great for storing data between ticks
7. **Check device validity** with `bdnvl`/`bdnvs` before load/store
8. **Use HASH()** for device type hashes in batch operations
9. **Comments** start with `#`
10. **Labels** end with `:` and can't have comments on the same line
