# Ejercicios de GitLab CI/CD

## **1. Configuración de Etapas y Jobs**

### Ejercicio 1.1: Crear un stage `build` y un job `job_build`

**Objetivo:** Crear un stage `build` y un job `job_build` que lo utilice.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas del pipeline
stages:
- build  # Etapa de construcción

# Job de construcción
job_build:
stage: build  # Este job pertenece a la etapa "build"
script:
- echo 'Building the project...'  # Mensaje de construcción
```

</details>

---

### Ejercicio 1.2: Crear un stage `test` y un job `job_test`

**Objetivo:** Crear un stage `test` y un job `job_test` que lo utilice.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas del pipeline
stages:
- test  # Etapa de pruebas

# Job de pruebas
job_test:
stage: test  # Este job pertenece a la etapa "test"
script:
- echo 'Running tests...'  # Mensaje de pruebas
```

</details>

---

### Ejercicio 1.3: Crear un stage `deploy` y un job `job_deploy`

**Objetivo:** Crear un stage `deploy` y un job `job_deploy` que lo utilice.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas del pipeline
stages:
- deploy  # Etapa de despliegue

# Job de despliegue
job_deploy:
stage: deploy  # Este job pertenece a la etapa "deploy"
script:
- echo 'Deploying to production...'  # Mensaje de despliegue
```

</details>

---

## **2. Configuración del Pipeline**

### Ejercicio 2.1: Crear un pipeline que solo se ejecute en la rama `main`

**Objetivo:** Configurar el pipeline para que solo se ejecute en la rama `main`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Configuración del pipeline
workflow:
rules:
- if: '$CI_COMMIT_REF_NAME == "main"'  # Solo se ejecuta en la rama "main"

# Definición de etapas
stages:
- build

# Job de construcción
job_build:
stage: build
script:
- echo 'Building the project...'
```

</details>

---

## **3. Uso de Variables de Entorno**

### Ejercicio 3.1: Usar variables de entorno

**Objetivo:** Definir y utilizar variables de entorno en el job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de variables de entorno
variables:
DEPLOY_ENV: "production"  # Variable de entorno para el entorno de despliegue

# Definición de etapas
stages:
- deploy

# Job de despliegue
job_deploy:
stage: deploy
script:
- echo "Deploying to $DEPLOY_ENV..."  # Usa la variable de entorno
```

</details>

---

## **4. Configuración de Jobs Interruptibles**

### Ejercicio 4.1: Configurar un job interruptible

**Objetivo:** Configurar el job de despliegue como interruptible.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- deploy

# Job de despliegue
job_deploy:
stage: deploy
interruptible: true  # Permitir que este job sea interrumpido
script:
- echo 'Deploying to production...'  # Mensaje de despliegue
```

</details>

## **5. Definición de Etapas del Pipeline**

### Ejercicio 5.1: Definir múltiples etapas en el pipeline

**Objetivo:** Definir un pipeline con etapas adicionales.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas del pipeline
stages:
- .pre    # Etapa previa
- build   # Etapa de construcción
- test    # Etapa de pruebas
- deploy  # Etapa de despliegue
- .post   # Etapa posterior
  ```

</details>

---

## **6. Variables Globales**

### Ejercicio 6.1: Definir variables globales en el pipeline

**Objetivo:** Definir una variable global que estará disponible en todos los jobs.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de variables globales
variables:
ENVIRONMENT: 'production'  # Variable global para el entorno de despliegue
```

</details>

---

## **7. Job de Construcción con Variables**

### Ejercicio 7.1: Crear un job de construcción que use una variable global

**Objetivo:** Crear un job que imprima el valor de una variable global.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- build

# Job de construcción
job_build:
stage: build
script:
- echo $ENVIRONMENT  # Imprime el entorno de despliegue
```

</details>

---

## **8. Job de Pruebas Condicionales**

### Ejercicio 8.1: Configurar un job de pruebas que solo se ejecute en la rama `develop`

**Objetivo:** Crear un job que solo se ejecute si la rama es `develop`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- test

# Job de pruebas
job_test:
stage: test
script:
- echo $ENVIRONMENT  # Imprime el entorno de despliegue
rules:
- if: '$CI_COMMIT_BRANCH == "develop"'  # Solo se ejecuta en la rama "develop"
```

</details>

---

## **9. Job de Despliegue con Sobrescritura de Variables**

### Ejercicio 9.1: Crear un job de despliegue que sobrescriba la variable `ENVIRONMENT`

**Objetivo:** Configurar un job que sobrescriba una variable global a nivel de job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- deploy

