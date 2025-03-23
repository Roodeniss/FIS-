### Fuentes de información 

- [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [Documentación Oficial de Git](https://git-scm.com/docs)
- [Varonis](https://www.varonis.com/blog/git-branching "https://www.varonis.com/blog/git-branching")
- [Git - Merge - GeeksforGeeks](https://www.geeksforgeeks.org/git-merge/ "https://www.geeksforgeeks.org/git-merge/")
- [Git Tutorial](https://www.geeksforgeeks.org/git-tutorial/?ref=header_outind)
### Definiciones 

1) **Branch**:
   Las git branches son un punto de referencia a una instancia. Con instancia nos referimos a cualquier tipo de cambio en el código, ya sea un nuevo desarrollo dentro de un proyecto, la corrección de un bug, entre otras. Sin importar la magnitud de la modificación, esta puede y debería ser encapsulada en una branch. Esto aporta mucho valor a la hora de manejar código inestable, ya que no queremos que dicho código se integre en la rama principal sin antes completarse su ciclo de vida.
2) **Merge**:
	Git merge es un comando de git que se encarga de combinar múltiples commits, unificando las historias de dos branches. Los casos más frecuentes de uso del git merge son para combinar los cambios de dos branches distintas. Git merge busca un punto base o commit base desde el cual iniciar la fusión de las ramas y a partir de eso crea un nuevo commit de fusión que combinará ambas ramas.
3) **Resolución de conflictos**:
   Si las dos ramas que intentas fusionar cambiaron la misma parte del mismo archivo, Git no podrá determinar qué versión usar. En tal caso, se detiene justo antes de la confirmación de fusión para que puedas resolver los conflictos manualmente. Cuando se produce un conflicto de fusión, al ejecutar el comando ``git status`` se muestran los archivos en los que git encontró conflictos para mergear automáticamente.

### Demostración práctica

1) **Branch**:
	- El comando ``git branch`` nos permite manejar la creación, listado, renombrado y borrados de las ramas en git. Este comando se integra junto con ``git checkout/switch <nombre_branch_destino>`` para movernos entre ramas y con el comando ``git merge/rebase <nombre_branch>`` para la fusión entre ramas.
	  
	- A continuación un ejemplo práctico de creación, listado, renombrado y eliminación de una branch.
	  
	-  **Creacion**:
		  - Utilizamos el comando git branch <nombre_de_la_nueva_rama> para crear una rama llamada branch_a:  `git branch`
		    
		  - Luego utilizando git checkout branch_a nos ubicamos en la rama recién creada.
		  
		```bash
		$ git checkout branch_a
		  Switched to branch 'branch_a'
		```
		  
		- Luego publicamos la nueva rama desde nuestro repositorio local al repositorio remoto. Para esto podemos hacer un ``git push`` de nuestros cambios o crearemos un nuevo commit e incorporaremos la nueva rama con los nuevos cambios a subir.
		  
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
		- Agregamos los nuevos archivos a commitear: `git add .`
		  
		- Creamos el nuevo commit que publicará la nueva rama en el repositorio remoto:
		  
		  ``` bash
			$ git commit -m "First branch_a commit"
			[branch_a 2b50160] First branch_a commit
			1 file changed, 0 insertions(+), 0 deletions(-)
			create mode 100644 branch_a.txt
			```

		- Luego, al ejecutar ``git push`` para subir nuestros cambios, puede que la ejecución del comando devuelva el siguiente error, debido a que no se ha definido remotamente donde debe dirigirse la rama recién creada:
		  
		  ``` bash
		  $ git push
			`fatal: The current branch branch_a has no upstream branch.`
			`To push the current branch and set the remote as upstream, use`
				`git push --set-upstream origin branch_a`
			`To have this happen automatically for branches without a tracking`
			`upstream, see 'push.autoSetupRemote' in 'git help config'.`
			```
		- Si esto sucede, ejecutamos el comando sugerido para setear un destino a nuestra nueva branch: `git push --set-upstream origin branch_a`
		  
		- Si todo sale bien, deberíamos ver un mensaje del estilo:
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
		 - El comando ``git branch`` también nos permite listar las ramas existentes en el proyecto. Si no se agrega ninguna flag (bandera o argumento), este mostrará las ramas existentes a nivel local, si le colocamos la flag ``-l`` también obtendremos como resultado, y si quisiéramos listar todas las branches tanto remotas como locales, usaríamos la flag ``-a``.
		   
		- Ejemplos:
			- ``git branch o git branch -l``: Listado de las ramas locales.
			- ``git branch -a``: Listado de todas las ramas tanto locales como remotas.
			  
	-  **Renombrado**:
		- La forma que tenemos por consola de renombrar una rama es utilizando la flag ``-m`` de este modo podemos cambiar el nombre de la rama en la que estamos actualmente. O indicar la rama a renombrar seguido del nuevo nombre.
		- Ejemplo: 
			- ``git branch -m <nuevo_nombre>``  
			- ``git branch -m <branch_actual> <nuevo_nombre>``
	-  **Eliminacion**:
		- Existen dos formas de eliminar branches la forma simple, que de alguna manera es "segura", ya que previene que no elimines una rama que tiene cambios que aún no se han mergeado. O luego la forma forzada que elimina la rama sin importarle el punto anterior.
		  
		- Ejemplo:
			- ``git branch -d``: Utilizando la flag ``-d`` se realiza un borrado simple de la rama.
				- En caso de error lanza algo similar a esto:
				  ```bash
				  	error: The branch 'nombre-branch' is not fully merged. If you are sure you want to delete it, run 'git branch -D crazy-experiment'.
					```
			- ``git branch -D``: Utilizando la flag ``-D`` se realiza un borrado forzado de la rama.

	Todos los comandos que modifiquen características de la branch quedarán a nivel local hasta que se ejecute un commit y/o un push para impactar dichos cambios en la rama remota.
	
2) **Merge**:
	1) Supongamos que estuve trabajando en algunas nuevas funcionalidades en una rama llamada "branch_de_ejemplo" y ahora quiero integrar esos cambios a mi rama main, para esto debería realizar los siguientes pasos:
	   
	2) Me aseguro de que tanto mi rama actual como mi rama main tienen los últimos cambios localmente, realizando un ``git pull`` en ambas ramas.
	   
	3) Me aseguro que estoy posicionado en la rama que quiero que reciba los cambios en este caso mi rama destino seria ``main``.
	   
	4) Una vez hecho esto ejecuto ``git merge branch_de_ejemplo``.
	   
	5) De ser necesario, resuelvo los conflictos que puedan ocurrir.
		- Una vez resueltos los conflictos, ejecuto el comando ``git merge --continue`` en caso de que git haya quedado en estado ``MERGING``.
	6) Luego de continuar el mergeo automaticamente se crea un nuevo commit de fusion de ramas o en su defecto debera ejecutarce el comando ``git commit -m "...."``.
	   
	7) Y finalmente subo el resultado de mi fusión a la rama remota con un ``git push``.

	Como alternativa al comando, `git merge` tenemos el comando `git rebase` este funciona igual que el git merge. Se diferencia en el hecho de que el `git merge` combina dos ramas creando un nuevo commit de merge que une las historias de ambas ramas, mientras que el rebase toma tus cambios locales y los aplica encima del commit más reciente de la rama base, en esencia, reescribe la historia de commits.
	
