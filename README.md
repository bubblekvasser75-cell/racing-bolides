# Racing Bolides — мод на гоночные болиды (Fabric 1.21.10)

Мод добавляет управляемый гоночный болид (race car), на котором можно ездить.

## Что внутри

- **Сущность `racingbolides:bolide`** — болид, на который садятся правым кликом и едут на WASD.
  - Рулёжка идёт по направлению взгляда водителя, задний ход медленнее.
  - Повышенная скорость (множитель `SPEED_MULTIPLIER` в `BolideEntity`).
  - Шаг 1 блок (заезжает на бордюры), меньше урона от падения.
- **Предмет `racingbolides:bolide_spawner`** — ставит болид правым кликом по блоку. Лежит во вкладке «Инструменты» творческого режима.

## Сборка

Требуется **JDK 21** и интернет (Gradle скачает Minecraft, Yarn и Fabric API).

1. Создайте Gradle wrapper (в проекте нет бинарного `gradle-wrapper.jar`):
   ```bash
   gradle wrapper --gradle-version 8.10
   ```
   Либо просто откройте папку в **IntelliJ IDEA** — плагин Loom настроит всё сам.
2. Соберите:
   ```bash
   ./gradlew build
   ```
3. Готовый `.jar` будет в `build/libs/`. Киньте его в `mods/` вместе с **Fabric API** и **Fabric Loader**.

Запуск клиента для теста: `./gradlew runClient`.

## Важно про версии

В `gradle.properties` указаны примерные номера сборок. Перед сборкой сверьте их на https://fabricmc.net/develop/ для 1.21.10:
- `yarn_mappings` (например `1.21.10+build.X`)
- `loader_version`
- `fabric_version` (Fabric API под 1.21.10)

Код написан под API 1.21.10 (новая система рендер-стейтов сущностей и ключи регистрации). Если маппинги немного отличаются, IDE подскажет точные имена методов.

## Автосборка через GitHub Actions (готовый .jar без локальной сборки)

В проекте есть воркфлоу `.github/workflows/build.yml`. Он сам ставит JDK 21 и Gradle 8.10, собирает мод и выкладывает готовый `.jar` как artifact.

1. Создайте репозиторий на GitHub и залейте туда эту папку:
   ```bash
   git init && git add . && git commit -m "Racing Bolides mod"
   git branch -M main
   git remote add origin <ваш-репозиторий>.git
   git push -u origin main
   ```
2. На GitHub откройте вкладку **Actions** → сборка «Build mod» запустится автоматически (или жмите **Run workflow**).
3. После успешной сборки скачайте artifact **`racing-bolides-jar`** — внутри готовый `.jar`.

## Структура

```
src/main/java/com/bolides/racing/
  RacingBolides.java          # точка входа (сервер+общая)
  RacingBolidesClient.java    # клиентская точка входа (рендер)
  entity/BolideEntity.java    # логика и управление болидом
  entity/ModEntities.java     # регистрация сущности
  item/BolideItem.java        # предмет-спавнер
  item/ModItems.java          # регистрация предметов
  client/...                  # модель, рендерер, рендер-стейт, слои модели
src/main/resources/
  fabric.mod.json
  assets/racingbolides/...    # текстуры, модели, локализация, иконка
```
