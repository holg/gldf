# Release Notes v1.0.0-rc.4

## Spelling Cleanup with Backwards-Compatible Deprecations

This release corrects long-standing spelling mistakes in the GLDF schema. All previously valid rc.3 instances continue to validate against rc.4 — the misspelled forms remain accepted but are marked deprecated and scheduled for removal in GLDF 2.0.0.

### Documentation-string fixes

Pure text corrections to `<xs:documentation>` annotations. No schema-shape impact.

| Location | Before | After |
|---|---|---|
| `Photometry` documentation | `descriptive informations` | `descriptive information` |
| EU energy label documentation (3 occurrences) | `accoring to the energy consumption labelling scheme` | `according to the energy consumption labelling scheme` |
| `ControlGear` id documentation | `Unique identiofier of the control gear` | `Unique identifier of the control gear` |
| `TemperatureRange` documentation | `Definition of a remperature range, with an inclusive highest and lowest temparature value` | `Definition of a temperature range, with an inclusive highest and lowest temperature value` |

### Enumeration value cleanup (`Cie97LampType`)

Two misspelled enum values were corrected. Both old and new spellings are now valid.

| Old (deprecated) | New (preferred) |
|---|---|
| `Flourescent Triphosphor` | `Fluorescent Triphosphor` |
| `Flourescent Halophosphate` | `Fluorescent Halophosphate` |

### Element-name cleanup (`RatedChromacityCoordinateValues`)

The misspelled element name was corrected. Both old and new names are accepted via `<xs:choice>` wrappers; the deprecated form is marked for removal in 2.0.0. Affects 4 occurrences across `ChangeableLightSource`, `FixedLightSource`, `MultiChannelLightSource`, and `Emitter`.

| Old (deprecated) | New (preferred) |
|---|---|
| `RatedChromacityCoordinateValues` | `RatedChromaticityCoordinateValues` |

### Backwards Compatibility

✅ **All rc.3 files validate against rc.4 unchanged.**
✅ **No required elements added, no enumerations removed, no element renames without alias.**
✅ **No breaking changes.**

### Deprecations (scheduled for removal in 2.0.0)

Producers should emit the corrected forms going forward. Consumers must continue to accept both. Each deprecated form carries an `xs:appinfo` marker:

```xml
<xs:appinfo>
    <deprecated since="1.0.0-rc.4" removeIn="2.0.0" replacement="…"/>
</xs:appinfo>
```

| Deprecated form | Replacement | Removal target |
|---|---|---|
| Enum value `Flourescent Triphosphor` | `Fluorescent Triphosphor` | 2.0.0 |
| Enum value `Flourescent Halophosphate` | `Fluorescent Halophosphate` | 2.0.0 |
| Element `RatedChromacityCoordinateValues` | `RatedChromaticityCoordinateValues` | 2.0.0 |

---

## Summary

| Category | Changes |
|----------|---------|
| 🚀 New Feature | - |
| 💥 Breaking Changes | None |
| 💅 Polish | Spelling cleanup across documentation, one enumeration, and one element name |
| 🐛 Bug Fix | - |
| ⚠️ Deprecations | 2 enum values + 1 element name (removal scheduled for 2.0.0) |
