#Gantry
### Copying Particles to existing theme

This tutorial shows you the safe way to copy [Gantry](https://gantry.org/) particles to your themes that won’t be overwritten again by theme updates. 

A particle is made up of these things:
* A `Yaml` file that tells Gantry 5 what fields the particle uses
* A `twig` file that uses those fields to render output on your page
* A `scss` file that contains the **SASS/CSS** to style the rendered particle on your page
* One or more `css/js` files that contain **JavaScript/CSS** code to make the particle work.

Unzip the `particles.zip` package, navigate inside `/particles/` folder then choose the particle or atom you wish to implement for your existing site then follow the steps below:

* Copy `particle_name.html.twig` and `particle_name.yaml` to `THEMENAME/custom/particles`
* Copy all js files (if any) to `THEMENAME/custom/js` and all css files (if any) to `THEMENAME/custom/css`
* Copy `_jlparticles.scss` to `THEMENAME/custom/scss`.

**Note**: The `_jlparticles.scss` is global scss file for [JoomLead](https://joomlead.com/gantry-5-particles/) particles.

The next thing you need to do is to ensure that the SCSS for the particle is loaded too. To do this, navigate in the directory structure to `THEME_DIR/custom/scss` and create a file called `custom.scss` if one doesn’t already exist.

If the `/scss/` directory doesn’t exist within your custom folder, you will need to create that, too. If it already does, just open it and make your additions/changes directly to the file.

Open `custom.scss` file then add

```scss 
@import "dependencies";
@import "jlparticles";
```

Go to the base outline Styles tab and Hit Recompile CSS button in the upper right corner. 

**Important**
Some particles using their custom scss files for custom styling, if you use one of these particles, make sure to import the particle’s scss file name. Otherwise, it has no visual effect.

**Important**
Our Gantry5 Particles/Atoms are built on the Uikit 3 front-end framework, to get started right away, you need to enable the Uikit 3 for Gantry 5 in your Gantry 5 Atom setting. 

#### Enable Uikit 3 Gantry5 Atom

After install the theme/template or copying the particles to existing theme, you need to check and enable the Uikit3 for Gantry 5 atom FIRST.
Go to **Extensions** → **Template Manager**, select the **Base Outline** → tab **Page Settings**, and scroll down to **Atom** section in the bottom then enable it.