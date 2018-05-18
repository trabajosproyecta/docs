# Como hacer un tutorial

Ya sabemos que en esta página (y repositorio) colocaremos toda la información de problemas que nos ha tocado resolver, y cual fue el modo de realizarlo. Es por esto que es necesario saber como agregar contenido y compartir el conocimiento con el equipo.

La idea es que todo lo que publiquemos sea muy entendible por alguien que no sabe del tema, por lo que al escribir siempre piensen y escriban **TODO** lo que deberían de saber para llegar a una correcta solución.

Vamos por pasos:

1. **Crear Branch**
  Primero debemos ir al directorio donde tenemos descargado este repositorio. Luego de comprobar que estamos en la última versión, creamos una nueva rama de trabajo (*Branch*), con el nombre del tutorial/documentación que haremos

  ```bash
  cd docs
  git pull origin master
  git checkout -b nombre-tutorial
  ```

2. **Crear Archivo**
  Como metodo para ordenar, a cada tutorial le escogeremos una categoría. Estas pueden ser: DevOps, Backend, Frontend, Mobile, *inserteotracategoria*.
  Sabiendo cual es la categoría, procedemos a entrar a la carpeta relacionada a la categoría, y crear el archivo con nombre explicativo.
  Por ejemplo, si la documenación sobre algo relacionado a frontend haremos:

  ```bash
  cd frontend
  touch tutorial.md
  ```

3. **Escribir**
  Ahora procedemos a escribir el tutorial. Es muy importante considerar que debe aparecer todo lo importante para lograr el objetivo, vale decir, que nadie tenga que buscar otro tutorial para poder hacerlo.
  Idealmente harémos un *"paso a paso"* para llegar a una solución. En caso de que haya algun paso muy extenso o complejo o simplemente que se escapa del foco, podemos referirnos a algun link donde explique a cavalidad el contenido.
  Como se ve en el nombre del archivo en el paso anterior, usaremos *MarkDown* para esta documentación. Para ver más información sobre la syntax, recomiendo este [link](https://www.markdownguide.org/cheat-sheet).
  Además, siempre podemos ayudarnos de algún editor. Puede ser con alguna aplicación como [VisualStudio](https://code.visualstudio.com/) con complementos, o algun editor online como [Dillinger](https://dillinger.io/) o [StackEdit](https://stackedit.io/app), el que sea de nuestro mayor agrado resultara útil.
  > No olvides poner codigo importante, comandos de terminal o fotos para ayudar a que todo sea mucho más facil!

4. **Agregar tutorial al indice**
  Es muy importante que el contenido sea accesible, por lo que agregaremos el link a nuestro indice.
  Para esto, lo unico que debes hacer, es editar el archivo ```index.md``` ubicado en la carpeta raiz de este repositorio, y agregar un nuevo item en la categoria correspondiente, de la forma:

  ```markown
  [Titulo](http://docs.trabajosproyecta.cl/categoria/nombre-archivo.html)
  ```

  Cabe destacar que tu archivo tiene de nombre *nombre-archivo.md* y en el link pones *nombre-archivo.html*. Esta es una de las magias de [Jekyll](https://jekyllrb.com/docs/themes/)

5. **Pull Request**
  Una vez completo el tutorial necesitamos juntarlo con el resto.
  Para mantener orden, trabajaremos con Pull Requests. Estos, basicamente son solicitudes de *merge* de las ramas de trabajo. Con estas buscaremos que alguien más del equipo vea el contenido y sugiera cambios antes de publicarlo.
  Si no sabes como hacerlos, te recomiendo seguir este [tutorial](https://yangsu.github.io/pull-request-tutorial/).
  Resumen del tutorial:

  ```bash
  git add .
  git commit -m "Mensaje descriptivo del tutorial realizado"
  git push origin nombre-tutorial

  /* En caso de que tengas instalado hub */
  hub pull-request
  ```

  Luego, si no tienes [hub](https://github.com/github/hub), ir a github a abrir el pullrequest.
  Ahora solo debes esperar a que alguien lo revise y lo acepte para tener tu tutorial publicado.

Espero haya quedado todo claro! A comenzar haciendo esos tutoriales :muscle: :muscle:
