# Mundial 2026 Predictor

> Modelo predictivo hÃ­brido emocional-futbolÃ­stico para el FIFA World Cup 2026.
> ReplicaciÃ³n y extensiÃ³n metodolÃ³gica de Hopfensitz & Mantilla (2018, *Journal of Economic Psychology*).

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status](https://img.shields.io/badge/status-active-success.svg)]()
[![World Cup 2026](https://img.shields.io/badge/FIFA%20World%20Cup-2026-red.svg)]()

---

## ðŸ“– Resumen

Este proyecto construye un modelo predictivo para el Mundial FIFA 2026 combinando dos enfoques:

1. **AnÃ¡lisis emocional automatizado** de las fotografÃ­as oficiales de los jugadores, replicando la metodologÃ­a del paper [Hopfensitz & Mantilla (2018)](https://doi.org/10.1016/j.joep.2018.04.008), que demostrÃ³ correlaciÃ³n entre expresiones faciales (ira y felicidad) y desempeÃ±o deportivo en mundiales 1970-2014.

2. **Modelo predictivo moderno** con variables tradicionales (ranking FIFA, ELO, valor de mercado, forma reciente, head-to-head) usando gradient boosting (LightGBM) con objetivo Poisson para modelar goles esperados.

3. **SimulaciÃ³n Monte Carlo** del torneo completo (50,000 iteraciones) para generar probabilidades calibradas de cada equipo en cada fase del torneo.

## ðŸŽ¯ Objetivos

- Replicar los hallazgos de Hopfensitz & Mantilla con datos actualizados (mundiales 2018, 2022).
- Cuantificar el aporte marginal de las variables emocionales sobre un modelo predictivo moderno.
- Generar predicciones probabilÃ­sticas pÃºblicas para el Mundial 2026 antes del partido inaugural (11 junio 2026).
- Validar el modelo con resultados reales durante el torneo.

## ðŸ—ï¸ Arquitectura del proyecto

```
mundial-2026-predictor/
â”œâ”€â”€ data/
â”‚   â”œâ”€â”€ raw/                    # Datos crudos descargados
â”‚   â”‚   â”œâ”€â”€ photos/             # FotografÃ­as de jugadores
â”‚   â”‚   â””â”€â”€ matches/            # Resultados histÃ³ricos
â”‚   â””â”€â”€ processed/              # Datos procesados listos para modelo
â”œâ”€â”€ notebooks/
â”‚   â”œâ”€â”€ 01_data_collection.ipynb
â”‚   â”œâ”€â”€ 02_photo_collection.ipynb
â”‚   â”œâ”€â”€ 03_emotion_extraction.ipynb
â”‚   â”œâ”€â”€ 04_feature_engineering.ipynb
â”‚   â”œâ”€â”€ 05_model_training.ipynb
â”‚   â”œâ”€â”€ 06_tournament_simulation.ipynb
â”‚   â””â”€â”€ 07_interpretability.ipynb
â”œâ”€â”€ src/
â”‚   â”œâ”€â”€ data_collection.py
â”‚   â”œâ”€â”€ photo_collection.py
â”‚   â”œâ”€â”€ emotion_extraction.py
â”‚   â”œâ”€â”€ features.py
â”‚   â”œâ”€â”€ model.py
â”‚   â”œâ”€â”€ tournament_simulator.py
â”‚   â””â”€â”€ visualization.py
â”œâ”€â”€ models/                     # Modelos entrenados (.pkl)
â”œâ”€â”€ outputs/
â”‚   â”œâ”€â”€ predictions_2026.csv
â”‚   â”œâ”€â”€ figures/
â”‚   â””â”€â”€ paper/
â”œâ”€â”€ app.py                      # Dashboard Streamlit
â”œâ”€â”€ requirements.txt
â””â”€â”€ README.md
```

## ðŸš€ InstalaciÃ³n

### Requisitos previos

- Python 3.11 o superior
- Cuenta Google Colab (Pro recomendado para procesamiento facial con GPU)
- API keys de [football-data.org](https://www.football-data.org/) y [api-football.com](https://www.api-football.com/) (planes gratuitos suficientes)

### Setup local

```bash
# Clonar repositorio
git clone https://github.com/jorgesantanderf/mundial-2026-predictor.git
cd mundial-2026-predictor

# Crear entorno virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Instalar dependencias
pip install -r requirements.txt

# Configurar API keys
cp .env.example .env
# Editar .env con tus API keys
```

## ðŸ“Š MetodologÃ­a

### Datos

| Fuente | Uso | PerÃ­odo |
|---|---|---|
| Panini Sticker Album | FotografÃ­as oficiales | 2010-2026 |
| Transfermarkt | Valor de mercado plantillas | 2010-2026 |
| FIFA Rankings | Ranking oficial mensual | 2010-2026 |
| eloratings.net | ELO ratings por equipo | 2010-2026 |
| RSSSF | Resultados histÃ³ricos | 2010-2022 |
| football-data.org | Resultados recientes | 2020-2026 |

### Modelo

**Nivel 1 - PredicciÃ³n de partido:** LightGBM con objetivo Poisson dual (goles esperados de cada equipo).

**Nivel 2 - SimulaciÃ³n de torneo:** Monte Carlo con 50,000 iteraciones, propagando incertidumbre del modelo.

**ValidaciÃ³n temporal estricta:**
- Train: Mundiales 2010 y 2014
- Validation: Mundial 2018
- Test out-of-sample: Mundial 2022
- PredicciÃ³n: Mundial 2026

## ðŸ“ˆ Resultados

> Las predicciones finales se publicarÃ¡n el **9 de junio de 2026**, antes del partido inaugural del 11 de junio.

## ðŸ“š Referencias

- Hopfensitz, A., & Mantilla, C. (2018). Emotional expressions by sports teams: An analysis of World Cup soccer player portraits. *Journal of Economic Psychology*, 75, 102071. https://doi.org/10.1016/j.joep.2018.04.008

- Ekman, P., & Rosenberg, E. L. (1997). *What the face reveals: Basic and applied studies of spontaneous expression using the facial action coding system (FACS)*. Oxford University Press.

- Groll, A., Schauberger, G., & Tutz, G. (2015). Prediction of major international soccer tournaments based on team-specific regularized Poisson regression. *Journal of Quantitative Analysis in Sports*, 11(2), 97-115.

## âš–ï¸ Consideraciones Ã©ticas

Este proyecto utiliza Ãºnicamente imÃ¡genes pÃºblicas oficiales de figuras pÃºblicas (jugadores profesionales). El anÃ¡lisis facial es agregado a nivel equipo, no individual. No se utiliza para fines comerciales ni de apuestas. El cÃ³digo y los datos derivados se publican bajo licencia abierta para fines acadÃ©micos y educativos.

## ðŸ‘¤ Autor

**Jorge Enrique Santander Fernandez**
Ingeniero QuÃ­mico | Coordinador de MediciÃ³n y Balance, Ecopetrol GRB | Fundador, Santander Oil Consulting
ðŸŒ Barrancabermeja, Colombia

- ðŸ’¼ [LinkedIn](https://www.linkedin.com/in/jorge-enrique-santander-fernandez/)
- ðŸ™ [GitHub](https://github.com/jorgesantanderf)

## ðŸ“„ Licencia

Este proyecto estÃ¡ bajo licencia MIT. Ver [LICENSE](LICENSE) para mÃ¡s detalles.

## ðŸ™ Agradecimientos

A Astrid Hopfensitz y CÃ©sar Mantilla por su trabajo seminal que inspira esta extensiÃ³n metodolÃ³gica.

---

## ðŸ“ CÃ³mo citar este trabajo

```bibtex
@misc{santander2026mundial,
  author = {Santander Fernandez, Jorge Enrique},
  title = {Mundial 2026 Predictor: Hybrid Emotional-Football Model for FIFA World Cup 2026},
  year = {2026},
  publisher = {GitHub},
  url = {https://github.com/jorgesantanderf/mundial-2026-predictor}
}
```
