# EEPROM — Memory Map

## Byte Format (Property Region: `0x50–0x7F`)

Each property occupies **one byte**. The byte encodes two things:

```
  Byte value:  0x 5 1
                  │ └─ Low nibble = Player ID (owner)
                  └─── High nibble = Rent level (1–5), or 0 if unowned
```

### Player IDs

| Value | Player Token | Colour |
|-------|-------------|--------|
| `1`   | Car         | Red    |
| `2`   | Helicopter  | Green  |
| `3`   | Boat        | Blue   |
| `4`   | Aeroplane   | White  |

### Examples

| Byte Value | Meaning |
|-----------|---------|
| `0x00`    | Property unowned |
| `0x11`    | Owned by Car (Red), rent level 1 |
| `0x13`    | Owned by Boat (Blue), rent level 1 |
| `0x21`    | Owned by Car (Red), rent level 2 |
| `0x51`    | Owned by Car (Red), rent level 5 (max) |

---

## Property Address Map

The following table was established by buying specific properties and comparing dumps.

| Property # | EEPROM Offset | Confirmed? |
|-----------|--------------|-----------|
| 1  | `0x51` | ✅ Yes |
| 9  | `0x59` | ✅ Yes |
| 19 | `0x69` | ✅ Yes |
| 21 | `0x71` | ✅ Yes |

Using this we now know that :
`0x51` to `0x59` is property 1-9, 
`0x60` to `0x69` is property 10-19,
`0x70` to `0x72` i propery 20-22
---

## Comparative Dumps Used to Decode This

### Dump 1 — Car (Red) buys properties 1, 19, 21, 09 — all rent level 1

```
00:  05 30 9E 7F 32 BE B8 12 08 00 FF FF FF FF FF FF
10:  05 2F 89 74 2B AF A2 12 14 00 00 79 FF FF FF FF
20:  05 35 9B 7F 33 B5 B6 12 01 01 01 50 FF FF FF FF
30:  FF 36 8E 7C 37 AC A8 12 06 00 01 50 FF FF FF FF
40:  FF 30 8D 71 36 A3 A7 12 06 01 01 50 FF FF FF FF
50:  00 11 00 00 00 00 00 00 00 11 00 00 00 00 00 00
60:  00 00 00 00 00 00 00 00 00 11 00 00 00 00 00 00
70:  00 11 00 04 00 00 00 00 00 00 00 00 00 00 00 00
80:  FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
...
```

### Dump 2 — Boat (Blue) buys properties 1, 19, 21, 10 — all rent level 1

```
00:  05 30 9E 7F 32 BE B8 12 08 00 FF FF FF FF FF FF
10:  05 2F 89 74 2B AF A2 12 14 00 01 50 FF FF FF FF
20:  05 35 9B 7F 33 B5 B6 12 01 01 01 50 FF FF FF FF
30:  FF 36 8E 7C 37 AC A8 12 06 00 00 79 FF FF FF FF
40:  FF 30 8D 71 36 A3 A7 12 06 01 01 50 FF FF FF FF
50:  00 13 00 00 00 00 00 00 00 00 00 00 00 00 00 00
60:  13 00 00 00 00 00 00 00 00 13 00 00 00 00 00 00
70:  00 13 00 04 00 00 00 00 00 00 00 00 00 00 00 00
80:  FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
...
```

### Dump 3 — Car (Red) buys properties 1, 19, 21, 10 — rent levels 5, 4, 2, 1

```
00:  05 30 9E 7F 32 BE B8 12 08 00 FF FF FF FF FF FF
10:  05 2F 89 74 2B AF A2 12 14 00 00 79 FF FF FF FF
20:  05 35 9B 7F 33 B5 B6 12 01 01 01 50 FF FF FF FF
30:  FF 36 8E 7C 37 AC A8 12 06 00 01 50 FF FF FF FF
40:  FF 30 8D 71 36 A3 A7 12 06 01 01 50 FF FF FF FF
50:  00 51 00 00 00 00 00 00 00 00 00 00 00 00 00 00
60:  11 00 00 00 00 00 00 00 00 41 00 00 00 00 00 00
70:  00 21 00 04 00 00 00 00 00 00 00 00 00 00 00 00
80:  FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
...
```

**Key deduction from Dump 3:**
- `0x51` = `0x51` → property 1, rent level 5, Car ✅
- `0x69` = `0x41` → property 19, rent level 4, Car ✅
- `0x71` = `0x21` → property 21, rent level 2, Car ✅
- `0x59` = `0x11` → property 10, rent level 1, Car ✅

---