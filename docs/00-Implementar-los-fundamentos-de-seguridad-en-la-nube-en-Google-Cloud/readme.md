

# LaB 1:Roles basicos y ddefinidos por 
cloud iam:
En este laboratorio se conocen los permisos basicos y otorgar acceso a un usuario solo como vista de un cloud storage object, el usauri 1 crea el recurso y da accesos al usuario 2, para solo ver el buckets creado por el usuario 1 , el buckets es como una carpeta de recursos para alamacenar archivos.

* Se realizo configuracion desde IAM parausuarios y el uso de recursos cloud storage.
* El usuario principal creo un recurso bucket llamado testing-21d , en este se cargo un archivo html luego se edito para que sea txt, tambien se compartieron acceso a usuario para cloud objet storage, al usuario 2.

# Lab 2:

NO se puede crear un rol personalizado a nivel de carpetas, pero si a nivel de una organizacion y proyectos.
LOs roles personalizados son para dar permisos a los usuarios a acciobines espesificas en los recursos de google cloud.
LOs permisos siempre se corresponden 1 a 1.
A los usuarios que no son propietarios, incluidos los administradores de la organización, se les debe asignar el rol de administrador de roles de la organización (roles/iam.organizationRoleAdmin) o el rol de administrador de roles de IAM (roles/iam.roleAdmin). El rol de revisor de seguridad de IAM (roles/iam.securityReviewer) permite ver los roles personalizados, pero no administrarlos.

No puedes otorgar roles personalizados de una organización o proyecto en un recurso que es propiedad de una organización o proyecto diferente.



## Crea un rol personalizado
Para crear un rol personalizado, un llamador debe poseer el permiso iam.roles.create. De forma predeterminada, el propietario de un proyecto o una organización tiene este permiso y puede crear y administrar roles personalizados.

A los usuarios que no son propietarios, incluidos los administradores de la organización, se les debe asignar el rol de administrador de roles de la organización o el rol de administrador de roles de IAM.

Crea un rol personalizado con un archivo YAML
Crea un archivo YAML que contenga la definición de tu rol personalizado. El archivo debe estar estructurado de la siguiente manera:
```
title: [ROLE_TITLE]
description: [ROLE_DESCRIPTION]
stage: [LAUNCH_STAGE]
includedPermissions:
- [PERMISSION_1]
- [PERMISSION_2]
```

Cada uno de los valores de marcador de posición se describe a continuación:
```
[ROLE-TITLE] es un título descriptivo para el rol, como Visualizador de roles.
[ROLE-DESCRIPTION] es una descripción corta sobre el rol, como Descripción de mi rol personalizado.
[LAUNCH_STAGE] indica la etapa de un rol en el ciclo de vida del lanzamiento, como ALFA, BETA o DG.
```


includedPermissions especifica la lista de uno o más permisos para incluir en el rol personalizado, como iam.roles.get.
Ahora, comencemos. Para crear tu archivo YAML de definición del rol, ejecuta el siguiente comando:
nano role-definition.yaml
ç



Ahora, comencemos. Para crear tu archivo YAML de definición del rol, ejecuta el siguiente comando:
nano role-definition.yaml
Se copió correctamente
Agrega esta definición para el rol personalizado al archivo YAML:
title: "Role Editor"
description: "Edit access for App Versions"
stage: "ALPHA"
includedPermissions:
- appengine.versions.create
- appengine.versions.delete
Se copió correctamente
Luego, guarda y cierra el archivo presionando CTRL + X, Y y, luego, INTRO.

Ejecuta el siguiente comando de gcloud:

gcloud iam roles create editor --project $DEVSHELL_PROJECT_ID \
--file role-definition.yaml
Se copió correctamente
Si el rol se creó correctamente, se devuelve la siguiente respuesta:

Created role [editor].
description: Edit access for App Versions
etag: BwVs4O4E3e4=
includedPermissions:
- appengine.versions.create
- appengine.versions.delete
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/editor
stage: ALPHA
title: Role Editor
Haz clic en Revisar mi progreso para verificar el objetivo.
Crear un rol personalizado con un archivo YAML


Crea un rol personalizado usando marcas
Ahora, usarás el método de marcas para crear un nuevo rol personalizado. Las marcas tienen una forma similar al archivo YAML, por lo que reconocerás cómo se crea el comando.




Crea un rol personalizado usando marcas
Ahora, usarás el método de marcas para crear un nuevo rol personalizado. Las marcas tienen una forma similar al archivo YAML, por lo que reconocerás cómo se crea el comando.

Ejecuta el siguiente comando de gcloud para crear un nuevo rol usando marcas:
gcloud iam roles create viewer --project $DEVSHELL_PROJECT_ID \
--title "Role Viewer" --description "Custom role description." \
--permissions compute.instances.get,compute.instances.list --stage ALPHA
Se copió correctamente
Resultado de ejemplo:

Created role [viewer].
description: Custom role description.
etag: BwVs4PYHqYI=
includedPermissions:
- compute.instances.get
- compute.instances.list
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/viewer
stage: ALPHA
title: Role Viewer
Haz clic en Revisar mi progreso para verificar el objetivo.
Crear un rol personalizado usando marcas


Tarea 5. Haz una lista de los roles personalizados
Ejecuta el siguiente comando de gcloud para enumerar los roles personalizados y especifica si son a nivel del proyecto o a nivel de la organización:
gcloud iam roles list --project $DEVSHELL_PROJECT_ID
Se copió correctamente
Resultado de ejemplo:

---
description: Edit access for App Versions
etag: BwVxLgrnawQ=
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/editor
title: Role Editor
---
description: Custom role description.
etag: BwVxLg18IQg=
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/viewer
title: Role Viewer
Para enumerar los roles borrados, también puedes especificar la marca --show-deleted.

Ejecuta el siguiente comando de gcloud para enumerar los roles predefinidos:
gcloud iam roles list
Se copió correctamente
Tarea 6. Actualiza un rol personalizado existente
Un patrón común para actualizar los metadatos de un recurso, como un rol personalizado, es leer su estado actual, actualizar los datos de forma local y, luego, enviar los datos modificados para su escritura. Este patrón podría causar un conflicto si dos o más procesos independientes intentan la secuencia en simultáneo.

Por ejemplo, si dos propietarios de un proyecto intentan hacer cambios conflictivos en un rol al mismo tiempo, algunos cambios podrían fallar.

Cloud IAM resuelve este problema usando una propiedad etag en los roles personalizados. Esta propiedad se usa para verificar si el rol personalizado cambió desde la última solicitud. Cuando realizas una solicitud a Cloud IAM con un valor de ETag, Cloud IAM compara este valor en la solicitud con el valor de ETag existente asociado al rol personalizado y solo escribe el cambio si los valores de ETag coinciden.

Usa el comando gcloud iam roles update para actualizar los roles personalizados de una de estas dos maneras:

Con un archivo YAML que contenga la definición del rol actualizado
Con marcas que especifiquen la definición del rol actualizado
Cuando actualices un rol personalizado, debes especificar si se aplica a nivel de la organización o a nivel del proyecto usando las marcas --organization [ORGANIZATION_ID] o --project [PROJECT_ID]. En cada uno de los siguientes ejemplos, se crea un rol personalizado a nivel del proyecto.

El comando describe muestra la definición del rol y, además, incluye un valor ETag que identifica de forma única la versión actual del rol. El valor ETag debe proporcionarse en la definición del rol actualizado para garantizar que no se reemplace ningún cambio de rol simultáneo.

Actualiza un rol personalizado con un archivo YAML
Obtén la definición actual del rol ejecutando el siguiente comando gcloud y reemplazando [ROLE_ID] con editor.
gcloud iam roles describe [ROLE_ID] --project $DEVSHELL_PROJECT_ID
Se copió correctamente
El comando describe devuelve el siguiente resultado:

description: [ROLE_DESCRIPTION]
etag: [ETAG_VALUE]
includedPermissions:
- [PERMISSION_1]
- [PERMISSION_2]
name: [ROLE_ID]
stage: [LAUNCH_STAGE]
title: [ROLE_TITLE]
Copia el resultado para crear un nuevo archivo YAML en los próximos pasos.

Crea un archivo new-role-definition.yaml con tu editor:

nano new-role-definition.yaml
Se copió correctamente
Pega el resultado del último comando y agrega estos dos permisos en includedPermissions:
- storage.buckets.get
- storage.buckets.list
Se copió correctamente
Tu archivo YAML se verá así cuando termines:

description: Edit access for App Versions
etag: BwVxIAbRq_I=
includedPermissions:
- appengine.versions.create
- appengine.versions.delete
- storage.buckets.get
- storage.buckets.list
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/editor
stage: ALPHA
title: Role Editor
Para guardar y cerrar el archivo, presiona CTRL + X, Y y, luego, INTRO.

Ahora usa el comando update para actualizar el rol. Ejecuta el siguiente comando gcloud y reemplaza [ROLE_ID] con editor:

gcloud iam roles update [ROLE_ID] --project $DEVSHELL_PROJECT_ID \
--file new-role-definition.yaml
Se copió correctamente
Si el rol se actualizó con éxito, se devuelve la siguiente respuesta:

description: Edit access for App Versions
etag: BwVxIBjfN3M=
includedPermissions:
- appengine.versions.create
- appengine.versions.delete
- storage.buckets.get
- storage.buckets.list
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/editor
stage: ALPHA
title: Role Editor
Haz clic en Revisar mi progreso para verificar el objetivo.
Actualizar un rol personalizado con un archivo YAML


Actualiza un rol personalizado usando marcas
Cada parte de la definición de un rol se puede actualizar usando la marca correspondiente. Para obtener una lista de todas las marcas posibles de la documentación de referencia del SDK, consulta el tema Actualización de los roles de IAM de gcloud.

