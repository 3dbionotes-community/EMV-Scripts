EMV-Scripts

Version 0.3.0

This GIT repo gather all scripts that run in finlay.cnb.csic.es under user bioinfo
to automatically validate new entries from EMDB+PDB.

Currently there are 6 methods implemented:
- MAPQ
- FSCQ
- DAQ
- DEEPRES
- MONORES
- LOCALRES

Every week, a new entries list from EMDB+PDB is collected and a set of jobs are 
launched through SLURM to run the corresponding scripts.

A set of commands are run throgh crontab at different times, according to:
- crontab.txt

Some of these commnands also take care of cleaning intermediate results and big files that might clutter the HD.

Every week, after all the corresponding computations have finished, basic statistics are collected
that indicate how well a particular entry is performing in relation with the rest of the database.

Results are stored under /home/bioinfo/services/emv/data/emdbs/emd-*****
The JSON files emd-<EMDB_ID>_<PDB_ID>_emv_<method>.json files are automatically and regularl retrieved from Campins (also from Rinchen-dos for dev/testing purposes). This allow to populate the corresponding WebServices (BWS) that will allow to show the _Validation and Quality_ tracks in 3DBionotes.

## Notes

### Chimera

For some PDBs, we got an error on the calculation of Q-score. The problem is at `~/.local/UCSF-Chimera64-1.14/share/mapq/qscores.py`, line 1786:

```
if at.isBB :
```

Some entries do not have this property, so we need to check the property existence first:

```
if hasattr(at, 'isBB') and at.isBB:
```

Note: This folder `mapq` is not part of the UCSF Chimera core, but an external plugin (mapq):

https://github.com/gregdp/mapq/blob/v1.6/mapq_chimera/qscores.py#L1905
