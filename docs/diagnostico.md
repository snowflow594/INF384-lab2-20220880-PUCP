# 4 errores en el pipeline

1.- El primer error que se puede encontrar es que el job de publicar no depende de validar por lo que siempre se realizará la publicación siempre en todas las corridas del workflow

2.- Se está utilizando el archivo del tipo requeriments.txt en vez del archivo de lockeo de requeriments.lock

3.- En el pipeline se menciona que la variable del project key debe ser una llamada "vars.SONAR_PROJECT_KEY", pero en el README se menciona que este es un valor constante igua la "INF384-lab2" por lo que se está utilizando una variable que no existe en el repositorio

4.- Tiene relación con la primera y la publicación no está protegida frente a el análisis de calidad que se realizar en sonar cloud por lo que se puede presentar código defectuoso. Lo ideal sería realizar la dependencia del job de validar antes de realizar el job de publicar.
publicar:
	needs: validar

# Responsable del tiempo de ejecución del pipeline

El tiempo mayormente está relacionado con el análisis de calidad que se está haciendo con sonar cloud. Dentro del apartado de actions y evaluando cada job en todos los runs que se realizaron, esta es la parte que más tiempo consume con un promedio de 35 - 45 segundos

# Vinculo con el caso

Escogería el Lead Time, ya que se estarían resolviendo el tema de reducir el tiempo que puede tardar el pipeline en validar y publicar el artefacto dejando de lado los errores en las variables y en la dependencia del job de publicar sobre el job de validar. Con esto, se estaría ahorrando tiempo extra en los futuros usos del pipeline.

# El proxy

Se medirá el número relacionado a el tiempo de duración de cada pipeline

| Ejecucion | Duracion | URL |
|---|---|---|
| 1 | 59s | https://github.com/snowflow594/INF384-lab2-20220880-PUCP/actions/runs/34443492240 |
| 2 | 1m 14s | https://github.com/snowflow594/INF384-lab2-20220880-PUCP/actions/runs/34443661307 |
| 3 | 54s | https://github.com/snowflow594/INF384-lab2-20220880-PUCP/actions/runs/34443914408 |

## Declaracion de uso de IA generativa

Indicar si se utilizaron herramientas de IA generativa para completar este
trabajo previo, cuales, y con que proposito. Adjuntar los prompts utilizados.

# IA Usada

Se utilizó ChatGPT para la identificación de los problemas relacionados al pipeline con los siguientes prompts:

name: pipeline on: push: workflow_dispatch: jobs: validar: name: Validar runs-on: ubuntu-latest steps: - name: Descargar el codigo uses: actions/checkout@v4 with: fetch-depth: 0 - name: Preparar Python uses: actions/setup-python@v5 with: python-version: '3.11' - name: Instalar dependencias run: | python -m pip install --upgrade pip pip install -r requirements.txt - name: Ejecutar pruebas run: pytest --cov=src --cov-report=xml - name: Analisis de calidad uses: SonarSource/sonarqube-scan-action@v8 env: SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }} SONAR_HOST_URL: https://sonarcloud.io with: args: > -Dsonar.organization=${{ vars.SONAR_ORG }} -Dsonar.projectKey=${{ vars.SONAR_PROJECT_KEY }} publicar: name: Publicar artefacto runs-on: ubuntu-latest steps: - name: Descargar el codigo uses: actions/checkout@v4 - name: Preparar Python uses: actions/setup-python@v5 with: python-version: '3.11' - name: Instalar dependencias run: | python -m pip install --upgrade pip pip install -r requirements.txt - name: Construir el paquete run: python -m build - name: Publicar el paquete uses: actions/upload-artifact@v4 with: name: paquete path: dist/ overwrite: true puedes decirme los 4 errores que están presentes en este pipeline?

