# Encrypted DVD Backups on Linux

A simple, offline, **offsite backup** for irreplaceable files.

#### What you need
- A DVD burner and DVD-R discs from a good brand (Verbatim, Taiyo Yuden). Avoid DVD-RW for long-term storage: it can be overwritten and tends to age worse.
- `p7zip-full` and `par2` (`sudo apt install p7zip-full par2`)
- Xfburn (or any burning program you trust)
- A password made from 6+ random **Diceware** words

#### Encrypt
Create one archive per folder, each under about 4 GB so it fits on a disc:

```
7z a -t7z -mhe=on -p archive.7z folder/
```

- `-t7z` uses AES-256. Don't use zip: it may fall back to weak encryption.
- `-mhe=on` also encrypts file names. Without it, anyone can see what's inside.
- `-p` with nothing after it makes 7z ask for the password, so it doesn't end up in your shell history.
- Several smaller archives are better than one big one: a single bad spot then costs you less.

#### Add repair data (optional but recommended)
```
par2 create -r10 archive.7z
```
This creates recovery files that can repair small disc damage. Burn them next to the archive.

#### Checksums
```
sha256sum archive.7z > SHA256SUMS
```

#### Burn
In Xfburn: new data composition, add the files, burn at 1-4x speed with default settings, and let it finalize the disc.

#### Verify (don't skip)
Mount the disc, ideally in a different drive than the one that burned it, then:
```
cd /media/$USER/DISCNAME
sha256sum -c SHA256SUMS
7z t archive.7z
```
`7z t` checks the whole archive without extracting it.

#### Label and store
- Write the year and contents with a soft-tip marker on the printed side. Never write the password on the disc or case.
- Use polypropylene sleeves or jewel cases, kept upright in a cool, dry, dark place. Add silica gel if it's damp.
- Keep at least one copy somewhere else than your home.

#### Keep it working
- Store the password in a password manager and on paper at home, with the exact format (spaces, dashes, capitals).
- Once a year, when you burn the new discs, read back one old disc and test-extract it.
- Keep a working burner or reader around: the discs may outlast the drives.
