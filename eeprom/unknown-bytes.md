# EEPROM — Unknown Bytes

We decoded 22 of the 256 bytes (the property region at `0x50–0x72`). The rest remain unknown. This file documents what we observed and our best guesses — contributions welcome.

---

## Region: `0x00–0x09` & `0x10–0x19` & `0x20–0x29` & `0x30–0x39` & `0x40–0x49` (Rows 00–40)

These bytes don't change between sessions.

```
00:  05 30 9E 7F 32 BE B8 12 08 00 FF FF FF FF FF FF
10:  05 2F 89 74 2B AF A2 12 14 00 01 26 FF FF FF FF
20:  05 35 9B 7F 33 B5 B6 12 01 01 01 05 FF FF FF FF
30:  FF 36 8E 7C 37 AC A8 12 06 00 01 40 FF FF FF FF
40:  FF 30 8D 71 36 A3 A7 12 06 01 01 50 FF FF FF FF
```

### Observations

- Bytes at offsets `0x00`, `0x10`, `0x20` are often `05` — could be a row marker or version byte
- The pattern `12` appears repeatedly at `0x07`, `0x17`, `0x27`, `0x37`, `0x47` — possibly a delimiter or fixed header byte

### Our Hypothesis on Player Balances

We tried to find where player money is stored but couldn't isolate it. We tested:
- bringing players to 0 networth
- increasing networth of other players

---

## 📢 Call for Contributions

If you own a Monopoly Ultimate Banking unit and want to help:

1. Read your EEPROM using the method in [`hardware/arduino-setup.md`](../hardware/arduino-setup.md)
2. Try controlled tests (e.g. start with a known balance, buy nothing, read the EEPROM)
3. Open an issue or PR with your dumps and observations

We're especially looking for help with:
- [ ] Where player balances are stored
- [ ] What the `0x00–0x09` region encodes per row
- [ ] What `0x73 = 0x04` means
- [ ] Whether the upper nibble of row-header bytes means anything