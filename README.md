## Configurar Lint & Talisman Hook

La intención de este repo es crear una receta para añadir _hooks_ a git. 

**Hooks instalados:**
- [Lint-staged](https://github.com/okonet/lint-staged): Verifica el estilo del código 
- [Talisman](https://github.com/thoughtworks/talisman): Verifica que no se suba información sensible

**Herramientas:**
- [Husky](https://github.com/typicode/husky/blob/master/DOCS.md): Herramienta para correr un asociar un script a un hook de git


### Pasos

1. Crear una nueva carpeta e iniciar git y npm:
 
    ```npm init```
     
    ```git init``` 
    
2. Crear el archivo .gitignore

3. Instalar eslint (vamos a usar la guía de estilo de airbnb)

    3.1 Añadir los paquete con el siguiente comando 
    
    ```npm i eslint eslint-config-airbnb-base eslint-plugin-import --save-dev```
    
    3.2 Configurar lint para que use la guía de estilo de airbnb 
    
    ```echo 'module.exports = { "extends": "airbnb-base" };' > .eslintrc.js```
    
    3.3 Añadir script a package.json
    
    ```"lint": "./node_modules/.bin/eslint ./src --ext .js --ext .jsx --cache"```
    
4. Instalar Husky 🌵
    
    >Si ya tienes instalado talisman instalado primero borramos el hook `rm .git/hooks/pre-commit` antes de instalar Husky
    
    ```npm i husky --save-dev```
    
    Verificar los hooks instalados por husky
    
    ```ls -a .git/hooks```
    
5. Instalar lint-staged

     ```npm i lint-staged --save-dev```
     
     crear el archivo lint-staged.config.js y añadir la siguiente configuración
     
     ```
     module.exports = {
       linters: {
         '*.js': ['eslint'],
       },
     };
     ```

6. Instalar Talisman.

    6.1 Instalar Talisman globalmente
    
    ```curl --silent  https://raw.githubusercontent.com/thoughtworks/talisman/master/global_install_scripts/install.bash > /tmp/install_talisman.bash && /bin/bash /tmp/install_talisman.bash```
 
    6.2 Especificar TALISMAN_HOME (bashrc o profile)
    
    6.3 Especificar "Git template directory" (se recomienda dejarlo por defecto)
    
    6.4 Definir directorio de los repositorios de GIT que queremos escanear con **Talisman**  (Talisman va añadir el script de pre-commit a todos los repositorios del directorio especificado)
    
    6.5 Hacer un source del archivo donde se añadio la variable (bashrc o profile)
    
7. Configurar husky. En `package.json` añadir los siguiente: 

    ```
    "husky": {
       "hooks": {
         "pre-commit": "$TALISMAN_HOME/talisman_hook_script pre-commit && lint-staged"
       }
    }
    ```
    
8. Añadir excepciones en .talismanrc en caso de ser necesario

    ```
    fileignoreconfig:
    - filename: .env
      checksum: checksum-number
      ignore_detectors: [filename]
    
    - filename: file.js
      checksum: checksum-number
      ignore_detectors: [filecontent]
    ```

    
     

