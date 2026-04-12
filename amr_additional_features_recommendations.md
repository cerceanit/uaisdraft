# Recommended Additional Features for AMR Prediction Models

This document outlines additional features that could improve antimicrobial resistance (AMR) prediction models, particularly those using macroeconomic proxies for zero-shot country-level predictions.

---

## Current Baseline Features

Most AMR prediction models use:
- Organism (pathogen species)
- Antibiotic / antibiotic class
- Year
- GDP per capita
- Health expenditure (% of GDP)
- Population density
- Antibiotic consumption (DDD per 1,000 inhabitants per day)

---

## High Priority Additions

These features have strong theoretical links to AMR and data is readily available:

| Feature | Rationale | Data Source |
|---------|-----------|-------------|
| Agricultural antibiotic use | Major driver of resistance, often overlooked in human-focused models | OIE, FAO |
| Over-the-counter antibiotic availability | Directly affects misuse and self-medication rates | WHO, published indices |
| Access to improved sanitation (%) | Affects infection rates and subsequent antibiotic need | World Bank |
| Average temperature | Warmer climates associated with higher resistance rates | Climate databases |

### Why These Matter

**Agricultural use** is particularly important because:
- 70%+ of antibiotics globally are used in animals
- Resistance genes transfer between animal and human pathogens
- Especially relevant for *E. coli*, *Salmonella*, *Campylobacter*

**Temperature** has empirical support:
- MacFadden et al. (2018) found a 10°F increase associated with 4.2%, 2.2%, and 2.7% increases in antibiotic resistance for *E. coli*, *K. pneumoniae*, and *S. aureus* respectively
- Mechanism: faster bacterial growth, increased horizontal gene transfer

---

## Medium Priority Additions

| Feature | Rationale | Data Source |
|---------|-----------|-------------|
| Hospital bed density | Proxy for healthcare-associated infection burden | World Bank |
| Physician density | Healthcare access and prescribing patterns | WHO |
| Elderly population (%) | Higher healthcare contact = more antibiotic exposure | World Bank |
| Access to clean water (%) | Affects diarrheal disease burden and antibiotic use | World Bank |
| Urbanization rate | Urban areas have different resistance patterns than rural | World Bank |

---

## Lower Priority / Harder to Obtain

| Feature | Rationale | Data Source |
|---------|-----------|-------------|
| GINI coefficient / income inequality | Inequality affects healthcare access disparities | World Inequality Database (wid.world), World Bank |
| International travel volume | Resistance gene flow between countries | UNWTO |
| National AMR action plan existence | Policy indicator | WHO |
| Antibiotic stewardship program maturity | Institutional capacity for appropriate use | Published surveys |
| Livestock density | Agricultural AMR pressure | FAO |

---

## Implementation Notes

### Data Availability
- Most World Bank indicators available via API: `wbdata` Python package
- WHO Global Health Observatory: `who` Python package or direct API
- FAO data: FAOSTAT database

### Feature Engineering Tips
- Log-transform skewed features (GDP, consumption, density)
- Consider interactions: `temperature × antibiotic_consumption`
- Lag features may help: resistance often follows consumption changes by 1-2 years

### Potential Issues
- Missing data for some countries/years - use forward/backward fill or imputation
- Multicollinearity between socioeconomic indicators - consider PCA or feature selection
- Temporal alignment - ensure all features match the year of AMR observation

---

## References

- MacFadden DR, et al. (2018). Antibiotic resistance increases with local temperature. *Nature Climate Change*.
- World Bank Open Data: https://data.worldbank.org
- WHO Global Health Observatory: https://www.who.int/data/gho
- World Inequality Database: https://wid.world
- FAOSTAT: https://www.fao.org/faostat
