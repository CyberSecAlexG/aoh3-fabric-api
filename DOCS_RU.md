[🇺🇸 English](https://github.com/MushroomMif/aoh3-fabric-api/blob/master/DOCS.md)
-----
# Загрузчик ресурсов
Вы можете добавлять файлы в папки `audio`, `game`, `gfx`, `map` и `ui`
в ресурсах вашего мода. Они будут загружены загрузчиком ресурсов и будут
доступны в `FileManager`. Если у какого-нибудь файла в ресурсах точно такой же
путь, как и у какого-нибудь файла игры, то файл игры будет переопределён файлом
из ресурсов. Также, вы можете создавать `.properties` файлы (посмотрите в 
папку `game/languages` в директории игры, чтобы увидеть доступные для них имена)
в папке `languages`, они тоже будут загружены и переданы в `LanguageManager`.

# Система событий
AOH3 Fabric Api предоставляет возможность слушать различные события.
Помимо этого, вы также можете создать и свои события используя это систему.
Вот список доступных на данный момент событий:
- `GameLoadingEvents.ON_LOADED`
- `FileLoadingEvents.IMAGES`
- `FileLoadingEvents.END`
- `ResourceLoaderEvents.SHOULD_LOAD_MOD_RESOURCES`
- `ResourceLoaderEvents.AFTER_RESOURCES_STORE`

Вы можете слушать события вот так:
```java
GameLoadingEvents.ON_LOADED.register(() -> {
    System.out.println("The game was fully loaded!");
});
```
И можете создать своё с помощью метода `FabricEvent.create`

# Система модификации файлов
Используя эту систему вы можете легко изменить файлы игры (и файлы от модов)
без их перезаписывания. Это означает, что несколько модов могут делать изменения в
одном и том же файле. Вот пример использования этой системы
(всегда вызывайте методы из`FileModifier` в методе `onInitialize` вашего мода,
иначе может быть слишком поздно):
```java
FileModifier.INSTANCE
        .modFile("game/gameValues/GV_Battle.json", (file) -> {
            file.selectFirst("DRAW_BATTLE_NOT_IN_VIEW: true", (setting) -> {
                setting.replaceFirst("true", "false");
            });
        });
```

# Система регистрации
Эта система позволяет вам легко сказать игре, чтобы она загрузила ваши
кастомные карты, сценарии и цивилизации. Для этого вы можете использовать
методы `MapRegistry.addMap`,`MapRegistry.addScenario` и 
`CivilizationRegistry.addCiv`
