# Input Manifest - KoopHuurBepaling

**Source System:** on-prem  
**Scenario:** KoopHuurBepaling  
**Analysis Date:** 2026-03-19  

## Input Files Overview

| # | File Name | Type | Purpose | Status |
|---|-----------|------|---------|--------|
| 1 | `1. Stored Procedure voor vullen 2 dimensies DWH Dim Koophuurbepaling (onPrem).sql` | SQL Script | Core SP for populating Dim_KoopHuurBepaling | ✅ Analyzed |
| 2 | `2. Brontabellen DataStore voor spKoopHuurBepaling (onPrem).sql` | SQL Schema | DataStore source tables (BRP, WOZ) | ✅ Analyzed |
| 3 | `3. Brontabellen DWH voor spKoopHuurBepaling (onPrem).sql` | SQL Schema | DWH target table schema | ✅ Analyzed |
| 4 | `4. DWH Functie convertRD_to_LatLong (onPrem).sql` | SQL Function | RD to Lat/Long coordinate conversion | ✅ Analyzed |
| 5 | `5. Basis.Dim_KoopHuurBepaling (onPrem).dtsx` | SSIS Package | Orchestration and execution | ✅ Analyzed |
| 6 | `6. Toelichting op stored procedure DWH Basis spKoopHuurBepaling (onPrem).docx` | Documentation | Business logic explanation | ⚠️ Converted to MD (pending full read) |
| 7 | `Proces beschrijving verladen data on-premises.docx` | Documentation | Process description | ⚠️ Converted to MD (pending full read) |

## Source Systems

### DataStore (DATASTORE database)
- **BRP_Cloud.Persoon** - Current resident registration
- **BRP_Cloud.verblijfplaats_historie** - Historical residence data
- **BRP_Cloud.huwelijk** - Marriage/partnership data
- **LVBAG.Verblijfsobject** - Building Address Register (BAG) residential objects
- **WOZ.Woz_Object** - Real estate valuation (WOZ) objects
- **WOZ.Afgeleid_Woz_Aot** - Derived WOZ-AOT links
- **WOZ.Belang** - Ownership/user interest relationships
- **WOZ.Natuurlijk_Persoon** - Natural person subjects
- **WOZ.Niet_Natuurlijk_Persoon** - Non-natural person subjects (corporations)
- **WOZ.Vestiging** - Business establishments

### DWH (DWH database)
- **Basis.Dim_KoopHuurBepaling** - Target dimension table
- **Basis.Dim_WozObject** - WOZ object dimension
- **Basis.Dim_VerblijfsObject** - Residential object dimension
- **Basis.Dim_NummerAanduiding** - Address number dimension
- **Basis.Fact_AdresGebruiksdoel** - Address purpose fact table
- **EnergiePaspoort.Dim_KoopHuurBepaling** - Energy passport variant

## Key Observations

1. **No .docx content analyzed yet** - The Word documents were converted to Markdown but not yet fully read due to file path issues
2. **Complex multi-source integration** - Combines BRP (residents), WOZ (valuation), and BAG (addresses)
3. **Historical tracking** - Maintains temporal validity with RowStartDate/RowEndDate
4. **Partnership logic** - Includes marriage/partnership data to determine owner-occupancy
5. **SSIS orchestration** - Package executes SP and manages audit logging
6. **Geocoding** - Includes RD to Lat/Long conversion for spatial analysis

## Data Volume Indicators

Based on SQL comments:
- ~586,297 distinct BSN (person identifiers)
- ~570,872 WOZ objects
- ~119,184 WOZ objects with ownership data
- Target dimension contains ownership/rental classification per time period

## Dependencies

- DWH.dbo.ConvertRD_To_LatLong function (coordinate conversion)
- MetaDataDWH.Audit.ProcessCount (audit logging)
- MetaDataDWH.Audit.usp_UpdatePackageInfo (SSIS audit procedures)
- MetaDataDWH.Audit.usp_UpdateProcessInfo (process tracking)

## Next Steps

1. ✅ Read converted .docx files for business context
2. Extract business rules from documentation
3. Map source-to-target field lineage
4. Identify Silver/Gold transformation requirements
