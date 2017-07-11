npm 梳理, package, devDepences and dependce, cmd, umd, amd, commonjs

> package中 devDependcies and dependcies

    devDependcies 里面的包跟编译有关,比如各种loader， babel, css-loader之类的 （npm i package --save-dev）

    dependcies 里面的包是开发项目中依赖的, 比如 react

    本质上并没有太多的不同,只是规范,有些应用,类似于 electron 打包应用会剔除devdependcies里面的包

    