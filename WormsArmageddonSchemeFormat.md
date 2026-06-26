# Worms Armageddon Scheme File Format (WSC)

This document describes the Worms Armageddon scheme file format for versions 1 and 2 only.
Version 3 and the extended options section are intentionally excluded.

## Summary

- Signature: 4 bytes, ASCII `SCHM` (hex `53 43 48 4D`)
- Version byte: 1 byte after the signature
- Version 1 size: 221 bytes
- Version 2 size: 297 bytes
- Version 1 includes: header, options, 45 normal weapon records
- Version 2 includes: header, options, 45 normal weapon records, 19 super weapon records

## Data types

- `uint8`: 8-bit unsigned integer
- `sint8`: 8-bit signed integer
- `uint16`: 16-bit unsigned integer, little-endian
- `sint16`: 16-bit signed integer, little-endian
- `fixed-point`: 32-bit signed integer, little-endian, representing value in 1/65536 units
- `bool`: 0 = false, 1 = true
- `tri-state`: 0 = false, 1 = true, 128 (`0x80`) = default

## File layout

### Header

| Offset | Size | Type   | Name      | Description |
|--------|------|--------|-----------|-------------|
| 0x00   | 4    | char[4] | Signature | ASCII `SCHM` |
| 0x04   | 1    | uint8   | Version   | `1` or `2` |

### Options section

| Offset | Size | Type   | Name | Description |
|--------|------|--------|------|-------------|
| 0x05 | 1 | uint8 | Hot-Seat Delay | Seconds between turns for seat switching/planning |
| 0x06 | 1 | uint8 | Retreat Time | Seconds after firing while grounded |
| 0x07 | 1 | uint8 | Rope Retreat Time | Seconds after firing while roping |
| 0x08 | 1 | bool | Display Total Round Time | Show total round time with turn time |
| 0x09 | 1 | bool | Automatic Replays | Automatic replay of significant endings |
| 0x0A | 1 | uint8 | Fall Damage | Damage from hitting ground at critical velocity |
| 0x0B | 1 | bool | Artillery Mode | Worms cannot move by walking/jumping |
| 0x0C | 1 | bool | Bounty Mode | Unused by game; editor can use as custom marker |
| 0x0D | 1 | enum | Stockpiling Mode | `0`=Off, `1`=On, `2`=Anti |
| 0x0E | 1 | enum | Worm Select | `0`=Sequential, `1`=On, `2`=Random |
| 0x0F | 1 | enum | Sudden Death Event | `0`=Round ends, `1`=Nuclear strike, `2`=HP=1, `3`=Nothing |
| 0x10 | 1 | uint8 | Water Rise Rate | Water rise speed in sudden death |
| 0x11 | 1 | sint8 | Weapon Crate Probability | Relative chance for weapon crate contents |
| 0x12 | 1 | bool | Donor Cards | Drop donor card on defeat |
| 0x13 | 1 | sint8 | Health Crate Probability | Relative chance of health crate drop |
| 0x14 | 1 | uint8 | Health Crate Energy | Energy granted by health crate |
| 0x15 | 1 | sint8 | Utility Crate Probability | Relative chance of utility crate drop |
| 0x16 | 1 | uint8 | Hazardous Object Types | Which hazards appear on landscape |
| 0x17 | 1 | sint8 | Mine Delay | `4` or `0x80-0xFF` = random 0-3 seconds; otherwise seconds |
| 0x18 | 1 | bool | Dud Mines | Some mines may fail to explode |
| 0x19 | 1 | bool | Manual Worm Placement | Player places worms manually |
| 0x1A | 1 | uint8 | Initial Worm Energy | `0`=all worms die; otherwise energy amount |
| 0x1B | 1 | sint8 | Turn Time | `0x00-0x7F` seconds; `0x80-0xFF` large count-up values |
| 0x1C | 1 | sint8 | Round Time | `0`=sudden death start, `0x01-0x7F` minutes, `0x80-0xFF` seconds |
| 0x1D | 1 | uint8 | Number of Wins | Wins required to win match |
| 0x1E | 1 | bool | Blood | Use red blood particles |
| 0x1F | 1 | bool | Aqua Sheep | Convert super sheep to aqua sheep |
| 0x20 | 1 | bool | Sheep Heaven | Exploding sheep jump from crates |
| 0x21 | 1 | bool | God Worms | Infinite health for worms |
| 0x22 | 1 | bool | Indestructible Land | Landscape cannot be destroyed |
| 0x23 | 1 | bool | Upgraded Grenade | Grenades are more powerful |
| 0x24 | 1 | bool | Upgraded Shotgun | Shotgun fires 2 consecutive shots |
| 0x25 | 1 | bool | Upgraded Clusters | Clusters contain more bomblets |
| 0x26 | 1 | bool | Upgraded Longbow | Longbows are more powerful |
| 0x27 | 1 | bool | Team Weapons | Use preselected team weapons |
| 0x28 | 1 | bool | Super Weapons | Allow super weapons in crates |

