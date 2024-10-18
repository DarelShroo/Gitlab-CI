# Conceptos y Ejemplos de GitLab CI

Este repositorio contiene ejemplos y ejercicios relacionados con pipelines de GitLab CI/CD, diseñados para ayudar a los usuarios a entender y dominar el proceso de GitLab CI. A continuación, encontrarás una descripción general de los conceptos clave de GitLab CI, junto con ejemplos prácticos para demostrar su uso.

---

## Tipos de Pipelines

### 1. **Pipelines de Solicitud de Fusión**
Los pipelines de solicitud de fusión se ejecutan contra la rama de origen de una solicitud de fusión siempre que se hagan commits en esa rama. Se asegura de que los cambios en la rama de origen sean válidos antes de fusionarlos con la rama de destino.

### 2. **Pipelines de Resultado de Fusión**
Los pipelines de resultado de fusión son un tipo especial de pipelines de solicitud de fusión. Se ejecutan contra una fusión temporal de la rama de origen de una solicitud de fusión con la rama de destino. Esto asegura que los cambios en la rama de origen se integrarán bien con la rama de destino sin realizar una fusión real.

> Este tipo de pipeline no fusiona las ramas realmente, solo simula la fusión y ejecuta un pipeline contra esa simulación. Es útil para verificar cómo se comportará tu rama al integrarse con una rama estable.

### 3. **Trenes de Fusión**
Los trenes de fusión son un tipo especial de pipeline de resultado de fusión. Se agrupan varias solicitudes de fusión y se ejecutan pipelines de resultado de fusión separados para cada solicitud en paralelo. Sin embargo, en lugar de hacer una fusión temporal entre la rama de origen y la de destino de la solicitud de fusión, el tren de fusión crea una fusión temporal de todas las ramas de origen de las solicitudes anteriores en la cola.

> Los trenes de fusión son útiles para asegurarte de que varias ramas se integren bien en una rama de destino que cambia rápidamente cuando se fusionan.

---

## Componentes Clave

### 1. **GitLab**
GitLab es la plataforma que gestiona los proyectos y pipelines. Aquí es donde defines, configuras y activas los pipelines para tus proyectos.

### 2. **GitLab Runner**
GitLab Runner es un programa que recibe trabajos del pipeline de GitLab y los ejecuta. Es el "músculo" que ejecuta los trabajos del pipeline en tu infraestructura.

### 3. **Executor**
El executor es el entorno donde se ejecutan los trabajos. Puede ser un contenedor Docker, una máquina virtual o un servidor físico.

---

## Ejemplo de Código con Artifacts

El siguiente ejemplo demuestra el uso de **artifacts** en un trabajo. Los artifacts son archivos generados por un trabajo que se preservan para su uso posterior por otros trabajos.

```yaml
unit-tests:
  stage: test
  script:
    - pip install pytest
    - pytest test/ --junitxml=unit_test_results.xml
  artifacts:
    reports:
      junit: unit_test_results.xml
    when: always
```

En este ejemplo, los resultados de las pruebas se guardan en un archivo llamado `unit_test_results.xml`. El keyword `artifacts` se utiliza para preservar estos archivos para su uso posterior en otros trabajos o etapas.

---

## Evitar la Ejecución del Pipeline

Para evitar que un pipeline se ejecute después de un commit, puedes escribir las siguientes palabras clave en tu mensaje de commit:

- `[skip ci]`
- `[ci skip]`

Esto evitará que el pipeline se dispare para el commit.

---

## Ejemplo: Despliegue a Staging

Aquí tienes un ejemplo de un trabajo de despliegue que usa las palabras clave `script` y `tags`:

```yaml
deploy-to-staging:
  stage: staging
  script: ./deploy-staging.sh
  tags:
    - windows
    - staging
```

- **script**: Especifica el comando o script que debe ejecutarse en este trabajo. En este caso, es el despliegue en el entorno de staging.
- **tags**: Se usa para especificar el tipo de runner que debe ejecutar el trabajo. En este caso, usa las etiquetas `windows` y `staging` para dirigirse a runners específicos.

---

## Variables

Aquí tienes algunas variables predefinidas de GitLab CI/CD que puedes utilizar en tu archivo `.gitlab-ci.yml`:

- `$CI_COMMIT_BRANCH`: El nombre de la rama actual.
- `$CI_COMMIT_REF_NAME`: El nombre de la rama o tag que disparó el pipeline.
- `$CI_PIPELINE_SOURCE`: El origen del pipeline (por ejemplo, push, merge_request_event, schedule, etc.).
- `$CI_COMMIT_SHA`: El hash completo (SHA) del commit actual.
- `$CI_JOB_ID`: El ID del trabajo actual.
- `$CI_PROJECT_NAME`: El nombre del proyecto.
- `$CI_PROJECT_PATH`: La ruta completa del proyecto en GitLab.
- `$CI_ENVIRONMENT_NAME`: El nombre del entorno que se está desplegando (si está definido).

---

## Incluir Otros Archivos de Configuración CI/CD

Puedes incluir otros archivos `.gitlab-ci.yml` desde diferentes fuentes utilizando el keyword `include`. Algunas de las fuentes posibles son:

```yaml
include:
  - component
  - local
  - project
  - remote
  - template
```

También puedes incluir reglas adicionales, como:

```yaml
include:
  - rules:
      - if: '$CI_COMMIT_REF_NAME == "main"'
      - changes:
          - src/**/*
```

---

## Etapas del Pipeline

Si no defines las etapas en tu archivo `.gitlab-ci.yml`, las etapas predeterminadas del pipeline son las siguientes:

1. `.pre`
2. `build`
3. `test`
4. `deploy`
5. `.post`

Puedes definir tus etapas personalizadas en la configuración del pipeline para controlar el orden de ejecución de los trabajos.

---

## Ejemplo de Configuración de Workflow

Puedes configurar el workflow de tu pipeline de la siguiente manera:

```yaml
workflow:
  name: 'Pipeline for branch: $CI_COMMIT_BRANCH'
  auto_cancel:
    on_job_failure: all
    on_new_commit: interruptible
```

- **auto_cancel**: Si un trabajo falla, se cancela el pipeline, y si se hace un nuevo commit, el pipeline actual se interrumpe a favor del nuevo.
- **workflow**: Define el nombre del pipeline y configuraciones adicionales, como la cancelación en caso de fallo de un trabajo.

---

## Ejemplo de Definición de Job

Aquí tienes un ejemplo de definición de un job para construir el proyecto:

```yaml
job_build:
  stage: build
  interruptible: true
  script:
    - echo 'Building project...'
```
Este trabajo se ejecutará en la etapa `build` y es interruptible, lo que significa que puede ser cancelado si se dispara un nuevo pipeline.

---

## Ejemplo de Especificación para Inputs

```yaml
spec:
  inputs:
    version:
      regex: ^v\d\.\d+(\.\d+)$
    environment:
      options:
        - development
        - staging
        - production
    job-stage:
      available:
        type: boolean
    website:
    user:
      default: 'test-user'
    flags:
      default: ''
      description: 'Sample description of the `flags` input details.'
```

En este ejemplo, se definen variables de entrada con patrones regex, valores predeterminados y descripciones opcionales para mejorar la legibilidad.

---

## Conclusión

Este repositorio está diseñado para ayudarte a entender y practicar los conceptos de GitLab CI a través de ejemplos prácticos y ejercicios. ¡No dudes en explorar y modificar los ejemplos para ajustarlos a tus necesidades!

Para más información, consulta la [documentación oficial de GitLab CI](https://docs.gitlab.com/ee/ci/).
