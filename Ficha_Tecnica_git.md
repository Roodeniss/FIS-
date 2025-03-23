### Fuentes de información 

- [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [Documentación Oficial de Git](https://git-scm.com/docs)
- [Varonis](https://www.varonis.com/blog/git-branching "https://www.varonis.com/blog/git-branching")
- [Git - Merge - GeeksforGeeks](https://www.geeksforgeeks.org/git-merge/ "https://www.geeksforgeeks.org/git-merge/")
- [Git Tutorial](https://www.geeksforgeeks.org/git-tutorial/?ref=header_outind)
### Definiciones 

1) **Branch**:
   Las git branches son un punto de referencia a una instancia. Con instancia nos referimos a cualquier tipo de cambio en el codigo ya sea un nuevo desarrollo dentro de un proyecto, la coreccion de un bug, entre otras. Sin importar la maginitud de la modificacion esta puede y deberia ser encapsulada en una branch, esto aporta mucho valor a la hora de manejar codigo inestable, ya que no queremos que dicho codigo se integre al cdigo principal sin antes completarse su ciclo de vida.
2) **Merge**:
   Git merge es un comando de git que se encarga de combinar multiples commits, unificando las historias de dos branches. Los casos mas frecuentes de uso del git merge es para combinar los cambios de dos branches distintas. Git merge busca un punto base o commit base desde el cual iniciar la fusion de las ramas y apartir de ese crea un nuevo commit de fusion que combinara ambas ramas.
3) **Resolución de conflictos**:
   Si las dos ramas que intentas fusionar cambiaron la misma parte del mismo archivo, Git no podrá determinar qué versión usar. En tal caso, se detiene justo antes de la confirmación de fusión para que puedas resolver los conflictos manualmente. Cuando se produce un conflicto de fusión, al ejecutar el comando ``git status`` se muestran los archivos en los que git encontro conflictos para mergear automaticamente. 

### Demostración práctica

1) **Branch**:
	- El comando ``git branch`` nos permite manejar la creacion, listado, renombreado y borrados de las ramas en git. Este comando se integra junto con ``git checkout/switch <nombre_branch_destino>`` para movernos entre ramas y con el comado  ``git merge/rebase <nombre_branch>`` para la fusion entre ramas.
	- Acontinuacion un ejemplo practico de creacion, listado, renombreado y eliminacion de una branch:  
	-  **Creacion**:
		  - Utilizamos el comando `git branch <nombre_de_la_nueva_rama>` para crear una rama llamada branch_a.
		    `git branch`
		  - Luego utilizando `git checkout branch_a` nos ubicamos en la rama recien creada.
		    
		```bash
		$ git checkout branch_a
		  Switched to branch 'branch_a'
		```
		  
		- Luego publicamos la nueva rama desde nuestro repositorio local al repositorio remoto, para esto podemos hacer ``git push`` de nuestros cambios o crearemos un nuevo commit e incorporar la nueva rama con los nuevos cambios a subir:
		  - Creamos un nuevo archivo llamado branch_a.txt:
		```bash
	    touch branch_a.txt
		```
		  - Luego verificamos el status:
		```bash
		$ git status
		On branch branch_a
		Untracked files:
		(use "git add <file>..." to include in what will be committed)
			branch_a.txt
		nothing added to commit but untracked files present (use "git add" to track)
		```
		- Agregamos los nuevos archivos a commitear:
		  `git add .`
		- Creamos el nuevo commit que publicara la nueva rama en el repositorio remoto:
		  ``` bash
			$ git commit -m "First branch_a commit"
			[branch_a 2b50160] First branch_a commit
			1 file changed, 0 insertions(+), 0 deletions(-)
			create mode 100644 branch_a.txt
			```

		- Luego al ejecutar push para subir nuestros cambios puede que devuelva el siguiente error, debido a que no se ha definido remotamente donde debe dirigirse la rama recien creada:
		  
		  ``` bash
		  $ git push
			`fatal: The current branch branch_a has no upstream branch.`
			`To push the current branch and set the remote as upstream, use`
				`git push --set-upstream origin branch_a`
			`To have this happen automatically for branches without a tracking`
			`upstream, see 'push.autoSetupRemote' in 'git help config'.`
			```
		- Si esto sucede ejecutamos el comando sugerido para setear un destino a nuestra nueva branch: `git push --set-upstream origin branch_a`
		- Si todo sale bien deberiamos ver un mensaje del estilo:
			``` bash
			Enumerating objects: 4, done.
			Counting objects: 100% (4/4), done.
			Delta compression using up to 8 threads
			Compressing objects: 100% (2/2), done.
			Writing objects: 100% (3/3), 286 bytes | 143.00 KiB/s, done.
			Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
			remote:
			remote: Create a pull request for 'branch_a' on GitHub by visiting:
			remote:      https://github.com/nombre_del_usuario/nombre_del_repo
			remote: To https://github.com/nombre_del_usuario/nombre_del_repo
			 * [new branch]      branch_a -> branch_a
			branch 'branch_a' set up to track 'origin/branch_a'.
			```
	-  **Listado**:
		 - El comando ``git branch`` tambien nos permite listar las ramas existentes en el proyecto, si no se agrega ninguna flag (bandera o argumento) este mostrara las ramas existentes a nivel local, si le colocamos la flag ``-l`` tambien obtendremos como resultado, y si quisieramos listar todas las branches tanto remotas como locales usariamos la flag ``-a``.
		- Ejemplos:
			- ``git branch o git branch -l``: Listado de las ramas locales.
			- ``git branch -a``: Listado de todas las ramas tanto locales como remotas.
	-  **Renombrado**:
		- La forma que tenemos por consola de renombrear una rama es utilizando la flag ``-m`` de este modo podemos cambiar el nombre de la rama en al que estamos actualmente. O indicar la rama a renombrar seguido del nuevo nombre.
		- Ejemplo: 
			- ``git branch -m <nuevo_nombre>``  
			- ``git branch -m <branch_actual> <nuevo_nombre>``
	-  **Eliminacion**:
		- Existen dos formas de eliminar branches la forma simple que de alguna manera es "segura" ya que previene que no elimines una rama que tiene cambios que aun no se han mergeado. O luego la forma forzada que elimina la rama sin importarle el punto anterior.
		- Ejemplo:
			- ``git branch -d``: Utilizando la flag ``-d`` se realiza un borrado simple de la rama.
				- En caso de error lanza algo similar a esto:
				  ```bash
				  	error: The branch 'nombre-branch' is not fully merged. If you are sure you want to delete it, run 'git branch -D crazy-experiment'.
					```
			- ``git branch -D``: Utilizando la flag ``-D`` se realiza un borrado forzado de la rama.

	Todos los comandos que modifiquen caracteristicas de la branch quedaran a nivel local hasta que se ejecute un commit y/o un push para impactar dichos cambios en la rama remota.

2) **Merge**:
	1) Supongamos que estuve trabajando en algunas nuevas funcionalidades en una rama llamada "branch_de_ejemplo" y ahora quiero integrar esos cambios mi rama main, para esto deberia realizar los siguientes pasos:
	2) Me aseguro que tanto mi rama actual como mi rama main tienen los ultimos cambios localmente, realizando un ``git pull`` en ambas ramas.
	3) Me aseguro que estoy posicionado en la rama que quiero que reciba los cambios en este caso mi rama destino seria ``main``.
	4) Una vez hecho esto ejecuto ``git merge branch_de_ejemplo``.
	5) De ser necesario resuelvo los conflictos que puedan ocurrir.
		- Una vez resueltos los conflictos ejecuto el comando ``git merge --continue`` en caso de que git haya quedado en estado ``MERGING``.
	6) Luego de continuar el mergeo automaticamente se crea un nuevo commit de fusion de ramas o en su defecto debera ejecutarce el comando ``git commit -m "...."``.
	7) Y finalmente subo el resultado de mi fusion a la rama remota con un ``git push``.

	Como alternatica al comando `git merge` tenemos el comando `git rebase` este funciona igual que el git merge. Se diferencia en el hecho que el `git merge` combina dos ramas creando un nuevo commit de merge que une las historias de ambas ramas, mientras que el rebase toma tus cambios locales y los aplica encima del commit más reciente de la rama base, en escencia reescribe la historia de commits.

