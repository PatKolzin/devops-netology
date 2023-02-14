1. Команда git show aefea показывает нам полный хэш и комментарий коммита

 ![image](https://user-images.githubusercontent.com/75835363/218839139-b1614f3f-35c4-426b-a355-48ca24e559c7.png)

2. Команда git show 85024d3 выводит нам хэш и тэг коммита


![image](https://user-images.githubusercontent.com/75835363/218839174-645ad4a0-6a38-435c-8c50-850c3d5f2f9d.png)

3. Команда git show b8d720 в строке merge показывает нам родителей коммита

 ![image](https://user-images.githubusercontent.com/75835363/218839184-6caae3d9-aa2f-431a-9506-391e57066a25.png)


4. Полные хэши и комменты задаем следующей коммандой
git log --pretty=format:"%H %s" v0.12.23..v0.12.24

33ff1c03bb960b332be3af2e333462dde88b279e v0.12.24
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md
225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release


5. При помощи команды git log с флагом -S находим искомые комиты. Первый из них является искомым.
git log -S'func providerSource'

 ![image](https://user-images.githubusercontent.com/75835363/218839242-2d986088-ca75-44b1-8ca5-338d737639ef.png)


6. Сначала находим файлы в которых эта функция есть, командой
7. 
git grep -p "globalPluginDirs"

commands.go=func initCommands(
commands.go:            GlobalPluginDirs: globalPluginDirs(),
commands.go=func credentialsSource(config *cliconfig.Config) (auth.CredentialsSource, error) {
commands.go:    helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
internal/command/cliconfig/config_unix.go=func homeDir() (string, error) {
internal/command/cliconfig/config_unix.go:              // FIXME: homeDir gets called from globalPluginDirs during init, before
plugins.go=import (
plugins.go:// globalPluginDirs returns directories that should be searched for
plugins.go:func globalPluginDirs() []string {


Далее перебором этих файлов определяем где она изменялась

xxeli@olZion MINGW64 /q/terraform (main)
$ git log --oneline -L :globalPluginDirs:commands.go
fatal: -L parameter 'globalPluginDirs' starting at line 1: no match

xxeli@olZion MINGW64 /q/terraform (main)
$ git log --oneline -L :globalPluginDirs:config_unix.go
fatal: There is no path config_unix.go in the commit

xxeli@olZion MINGW64 /q/terraform (main)
$ git log --oneline -L :globalPluginDirs:internal/command/cliconfig/config_unix.go
fatal: -L parameter 'globalPluginDirs' starting at line 1: no match

xxeli@olZion MINGW64 /q/terraform (main)
$ git log --oneline -L :globalPluginDirs:plugins.go
78b1220558 Remove config.go and update things using its aliases
52dbf94834 keep .terraform.d/plugins for discovery
41ab0aef7a Add missing OS_ARCH dir to global plugin paths
66ebff90cd move some more plugin search path logic to command
8364383c35 Push plugin discovery down into command package


7. Ищем автора с помощью команды, задавая параметры отображения хэша,имени, даты и комментария

git log -S"func synchronizedWriters(" --pretty=format:’%h %an %ad %s’

bdfea50cc8 James Bardin Mon Nov 30 18:02:04 2020 -0500
5ac311e2a9 Martin Atkins Wed May 3 16:25:41 2017 -0700 - получается автор Martin Atkins 