# Job de despliegue
job_deploy:
stage: deploy
variables:
ENVIRONMENT: 'staging'  # Sobrescribe la variable a nivel de job
script:
- echo $ENVIRONMENT  # Imprime 'staging'
```

</details>

---

## **10. Job de Despliegue Condicional**

### Ejercicio 10.1: Configurar un job de despliegue que solo se ejecute en la rama `main`

**Objetivo:** Crear un job de despliegue que se ejecute solo en la rama `main` y no en merge requests.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- deploy

# Job de despliegue
job_deploy:
stage: deploy
variables:
ENVIRONMENT: 'staging'  # Sobrescribe la variable a nivel de job
script:
- echo $ENVIRONMENT  # Imprime 'staging'
rules:
- if: '$CI_COMMIT_BRANCH == "main" && $CI_PIPELINE_SOURCE != "merge_request_event"'  # Solo se ejecuta en 'main'
```

</details>

## **11. Definición de Inputs**

### Ejercicio 11.1: Especificar un input `environment`

**Objetivo:** Definir un input `environment` que permite especificar el entorno de despliegue.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Especificación de configuración de inputs
spec:
inputs:
environment:
description: 'Define the environment for the pipeline. Can be "development", "staging", or "production".'
options:
- development
- staging
- production
default: 'development'  # Valor por defecto
```

</details>

---

### Ejercicio 11.2: Especificar un input `version`

**Objetivo:** Definir un input `version` que valida el formato de la versión.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Especificación de configuración de inputs
spec:
inputs:
version:
description: 'Specify the version number. Must be in format x.y.z.'
validation:
regex: '^\\d+\\.\\d+\\.\\d+$'  # Validación del formato de versión
```

</details>

---

## **12. Documentación y Descripción del Pipeline**

### Ejercicio 12.1: Agregar una descripción al pipeline

**Objetivo:** Agregar una descripción general al pipeline.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Descripción del pipeline
description: 'This pipeline handles the building, testing, and deployment of the application.'
```

</details>

---

## **13. Trabajando con Archivos de Configuración Externos**

### Ejercicio 13.1: Cargar configuración de un archivo externo

**Objetivo:** Cargar configuración desde un archivo externo llamado `config.yml`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Carga de configuración de archivo externo
include:
- local: 'config.yml'  # Incluye la configuración desde 'config.yml'
  ```

</details>

---

### Ejercicio 13.2: Usar una plantilla externa

**Objetivo:** Incluir una plantilla de CI/CD desde una ubicación externa.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Carga de plantilla de CI/CD
include:
- template: 'Auto-DevOps.gitlab-ci.yml'  # Incluye la plantilla de Auto-DevOps
  ```

</details>

---

## **14. Configuración de Recursos del Job**

### Ejercicio 14.1: Definir los recursos del job

**Objetivo:** Especificar el tamaño del runner y la imagen a utilizar en el job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de recursos del job
job_build:
image: 'node:14'  # Usa la imagen de Node.js
resources:
requests:
memory: '512Mi'  # Solicita 512Mi de memoria
```

</details>

---

## **15. Artifacts**

### Ejercicio 15.1: Configurar artifacts

**Objetivo:** Definir artifacts que serán guardados después de la ejecución del job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- build

# Job de construcción
job_build:
stage: build
script:
- echo 'Building the project...'
- touch artifact.txt  # Crea un archivo de artifact
artifacts:
paths:
- artifact.txt  # Archivos a guardar como artifacts
```

</details>

---

## **16. Cache**

### Ejercicio 16.1: Configurar cache para dependencias

**Objetivo:** Configurar cache para optimizar el proceso de instalación de dependencias.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Configuración de cache
cache:
paths:
- node_modules/  # Cache de la carpeta de dependencias de Node.js

# Definición de etapas
stages:
- build

# Job de construcción
job_build:
stage: build
script:
- echo 'Installing dependencies...'
- npm install  # Instalación de dependencias
```

</details>

---

## **17. Notificaciones**

### Ejercicio 17.1: Configurar notificaciones de Slack

**Objetivo:** Configurar notificaciones para enviar mensajes a un canal de Slack después de la ejecución del job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Configuración de notificaciones
notifications:
slack:
webhook: 'https://hooks.slack.com/services/...'  # URL del webhook de Slack
channel: '#ci-notifications'  # Canal de notificaciones
```

</details>

---

## **18. Jobs Dependientes**

### Ejercicio 18.1: Crear jobs que dependen de otros

**Objetivo:** Crear un job de despliegue que dependa de la finalización del job de construcción.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- build
- deploy

# Job de construcción
job_build:
  stage: build
  script:
    - echo 'Building the project...'
    # Job de despliegue
  job_deploy:
    stage: deploy
    needs: ['job_build']  # Este job depende de job_build
    script:
    - echo 'Deploying to production...'
```

</details>

---

## **19. Jobs con Recursos Externos**

### Ejercicio 19.1: Crear un job que use un recurso externo

**Objetivo:** Crear un job que utilice un recurso externo para la construcción.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- build

