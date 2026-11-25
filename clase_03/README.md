## Clase N° 3 - GitHub, Codespaces y control de versiones

#### 🎯 Objetivos:
Conectar el repositorio local con GitHub. Importancia del README.

#### 📂 Contenido de esta Carpeta
| Nombre del archivo | Descripción |
|---|---|
| `README.md` | Pasos para conectar repositorio local con remoto |

#### ⚙️ Requisitos Previos
- Tener inicializado Git en el repositorio local y haberse autenticado por única vez.

#### 📋 Desarrollo

1. Generar una cuenta en GitHub.

2. Crear el primer repositorio con el botón **New** y brindar nombre y caracter de público o privado. 

**ATENCIÓN!** Si ya se generó un archivo README.md en el repositorio local, al crear un repositorio remoto no tildar la opción *generar un archivo README.md*

**NOTA**: crear un archivo README.md es opcional tecnicamente, pero es lo primero que se ve al entrar en cualquier repositorio.

3. Una vez creado el repositorio, con el botón **Code** -> HTTPS copiar la URL para utilizarla en el futuro.

4. Se genera un Codespace: un entorno de desarrollo brindado por GitHub que corre en Linux. Ideal para cuando no se dispone de máquina o no se desea instalar programas en ella. Gratuito, pero de uso limitado. Se conecta automaticamente al repositorio en GitHub al inicializarlo:

**Terminal CODESPACE**
```bash
$ git remote -v
origin  https://github.com/usuario/repo.git (fetch)
origin  https://github.com/usuario/repo.git (push)
```

5. Conectar el repositorio local (del dispositivo) con el repositorio remoto (en cuenta de GitHub):

```bash
git remote -v
git remote add origin https://github.com/usuario/repo.git # enlace al repositorio remoto en GitHub
git remote -v
git push -u origin main
```
*git push origin main* envía todos los archivos que se guardan en el **staging area** con *git add* y luego de guardar el punto de control con *git commit*.

Se establece la diferencia entre la rama "*main*" (local) y la rama "*origin*" (remoto).

6. Comandos básicos para manipular ramas en Git:

```bash
git branch              # Lista las ramas, marca con * la activa
#   main
# * feature-login
git checkout main       # (o: git switch main) Cambiar entre ramas
git branch -m nombre_viejo nombre_nuevo     # Renombrar rama
```