### Weapon record format

Each weapon record is 4 bytes:

| Offset within record | Size | Type | Name |
|----------------------|------|------|------|
| 0 | 1 | uint8 | Ammunition |
| 1 | 1 | uint8 | Power |
| 2 | 1 | uint8 | Delay |
| 3 | 1 | uint8 | Probability |

- `Ammunition`: `10` or `0x80-0xFF` = unlimited
- `Delay`: `0x80-0xFF` = unlimited delay

### Version 1 weapon table

- Starts at offset `0x29`
- Contains 45 weapon records, one per weapon
- Normal weapon record offset = `0x29 + 4 * index`

| Index | Offset | Weapon |
|-------|--------|--------|
| 0 | 0x29 | Bazooka |
| 1 | 0x2D | Homing Missile |
| 2 | 0x31 | Mortar |
| 3 | 0x35 | Grenade |
| 4 | 0x39 | Cluster Bomb |
| 5 | 0x3D | Skunk |
| 6 | 0x41 | Petrol Bomb |
| 7 | 0x45 | Banana Bomb |
| 8 | 0x49 | Handgun |
| 9 | 0x4D | Shotgun |
| 10 | 0x51 | Uzi |
| 11 | 0x55 | Minigun |
| 12 | 0x59 | Longbow |
| 13 | 0x5D | Airstrike |
| 14 | 0x61 | Napalm Strike |
| 15 | 0x65 | Mine |
| 16 | 0x69 | Fire Punch |
| 17 | 0x6D | Dragon Ball |
| 18 | 0x71 | Kamikaze |
| 19 | 0x75 | Prod |
| 20 | 0x79 | Battle Axe |
| 21 | 0x7D | Blowtorch |
| 22 | 0x81 | Pneumatic Drill |
| 23 | 0x85 | Girder |
| 24 | 0x89 | Ninja Rope |
| 25 | 0x8D | Parachute |
| 26 | 0x91 | Bungee |
| 27 | 0x95 | Teleport |
| 28 | 0x99 | Dynamite |
| 29 | 0x9D | Sheep |
| 30 | 0xA1 | Baseball Bat |
| 31 | 0xA5 | Flame Thrower |
| 32 | 0xA9 | Homing Pigeon |
| 33 | 0xAD | Mad Cow |
| 34 | 0xB1 | Holy Hand Grenade |
| 35 | 0xB5 | Old Woman |
| 36 | 0xB9 | Sheep Launcher |
| 37 | 0xBD | Super Sheep |
| 38 | 0xC1 | Mole Bomb |
| 39 | 0xC5 | Jet Pack |
| 40 | 0xC9 | Low Gravity |
| 41 | 0xCD | Laser Sight |
| 42 | 0xD1 | Fast Walk |
| 43 | 0xD5 | Invisibility |
| 44 | 0xD9 | Damage x2 |

### Version 2 super weapon table

- Starts at offset `0xDD`
- Contains 19 additional weapon records
- Super weapon record offset = `0xDD + 4 * (index - 45)`

| Index | Offset | Weapon |
|-------|--------|--------|
| 45 | 0xDD | Freeze |
| 46 | 0xE1 | Super Banana Bomb |
| 47 | 0xE5 | Mine Strike |
| 48 | 0xE9 | Girder Starter Pack |
| 49 | 0xED | Earthquake |
| 50 | 0xF1 | Scales Of Justice |
| 51 | 0xF5 | Ming Vase |
| 52 | 0xF9 | Mike's Carpet Bomb |
| 53 | 0xFD | Patsy's Magic Bullet |
| 54 | 0x101 | Indian Nuclear Test |
| 55 | 0x105 | Select Worm |
| 56 | 0x109 | Salvation Army |
| 57 | 0x10D | Mole Squadron |
| 58 | 0x111 | MB Bomb |
| 59 | 0x115 | Concrete Donkey |
| 60 | 0x119 | Suicide Bomber |
| 61 | 0x11D | Sheep Strike |
| 62 | 0x121 | Mail Strike |
| 63 | 0x125 | Armageddon |

## Notes for implementation

- Version 1 files end immediately after the 45 normal weapon records at offset `0xDD`.
- Version 2 files continue with 19 super weapon records from offset `0xDD` through `0x125`.
- Do not parse or generate version 3 extended options for the current implementation.
- Use the header version byte to decide whether the file contains super weapons.
- Always parse integers as little-endian.