# Job de construcción
job_build:
stage: build
script:
- echo 'Building the project...'
- wget http://example.com/resource.zip  # Usa un recurso externo
```

</details>

---

## **20. Multi-Project Pipelines**

### Ejercicio 20.1: Configurar pipelines de múltiples proyectos

**Objetivo:** Configurar un pipeline que invoque un job en otro proyecto.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
- trigger

# Job que invoca otro proyecto
trigger_another_project:
stage: trigger
trigger:
project: 'another-group/another-project'  # Proyecto a invocar
branch: 'main'  # Rama a invocar
```

</details>

## **21. Job de Validación Previa**

### Ejercicio 21.1: Crear un job de validación previa

**Objetivo:** Crear un job en la etapa `.pre` que realice validaciones previas al proceso de construcción.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - .pre
  - build

# Job de validación previa
job_validation:
  stage: .pre  # Este job pertenece a la etapa "pre"
  script:
    - echo 'Validating configuration...'  # Mensaje de validación
```

</details>

---

## **22. Job de Construcción con Dependencias**

### Ejercicio 22.1: Crear un job de construcción que instale dependencias

**Objetivo:** Crear un job de construcción que instale dependencias necesarias antes de construir el proyecto.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - build

# Job de construcción
job_build:
  stage: build
  script:
    - echo 'Installing dependencies...'  # Mensaje de instalación
    - npm install  # Instala las dependencias
```

</details>

---

## **23. Job de Pruebas Unitarias**

### Ejercicio 23.1: Crear un job de pruebas unitarias

**Objetivo:** Crear un job en la etapa `test` que ejecute pruebas unitarias.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - test

# Job de pruebas unitarias
job_unit_tests:
  stage: test
  script:
    - echo 'Running unit tests...'  # Mensaje de ejecución de pruebas
    - npm test  # Ejecuta las pruebas unitarias
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'  # Solo se ejecuta en la rama "develop"
```

</details>

---

## **24. Job de Despliegue en Múltiples Entornos**

### Ejercicio 24.1: Configurar un job de despliegue para múltiples entornos

**Objetivo:** Crear un job de despliegue que se pueda ejecutar en `staging` y `production` dependiendo de la rama.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - deploy

# Job de despliegue
job_deploy_multi:
  stage: deploy
  script:
    - echo "Deploying to $ENVIRONMENT..."  # Muestra el entorno de despliegue
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'  # Despliegue en producción
      variables:
        ENVIRONMENT: 'production'  # Establece el entorno como 'production'
    - if: '$CI_COMMIT_BRANCH == "develop"'  # Despliegue en staging
      variables:
        ENVIRONMENT: 'staging'  # Establece el entorno como 'staging'
```

</details>

---

## **25. Job de Notificaciones al Desplegar**

### Ejercicio 25.1: Configurar un job que envíe notificaciones al finalizar el despliegue

**Objetivo:** Crear un job que envíe una notificación a un canal de Slack después de que se complete el despliegue.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - deploy

# Job de despliegue
job_deploy:
  stage: deploy
  script:
    - echo 'Deploying to production...'  # Mensaje de despliegue
    - curl -X POST -H 'Content-type: application/json' --data '{"text":"Deployment completed successfully!"}' https://hooks.slack.com/services/your/slack/webhook  # Enviar notificación a Slack
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'  # Solo se ejecuta en la rama "main"
```

</details>

---

## **26. Job de Limpieza Posterior al Despliegue**

### Ejercicio 26.1: Crear un job de limpieza posterior al despliegue

**Objetivo:** Crear un job que limpie los recursos después del despliegue.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - .post

# Job de limpieza
job_cleanup:
  stage: .post
  script:
    - echo 'Cleaning up resources...'  # Mensaje de limpieza
    - rm -rf temp_files/  # Elimina archivos temporales
```

</details>

---

## **27. Job con Dependencias de Otros Jobs**

### Ejercicio 27.1: Configurar un job que dependa de otros jobs

**Objetivo:** Crear un job que solo se ejecute después de la finalización exitosa de otros jobs.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - build
  - test
  - deploy

# Job de construcción
job_build:
  stage: build
  script:
    - echo 'Building the project...'

# Job de pruebas
job_test:
  stage: test
  script:
    - echo 'Running tests...'

# Job de despliegue que depende de build y test
job_deploy:
  stage: deploy
  needs:
    - job_build  # Depende del job de construcción
    - job_test   # Depende del job de pruebas
  script:
    - echo 'Deploying to production...'
```

</details>

---

## **28. Uso de Artifacts para Almacenar Resultados de Pruebas**

### Ejercicio 28.1: Configurar artifacts para almacenar resultados de pruebas

**Objetivo:** Configurar un job de pruebas que guarde los resultados en un archivo y los almacene como artifacts.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - test

# Job de pruebas
job_test:
  stage: test
  script:
    - echo 'Running tests...'  # Mensaje de ejecución de pruebas
    - echo 'Test results...' > test_results.txt  # Genera resultados de pruebas
  artifacts:
    paths:
      - test_results.txt  # Guarda el archivo de resultados como artifact
```

</details>

---

## **29. Cache de Dependencias en el Job de Construcción**