3) **Resolución de conflictos**:
	- Ahora veremos un ejemplo de cómo puede suceder un conflicto al mergear dos branches que modificaron los mismos archivos.
	  
	- Por un lado tenemos la branch_a y por otro las branch_b y ambas modificarán el archivo conflito.txt con cosas distintas.
	  
	- Al querer mergear la rama branch_b dentro de branch_a esto producirá un conflicto que se visualiza de la siguiente manera:
	  
	  ```bash
	  	$ git merge branch_b
		Auto-merging conflicto.txt
		CONFLICT (add/add): Merge conflict in conflicto.txt
		Automatic merge failed; fix conflicts and then commit the result.
	```
	-  En el archivo conflicto nos encontraremos con lo siguiente:
		  ``` bash
			<<<<<<< HEAD
				Este archivo puede tener conflico en la branch_a
			 =======
				Este archivo generara conflicto.
		     >>>>>>> branch_b
		```
		Como podemos apreciar dentro del archivo conflicto.txt, nos encontramos tanto con los cambios locales (entre el `<<<<<<< HEAD` y la barra `=======`) como con los remotos. En este caso vemos que los textos del archivo fueron modificados en ramas distintas, por lo que se debe corregir este conflicto.
		
	- Tenemos varias formas de corregir un conflicto, podemos hacerlo manualmente ingresando al archivo y quedándonos con los cambios que decidamos mantener. Luego agregamos los archivos modificados con el comando `git add`. Otra opción es utilizar el comando `git mergetool` esto nos mostrará una interfaz que podemos configurar con la herramienta que prefiramos (algunos ejemplos son opendiff, kdiff3, tkdiff, xxdiff, meld, vimdiff, nvimdiff) para facilitar la visualización del conflicto y poder corregirlo. Y como opción adicional existen extensiones o software dedicado a la resolución de conflictos en git, como son SourceTree, GitKraken, etc.
	  
	- Una vez resuelto el conflicto, ejecutamos el comando `git merge --continue` para que git salga del estado de MERGING y cree el commit de fusión correspondiente.
	  
	- Una vez completado el paso anterior, podemos ejecutar el git push como se describió en el apartado anterior ( 2) Merge)


