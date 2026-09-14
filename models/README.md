# Del Norte RP � custom models (open.mp artwork)

The server loads `models/artconfig.txt` at startup. Clients download these files over HTTP from the **artwork web server** (default port **8889**, see `config.json` ? `artwork.port`).

## Required files

Place these under the server root (same folder as `config.json`):

| File | Used for |
|------|----------|
| `models/phone.dff` | Phone UI (models -29994 / -29995) |
| `models/phone.txd` | Phone textures |
| `models/phone_nice.txd` | Phone extra textures |
| `models/object.dff` | Login UI backdrop (model -9531, `serverui.txd`) |
| `models/serverui.txd` | Login / auth textdraw skins (`mdl-9531:...`) |
| `models/license.dff` | License object (model -30002) |
| `models/license.txd` | License texture |
| `models/fuel/pump_none.dff` | Fuel pumps / GUI |
| `models/fuel/pump_ron.dff` | Ron pump |
| `models/fuel/pump_globeoil.dff` | Globeoil pump |
| `models/fuel/pump_terroil.dff` | Terroil pump |
| `models/fuel/pumps.txd` | Pump textures |
| `models/fuel/fuel.txd` | Fuel UI |
| `models/fuel/jerrycan.txd` | Jerrycan texture |

**This repository may not ship real `.dff` / `.txd` assets.** Copy them from your last working server backup.

**Common mistake:** `phone.dff`, `object.dff`, and `license.dff` must be **three different files**. If you copied one file three times (same file size, e.g. all 8279 bytes), the open.mp launcher reports download/model errors. Run `tools\verify_artmodels.ps1` � it checks for duplicate placeholders.

## Verify before starting the server

```powershell
cd C:\Users\oldro\OneDrive\Desktop\dnrpof
.\tools\verify_artmodels.ps1
```

On server start, the gamemode also prints `[ARTMODELS] MISSING: ...` for any absent file.

## If the open.mp launcher shows �(7) errors�

That usually means **7 model files failed** (missing on disk or download blocked). Fix in order:

1. **Put all files in the table above** into `models/` (run the verify script).
2. **Restart the server** after adding files (so CRCs are rebuilt).
3. **Open firewall port 8889** (TCP) on the host � same machine as the game port.
4. If players connect from the **internet**, set your public IP in `config.json`:

   ```json
   "network": {
       "public_addr": "YOUR.PUBLIC.IP.HERE",
       ...
   }
   ```

   Without this, remote clients may try to download from the wrong address (console: `No public address provided`).

5. **Local testing only:** connect to `127.0.0.1:8888` with artwork enabled; downloads use `http://127.0.0.1:8889/`.

6. Optional: host files on a CDN and set `artwork.cdn` to e.g. `https://dnrp.ge/models/` (trailing slash required).

## �Model -29994 is already in use� in server log

Harmless **open.mp quirk**: `artconfig.txt` is parsed at server boot and again when the gamemode loads. IDs are already registered, so the second pass logs errors. **Do not** call `AddSimpleModel()` in Pawn for the same IDs.

## Client cache

After fixing server files, players may need to delete stale cache:

`Documents\GTA San Andreas User Files\SAMP\cache\<server-ip-port>\`

Then reconnect and wait for downloads to finish before spawning.