### Ejercicio 29.1: Configurar cache en el job de construcción

**Objetivo:** Configurar un job de construcción que utilice cache para las dependencias, mejorando la eficiencia.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Configuración de cache
cache:
  paths:
    - node_modules/  # Cache de las dependencias de Node.js

# Definición de etapas
stages:
  - build

# Job de construcción
job_build:
  stage: build
  script:
    - echo 'Installing dependencies...'  # Mensaje de instalación
    - npm install  # Instala las dependencias
```

</details>

---

## **30. Uso de Variables de Entorno en el Job de Despliegue**

### Ejercicio 30.1: Crear un job de despliegue que use variables de entorno

**Objetivo:** Configurar un job de despliegue que imprima el valor de una variable de entorno definida en el pipeline.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - deploy

# Job de despliegue
job_deploy:
  stage: deploy
  script:
    - echo "Deploying to $ENVIRONMENT..."  # Usa la variable global ENVIRONMENT
```
</details>

## **31. Definición de Inputs Personalizados**

### Ejercicio 31.1: Crear un job que utilice un input personalizado para el entorno

**Objetivo:** Crear un job que imprima el entorno de despliegue utilizando el input definido en `spec.inputs.environment`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de inputs
spec:
  inputs:
    environment:
      description: 'Define the environment for the pipeline. Can be "development", "staging", or "production".'
      options:
        - development
        - staging
        - production
      default: 'development'

# Definición de etapas
stages:
  - build

# Job que usa el input
job_use_environment_input:
  stage: build
  script:
    - echo "Deploying in the $[[ inputs.environment ]] environment"  # Imprime el entorno
```
</details>

## **32. Validación de Inputs con Expresiones Regulares**

### Ejercicio 32.1: Crear un job que valide el formato de la versión

**Objetivo:** Crear un job que imprima un mensaje de validación si el input de versión sigue el formato correcto.

<details>
<summary>Contenido del archivo</summary>

\`\`\`yaml
# .gitlab-ci.yml

# Definición de inputs
spec:
inputs:
version:
description: 'Version format must follow the pattern: v<major>.<minor>.<patch>.'
regex: ^v\d\.\d+(\.\d+)?$

# Definición de etapas
stages:
- validate

# Job que valida la versión
job_validate_version:
stage: validate
script:
- if [[ "$[[ inputs.version ]]" =~ ^v[0-9]+\.[0-9]+(\.[0-9]+)?$ ]]; then
echo "Version format is valid: $[[ inputs.version ]]";
else
echo "Invalid version format!";
fi
\`\`\`

</details>

---

## **33. Uso de Inputs en Diferentes Etapas del Pipeline**

### Ejercicio 33.1: Crear un job en la etapa de despliegue que use un input para el job_stage

**Objetivo:** Crear un job que imprima la etapa de despliegue utilizando el input definido en `spec.inputs.job_stage`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de inputs
spec:
  inputs:
    job_stage:
      description: 'Define the stage for the pipeline. Example: "build" or "deploy".'

# Definición de etapas
stages:
  - deploy

# Job que usa el input job_stage
job_use_job_stage_input:
  stage: deploy
  script:
    - echo "Deploying to the stage: $[[ inputs.job_stage ]]"  # Imprime la etapa de despliegue
```

</details>

---

## **34. Manejo de Errores Específicos en el Job de Construcción**

### Ejercicio 34.1: Crear un job de construcción que maneje errores específicos

**Objetivo:** Modificar el job de construcción para manejar errores específicos y permitir que el pipeline continúe.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - build

# Job de construcción
job_build:
  stage: build
  script:
    - echo 'Building in the environment $[[ inputs.environment ]]'  # Muestra el entorno
    - exit 137  # Simula un error
  allow_failure:
    exit_codes:
      - 137  # Permite fallo
      - 255  # Permite fallo
```

</details>

---

## **35. Uso de after_script en el Job de Despliegue**

### Ejercicio 35.1: Crear un job de despliegue que use after_script

**Objetivo:** Modificar el job de despliegue para incluir un `after_script` que imprima un mensaje después de que el job haya terminado.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas
stages:
  - deploy

# Job de despliegue
job_deploy:
  stage: deploy
  script:
    - echo 'Deploying to the stage $[[ inputs.job_stage ]]'  # Imprime la etapa
  after_script:
    - echo 'After script executed. This runs in a separate shell.'  # Mensaje del after_script
```

</details>

---

## **36. Combinación de Inputs y Outputs en el Pipeline**

### Ejercicio 36.1: Crear un job que use inputs y genere un output

**Objetivo:** Crear un job que tome un input y genere un archivo de salida que contenga esa información.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de inputs
spec:
  inputs:
    environment:
      description: 'Define the environment for the pipeline.'

# Definición de etapas
stages:
  - build

# Job que usa inputs y genera output
job_generate_output:
  stage: build
  script:
    - echo "Environment: $[[ inputs.environment ]]" > output.txt  # Genera un archivo de salida
  artifacts:
    paths:
      - output.txt  # Guarda el archivo generado como artifact
```

</details>

---

## **37. Implementación de múltiples Inputs en un Job**

### Ejercicio 37.1: Crear un job que utilice múltiples inputs

**Objetivo:** Crear un job que imprima mensajes basados en múltiples inputs definidos en la especificación.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de inputs
spec:
  inputs:
    environment:
      description: 'Define the environment for the pipeline.'
    version:
      description: 'Version format must follow the pattern: v<major>.<minor>.<patch>.'

# Definición de etapas
stages:
  - build

# Job que usa múltiples inputs
job_multiple_inputs:
  stage: build
  script:
    - echo "Building version $[[ inputs.version ]] in the $[[ inputs.environment ]] environment"  # Imprime ambos inputs
```

</details>

---

## **38. Validación de Inputs y Manejo de Errores**

### Ejercicio 38.1: Crear un job que valide inputs y maneje errores

**Objetivo:** Crear un job que valide los inputs y salga con un código de error si la validación falla.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# .gitlab-ci.yml

# Definición de inputs
spec:
  inputs:
    environment:
      description: 'Define the environment for the pipeline.'

# Definición de etapas
stages:
  - validate

# Job que valida inputs
job_validate_inputs:
  stage: validate
  script:
    - |
      if [[ "$CI_ENVIRONMENT_NAME" != "development" && "$CI_ENVIRONMENT_NAME" != "staging" && "$CI_ENVIRONMENT_NAME" != "production" ]]; then
        echo "Invalid environment!"
        exit 1
      else
        echo "Environment is valid: $CI_ENVIRONMENT_NAME"
      fi
```

</details>

---

## **39. Implementación de Defaults en Inputs**

### Ejercicio 39.1: Crear un job que use el valor por defecto de un input

**Objetivo:** Crear un job que utilice el valor por defecto de `inputs.environment` si no se proporciona uno.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de inputs
spec:
  inputs:
    environment:
      description: 'Define the environment for the pipeline.'
      default: 'development'

# Definición de etapas
stages:
  - build

# Job que usa el valor por defecto
job_use_default_input:
  stage: build
  script:
    - echo "Using default environment: $[[ inputs.environment ]]"  # Imprime el entorno por defecto
```

</details>

## **40. Configuración del Entorno con Dependencias**

### Ejercicio 40.1: Crear un job que configure el entorno e instale dependencias

**Objetivo:** Crear un job que instale `curl` y realice una solicitud HTTP a un servicio externo.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job para configurar el entorno
job_configure_environment:
  before_script:
    - apt-get update && apt-get install -y curl  # Actualiza repositorios e instala 'curl'.
  
  script:
    - curl http://example.com/  # Realiza una solicitud GET a una URL externa.
```

</details>

---

## **41. Configuración de Caché para Dependencias de Node.js**

### Ejercicio 41.1: Crear un job que instale dependencias y configure la caché

**Objetivo:** Crear un job que instale las dependencias de un proyecto Node.js y las almacene en caché para su reutilización.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job para configurar la caché
job_configure_cache:
  script:
    - npm install  # Instala dependencias de Node.js a partir de `package.json`.

  cache:
    key: deps-$CI_COMMIT_REF_SLUG  # Clave de caché basada en la rama del commit.
    paths:
      - node_modules/  # Almacena el directorio `node_modules` en caché.
```

</details>

---

## **42. Uso de Claves de Caché Avanzadas**

### Ejercicio 42.1: Crear un job que utilice una clave de caché avanzada basada en un archivo

**Objetivo:** Crear un job que utilice una clave de caché avanzada para almacenar dependencias de Node.js.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que utiliza una clave de caché avanzada
job_advanced_cache_key:
  script:
    - echo "Usando caché basada en archivos clave"  # Mensaje indicando el uso de la caché avanzada.

  cache:
    key:
      files:
        - package.json  # Clave basada en el contenido del archivo `package.json`.
    paths:
      - node_modules/  # Almacena el directorio `node_modules` en caché.
```

</details>

---

## **43. Integración de `before_script` y `script` en un Job**

### Ejercicio 43.1: Crear un job que utilice `before_script` para preparar el entorno

**Objetivo:** Crear un job que instale `curl` y luego imprima la versión de `curl`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que utiliza before_script
job_install_and_check_curl:
  before_script:
    - apt-get update && apt-get install -y curl  # Instala 'curl'.
  
  script:
    - curl --version  # Imprime la versión de 'curl'.
```

</details>

---

## **44. Instalación de Dependencias con Manejo de Errores**

### Ejercicio 44.1: Crear un job que maneje errores durante la instalación de dependencias

**Objetivo:** Crear un job que instale dependencias y maneje el fallo en caso de que `npm install` falle.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que maneja errores en la instalación
job_install_with_error_handling:
  script:
    - npm install || { echo 'Fallo en la instalación de dependencias'; exit 1; }  # Maneja el error de instalación
