npm 梳理, package, devDepences and dependce, cmd, umd, amd, commonjs

> package中 devDependcies and dependcies

    devDependcies 里面的包跟编译有关,比如各种loader， babel, css-loader之类的 （npm i package --save-dev）

    dependcies 里面的包是开发项目中依赖的, 比如 react

    本质上并没有太多的不同,只是规范,有些应用,类似于 electron 打包应用会剔除devdependcies里面的包

概念
.npmrc The npm config files
npm gets its config settings from the command line, environment variables, and npmrc files.

npm 5.0注意问题
1.npm publish 命令 
        修改完package.json version字段,
        使用 npm adduser && npm publish --update-readme 就可以强推到npm registry
        npm publish --registry http://registry.npmjs.org 指定操作的源

2.npm update命令
>5.0以后会生成package-lock.json 会使npm update命令 失效,并不会强更新
>解决方案  rm -f package-lock.json && npm install
        Put "preinstall": "npm config set package-lock false" in your scripts to disable package-lock.json.这样以后使用
        npm update就可以生效了
3.npm cache clean --force 清空npm缓存


