# Release Notes v1.0.0-rc.4

## 🚀 New Feature - EU Digital Product Passport (DPP) Support

Adds comprehensive support for the EU Digital Product Passport regulation (ESPR), preparing GLDF for 2026+ compliance requirements.

### New Complex Types

| Type | Description | Commit |
|------|-------------|--------|
| `DigitalProductPassport` | DPP ID, data carrier URI, QR code type | 7afe3f9 |
| `CarbonFootprint` | Total CO2e, lifecycle phases, EPD references | 7afe3f9 |
| `MaterialComposition` | Total weight, materials list, recycled content | 7afe3f9 |
| `Circularity` | Disassembly time/instructions, recycling guidance | 7afe3f9 |
| `Durability` | Expected lifespan, warranty years/terms | 7afe3f9 |
| `Repairability` | EU score (1-10), spare parts, availability | 7afe3f9 |
| `Compliance` | RoHS, REACH, CE declaration, SVHC substances | 7afe3f9 |
| `Traceability` | Country of manufacture, plant, date, batch | 7afe3f9 |

### Extended Elements

| Element | New Children | Commit |
|---------|--------------|--------|
| `Header` | `TakeBackProgram` | 7afe3f9 |
| `ProductMetaData` | `Compliance`, `Repairability` | 7afe3f9 |
| `Variant` | `DigitalProductPassport`, `CarbonFootprint`, `WaterConsumption`, `MaterialComposition`, `Circularity`, `Durability`, `Repairability`, `Compliance`, `Traceability`, `LuminaireEnergyLabel` | 7afe3f9 |

### Backwards Compatibility

✅ **All new elements are optional** (`minOccurs="0"`)
✅ **Existing rc.3 files validate against rc.4**
✅ **No breaking changes**

### DPP Element Summary by Priority

| Priority | Count | Elements |
|----------|-------|----------|
| 🔴 HIGH | 14 | GTIN, DPP ID, CarbonFootprint, MaterialComposition, RecycledContent, Recyclability, Lifespan, Warranty, RepairabilityScore, SparePartsAvailability, RoHS, REACH, CE, HazardousSubstances |
| 🟡 MEDIUM | 12 | DataCarrierURI, CarbonFootprintByPhase, EPD, DisassemblyTime/Instructions, RecyclingInstructions, TakeBackProgram, RepairInstructions, SpareParts, CountryOfManufacture, EnergyLabel |
| 🟢 LOW | 3 | WaterConsumption, ManufacturingPlant, ManufacturingDate |
| ✅ EXISTS | 2 | LightSource lifetime, LightSource EnergyLabels |

### References

- [EU ESPR Regulation](https://environment.ec.europa.eu/topics/circular-economy/ecodesign-sustainable-products-regulation_en)
- [ISO 14067:2018 - Carbon Footprint](https://www.iso.org/standard/71206.html)
- [RoHS Directive 2011/65/EU](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32011L0065)
- [REACH Regulation (EC) 1907/2006](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:02006R1907-20221217)

---

## Summary

| Category | Changes |
|----------|---------|
| 🚀 New Feature | EU Digital Product Passport (DPP) support - 8 new complex types, 10+ new elements |
| 💥 Breaking Changes | None |
| 💅 Polish | - |
| 🐛 Bug Fix | - |
