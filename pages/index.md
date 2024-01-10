---
title: Welcome to Evidence
---

## Consommation électrique par région

```sql years
select distinct CAST(CAST(annee as INT) as CHAR) as str_year from conso_elec_gaz_region_naf order by annee
```

<Dropdown
    title="Année"
    data={years} 
    name=selected_year
    value=str_year
/>

{#if inputs.selected_year}

```sql electricity
select libelle_region as region, annee, sum(conso) as consommation, filiere as kind
from conso_elec_gaz_region_naf 
where annee = '${inputs.selected_year}'
group by libelle_region, annee, filiere
order by libelle_region, annee
```

{#if electricity.length !== 0}
<BarChart
    data={electricity}
    swapXY=true 
    yAxisTitle="Consommation" 
    x=region
    y=consommation
    series=kind
/>
{/if}
{/if}

## Zones de covoiturage en France

```sql covoit
select departements.column1 as department, sum(1) as spots
from covoiturage
join departements ON LEFT(covoiturage.insee, length(departements.column0)) = departements.column0
group by department
order by department

```

<BarChart
    data={covoit}
    swapXY=true 
    yAxisTitle="Nombre de zones de covoiturage par département" 
    x=department
    y=spots
/>