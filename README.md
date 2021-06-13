# Tables exercise Vue.js

Mini game built in vue for personal use.

[Github repo](https://github.com/vis97c/tablas-ejercicio) - [Live preview](https://tablas.surge.sh/)

To developt the project quickly i used a modified version of the custom css library that i was developing for a personal project.

## development

Install the node dependencies.

```bash
    yarn
    #Or
    npm install
```

Then build the project, thats all.

```bash
    yarn dev
    #Or
    npm run dev
```

While "**watch**" mode is available, i recommend to run the dev command at least once to copy the static assets to the "**public**" directory

Also while developing you can optionally provide the enviroment variable "WATCH_BROWSER" to open a different browser than the system default

The source files will be available at "**src**" directory

## deployment

This project is hosted on surge

```bash
    yarn deploy
    #Or
    npm run deploy
```

Optionally you can run the "**build**" command to generate the required assets to deploy in other enviroment
