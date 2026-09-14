Fuel models for DNRP
=====================

Keep these files in this folder:  models/fuel/

  pump_none.dff
  pump_ron.dff
  pump_globeoil.dff
  pump_terroil.dff
  pumps.txd
  fuel.txd
  jerrycan.txd

The server loads them via models/artconfig.txt using paths like:
  fuel/pump_none.dff
  fuel/pumps.txd

Do NOT copy into models/ root — the fuel/ subfolder is required.

Do NOT call AddSimpleModel() in the gamemode for these IDs; artconfig already registers them.