```

</details>

---

## **45. Reutilización de Caché en Diferentes Jobs**

### Ejercicio 45.1: Crear múltiples jobs que reutilicen la caché

**Objetivo:** Crear dos jobs que instalen dependencias y reutilicen la caché configurada.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que instala dependencias
job_install_dependencies:
  script:
    - npm install  # Instala dependencias
  cache:
    key: deps-$CI_COMMIT_REF_SLUG  # Clave de caché
    paths:
      - node_modules/  # Almacena el directorio `node_modules`

# Job que verifica las dependencias
job_check_dependencies:
  script:
    - ls node_modules/  # Lista el contenido de `node_modules`
  cache:
    key: deps-$CI_COMMIT_REF_SLUG  # Reutiliza la caché
    paths:
      - node_modules/  # Almacena el directorio `node_modules`
```

</details>

---

## **46. Configuración de Caché Condicional**

### Ejercicio 46.1: Crear un job que configure la caché de forma condicional

**Objetivo:** Crear un job que configure la caché solo si se han instalado dependencias.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que configura caché condicionalmente
job_conditional_cache:
  script:
    - npm install  # Instala dependencias
  cache:
    key: deps-$CI_COMMIT_REF_SLUG  # Clave de caché
    paths:
      - node_modules/  # Almacena el directorio `node_modules`
    policy: pull  # Solo recupera la caché, no la almacena si no hay cambios
```

</details>

---

## **47. Integración de `before_script` y `after_script`**

### Ejercicio 47.1: Crear un job que use `before_script` y `after_script`

**Objetivo:** Crear un job que imprima un mensaje antes y después de ejecutar la instalación de dependencias.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que usa before_script y after_script
job_before_after_script:
  before_script:
    - echo "Preparando la instalación de dependencias"  # Mensaje antes de la instalación
  
  script:
    - npm install  # Instala dependencias

  after_script:
    - echo "Instalación de dependencias completa"  # Mensaje después de la instalación
```

</details>

---

## **48. Verificación de Dependencias Instaladas**

### Ejercicio 48.1: Crear un job que verifique que las dependencias se han instalado correctamente

**Objetivo:** Crear un job que instale dependencias y verifique que `node_modules` no está vacío.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que verifica la instalación de dependencias
job_verify_dependencies:
  script:
    - npm install  # Instala dependencias
    - if [ -z "$(ls -A node_modules)" ]; then echo "No se han instalado dependencias"; exit 1; fi  # Verifica el directorio
```

</details>

---

## **49. Uso de `after_script` para Limpiar el Entorno**

### Ejercicio 49.1: Crear un job que limpie el entorno después de la instalación

**Objetivo:** Crear un job que instale dependencias y limpie cualquier archivo temporal en `after_script`.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que limpia el entorno
job_cleanup_after_install:
  script:
    - npm install  # Instala dependencias

  after_script:
    - rm -rf temp-files/  # Limpia archivos temporales
```

</details>

## **50. Excluir Archivos Temporales en Artefactos**

### Ejercicio 50.1: Crear un job que excluya archivos temporales al almacenar artefactos

**Objetivo:** Crear un job que genere un directorio y varios archivos, excluyendo los archivos temporales al almacenar los artefactos.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que excluye archivos temporales
job_exclude:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - mkdir binaries  # Crea el directorio 'binaries'.
    - echo "Compilando código..." > binaries/programa.bin  # Crea un archivo de binario en el directorio.
    - echo "Generando logs..." > build.log  # Genera un archivo de log.
    - echo "Archivo temporal..." > temp.tmp  # Genera un archivo temporal.

  artifacts:
    paths:
      - binaries/  # Guarda todo el contenido del directorio 'binaries' como artefacto.
    exclude:
      - '*.temp'  # Excluye archivos con la extensión '.temp'.
```

</details>

---

## **51. Expirar Artefactos Después de un Período de Tiempo**

### Ejercicio 51.1: Crear un job que almacene artefactos con fecha de expiración

**Objetivo:** Crear un job que compile un archivo y lo almacene como artefacto, con una expiración de 2 días.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que expira artefactos
job_expires:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo "Compilando código..." > output.txt  # Compila el código y lo guarda en 'output.txt'.

  artifacts:
    paths:
      - output.txt  # Almacena 'output.txt' como artefacto.
    expire_in: 2 days  # El artefacto expirará después de 2 días.
```

</details>

---

## **52. Exponer Artefactos en la Interfaz de GitLab**

### Ejercicio 52.1: Crear un job que genere un archivo de resultados y lo exponga

**Objetivo:** Crear un job que genere un archivo de resultados de prueba y lo exponga como artefacto visible en la interfaz de GitLab.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que expone artefactos en la UI
job_ui:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo 'Test realizado' > resultado.txt  # Guarda el resultado del test en 'resultado.txt'.

  artifacts:
    expose_as: 'Resultados de la Prueba'  # Nombre que se muestra en la UI de GitLab.
    paths:
      - resultado.txt  # Almacena 'resultado.txt' como artefacto.