3) **Resolución de conflictos**:
	- Ahora veremos un ejemplo de como pude suceder un conflicto al mergear dos branches que modificaron los mismos archivos.
	- Por un lado tenemos la branch_a y por otro las branch_b y ambas modificaran el archivo conflito.txt con cosas distintas.
	- Al querer mergear la rama branch_b dentro de branch_a esto producira un conflicto que se visualiza de la siguiente manera:
	  	```bash
	  	$ git merge branch_b
		Auto-merging conflicto.txt
		CONFLICT (add/add): Merge conflict in conflicto.txt
		Automatic merge failed; fix conflicts and then commit the result.
		```
	-  En el archivo conflicto nos encontraremos lo siguiente:
		  ``` bash
			<<<<<<< HEAD
				Este archivo puede tener conflico en la branch_a
			 =======
				Este archivo generara conflicto.
		     >>>>>>> branch_b
		```
		Como podemos apreciar dentro del archivo conflicto.txt nos encontramos tanto los cambios locales (Entre el `<<<<<<< HEAD` y la barra `=======`) como los remotos. En este caso vemos que los textos del achivo fueron modificados en ramas distintas, por lo que se debe corregir este conflitos.
	- Tenemos varias formas de corregir un conflito, podemos hacerlo manualmente ingresando al archivo y quedandonos con los cambios que decidamos mantener, luego agregamos los archivos modificados con el comando `git add`. Otra opcion es utilizar el comando `git mergetool` esto nos mostrara una interfaz que podemos configurar con la herramienta que prefiramos (algunos ejemplos son opendiff, kdiff3, tkdiff, xxdiff, meld, vimdiff, nvimdiff) para facilitar la vizualizacion del conflicto, y poder corregirlo. Y como opcion adicional existen extenciones o software dediaco a la resolucion de conflictos en git como son SourceTree, Gitkraken, etc.
	- Una vez resuelto el conflicto ejecutamos el comando `git merge --continue` para que git salga del estado de MERGING y cree el commit de fusion correspondiente.
	- Una vez completado el paso anterior podemos ejecutar el git push como se describio en el apartado anterior ( 2) Merge:)


