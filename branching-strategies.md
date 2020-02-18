Para el versionamiento de código fuentes... los siguientes modelos:

- Feature Branches
- Gitflow Workflow
- Trunk Based Development (TBD)

A continuación se presentan algunos patrones que dan soporte a las estratégias de versionamiento mencionadas. Dependerá de la aplicación y el equipo decidir cual utilizar y en que situaciones.

#### Branch by Abstraction (BBA)

Es un patrón que facilita la realización de un cambio a gran escala de forma gradual permitiendo liberar nuevas versiones (releases) de una aplicación regularmente mientras el cambio aún está en progreso. Consiste en utilizar una capa de abstracción para soportar que múltiples implementaciones coexistan en una misma aplicación, para lo cual deb utilizar una abstracción y múltiples implementaciones para migrar de una implementación a otra.

Pasos básico para su aplicación:

1. Cree una abstacción para la funcionalidad (feature) de la aplicación que necesita ser modificada.
2. Modifique la aplicación para que utilice la nueva abstracción creada para la funcionalidad. 
3. Adapte el código existente de la funcionalidad para que este sea una implementación de la abstracción.
4. Cree la nueva implementación para la funcionalidad que está modificando.
5. Modifique la aplicación para que utilice la nueva implementación en lugar de la anterior (paso 3).
4. Si la implementación anterior ya no es requerida elimínela.
5. Repita los pasos anteriores para todas las funcionalidad necesarias.

> Nota: Una vez que las implementaciones anteriores han sido remplazadas puede considerar en remover la capa de abstracción creada.

| **Ventajas** | **Desventajas** |
|---|---|
| El código funcionará durante todo el tiempo de la restructuración, permitiendo el despliegue continuo. | Este patrón puede agregar sobrecarga de trabajo al proceso de desarrollo, especialmente en casos donde el código es espagueti. |
| Tu calendario de liberaciones no se verá afectado por los cambios estructurales de la aplicación. | El exceso de abstracción puede relentizar el entendimiento y mantenimiento de la aplicación por lo cual siempre se debe evaluar si estas se deben mantener o no. |

##### Referencias

- https://continuousdelivery.com/2011/05/make-large-scale-changes-incrementally-with-branch-by-abstraction/
- https://paulhammant.com/blog/branch_by_abstraction.html
- https://martinfowler.com/bliki/BranchByAbstraction.html
- https://paulhammant.com/2014/09/24/a-functional-branch-by-abstraction-case-study/

#### Feature Toggle

También conocido como *Feature Flag*, es un patrón que permite controlar que funcionalidades (features) de una aplicación estarán disponibles en su versión liberada (release). La idea básica es tener un archivo o repositorio de configuración externo a la aplicación donde se definen los interruptores (toggles) de las funcionalidades aun en proceso, estos interruptores son utilizados por la aplicación durante su ejecución para decidir si una funcionalidad estará disponible o no.

Este patrón consiste en aislar  el código de una funcionalidad en proceso colocando un interruptor para su ejecución, el código solo será ejecutado si es que el interruptor esta activado.

Ejemplos básicos de su aplicación:

```html
<toggle name="my-new-feature">
	<p>Take our new <a href = 'my-new-feature'>feature</a></p>
</toggle>
```

```java
if( featureFlags.isOn("my-new-feature") ){
	showNewFeatureInUI();
}
```
> Nota: Cuando una funcionalidad está terminada y liberada se deben de eliminar las configuraciones e interruptores de esta de todo el código.

| **Ventajas** | **Desventajas** |
|---|---|
| Facilita a los equipos el proceso de integración continua desacoplando los cambios en el código de las funcionalidades liberadas. | La implementación de interruptores puede aumentar la complejidad durante el desarrollo. |
| Desaparecen o disminuyen los grandes merge y el impacto de estos. | Los interruptores de las funcionalidades liberadas no eliminados pueden conducir a un código desordenado con el tiempo. |
| Se pueden manejar múltiples categorías de interruptores no solo para funcionalidades sino también para capacidades de la aplicación. | El error humano puede exponer accidentalmente una funcionalidad incompleta. |
|  | Las pruebas puede volverse más complicadas y laboriosas. |

##### Referencias

- https://devops.com/feature-branching-vs-feature-flags-whats-right-tool-job/
- https://martinfowler.com/bliki/FeatureToggle.html
- https://martinfowler.com/articles/feature-toggles.html

#### Cherry Pick

...

| **Ventajas** | **Desventajas** |
|---|---|
|  |  |
|  |  |

##### Referencias

- https://www.educba.com/git-cherry-pick/
- https://www.atlassian.com/git/tutorials/cherry-pick
- https://git-scm.com/docs/git-cherry-pick

---

## Feature Branches
...

### Workflow

### Consideraciones

| **Ventajas** | **Desventajas** |
|---|---|
| - ... | - ... |
| - ... | - ... |

## Gitflow Workflow

...

### Workflow
...

### Consideraciones
...


| **Ventajas** | **Desventajas** |
|---|---|
| - ... | - ... |
| - ... | - ... |

## Trunk Based Developmet (TBD)

...

### Workflow
...

### Consideraciones
...

| **Ventajas** | **Desventajas** |
|---|---|
| - ... | - ... |
| - ... | - ... |



https://paulhammant.com/2015/04/23/the-origins-of-trunk-based-development/
https://paulhammant.com/2014/08/12/trunk-supporting-practices/
https://paulhammant.com/2018/05/23/examining-ci-cd-and-branching-models/