Usa las siguientes marcas para agregar o quitar permisos:

--add-permissions: agrega uno o más permisos separados por comas al rol.
--remove-permissions: quita uno o más permisos separados por comas del rol.
De manera alternativa, puedes simplemente especificar los permisos nuevos con la marca --permissions [PERMISSIONS] y proporcionar una lista de permisos separados por comas para reemplazar la lista de permisos existente.

Ejecuta el siguiente comando de gcloud para agregar permisos al rol de visualizador a través del uso de marcas:
gcloud iam roles update viewer --project $DEVSHELL_PROJECT_ID \
--add-permissions storage.buckets.get,storage.buckets.list
Se copió correctamente
Si el rol se actualizó con éxito, se devuelve la siguiente respuesta:

description: Custom role description.
etag: BwVxLi4wTvk=
includedPermissions:
- compute.instances.get
- compute.instances.list
- storage.buckets.get
- storage.buckets.list
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/viewer
stage: ALPHA
title: Role Viewer
Haz clic en Revisar mi progreso para verificar el objetivo.
Actualizar un rol personalizado usando marcas


Tarea 7. Inhabilita un rol personalizado
Cuando se inhabilita un rol, todas las vinculaciones de políticas relacionadas con él se desactivan, lo que significa que no se concederán los permisos del rol, incluso si lo asignas a un usuario.

La forma más fácil de inhabilitar un rol personalizado existente es usar la marca --stage y establecerla como DISABLED.

Ejecuta el siguiente comando de gcloud para inhabilitar el rol de visualizador:
gcloud iam roles update viewer --project $DEVSHELL_PROJECT_ID \
--stage DISABLED
Se copió correctamente
Si el rol se actualizó con éxito, se devuelve la siguiente respuesta:

description: Custom role description.
etag: BwVxLkIYHrQ=
includedPermissions:
- compute.instances.get
- compute.instances.list
- storage.buckets.get
- storage.buckets.list
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/viewer
stage: DISABLED
title: Role Viewer
Haz clic en Revisar mi progreso para verificar el objetivo.
Inhabilitar un rol personalizado


Tarea 8. Borra un rol personalizado
Usa el comando gcloud iam roles delete para borrar un rol personalizado. Cuando lo borres, el rol estará inactivo y no se podrá usar para crear nuevas vinculaciones de políticas de IAM:
gcloud iam roles delete viewer --project $DEVSHELL_PROJECT_ID
Se copió correctamente
Resultado de ejemplo:


deleted: true
description: Custom role description.
etag: BwVxLkf_epw=
includedPermissions:
- compute.instances.get
- compute.instances.list
- storage.buckets.get
- storage.buckets.list
name: projects/qwiklabs-gcp-04-4c72d63e4544/roles/viewer
stage: DISABLED
title: Role Viewer
Después de que hayas borrado el rol, las vinculaciones existentes permanecen, pero están inactivas. El rol se puede recuperar antes de los 7 días. Después de este plazo, se inicia un proceso de 30 días de duración para borrar el rol de manera permanente. Después de 37 días, el ID del rol pasa a estar disponible para volver a usarlo.

Nota: Si estás eliminando gradualmente un rol, cambia su propiedad role.stage a OBSOLETO y establece el `deprecation_message` para informar a los usuarios qué roles alternativos deberían usar o dónde obtener más información.
Tarea 9. Restablece un rol personalizado
Un rol se puede restablecer antes de que se cumpla el plazo de 7 días. El estado de los roles borrados es INHABILITADO. Para que un rol vuelva a estar disponible, actualiza la marca --stage:
gcloud iam roles undelete viewer --project $DEVSHELL_PROJECT_ID
Se copió correctamente
Haz clic en Revisar mi progreso para verificar el objetivo.
Recuperar un rol personalizado


¡Felicitaciones!
En este lab, creaste y administraste roles personalizados en IAM.

Próximos pasos y más información
Obtén más información sobre IAM en el artículo de IAM Cloud Identity and Access Management.

Capacitación y certificación de Google Cloud
Recibe la formación que necesitas para aprovechar al máximo las tecnologías de Google Cloud. Nuestras clases incluyen habilidades técnicas y recomendaciones para ayudarte a avanzar rápidamente y a seguir aprendiendo. Para que puedas realizar nuestros cursos cuando más te convenga, ofrecemos distintos tipos de capacitación de nivel básico a avanzado: a pedido, presenciales y virtuales. Las certificaciones te ayudan a validar y demostrar tus habilidades y tu conocimiento técnico respecto a las tecnologías de Google Cloud.

Última actualización del manual: 15 de abril de 2024

Prueba más reciente del lab: 13 de julio de 2023

Copyright 2025 Google LLC. All rights reserved. Google y el logotipo de Google son marcas de Google LLC. Los demás nombres de productos y empresas pueden ser marcas de las respectivas empresas a las que estén asociados.