# TestGitTools

https://github.com/netology-code/sysadm-homeworks/blob/devsys10/02-git-04-tools/README.md

# Задание

В клонированном репозитории https://github.com/hashicorp/terraform:

1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.
2. Ответьте на вопросы.

* Какому тегу соответствует коммит `85024d3`?
* Сколько родителей у коммита `b8d720`? Напишите их хеши.
* Перечислите хеши и комментарии всех коммитов, которые были сделаны между тегами  v0.12.23 и v0.12.24.
* Найдите коммит, в котором была создана функция `func providerSource`, её определение в коде выглядит так: `func providerSource(...)` (вместо троеточия перечислены аргументы).
* Найдите все коммиты, в которых была изменена функция `globalPluginDirs`.
* Кто автор функции `synchronizedWriters`? 

*В качестве решения ответьте на вопросы и опишите, как были получены эти ответы.*

# Решение

## 1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.

```
$ git show aefea
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400

    Update CHANGELOG.md

diff --git a/CHANGELOG.md b/CHANGELOG.md
index 86d70e3e0d..588d807b17 100644
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@ -27,6 +27,7 @@ BUG FIXES:
 * backend/s3: Prefer AWS shared configuration over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))
 * backend/s3: Prefer ECS credentials over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))
 * backend/s3: Remove hardcoded AWS Provider messaging ([#25134](https://github.com/hashicorp/terraform/issues/25134))
+* command: Fix bug with global `-v`/`-version`/`--version` flags introduced in 0.13.0beta2 [GH-25277]
 * command/0.13upgrade: Fix `0.13upgrade` usage help text to include options ([#25127](https://github.com/hashicorp/terraform/issues/25127))
 * command/0.13upgrade: Do not add source for builtin provider ([#25215](https://github.com/hashicorp/terraform/issues/25215))
 * command/apply: Fix bug which caused Terraform to silently exit on Windows when using absolute plan path ([#25233](https://github.com/hashicorp/terraform/issues/25233))
```


## 2. Ответьте на вопросы.

### Какому тегу соответствует коммит `85024d3`?
```
$ git show 85024d3 --no-patch
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Thu Mar 5 20:56:10 2020 +0000

    v0.12.23

# тегу v.0.12.23
```

### Сколько родителей у коммита `b8d720`? Напишите их хеши.

```
$ git log -1 --pretty=format:%P b8d720
56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b

# 2 родителя с указанными хешами
```

### Перечислите хеши и комментарии всех коммитов, которые были сделаны между тегами  v0.12.23 и v0.12.24.

```
$ git log v0.12.23..v0.12.24 --oneline
33ff1c03bb (tag: v0.12.24) v0.12.24
b14b74c493 [Website] vmc provider links
3f235065b9 Update CHANGELOG.md
6ae64e247b registry: Fix panic when server is unreachable
5c619ca1ba website: Remove links to the getting started guide's old location
06275647e2 Update CHANGELOG.md
d5f9411f51 command: Fix bug when using terraform login on Windows
4b6d06cc5d Update CHANGELOG.md
dd01a35078 Update CHANGELOG.md
225466bc3e Cleanup after v0.12.23 release
```

### Найдите коммит, в котором была создана функция `func providerSource`, её определение в коде выглядит так: `func providerSource(...)` (вместо троеточия перечислены аргументы).

```
$ git log -S'func providerSource(' --oneline
8c928e8358 main: Consult local directories as potential mirrors of providers

$ git show 8c928e835
...
+// command, unless overridden by the special -plugin-dir option.
+func providerSource(services *disco.Disco) getproviders.Source {
...
```
### Найдите все коммиты, в которых была изменена функция `globalPluginDirs`.

```
# ищем, где функция находится
$ git grep -i  "func globalPluginDirs"
internal/command/cliconfig/plugins.go:func GlobalPluginDirs() []string {


# ищем изменения, связанные с функцией внутри указанного файла
$ git log -L:GlobalPluginDirs:internal/command/cliconfig/plugins.go --oneline -q
7c4aeac5f3 stacks: load credentials from config file on startup (#35952)
78b1220558 Remove config.go and update things using its aliases
52dbf94834 keep .terraform.d/plugins for discovery
41ab0aef7a Add missing OS_ARCH dir to global plugin paths
66ebff90cd move some more plugin search path logic to command
8364383c35 Push plugin discovery down into command package
```

### Кто автор функции `synchronizedWriters`? 

```
$ git log -S 'func synchronizedWriters' --pretty=format:"%h %an"
bdfea50cc8 James Bardin
5ac311e2a9 Martin Atkins

Анализ коммитом показывает, что автор - Martin Atkins
```