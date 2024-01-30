# Importation automatiques des éléments 
## `components/`: __Vue components__ importé automatiquement dans des templates
* automatiquement importé,
    * [condition](https://nuxt.com/docs/guide/directory-structure/components) par défaut:
        * Cas 1, si `components/MyComponent.vue`, import de MyComponent. 
        * Cas 2, Si `components/ComponentGroup/MyComponent` Component importé sera ComponentGroupMyComponent
        * Cas 3, Si deux ou plusieurs composants ont le résultat de nom de fichier(depends du nom de dossier et de fichier) alors génèrera une erreur, exemple:
            * `components/Input/Field/Text.vue` donne `InputFieldText`
            * `components/InputField/Text.vue` donne `InputFieldText` aussi
    * Tout ca peut est configurable dans nuxt.config.ts
        ```ts
        export default defineNuxtConfig({
        components: [
                {
                    path: '~/components',
                    pathPrefix: false,//et donc pour le cas 2, MyComponent sera utilisé
                },
            ],
        });

        ```
* composant disponible globalement
    ```ts nuxt.config.ts
    //nuxt.config.ts
    export default defineNuxtConfig({
        components: {
            global: true,
            dirs: ['~/components/global']//par exemple les composants disponibles globalement se placera dans components/global
        },
    })
    ```
* [importation automatique des composants situés dans npm package](https://nuxt.com/docs/guide/directory-structure/components#npm-packages)
* `SomeDirectory/ComponentName.client.vue`: specifié les composants à rendre côté client, il suffit juste d'ajouter `*.client.vue`
    > Ceci n'est appliqué qu'à l'auto-importation et [`#components`](https://nuxt.com/docs/guide/directory-structure/components#dynamic-components)


## `composables/`: 
* __Vue composables__ importé automatiquement dans des templates et fichier JS/JS
* Par défault,seuls les fichiers au niveau supérieurs seront analysés par Nuxt et donc pour utiliser les fichiers qui y sont imbriqués, il faut les réexporté:
   ```ts composables/utils/show.ts
   //composables/utils/show.ts
   export const show = () => {
        alert('show')
    }
   ```
   et 
   ```ts composables/index.ts
   //composables/index.ts
   //il suffit juste de l'importer
   import { show } from "./utils/show";
   export {
        show
    }
   ```
* Préciser les importations automatiques avec nuxt.config.ts
    ```ts nuxt.config.ts
    export default defineNuxtConfig({
        imports: {
            dirs: [
                // Default, scan top-level modules
                'composables',
                // ... or scan modules nested one level deep with a specific name and file extension
                'composables/*/index.{ts,js,mjs,mts}',
                // ... or scan all modules within given directory
                'composables/**'
            ]
        }
    })

    ```
## `utils/`: 
* les __variables/fonctions JS/TS__ importé automatiquement dans des templates et fichier JS/TS


# layouts
* `layouts/`: repertoire pour les layouts
* deux methode permet de definir le layout utilisé pour une page:
    * `definePageMeta({...layout:String})`: permet de definir le layout utilisé pour la page
    * `<NuxtLayout>...<NuxtLayout/>` : permet d'indiquer à nuxt l'utilisation d'un tel layout pour un composant.
        * props:
            * `name`: string, 
                * permet de specifier le layout à utiliser, par défaut c'est `default`(`layouts/default.vue`)
                * valeur normalisé en kebak-case(ex: MyLayout.vue sera name="my-layout")
            * accepte d'autre props personnalisé supplémentaire qui sera disponible via `$attrs.someProp` ou `useAttrs().someProp`
    
* `<NuxtPage/>`: est utilisé pour iunjecter la page courant
* `<slot/>`: est utilisé pour injecter du contenu dans layout
    * `<slot name="some-layout"/>`: si on voudrais injecter du contenu autre que celui par défaut, pourrait le nommer, ainsi pour l'utiliser: `<template #some-layout>...<template/>`