### Recomendaciones de uso 

- A la hora de trabajar sobre una nueva funcionalidad o un nuevo desarrollo es recomendable crear una nueva branch que encapsule dichas modificaciones, esto permite que el desarrollo se mantenga independiente del codigo principal del proyecto minimizando una posible contaminacion de la rama principal con codigo que no se ha probado aun o que pueda contener algun bug.
- Al crear una nueva branch ya sea para un nuevo desarrollo o para la correcion de un incidente es recomendable que el ciclo de vida de esta branch no termine una vez finalizada la modificacion anterior. Ya que en un futuro si ocurre un nuevo bug o debe extenderse el desarrollo que tuvo inicio en dicha rama, es conveniente que la rama este actualizada y este actualizada con los cambios que se hayan realizado en la rama principal.
- Al la hora de realizar un `git merge` es altamente recomendable actualizar las ramas locales que estaran involucradas en la fusion. Ya que de no estar actualizadas podrian haber commits que queden por fuera del merge.
- Al realizar un `git merge` muchas veces nos veremos en la necesidad de resolver conflictos, esta tarea suele facilitarse ampliamente utilizando interfaces graficas en lugar de hacer las correciones manualmente desde la consola o directamente desde el IDE que estemos utilizando. Esto se debe a que dichas interfaces nos proveen una expriencia visual mucho mas amigable que permite saber exactamente que cambios estamos concervando o modificando.
- Tambien a la hora de resolver conflictos es muy importante mantener una buena comunicacion con el resto del equipo, esto nos permitira minimizar errores o perdidas de codigo. Ya que se puede dar el caso que a la hora de agregar mis cambios descarto el codigo hecho por otra miembro del equipo en pos de agregar mis cambios.

### Recursos de aprendizaje

- La documentacion oficial de git es una buena referencia para comprender los conceptos mencionados en esta ficha. [Documentación Oficial de Git](https://git-scm.com/docs)
- En la web [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows), se puede encontrar una explicacion un poco mas amigable que en la documentacion oficial de git.
- Para aprender sobre branching y git merge recomendamos el recurso: [learngitbranching](https://learngitbranching.js.org/?locale=es_AR), ya que tiene una interfaz grafica muy util para entender el funcionamiento de las branches, como moverse entre ellas y como se pueden mergear. Y cuenta con desafios para consolidar conocimientos.
