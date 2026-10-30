# SAFra methodology (v0.1)

## Scope

- Attributional, well-to-wake life cycle GHG assessment (ISO 14040/44 principles).
- Functional unit: 1 MJ of SAF (lower heating value).
- Characterisation: IPCC AR6 GWP100, as in the underlying inventories.
- Multifunctionality: energy (LHV) allocation, consistent with RED III and CORSIA practice.
- Biogenic CO₂ from fermentation and fuel combustion is treated as neutral.

## Stages

| Stage | Description |
|---|---|
| Field N₂O | Direct and indirect N₂O from synthetic and organic N (incl. vinasse, filter cake, residues) |
| Field CO₂ (lime, urea) | CO₂ from limestone and urea applied to soil |
| Production of agricultural inputs | Fertilisers, limestone, gypsum, seeds, pesticides |
| Diesel in field operations | Fuel combustion in farm operations (cane: includes cutting, loading and transport, CCT) |
| Straw burning | CH₄ and N₂O from pre-harvest burning (cane) |
| Feedstock transport | Corn haulage to the mill (50 km stand-alone; ~200 km multipurpose mill) |
| Ethanol plant utilities | Steam and power from eucalyptus chips (stand-alone corn) or surplus bagasse (integrated mills) |
| Ethanol plant chemicals and enzymes | Process chemicals, enzymes, yeast |
| ATJ conversion utilities | Dehydration, oligomerisation, hydrotreating and fractionation utilities, excluding H₂ |
| Hydrogen | H₂ for hydrotreating, five supply routes |
| SAF transport and distribution | Transport to the airport |
| Land-use change (optional) | CORSIA ILUC, BRLUC dLUC or consequential LUC |

## Equations

Ethanol energy per tonne of feedstock:

```
E_eth = Y_eth [L/t] × 0.789 [kg/L] × 26.8 [MJ/kg] / 1000          (GJ/t)
```

Energy allocation factor to ethanol:

```
cane:  a = 26.8 / (26.8 + 1.2 × 3.6)                        (surplus power 1.2 kWh/kg EtOH)  → 0.86
corn:  a = E_eth / (E_eth + 0.363 × 18.0 + 0.013 × 37.0)     (DDGS and corn oil)              → 0.56
```

Agricultural stage contribution (for each category *k* from Pereira et al. 2025, kg CO₂eq/t):

```
CI_k = (A_k / E_eth) × a × 1.01                              (g CO₂eq/MJ SAF)
```

Industrial ethanol stages (g/MJ ethanol, before allocation) are multiplied by `a × 1.01`. ATJ utilities (3.0 g/MJ) and SAF distribution (0.6 g/MJ) are added per MJ of SAF. For US corn and user-defined ethanol, the ethanol CI is entered directly and multiplied by 1.01.

Hydrogen:

```
CI_H2 = h × EF_H2 × 1000                                     (h in kg H₂/MJ SAF; default 0.5 g/MJ)

EF_SMR      = 11.0 kg CO₂eq/kg H₂
EF_PEM      = kWh_per_kg × EF_grid + 0.5                      (default 55 kWh/kg; EF_grid in kg/kWh)
EF_PEM,bag  = kWh_per_kg × 0.015 + 0.5
EF_PEM,wind = 1.8
EF_EtOH-SR  = (120 / 0.65) × CI_ethanol / 1000 + 2 × EF_grid  (ethanol consumed at its own CI)
```

Break-even grid factor between PEM and ethanol reforming:

```
EF_grid* = (EF_EtOH-SR − 0.5) / kWh_per_kg
```

Ethanol diverted to hydrogen (ethanol reforming): `h × 120 / 0.65` MJ ethanol per MJ SAF (≈ 9% at 0.5 g/MJ).

Total:

```
CI_SAF = Σ stages + LUC
Saving_CORSIA = 1 − CI_SAF / 89          Saving_RED = 1 − CI_SAF / 94
```

## Land-use change options

| Option | Cane | Corn 2nd crop | Corn 1st crop | US corn |
|---|---|---|---|---|
| CORSIA ILUC (g/MJ) | 8.7 | 9.3 (BR sequential, provisional) | 26.2 (global) | 18.3 |
| dLUC – BRLUC (Pereira et al. 2025) | per state | per state | per state | n/a |
| Consequential (Moreira et al. 2020) | n/a | −4.7 g/MJ ethanol | n/a | n/a |

## Validation

US corn ethanol (45 g CO₂eq/MJ; Lee et al. 2021) processed with the SAFra ATJ module and NG SMR hydrogen gives 54.6 g CO₂eq/MJ SAF, in line with the CORSIA core default for US corn-ethanol ATJ (54.1) and de Jong et al. 2017 (54.0). Brazilian corn ethanol at plant gate (18.9–20.5 g/MJ) is consistent with Moreira et al. 2020 under energy allocation (22.8 g/MJ).

## Known limitations

- Screening model: several industrial parameters are assumptions (see `data/model_parameters.csv`).
- Sugarcane yield and surplus electricity are fixed (86 L/t; 1.2 kWh/kg ethanol); no straw recovery or 2G ethanol yet.
- Agricultural values are medians; uncertainty ranges are not yet propagated.
- Only GHG emissions; other impact categories are planned.

## References

- Cavalett O. et al. (2013) Int. J. Life Cycle Assess. 18, 647–658.
- de Jong S. et al. (2017) Biotechnology for Biofuels 10, 64.
- Donke A.C.G. et al. (2017) Resources 6, 1.
- ICAO (2025) CORSIA Default Life Cycle Emissions Values for CORSIA Eligible Fuels, June 2025.
- Klein B.C. et al. (2018) Applied Energy 209, 290–305.
- Lee U. et al. (2021) Biofuels, Bioproducts and Biorefining, DOI 10.1002/bbb.2225.
- Macedo I.C., Seabra J.E.A., Silva J.E.A.R. (2008) Biomass and Bioenergy 32, 582–595.
- Moreira M.M.R. et al. (2020) Nature Sustainability 3, 209–216.
- Pereira L.G. et al. (2025) Sustainability 17, 8482, DOI 10.3390/su17188482.
- Picciotto Maniscalco M. et al. (2024) Environments 11, 108.
- Silva C.R.U. et al. (2014) Environmental Science & Technology 48, 12394–12402.
- Wang M. et al. (2025) Life Cycle Analysis of Greenhouse Gas Emissions of Clean Fuels with the R&D GREET 2024 Model, ANL/ESIA-24/21.