### Recomendaciones de uso 

- A la hora de trabajar sobre una nueva funcionalidad o un nuevo desarrollo, es recomendable crear una nueva branch que encapsule dichas modificaciones. Esto permite que el desarrollo se mantenga independiente del código principal del proyecto, minimizando una posible contaminación de la rama principal con código que no se ha probado aún o que pueda contener algún bug.
  
- Al crear una nueva branch ya sea para un nuevo desarrollo o para la corrección de un incidente, es recomendable que el ciclo de vida de esta branch no termine una vez finalizada la modificación anterior. Ya que en un futuro, si ocurre un nuevo bug o debe extenderse el desarrollo que tuvo inicio en dicha rama, es conveniente que la rama esté actualizada y esté actualizada con los cambios que se hayan realizado en la rama principal.
  
- A la hora de realizar una `git merge` fusión, es altamente recomendable actualizar las ramas locales que estarán involucradas en la fusión. Ya que de no estar actualizadas podrían haber commits que queden por fuera del merge.
  
- Al realizar un, `git merge` muchas veces nos veremos en la necesidad de resolver conflictos, esta tarea suele facilitarse ampliamente utilizando interfaces gráficas en lugar de hacer las correcciones manualmente desde la consola o directamente desde el IDE que estemos utilizando. Esto se debe a que dichas interfaces nos proveen una experiencia visual mucho más amigable que permite saber exactamente qué cambios estamos conservando o modificando.

- También a la hora de resolver conflictos es muy importante mantener una buena comunicación con el resto del equipo, esto nos permitirá minimizar errores o pérdidas de código. Ya que se puede dar el caso de que a la hora de agregar mis cambios descarto el código hecho por otro miembro del equipo en pos de agregar mis cambios.

### Recursos de aprendizaje

- La documentación oficial de git es una buena referencia para comprender los conceptos mencionados en esta ficha. [Documentación Oficial de Git](https://git-scm.com/docs)

- En la web [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows), se puede encontrar una explicación un poco más amigable que en la documentación oficial de git.

- Para aprender sobre branching y git merge, recomendamos el recurso: [learngitbranching](https://learngitbranching.js.org/?locale=es_AR), ya que tiene una interfaz gráfica muy útil para entender el funcionamiento de las branches, como moverse entre ellas y cómo se pueden mergear. Y cuenta con desafíos para consolidar conocimientos.
