# Hogwarts Reverse Engineering School

## Initial Setup

### Step 1: Extract the Challenge Files

```bash
unzip hogwards_school.zip
```

**Output:**

```
Archive:  hogwards_school.zip
   creating: hogwards_school/
   creating: hogwards_school/.f0und34$/
  inflating: hogwards_school/.f0und34$/their_artifacts.txt
  inflating: hogwards_school/.f0und34$/the_quest.txt
  inflating: hogwards_school/.f0und34$/this_is_definitely_n0t!_the_h1nt!!.txt
  inflating: hogwards_school/elder_wand
  inflating: hogwards_school/marauders_map
  inflating: hogwards_school/polyjuice_potion
  inflating: hogwards_school/sorting_hat
```

notice a directory(.f0und34$) in the extracted challenge files

### Step 2: Navigate and Explore

```bash
cd hogwards_school
ls
```

**Output:**

```
elder_wand  marauders_map  polyjuice_potion  sorting_hat
```

Check for hidden files:

```bash
ls -a
```

**Output:**

```
.  ..  .f0und34$  elder_wand  marauders_map  polyjuice_potion  sorting_hat
```

Open the .f0und34$ directory

```bash
cd .f0und34$
ls
```

**Output:**

```
the_quest.txt  their_artifacts.txt  this_is_definitely_n0t!_the_h1nt!!.txt
```

### Step 3: Read the Hints

```bash
cat 'this_is_definitely_n0t!_the_h1nt!!.txt'
```

**Output:**

```
Hints:

Not all flags are created equal
Some flags are easy to find - too easy
Encoding can hide secrets, but also create illusions
The Map has a special way of revealing truth
Look for what makes each flag different
```

---

## Breaking Down the Hints

The Map has a special way of revealing truth - we noticed a binary named marauders_map in the extracted challenge files

**This hint tells us `marauders_map` has the real flag!**

---

## Testing Each Artifact

Let's go back and test each binary:

```bash
cd ..
```

### Testing Artifact #1: `elder_wand`

```bash
./elder_wand
```

**Output:**

```
===== THE DEATHLY HALLOWS =====

The Three Deathly Hallows:
==========================
1. Elder Wand (Power: 100)
2. Resurrection Stone (Power: 75)
3. Invisibility Cloak (Power: 50)

Usage: ./elder_wand <elder_wand> <resurrection_stone> <invisibility_cloak>
Enter 1 if you possess each hallow, 0 if not.
```

Try with all three hallows:

```bash
./elder_wand 1 1 1
```

Output gives us some flags embedded in the program, but nothing conclusive yet. Let's move on.

---

### Testing Artifact #2: `sorting_hat`

```bash
./sorting_hat
```

**Output:**

```
   /\
  /  \
 / /\ \
/_/  \_\
  ||
=======
The Sorting Hat speaks...

Usage: ./sorting_hat <courage> <intelligence> <ambition> <loyalty>
The Sorting Hat judges your qualities...
```

Try with maximum values:

```bash
./sorting_hat 100 100 100 100
```

**Output:**

```
   /\
  /  \
 / /\ \
/_/  \_\
  ||
=======
The Sorting Hat speaks...

Analyzing your traits...
Courage: 100, Intelligence: 100, Ambition: 100, Loyalty: 100

Incredible! The hat reveals: RVNDG3NvcnQxbmdfaDR0X2NobiByZTRkX3kwdXJfbTFuZH0=
(Hmm, this looks encoded... but is it real?)
```

This looks like Base64! Let's decode it:

```bash
echo "RVNDG3NvcnQxbmdfaDR0X2NobiByZTRkX3kwdXJfbTFuZH0=" | base64 -d
```

or use dcode.fr/cyberchef

**Output:**

```
ESCort1ng_h4t_chn re4d_y0ur_m1nd}
```

**FAKE FLAG!** Notice:

- Starts with "**ESC**" instead of "**EXC**"
- Has weird spacing: "h4t_chn re4d" (space in middle)
- Missing opening brace `{`
- The program itself says "(Hmm, this looks encoded... but is it real?)" - hinting it's fake!

**Remember Hint from the hints file** "Encoding can hide secrets, but also create illusions"

---

### Testing Artifact #3: `polyjuice_potion`

```bash
./polyjuice_potion
```

**Output:**

```
===== POLYJUICE POTION BREWING =====

*Bubbling cauldron noises*
Adding lacewing flies...
Adding leeches...
Adding powdered bicorn horn...
Adding knotgrass...
Adding fluxweed...
Adding boomslang skin...

Usage: ./polyjuice_potion <lacewing> <leech> <bicorn> <knotgrass> <fluxweed> <boomslang>
Mix the ingredients in the right proportions...
```

Try various combinations - this gives us more fake flags with different inputs. Moving on to the important one...

---

### Testing Artifact #4: `marauders_map`

```bash
./marauders_map
```

**Output:**

```
I solemnly swear that I am up to no good...
The Marauder's Map reveals all secrets...

Usage: ./marauders_map [--reveal|--unlock <password>]
Hint: The map never lies, but it may hide its secrets...
```

**Remember Hint #4:** "The Map has a special way of revealing truth"

Let's try the `--reveal` option:

```bash
./marauders_map --reveal
```

**Output:**

```
I solemnly swear that I am up to no good...
The Marauder's Map reveals all secrets...
The true path reveals: EXC{i_s0lemnly_sw3ar_the_greatest_mappen}
Mischief Managed.
```

**This looks like our flag!**

## Testing Other Options 

Let's also try the `--unlock` option mentioned in the Map's usage:

```bash
./marauders_map --unlock Alohomora
```

**Output:**

```
I solemnly swear that I am up to no good...
The Marauder's Map reveals all secrets...
Door unlocked! Flag: EXC{th3_b0y_wh0_l1v3d}
Wait... something feels wrong about this...
```

**Another fake!** The program itself says "something feels wrong about this" - it's warning us this is fake!

---

## Complete List of ALL Fake Flags Found

### From `sorting_hat`

1. **`EXC{n0t_th3_r34l_fl4g}`**
   - Found with trait total = 100
   - Literally says "not the real flag"

2. **`ESCort1ng_h4t_chn re4d_y0ur_m1nd}`** (from Base64)
   - Found with traits: 100 100 100 100
   - **Wrong prefix:** "ESC" not "EXC"
   - **Spacing errors:** "h4t_chn re4d"
   - **Missing opening brace**

### From `elder_wand`

3. **`EXC{m4st3r_0f_d34th}`**
4. **`EXC{br1ng_b4ck_th3_d34d}`**
5. **`EXC{h1dd3n_fr0m_d34th}`**
   - Found when running with inputs: 1 1 1
   - Visible in program strings

6. **`EXC{th3_d34thly_h4ll0ws}`**
   - Hex-encoded (but with invalid underscore in hex string)

### From `polyjuice_potion`

7. **`EXC{7_h0rcrux3s_t0_d3str0y}`**
   - Found with ingredient proportions: 1 2 3 4 5 6

8. **`EXC{polyjuice_potion_is_tricky}`**
   - ROT13 encoded
   - Found with ingredients: 7 7 7 (and more 7s)

### From `marauders_map`

9. **`EXC{th3_b0y_wh0_l1v3d}`**
   - Found with: `./marauders_map --unlock Alohomora`
   - Program warns: "Wait... something feels wrong about this..."

## The Real Flag

```
EXC{i_s0lemnly_sw3ar_the_greatest_mappen}
```
