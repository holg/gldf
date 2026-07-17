# Release Notes v1.0.0-rc.4

This release combines a backwards-compatible spelling cleanup with three functional extensions: modern lighting control protocols, per-channel rated input power (issue [#31](https://github.com/globallightingdata/gldf/issues/31)), and decimal MacAdam ellipse steps (issue [#32](https://github.com/globallightingdata/gldf/issues/32)). **All rc.3 instance files validate unchanged.**

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

## Modern Lighting Control Protocols

Adds six standardized lighting control protocols to the `ControlGear/Interfaces/Interface` enumeration. The previous list reflected the protocol landscape from before mesh networking and IP-based control became common in commercial and consumer lighting.

| Value | Standard / Body | Notes |
|---|---|---|
| `Zigbee` | Zigbee 3.0, Connectivity Standards Alliance (CSA) | IEEE 802.15.4 mesh; widely deployed (Philips Hue, IKEA Tradfri, Ledvance) |
| `Matter` | Matter, Connectivity Standards Alliance (CSA) | IP-based, cross-vendor consumer/commercial standard |
| `Thread` | Thread Group, IEEE 802.15.4 | IPv6 mesh transport; commonly used as a transport for Matter |
| `Bluetooth Mesh` | Bluetooth SIG Mesh Profile (since 2017) | Provisioned many-to-many BLE mesh with standardized lighting models. **Distinct from** the existing `Bluetooth` value, which describes point-to-point BLE |
| `DALI-2` | IEC 62386 Edition 2, DiiA-certified | Bidirectional digital lighting bus with extended addressing |
| `DALI D4i` | DiiA D4i specification | Intra-luminaire data + outdoor extensions on top of DALI-2 |

Each new value carries an `xs:documentation` annotation pointing to the relevant standard. The list is restricted to **formal standards from recognized bodies**; vendor-proprietary mesh implementations (Casambi, CSRmesh, …) are deliberately excluded — `Bluetooth Mesh` is the appropriate value for any SIG-qualified product.

Additionally, the `Interface` element's `maxOccurs="12"` — which exactly matched the original enum count and was effectively arbitrary — is lifted to `maxOccurs="unbounded"`.

---

## Per-Channel Rated Input Power (issue [#31](https://github.com/globallightingdata/gldf/issues/31))

`MultiChannelLightSource/Channels/Channel` gains an optional `RatedInputPower` element, placed after `RatedLuminousFlux`.

Previously, the only power figure for a multichannel light source was the `RatedInputPower` of the whole `MultiChannelLightSource`. For RGBW and tunable-white luminaires, per-channel power could only be estimated by prorating the total via the channels' luminous flux ratios — which is physically incorrect (channel efficacies differ). Manufacturers resorted to encoding the wattage in the channel's `DisplayName`.

```xml
<Channel type="Red">
    <DisplayName>…</DisplayName>
    <SpectrumReference spectrumId="…" />
    <PhotometryReference photometryId="…" />
    <RatedLuminousFlux>350</RatedLuminousFlux>
    <RatedInputPower>4.8</RatedInputPower>   <!-- NEW, optional, watt -->
    <ColorInformation>…</ColorInformation>
</Channel>
```

The type mirrors the existing `RatedInputPower` elements elsewhere in the schema (`xs:double`, `minInclusive 0`, unit watt). The documentation notes explicitly that the sum of channel powers may differ from the light source's total `RatedInputPower` (driver losses, limits on channel combinations).

---

## Decimal MacAdam Ellipse Steps (SDCM) (issue [#32](https://github.com/globallightingdata/gldf/issues/32))

`InitialColorTolerance` and `MaintainedColorTolerance` change from a closed integer enumeration (`1 SDCM` … `7 SDCM`) to the new shared `ColorTolerance` type: an `xs:string` restricted by the pattern

```
[0-9](\.[0-9]{1,2})? SDCM
```

i.e. the same `"N SDCM"` lexical format as before, now accepting any decimal step with up to two fraction digits (`"1.5 SDCM"`, `"1.47 SDCM"`, `"3 SDCM"`).

**Motivation.** Fine-binned LED modules are commonly specified at half-step MacAdam ellipses (1.5 SDCM in particular), which the integer enumeration could not express. Rather than adding `1.5`/`2.5`/`3.5` as enum values — and re-opening the issue for every finer step a manufacturer specifies next — the value space is opened to decimals once and for all, while the pattern still enforces the exact lexical format (decimal point, single space, `SDCM` suffix — `"1,5 SDCM"`, `"3SDCM"` and free text remain invalid).

**Refactoring.** The previously identical inline enumerations at all four occurrences are consolidated into one named `xs:simpleType` `ColorTolerance`:

- `ChangeableLightSource/ColorInformation`
- `FixedLightSource/ColorInformation`
- `MultiChannelLightSource/ColorInformation`
- `MultiChannelLightSource/Channels/Channel/ColorInformation`

**Note for consumers.** Every previously valid value matches the pattern, so all existing files validate unchanged. However, software that mapped the seven enumeration values to a closed list (e.g. a code-generated enum type) must switch to parsing the numeric part.

---

## Backwards Compatibility (whole release)

✅ **All changes are purely additive or format-preserving.** New elements are `minOccurs="0"`; enumeration values are added, none removed; the SDCM pattern accepts every previously valid value.
✅ Representative rc.3-era example files from [globallightingdata/examples](https://github.com/globallightingdata/examples) validate unchanged against this schema.

---

## Summary

| Category | Changes |
|----------|---------|
| 🚀 New Feature | 6 standardized control protocols added to the `Interface` enumeration |
| 🚀 New Feature | Optional `RatedInputPower` on `Channel` (issue #31) |
| 🚀 New Feature | `InitialColorTolerance`/`MaintainedColorTolerance` accept decimal SDCM steps via new pattern-based `ColorTolerance` type (issue #32) |
| 💥 Breaking Changes | None (schema-level; consumers with hardcoded SDCM value lists must parse the numeric part instead) |
| 💅 Polish | Spelling cleanup across documentation, one enumeration, and one element name; `Interface` `maxOccurs` cap lifted; 8 duplicated inline SDCM enumerations consolidated into one named type |
| 🐛 Bug Fix | - |
| ⚠️ Deprecations | 2 enum values + 1 element name (removal scheduled for 2.0.0) |
