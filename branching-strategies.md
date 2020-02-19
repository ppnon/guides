# Branching Strategies

1. [Introducción] (#introduccion)
	1.1. [Branch by Abstraction (BBA)] (#branch-by-Abstraction)
	1.2. [Feature Toggle]
	1.3. [Cherry Picking]

---

# Introducción

Para el versionamiento de código fuentes... los siguientes modelos:

- Feature Branches
- Gitflow Workflow
- Trunk Based Development (TBD)

A continuación se presentan algunos patrones que dan soporte a las estrategias de versionamiento mencionadas. Dependerá de la aplicación y el equipo decidir cual utilizar y en que situaciones.

#### Branch by Abstraction (BBA)

Es un patrón que facilita la realización de un cambio a gran escala de forma gradual permitiendo liberar nuevas versiones (releases) de una aplicación regularmente mientras el cambio aún está en progreso. Consiste en utilizar una capa de abstracción para soportar que múltiples implementaciones coexistan en una misma aplicación, para lo cual debe utilizar una abstracción y múltiples implementaciones para migrar de una implementación a otra.

Pasos básico para su aplicación:

1. Cree una abstracción para la funcionalidad (feature) de la aplicación que necesita ser modificada.
2. Modifique la aplicación para que utilice la nueva abstracción creada para la funcionalidad. 
3. Adapte el código existente de la funcionalidad para que este sea una implementación de la abstracción.
4. Cree la nueva implementación para la funcionalidad que está modificando.
5. Modifique la aplicación para que utilice la nueva implementación en lugar de la anterior (paso 3).
4. Si la implementación anterior ya no es requerida elimínela.
5. Repita los pasos anteriores para todas las funcionalidad necesarias.

> Nota: Una vez que las implementaciones anteriores han sido remplazadas puede considerar en remover la capa de abstracción creada.

| **Ventajas** | **Desventajas** |
|---|---|
| El código funcionará durante todo el tiempo de la reestructuración, permitiendo el despliegue continuo. | Este patrón puede agregar sobrecarga de trabajo al proceso de desarrollo, especialmente en casos donde el código es espagueti. |
| Tu calendario de liberaciones no se verá afectado por los cambios estructurales de la aplicación. | El exceso de abstracción puede ralentizar el entendimiento y mantenimiento de la aplicación por lo cual siempre se debe evaluar si estas se deben mantener o no. |

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
	if ( featureFlags.isOn("my-new-feature") ) {
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

#### Cherry Picking

Es el proceso de seleccionar un commit de un branch y replicarlo a otro branch. Este proceso puede ser muy útil en ciertas situaciones pero no siempre es una buena práctica por lo cual siempre debe analizar cual es la mejor situación para utilizarlo en lugar de un merge.

Situaciones donde puede ser útil:

- Para corregir un commit realizado en un branch equivocado.
- Para replicar un commit en múltiples branchs
- Para recuperación de commits perdidos
- Para corrección de bugs 

Cherry pick de un commit del branch Feature a Master

```bash
	a -b - c - d 		Master
	   \
	    e - f - g 		Feature

# Copiaremos el commit f del Feature al Master

	a -b - c - d - f 	Master
	   \
	    e - f - g 		Feature
```

| **Ventajas** | **Desventajas** |
|---|---|
| Permite integrar bugs o cambios sin necesidad de realizar un merge completo | Puede causar duplicación o pérdida de commits si no se usa de forma correcta. |
| Está incluido dentro de las funcionalidades de Git (cherry-pick) | No siempre se puede aplicar |

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

Es un modelo de ramificación de control de fuentes de una aplicación basado en Git Workflow y Feature Branch Workflow sin agregar nuevos conceptos sino más bien roles específicos para las diferentes ramas y definir el cómo y cuándo estas deben interactuar entre si.

Este modelo provee un robusto marco de trabajo para la gestión de grandes proyecto con ciclos de lanzamiento de versiones programados. Está compuesto de cinco ramas de trabajo, dos principales y tres de soporte.

### **Ramas:**

### Master

La rama origin/master es el origen de todo y tiene como único propósito contener la historia de los releases y hotfixes de la aplicación. Las única ramas que se integran a la rama master son las ramas release y hotfix, ambos generan un tag con un número de versión.

Esta rama tiene un tiempo de vida infinito y siempre debe reflejar la versión liberada en producción.

### Develop

Esta rama nace de la rama master al inicio del proyecto y tiene la función de ser la rama de integración para todos las ramas features. Desde esta rama son creadas las ramas release y a esta también se le integran las ramas bugfix.

Esta rama tiene un tiempo de vida infinito y siempre debe contener todas las ramas feature terminadas.

### Feature

Esta rama nace de la rama develop y termina al ser integrada a la misma. Cada nueva funcionalidad o cambio debe tener su propia rama feature.

Esta rama tiene un tiempo de vida medio o corto, en caso contrario se podría utilizar el patrón *Branch By Abstraction* para separar la funcionalidad a trabajar en funcionalidades más pequeñas.

### Release

Esta rama nace de la rama develop y termina al ser integrada a la rama master. El objetivo de esta rama es 


### Bugfix


**Workflow**
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