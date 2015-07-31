#Making a windows installer
starting with a basic app  
`warning: keep the app exe name short or windows will complain`
alter `main.coffee` by adding the following
```coffeescript
path = require 'path'

handleStartupEvent = ->
  if process.platform isnt 'win32' then return false
  squirrelCommand = process.argv[1]
  switch squirrelCommand
    when '--squirrel-install', '--squirrel-updated'
      target = path.basename process.execPath
      updateDotExe = path.resolve path.dirname(process.execPath), '..', 'update.exe'
      createShortcut = updateDotExe + ' --createShortcut=' + target + ' --shortcut-locations=Desktop,StartMenu'
      exec createShortcut;
      app.quit()
      true
    when '--squirrel-uninstall'
      target = path.basename process.execPath
      updateDotExe = path.resolve path.dirname(process.execPath), '..', 'update.exe'
      createShortcut = updateDotExe + ' --removeShortcut=' + target
      app.quit()
      true
    when '--squirrel-obsolete'
      app.quit()
      true
    else
      false
      
if handleStartupEvent()
  return
```
[package](https://github.com/ndxbxrme/useful-things/blob/newmain/guides/electron/packaging-your-app.md) your app
```bash
electron-packager ./ getting-started --platform=all --arch=all --version=0.30.1 --out=release
```
make `Gruntfile.coffee`
```coffeescript
module.exports = (grunt) ->
  require('load-grunt-tasks') grunt
  grunt.initConfig
    'create-windows-installer':
      x64:
        appDirectory: 'release/appName-win32-x64'
        outputDirectory: 'release/installer64'
        authors: 'ndxbxrme'
        exe: 'appName.exe'
        description: 'my first installer app'
```
make sure you have [Grunt](http://gruntjs.com/) installed
```bash
npm install -g grunt grunt-cli
```
install [load-grunt-tasks](https://github.com/sindresorhus/load-grunt-tasks) and [grunt-electron-installer](https://www.npmjs.com/package/grunt-electron-installer)
```bash
npm install --save-dev load-grunt-tasks grunt-electron-installer
```
now grunt it and you're done
```bash
grunt create-windows-installer
```
you will now have a folder called `release/installer64` with your installer in it  
ace
