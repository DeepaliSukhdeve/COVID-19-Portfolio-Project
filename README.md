# COVID-19-Portfolio-Project
SQL Data Exploration







This Project is focused on exploring global data related to confirmed COVID-19 deaths and vaccinations using SQL.

The data is sourced from "https://ourworldindata.org/covid-deaths" and is imported into Microsoft SQL Server Management Studio to help store,organize,process and analyze.

The project involves employing various SQL skills such as Joins, Common Table Expressions (CTEs), Temporary Tables, Window Functions, Aggregate Functions, Creating Views, and Converting Data Types to analyze the COVID-19 data effectively.



```

--Preview Data
Select *
From PortfolioProject.dbo.CovidDeaths
order by location

```

```

select *
from PortfolioProject.dbo.CovidVaccinations
order by location

```

```

-- Analyze Total Cases vs Total Deaths

select location,date,total_cases,total_deaths,(total_deaths/total_cases)*100 as Deathpercentage
from PortfolioProject.dbo.CovidDeaths
where location like '%india%'
order by location,date

```


```

-- Total Cases vs Population
-- Shows what percentage of population infected with Covid

select location,date,total_cases,population, (total_cases/population)*100 as Infected_percentage
from PortfolioProject.dbo.CovidDeaths
where location like '%states%'
order by location,date

```


```

-- Countries with Highest Infection Rate compared to population

select location, population, max(total_cases) as Highest_Infection_count,(max(total_cases)/population)*100 as PercentpopulationInfected
from PortfolioProject.dbo.CovidDeaths
where total_cases <>  0
and population <> 0
--where location like '%india%'
group by location,population
order by PercentpopulationInfected desc


```

```

-- Countries with Highest Death Count per Population

select location,population,max(total_deaths) as Highest_death_count
from PortfolioProject.dbo.CovidDeaths
where total_cases <>  0
and population <> 0
--where location like '%india%'
group by location,population
order by Highest_death_count desc

```

```

-- BREAKING THINGS DOWN BY CONTINENT

---- Showing contintents with the highest death count per population

select continent,max(total_deaths) as Highest_death_count
from PortfolioProject.dbo.CovidDeaths
where total_cases <>  0
and population <> 0
--where location like '%india%'
group by continent
order by Highest_death_count desc

```

-- GLOBAL NUMBERS

Select SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(New_Cases)*100 as DeathPercentage
From PortfolioProject..CovidDeaths
--Where location like '%states%'
where continent is not null 
--Group By date
order by total_cases,total_deaths

```

```

-- Total Population vs Vaccinations
-- Shows Percentage of Population that has recieved at least one Covid Vaccine

SELECT CD.continent, CD.location, CD.date, CD.population,CV.new_vaccinations
, SUM(CV.new_vaccinations) OVER (PARTITION BY CD.LOCATION ORDER BY CD.LOCATION,CD.DATE) AS ROLLINGPEOPLEVACCINATED
FROM DBO.CovidDeaths AS CD
JOIN DBO.CovidVaccinations AS CV
ON CD.location = CV.location
AND CD.date = CV.date
Where CD.continent is not null 
ORDER BY CD.location, CD.date

```