```

</details>

---

## **53. Asignar Nombres Personalizados a los Artefactos**

### Ejercicio 53.1: Crear un job que genere resultados y asigne un nombre personalizado

**Objetivo:** Crear un job que genere resultados de prueba y asigne un nombre personalizado al artefacto.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que asigna nombres personalizados a los artefactos
job_name:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo 'Tests realizado' > tests.txt  # Guarda los resultados del test en 'tests.txt'.

  artifacts:
    name: 'Tests-e2e'  # Nombre personalizado del artefacto.
    paths:
      - tests.txt  # Almacena 'tests.txt' como artefacto.
```

</details>

---

## **54. Restringir Acceso a Artefactos por Permisos**

### Ejercicio 54.1: Crear un job que restrinja el acceso a los artefactos

**Objetivo:** Crear un job que genere resultados de prueba, restringiendo el acceso a los artefactos solo a usuarios con permisos de desarrollador.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que restringe el acceso a los artefactos
job_limited_access:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo 'Tests realizado' > tests.txt  # Guarda los resultados del test en 'tests.txt'.

  artifacts:
    access: developers  # Solo los desarrolladores pueden acceder a este artefacto.
    paths:
      - tests.txt  # Almacena 'tests.txt' como artefacto.
```

</details>

---

## **55. Almacenar Archivos No Rastreables en Caso de Fallo**

### Ejercicio 55.1: Crear un job que almacene archivos solo si falla

**Objetivo:** Crear un job que genere un archivo de error y lo almacene como artefacto solo si el job falla.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que almacena archivos no rastreados en caso de fallo
job_untracked:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo 'Tests realizado' > error.txt  # Guarda un archivo de error en 'error.txt'.

  artifacts:
    untracked: true  # Incluir archivos no rastreados como artefactos.
    when: on_failure  # Almacena los artefactos solo si el job falla.
    paths:
      - error.txt  # Almacena 'error.txt' como artefacto.
```

</details>

---

## **56. Generar Varios Archivos de Salida como Artefactos**

### Ejercicio 56.1: Crear un job que genere múltiples archivos como artefactos

**Objetivo:** Crear un job que genere varios archivos de salida y los almacene como artefactos.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que genera múltiples archivos de salida
job_multiple_outputs:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo "Resultado 1" > resultado1.txt  # Genera el primer archivo.
    - echo "Resultado 2" > resultado2.txt  # Genera el segundo archivo.

  artifacts:
    paths:
      - resultado1.txt  # Almacena 'resultado1.txt' como artefacto.
      - resultado2.txt  # Almacena 'resultado2.txt' como artefacto.
```

</details>

---

## **57. Generación de Logs como Artefactos**

### Ejercicio 57.1: Crear un job que genere logs y los almacene como artefactos

**Objetivo:** Crear un job que genere un archivo de log y lo almacene como artefacto.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que genera logs
job_log_output:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - echo "Log de ejecución" > execution.log  # Genera un archivo de log.

  artifacts:
    paths:
      - execution.log  # Almacena 'execution.log' como artefacto.
```

</details>

---

## **58. Validación de Archivos Generados**

### Ejercicio 58.1: Crear un job que valide que se generaron los archivos esperados

**Objetivo:** Crear un job que valide que los archivos generados en un paso anterior están presentes.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que valida archivos generados
job_validate_outputs:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - test -f resultado1.txt && echo "resultado1.txt existe" || echo "resultado1.txt no existe"  # Valida el primer archivo.
    - test -f resultado2.txt && echo "resultado2.txt existe" || echo "resultado2.txt no existe"  # Valida el segundo archivo.
```

</details>

---

## **59. Limpieza de Archivos Temporales**

### Ejercicio 59.1: Crear un job que limpie archivos temporales generados

**Objetivo:** Crear un job que limpie los archivos temporales generados en pasos anteriores.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que limpia archivos temporales
job_cleanup_temp_files:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - rm -rf temp.tmp  # Elimina el archivo temporal.
```

</details>

---

## **60. Archivar Resultados de Prueba**

### Ejercicio 60.1: Crear un job que archive los resultados de prueba

**Objetivo:** Crear un job que archive los resultados de prueba en un directorio específico.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que archiva resultados de prueba
job_archive_test_results:
  stage: build  # Este job se ejecuta en la etapa `build`.

  script:
    - mkdir -p archive  # Crea un directorio de archivo.
    - echo "Resultados de prueba" > archive/test_results.txt  # Guarda resultados en el archivo.

  artifacts:
    paths:
      - archive/test_results.txt  # Almacena 'test_results.txt' como artefacto.
```

</details>

---

# Ejercicios de GitLab CI/CD (continuación)

## **61. Definir Etapas del Pipeline**

### Ejercicio 61.1: Crear un pipeline que defina etapas específicas

**Objetivo:** Crear un pipeline que defina la etapa `build` y prepare el entorno para compilar el proyecto.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Definición de etapas del pipeline
stages:
  - build  # Etapa de construcción donde se compila o prepara el proyecto.
```

</details>

---

## **62. Crear un Job de Construcción**

### Ejercicio 62.1: Crear un job que se ejecute en la etapa de construcción

**Objetivo:** Crear un job llamado `job_build` que se ejecute en la etapa `build` y muestre un mensaje de construcción.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que se ejecuta en la etapa build
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
```

</details>

---

## **63. Simular un Fallo Intencionado en el Job**

### Ejercicio 63.1: Crear un job que falle intencionadamente

**Objetivo:** Modificar el job para que falle intencionadamente, simulando un error en el proceso de construcción.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que simula un fallo
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.
```

</details>

---

## **64. Agregar Comandos de Limpieza Post-Jobs**

### Ejercicio 64.1: Añadir un `after_script` para la limpieza

**Objetivo:** Añadir un comando de limpieza que se ejecute después de que termine el `script`, independientemente de si el job tuvo éxito o falló.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job con comandos de limpieza
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - echo 'Cleaning up...'  # Mensaje indicando que se está ejecutando un proceso de limpieza post-job.
```

</details>

---

## **65. Verificar la Ejecución del `after_script`**

### Ejercicio 65.1: Crear un job y verificar la ejecución del `after_script` al fallar

**Objetivo:** Ejecutar el job y asegurarte de que el mensaje de limpieza se muestre incluso cuando el job falla.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que muestra el uso de after_script
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - echo 'Cleaning up...'  # Mensaje indicando que se está ejecutando un proceso de limpieza post-job.
```

</details>

---

## **66. Registrar Salidas en `after_script`**

### Ejercicio 66.1: Modificar el `after_script` para registrar información adicional

**Objetivo:** Modificar el `after_script` para incluir más detalles sobre el estado del job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que registra información adicional en after_script
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - echo 'Job failed, cleaning up...'  # Mensaje indicando que el job falló y se está limpiando.
```

</details>

---

## **67. Agregar Más Comandos en el Job de Construcción**

### Ejercicio 67.1: Incluir múltiples pasos en el script del job de construcción

**Objetivo:** Ampliar el `script` del job para incluir múltiples comandos de construcción antes de que ocurra el fallo.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job de construcción con múltiples pasos
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Starting build process...'  # Mensaje inicial de construcción.
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - echo 'Running tests...'  # Mensaje indicando que se están ejecutando las pruebas.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - echo 'Job failed, cleaning up...'  # Mensaje indicando que el job falló y se está limpiando.
```

</details>

---

## **68. Resumir Resultados en `after_script`**

### Ejercicio 68.1: Crear un resumen de la construcción en `after_script`

**Objetivo:** Crear un resumen en el `after_script` que indique el número de pasos que se intentaron ejecutar.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que resume resultados en after_script
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Starting build process...'  # Mensaje inicial de construcción.
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - echo 'Running tests...'  # Mensaje indicando que se están ejecutando las pruebas.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - echo 'Job failed after 3 steps, cleaning up...'  # Resumen indicando que falló después de 3 pasos.
```

</details>

---

## **69. Personalizar el Mensaje de Salida en Caso de Éxito**

### Ejercicio 69.1: Modificar el `after_script` para un mensaje personalizado

**Objetivo:** Cambiar el mensaje de `after_script` para mostrar un mensaje diferente si el job se completa exitosamente.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job que personaliza el mensaje de after_script
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Starting build process...'  # Mensaje inicial de construcción.
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - echo 'Job failed, performing cleanup...'  # Mensaje que indica limpieza tras el fallo.
    - echo 'If the job succeeds, different actions will occur.'  # Mensaje adicional para el caso de éxito.
```

</details>

---

## **70. Implementar Comandos Condicionales en `after_script`**

### Ejercicio 70.1: Implementar comandos en `after_script` según el resultado del job

**Objetivo:** Añadir lógica condicional en el `after_script` para ejecutar comandos diferentes según el resultado del job.

<details>
<summary>Contenido del archivo</summary>

```yaml
# .gitlab-ci.yml

# Job con lógica condicional en after_script
job_build:
  stage: build  # Define que este job se ejecuta en la etapa `build`.

  script:
    - echo 'Starting build process...'  # Mensaje inicial de construcción.
    - echo 'Building project...'  # Mensaje indicativo de que el proceso de construcción ha comenzado.
    - exit 1  # El código de salida '1' indica un fallo intencionado en el job.

  after_script:
    - |
      if [ $? -ne 0 ]; then  # Comprueba el código de salida del script anterior.
        echo 'Job failed, cleaning up...'  # Mensaje en caso de fallo.
      else
        echo 'Job succeeded!'  # Mensaje en caso de éxito.
      fi
```

</details>

---

