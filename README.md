# Variable Launcher - API Documentation

**Здесь описаны все функции, классы с их методами и хуки которые предоставляет API лаунчера Variable.**</br>
*Примечание: Я знаю что это трудно назвать документацией. Буду приводить эту хуйню в порядок до тех пор, пока сам не буду доволен. (лень чето, как нить потом (наверное, я бы победил в конкурсе за звание самого ленивого человека в мире))*

## Материал
- [Глобальные функции](#глобальные-функции)
- [Data](#data)
- [Module](#module)
- [ModuleManager](#modulemanager)
- [Setting](#setting)
- [ButtonSetting](#buttonsetting)
- [ModeSetting](#modesetting)
- [SliderSetting](#slidersetting)
- [StateSetting](#statesetting)
- [TextFieldSetting](#textfieldsetting)
- [Level](#level)
- [Block](#block)
- [LocalPlayer](#localplayer)
- [Player](#player)
- [Inventory](#inventory)
- [Item](#item)
- [Memory](#memory)
- [Константые классы](#константные-классы)
  - [ModuleCategory](#modulecategory)
  - [BlockSide](#blockside)
  - [MoveButton](#movebutton)
  - [PacketType](#packettype)
  - [Enchantment](#enchantment)
  - [ParticleType](#particletype)
- [Hooks](#hooks)

## Глобальные функции
- `getContext();` - возвращает контекст приложения.
- `print(String text);` - выводит контекстное сообщение.
- `preventDefault();` - в зависимости от места вызова, отменяет вызов [хука](#hooks).
- `getScreenName();` - возвращает название игрового экрана.
- `getFriends();` - возвращает массив `String[]` с никами друзей из `FriendManager`.
- `isFriend(String name);` - возвращает `true` если игрок с ником `name` находится в `FriendManager` и `false` если нет.
- `addFriend(String name);` - добавляет игрока с ником `name` в `FriendManager`.
- `removeFriend(String name);` - удаляет игрока с ником `name` из `FriendManager`.

## Data
*Класс для сохранения любых данных скрипта в корневой папке приложения*<br/>
**Статические методы**

- `Data.getString(String key, String defaultValue);` - ищет строку с ключом `key` и возвращает ее. Возвращает `defaultValue` если не находит ее.
- `Data.getNumber(String key, double defaultValue);` - ищет число с ключом `key` и возвращает его. Возвращает `defaultValue` если не находит его.
- `Data.getBoolean(String key, boolean defaultValue);` - ищет логическое значение (true/false) с ключом `key` и возвращает его. Возвращает `defaultValue` если не находит его.

- `Data.saveString(String key, String value);` - сохраняет строку с ключом `key` и значением `value`.
- `Data.saveNumber(String key, double value);` - сохраняет число с ключом `key` и значением `value`.
- `Data.saveBoolean(String key, boolean value);` - сохраняет логическое значение (true/false) с ключом `key` и значением `value`.

- `Data.remove(String key);` - удаляет значение с ключом `key`

## Module
*Класс для создания кастомных модулей (функции)*<br/>
**Конструктор: `Module(String name, boolean toggleable, boolean bindable, String category)`**<br/>
**Методы**

- `Module.getName();` - возвращает имя кастомного модуля.
- `Module.isToggleable();` - возвращает `true` если модуль можно переключать, и `false` если нет.
- `Module.isBindable();` - возвращает `true` если модуль можно биндить, и `false` если нет.
- `Module.getCategory();` - возвращает строку с названием категории в которой находится модуль.
- `Module.isActive();` - возвращает текущее состояние модуля (true/false). (*всегда возвращает `false` если модуль нельзя переключать*)
- `Module.isBindActive();` - возвращает `true` или `false` в зависимости от того, стоит ли бинд на модуле. (*всегда возвращает `false` если модуль нельзя биндить*)
- `Module.hasSettings();` - возвращает `true` если модуль имеет настройки, и `false` если нет.
- `Module.getSetting(String name);` - ищет среди настроек модуля настройку с названием `name` и возвращает объект `Setting` (объект будет являться экземпляром `ButtonSetting`, `ModeSetting`, `SliderSetting`, `StateSetting` или `TextFieldSetting` в зависимости от типа). Бросает `RuntimeException` если не находит настройку.
- `Module.getSettings();` - возвращает массив объектов `Setting`.
- `Module.addSetting(Setting setting);` - добавляет настройку к модулю.
- `Module.addSettings(Setting[] settings);` - тоже самое что и `Module.addSetting(Setting setting);`, просто позволяет одной строкой добавить сразу несколько настроек.

- `Module.setOnToggleListener(function(View view, boolean active));` - добавляет действие `function(View view, boolean active)` вызываемое при переключении модуля.
- `Module.setOnClickListener(function(View view));` - добавляет действие `function(View view)` вызываемое при клике по кнопке модуля.

**Статические методы**<br/>
*Эти методы предназначены для работы с дефолтными модулями, они делают тоже самое, что и методы выше, просто первым аргументом требуют имя метода. Бросают `RuntimeException` если модуль не найден. Пока что только так.*

- `Module.isToggleable(String moduleName);`
- `Module.isBindable(String moduleName);`
- `Module.getCategory(String moduleName);`
- `Module.isActive(String moduleName);`
- `Module.isBindActive(String moduleName);`
- `Module.hasSettings(String moduleName);`
- `Module.getSettingNames(String moduleName);`

**Примеры: скоро.**

## ModuleManager
*Класс для добавления/удаления кастомных модулей*<br/>
**Статические методы**

- `ModuleManager.addModule(Module module);` - добавляет модуль в одну из пяти [категорий](#modulecategory) меню.
- `ModuleManager.addModules(Modules[] modules);` - тоже самое, просто позволяет добавить сразу несколько модулей одной строкой.
- `ModuleManager.removeModule(Module module);` - удаляет модуль.
- `ModuleManager.removeModules(Module[] modules);` - тоже самое, просто позволяет удалить сразу несколько модулей одной строкой.
- `ModuleManager.getModuleNames();` - возвращает массив с именами всех модулей (имена кастомных модулей тоже).

**Примеры: скоро.**

## Setting
*Класс для получения какой-либо информации о настройках дефолтных модулей (ситуация та же что и с Module)*<br/>
**Статические методы**

- `Setting.getType(String moduleName, String settingName);` - ищет настройку `settingName` у модуля `moduleName` и возвращает его тип.
- `Setting.isVisible(String moduleName, String settingName);` - ищет настройку `settingName` у модуля `moduleName` и возвращает логическое значение (`true` если настройка видима и `false` если нет).
- `Setting.setVisibility(String moduleName, String settingName, boolean visibility);` - ищет настройку `settingName` у модуля `moduleName` и устанавливает ей видимость `visibility` (true/false).
- `Setting.getCurrentMode(String moduleName, String settingName);` - ищет настройку `settingName` у модуля `moduleName` и возвращает текущий выбранный режим. **Бросит `RuntimeException` если тип настройки окажется не `mode`**.
- `Setting.getCurrentValue(String moduleName, String settingName);` - ищет настройку `settingName` у модуля `moduleName` и возвращает текущее значение ползунка. **Бросит `RuntimeException` если тип настройки окажется не `slider`**.
- `Setting.isActive(String moduleName, String settingName);` - ищет настройку `settingName` у модуля `moduleName` и возвращает текущее состояние. **Бросит `RuntimeException` если тип настройки окажется не `state`**.
- `Setting.getText(String moduleName, String settingName);` - ищет настройку `settingName` у модуля `moduleName` и возвращает текущий текст. **Бросит `RuntimeException` если тип настройки окажется не `text-field`**.

***Обратите внимание, что каждый из этих методов может бросить `RuntimeException` если вы ошибетесь в названии модуля или настройки.***

**Примеры: скоро.**

## ButtonSetting
*Класс для создания настройки кнопки. Можно добавлять не только к кастомным модулям, но и к дефолтным*<br/>
**Конструктор: `ButtonSetting(String name, function(View view));`**<br/>
**Методы**

- `ButtonSetting.getName();` - возвращает имя настройки.
- `ButtonSetting.getType();` - возвращает `"button"`.
- `ButtonSetting.isVisible();` - возвращает `true` если настройка видима и `false` если нет.
- `ButtonSetting.setVisibility(boolean visibility);` - устанавливает настройке видимость `visibility` (true/false).

**Примеры: скоро.**

## ModeSetting
*Класс для создания настройки режима. Можно добавлять не только к кастомным модулям, но и к дефолтным*<br/>
**Конструктор: `ModeSetting(String name, String[] modes);`**<br/>
**Методы**

- `ModeSetting.getName();` - возвращает имя настройки.
- `ModeSetting.getType();` - возвращает `"mode"`.
- `ModeSetting.isVisible();` - возвращает `true` если настройка видима и `false` если нет.
- `ModeSetting.setVisibility(boolean visibility);` - устанавливает настройке видимость `visibility` (true/false).

- `ModeSetting.getCurrentMode();` - возвращает текущий выбранный режим.
- `ModeSetting.setOnModeSelectedListener(function(String mode));` - устанавливает действие `function(String mode)` выполняемое после выбора режима.

**Примеры: скоро.**

## SliderSetting
*Класс для создания настройки ползунка. Можно добавлять не только к кастомным модулям, но и к дефолтным*<br/>
**Конструктор: `SliderSetting(String name, [double default, double min, double max, double step]);`**<br/>
**Методы**

- `SliderSetting.getName();` - возвращает имя настройки.
- `SliderSetting.getType();` - возвращает `"slider"`.
- `SliderSetting.isVisible();` - возвращает `true` если настройка видима и `false` если нет.
- `SliderSetting.setVisibility(boolean visibility);` - устанавливает настройке видимость `visibility` (true/false).

- `SliderSetting.getCurrentValue();` - возвращает текущее значение.
- `SliderSetting.setOnCurrentValueChangedListener(function(double currentValue));` - устанавливает действие `function(double currentValue)` выполняемое после любого изменения значения.

**Примеры: скоро.**

## StateSetting
*Класс для создания настройки состояния. Можно добавлять не только к кастомным модулям, но и к дефолтным*<br/>
**Конструктор: `StateSetting(String name, boolean state);`**<br/>
**Методы**

- `StateSetting.getName();` - возвращает имя настройки.
- `StateSetting.getType();` - возвращает `"state"`.
- `StateSetting.isVisible();` - возвращает `true` если настройка видима и `false` если нет.
- `StateSetting.setVisibility(boolean visibility);` - устанавливает настройке видимость `visibility` (true/false).

- `StateSetting.isActive();` - возвращает `true` если настройка активна и `false` если нет.
- `StateSetting.setOnStateToggleListener(function(boolean state));` - устанавливает действие `function(boolean state)` выполняемое при переключении настройки.

**Примеры: скоро.**

## TextFieldSetting
*Класс для создания настройки текстового поля. Можно добавлять не только к кастомным модулям, но и к дефолтным*<br/>
**Конструктор: `TextFieldSetting(String name, String hint, String text);`**<br/>
**Методы**

- `TextFieldSetting.getName();` - возвращает имя настройки.
- `TextFieldSetting.getType();` - возвращает `"text-field"`.
- `TextFieldSetting.isVisible();` - возвращает `true` если настройка видима и `false` если нет.
- `TextFieldSetting.setVisibility(boolean visibility);` - устанавливает настройке видимость `visibility` (true/false).

- `TextFieldSetting.getText();` - возвращает текущий текст.
- `TextFieldSetting.setOnTextChangedListener(function(String text));` - устанавливает действие `function(String text)` выполняемое при изменении текста.

**Примеры: скоро.**

## Level
*Класс для работы с уровнем/сервером*<br/>
**Статические методы**

- `Level.getAddress();` - возвращает строку с адресом сервера на котором ты находишься.
- `Level.getPort();` - возвращает порт сервера на котором ты находишься.
- `Level.getAllPlayers();` - возвращает массив с айдишниками всех игроков с сервера.
- `Level.getTime();` - возвращает время суток в игре.
- `Level.getGameSpeed();` - возвращает текущую скорость игры.
- `Level.addParticle(int type, double x, double y, double z, double velX, double velY, double velZ, int size);` - добавляет партикл типа `type` на координатах `x` `y` `z` с ускорением `velX` `velY` `velZ` и размером `size`.
- `Level.setTime(int time);` - устанавливает время суток в игре.
- `Level.setGameSpeed(double speed);` - устанавливает скорость игры. (по умолчанию 20)

- `Level.setTitle(String text);` - вызывает сообщение на весь экран.
- `Level.setSubtitle(String text);` - вызывает сообщение на весь экран но поменьше.
- `Level.displayClientMessage(String text);` - отправляет в чат сообщение которое видишь только ты.
- `Level.showTipMessage(String text);` - вызывает сообщение-подсказку. (типо как когда берешь предмет в руку)

**Примеры: скоро.**

## Block
*Класс для работы с блоками*<br/>
**Статические методы**

- `Block.getID(int x, int y, int z);` - возвращает айди блока по координатам `x` `y` `z`.
- `Block.getBrightness(int x, int y, int z);` - возвращает яркость блока по координатам `x` `y` `z`.
- `Block.getFriction(int id);` - возвращает скольжение блока с айди `id`.
- `Block.isSolid(int id);` - возвращает `true` если блок с айди `id` является полным и наоборот. (Например, если блок является сундуком, вернет `false`)
- `Block.setFriction(int id, double friction);` - устанавливает скольжение `friction` для блока с айди `id`.
- `Block.setDestroyTime(int id, double time);` - устанавливает длительность разрушения `time` для блока с айди `id`.

**Примеры: скоро.**

## LocalPlayer
*Класс для работы с твоим игроком*<br/>
**Статические методы**

- `LocalPlayer.getUniqueID();` - возвращает айди твоего игрока.
- `LocalPlayer.getPointedPlayer();` - возвращает айди игрока на которого смотрит твой игрок.
- `LocalPlayer.getPointedVector();` - возвращает массив с координатами по которым направлен взгляд твоего игрока.
- `LocalPlayer.attack(int playerID);` - атакует игрока с айди `playerID`.
- `LocalPlayer.attackTp(int playerID);` - атакует игрока с айди `playerID` телепортируясь. Т.е. издалека.
- `LocalPlayer.click(boolean rightClick);` - кликает ЛКМ если `rightClick` равен `false` и ПКМ если `true`.
- `LocalPlayer.longClick();` - делает клик с зажатием. Так, например, можно кидать перлы или зельки.
- `LocalPlayer.buildBlock(int x, int y, int z, int side);` - ставит блок по координатам `x` `y` `z` со [стороной](#blockside) `side`.
- `LocalPlayer.destroyBlock(int x, int y, int z);` - уничтожает блок по координатам `x` `y` `z`.
- `LocalPlayer.openContainer(int x, int y, int z);` - открывает контейнер (сундук, верстак и т.п.) по координатам `x` `y` `z`.
- `LocalPlayer.openInventory();` - открывает инвентарь.
- `LocalPlayer.closeScreen();` - закрывает ЛЮБОЕ окно (можно даже "закрыть" рендер игры, так что осторожнее).
- `LocalPlayer.sendChatMessage(String text);` - отправляет в чат сообщение от твоего лица.
- `LocalPlayer.isMoveButtonPressed(int id);` - возвращает `true` если кнопка с таким-то `id` зажата и `false` если нет.
- `LocalPlayer.setMoveButtonStatus(int id, boolean status);` - выдает кнопке управления `id` статус `status`. (кароче чтобы зажимать кнопку)
- `LocalPlayer.setStepHeight(double height);` - устанавливает высоту шага. (по дефолту 0.5625).
- `LocalPlayer.setOnGround(boolean onGround);` - устанавливает состояние для переменной `onGround`, проще говоря, игра будет думать что вы на земле, даже если это не так.
- `LocalPlayer.setSprinting(boolean sprinting);` - включает или выключает спринт на 1 тик в зависимости от `sprinting`.
- `LocalPlayer.setStatusFlag(int flag, boolean status);` - устанавливает флагу `flag` статус `status`.
- `LocalPlayer.addEffect(int id, int duration, int amplifier, boolean showParticles);` - добавляет эффект на твоего игрока.
- `LocalPlayer.removeEffect(int id);` - удаляет эффект с твоего игрока.
- `LocalPlayer.setRot(double yaw, double pitch);` - поворот головы.

- `LocalPlayer.setPosition(double x, double y, double z);` - телепорт по координатам `x` `y` `z`.
- `LocalPlayer.setPositionX(double x);` - телепорт по `x`.
- `LocalPlayer.setPositionY(double y);` - телепорт по `y`.
- `LocalPlayer.setPositionZ(double z);` - телепорт по `z`.
- `LocalPlayer.setPositionRelative(double x, double y, double z);` - телепорт по координатам `x` `y` `z` относительно текущих.
- `LocalPlayer.setPositionRelativeX(double x);` - телепорт по `x` относительно текущего.
- `LocalPlayer.setPositionRelativeY(double y);` - телепорт по `y` относительно текущего.
- `LocalPlayer.setPositionRelativeZ(double z);` - телепорт по `z` относительно текущего.

- `LocalPlayer.setVelocity(double x, double y, double z);` - задает ускорение по `x` `y` `z`.
- `LocalPlayer.setVelocityX(double x);` - задает ускорение по `x`
- `LocalPlayer.setVelocityY(double y);` - задает ускорение по `y`
- `LocalPlayer.setVelocityZ(double z);` - задает ускорение по `z`

- `LocalPlayer.isInGame();` - возвращает `true` если ты находишься в мире или на сервере и наоборот.
- `LocalPlayer.getViewPerspective();` - возвращает число в зависимости от выбранного вида.
- `LocalPlayer.setViewPerspective(int view);` - устанавливает вид (типа F5)
- `LocalPlayer.isMovingForward();` - вернет `true` если игрок двигается вперед и `false` если нет.
- `LocalPlayer.getDistanceTo(int playerID);` - возвращает дистанцию до игрока с айди `playerID` в блоках.
- `LocalPlayer.getDistanceToCoords(double x, double y, double z);` - возвращает дистанцию до координат `x` `y` `z` в блоках.
- `LocalPlayer.getNearestPlayer(double radius);` - возвращает айди ближайшего игрока в радиусе `radius`.
- `LocalPlayer.lookAt(int playerID);` - резко поворачивает голову в сторону игрока `playerID`
- `LocalPlayer.smoothLookAt(int playerID, double smoothness);` - плавно поворачивает голову в сторону игрока с айди `playerID` с плавностью `smoothness` (чтобы работало нужно использовать либо в `onFastTick` либо в `onLevelTick`).
 
- `LocalPlayer.getNameTag();` - возвращает строку с никнеймом.
- `LocalPlayer.setNameTag(String nameTag);` - визуально меняет ник на `nameTag`.
- `LocalPlayer.getHealth();` - возвращает текущее здоровье.
- `LocalPlayer.getYaw();` - возвращает поворот головы по горизонтали в градусах.
- `LocalPlayer.getPitch();` - возвращает поворот головы по вертикали в градусах.
- `LocalPlayer.getPositionX();` - возвращает позицию по `x`.
- `LocalPlayer.getPositoinY();` - возвращает позицию по `y`.
- `LocalPlayer.getPositionZ();` - возвращает позицию по `z`.
- `LocalPlayer.getVelocityX();` - возвращает ускорение по `x`.
- `LocalPlayer.getVelocityY();` - возвращает ускорение по `y`.
- `LocalPlayer.getVelocityZ();` - возвращает ускорение по `z`.
- `LocalPlayer.getCollisionSize();` - возвращает массив с размерами хитбокса по горизонтали и вертикали.
- `LocalPlayer.getStatusFlag(int flag);` - возвращает  `true` если флаг `flag` активен и наоборот.
- `LocalPlayer.getFallDistance();` - возвращает дистанцию с которой падает игрок.
- `LocalPlayer.isInCreativeMode();` - возвращает `true` если игрок находится в креативе и наоборот.
- `LocalPlayer.isInLava();` - возвращает `true` если игрок находится в лаве.
- `LocalPlayer.isInvisible();` - возвращает `true` если игрок находится под эффектом невидимости.
- `LocalPlayer.isInWall();` - возвращает `true` если игрок находится в блоках.
- `LocalPlayer.isInWater();` - возвращает `true` если игрок находится в воде.
- `LocalPlayer.isOnFire();` - возвращает `true` если игрок находится в огне.
- `LocalPlayer.isOnGround();` - возвращает `true` если игрок находится на земле.
- `LocalPlayer.isFalling();` - возвращает `true` если игрок падает.
- `LocalPlayer.isImmobile();` - возвращает `true` если игрок не может двигаться.
- `LocalPlayer.isSitting();` - возвращает `true` если игрок сидит.
- `LocalPlayer.isSneaking();` - возвращает `true` если игрок на шифте.
- `LocalPlayer.isAlive();` - возвращает `true` если игрок живой.
- `LocalPlayer.isOnLadder();` - возвращает `true` если игрок находится на лестнице.
- `LocalPlayer.canFly();` - возвращает `true` если игрок может летать.
- `LocalPlayer.canShowNameTag();` - возвращает `true` если ник игрока видимый.

**Примеры: скоро.**

## Player
*Класс для работы с игроками на сервере*<br/>
**Статические методы**

- `Player.isLocalPlayer(int playerID);` - возвращает `true` если игрок является локальным игроком (тобой).
- `Player.getNameTag(int playerID);` - возвращает никнейм игрока.
- `Player.setNameTag(int playerID, String nameTag);` - визуально меняет ник игроку с айди `playerID` на `nameTag`.
- `Player.getYaw(int playerID);` - возвращает поворот головы по горизонтали в градусах.
- `Player.getPitch(int playerID);` - возвращает поворот головы по вертикали в градусах.
- `Player.getPositionX(int playerID);` - возвращает позицию по `x`.
- `Player.getPositoinY(int playerID);` - возвращает позицию по `y`.
- `Player.getPositionZ(int playerID);` - возвращает позицию по `z`.
- `Player.getVelocityX(int playerID);` - возвращает ускорение по `x`.
- `Player.getVelocityY(int playerID);` - возвращает ускорение по `y`.
- `Player.getVelocityZ(int playerID);` - возвращает ускорение по `z`.
- `Player.getCollisionSize(int playerID);` - возвращает массив с размерами хитбокса игрока по горизонтали и вертикали.
- `Player.setCollisionSize(int playerID, double horizontal, double vertical);` - устанавливает размеры хитбокса игрока по горизонтали `horizontal` и по вертикали `vertical`.
- `Player.setShadowRadius(int playerID, double radius);` - устанавливает радиус `radius` теней игрока.
- `Player.getStatusFlag(int playerID, int flag);` - возвращает  `true` если у игрока активен флаг `flag` и наоборот.
- `Player.getFallDistance(int playerID);` - возвращает дистанцию с которой падает игрок.
- `Player.isInCreativeMode(int playerID);` - возвращает `true` если игрок находится в креативе и наоборот.
- `Player.isInLava(int playerID);` - возвращает `true` если игрок находится в лаве.
- `Player.isInvisible(int playerID);` - возвращает `true` если игрок находится под эффектом невидимости.
- `Player.isInWall(int playerID);` - возвращает `true` если игрок находится в блоках.
- `Player.isInWater(int playerID);` - возвращает `true` если игрок находится в воде.
- `Player.isInWorld(int playerID);` - возвращает `true` если игрок находится на сервере.
- `Player.isOnFire(int playerID);` - возвращает `true` если игрок находится в огне.
- `Player.isOnGround(int playerID);` - возвращает `true` если игрок находится на земле.
- `Player.isFalling(int playerID);` - возвращает `true` если игрок падает.
- `Player.isImmobile(int playerID);` - возвращает `true` если игрок не может двигаться.
- `Player.isSitting(int playerID);` - возвращает `true` если игрок сидит.
- `Player.isSneaking(int playerID);` - возвращает `true` если игрок на шифте.
- `Player.isAlive(int playerID);` - возвращает `true` если игрок живой.
- `Player.isOnLadder(int playerID);` - возвращает `true` если игрок находится на лестнице.
- `Player.canFly(int playerID);` - возвращает `true` если игрок может летать.
- `Player.canShowNameTag(int playerID);` - возвращает `true` если ник игрока видимый.

***Аргумент `playerID` - это айди игрока.***

**Примеры: скоро.**

## Inventory
*Класс для работы с твоим инвентарем*<br/>
**Статические методы**

- `Inventory.swapSlots(int fromSlot, int toSlot);` - меняет слоты `fromSlot` и `toSlot` местами.
- `Inventory.dropSlot(int slot, boolean dropAll, boolean deleteDrop);` - выкидывает предмет из слота `slot`. Если dropAll равен `true` - выкидывает весь стак. Если deleteDrop равен `true` - удаляет выкидываемый предмет.
- `Inventory.clearSlot(int slot);` - очищает слот `slot`.
- `Inventory.getSelectedSlot();` - возвращает айди выбранного слота.
- `Inventory.getOffhandSlot();` - возвращает айди предмета в левой руке.
- `Inventory.getArmor(int armorSlot);` - возвращает айди предмета в слоте брони `armorSlot` (от 0 до 3).
- `Inventory.setSelectedSlot(int slot);` - выбирает слот `slot` из хотбара (от 0 до 8).
- `Inventory.setOffhandSlot(int slot);` - перемещает предмет из слота `slot` в левую руку.
- `Inventory.setArmor(int slot, int armorSlot, int freeSlot);` - надевает предмет из слота `slot` в слот брони `armorSlot`, и перетаскивает броню (если она есть) из слота `armorSlot` в слот `freeSlot`.

**Примеры: скоро.**

## Item
*Класс для работы с предметами из твоего инвентаря*<br/>
**Статические методы**

- `Item.getID(int slot);` - возващает айди предмета из слота `slot`.
- `Item.getData(int slot);` - возвращает вторичный айди предмета из слота `slot`. (Например, если предмет в слоте `slot` - диорит, то вернет 3).
- `Item.getName(int slot);` - возвращает имя предмета из слота `slot`.
- `Item.enchant(int slot, int enchantment, int level);` - зачаровывает предмет из слота `slot` на `enchantment` с уровнем `level`.
- `Item.getUseDuration(int slot);` - возвращает длительность использования предмета из слота `slot`.
- `Item.getMaxStackSize(int slot);` - возвращает максимальный размер стака предмета из слота `slot`.
- `Item.getMaxDamage(int slot);` - возвращает максимальную прочность предмета из слота `slot`.
- `Item.getDamage(int slot);` - возвращает текущую прочность предмета из слота `slot`.
- `Item.getCount(int slot);` - возвращает текущее количество предметов в слоте `slot`.
- `Item.isArmor(int slot);` - возвращает `true` если предмет из слота `slot` является броней.
- `Item.isBlock(int slot);` - возвращает `true` если предмет из слота `slot` является блоком.
- `Item.isDamageable(int slot);` - возвращает `true` если предмет из слота `slot` имеет прочность.
- `Item.isStackable(int slot);` - возвращает `true` если предмет из слота `slot` стакается.
- `Item.isFullStack(int slot);` - возвращает `true` если в слоте `slot` находится стак любых предметов.
- `Item.isThrowable(int slot);` - возвращает `true` если предмет из слота `slot` можно метать. (перл, снежок и т.п.)
- `Item.isDamaged(int slot);` - возвращает `true` если предмет из слота `slot` имеет не полную прочность.
- `Item.isPotion(int slot);` - возвращает `true` если предмет из слота `slot` является зельем.
- `Item.isEnchanted(int slot);` - возвращает `true` если предмет из слота `slot` зачарован.
- `Item.setName(int slot, String name);` - изменяет имя предмета из слота `slot` на `name`.
- `Item.setUseDuration(int slot, int duration);` - устанавливает предмету из слота `slot` длительность использования `duration`.
- `Item.setCount(int slot, int count);` - устанавливает количество предметов из слота `slot` на `count`.

**Примеры: скоро.**

## Memory
*Класс для работы с памятью (ТОЛЬКО ДЛЯ ОСОБО УМНЫХ)*<br/>
**Статические методы**

- `Memory.write(int address, int[] patch);` - редактирует участок памяти по адресу `address` на `patch`.
- `Memory.read(int address, int length);` - читает и возвращает строку байтов с адреса `address` в количестве `length`.
- `Memory.getLibrary();` - возвращает адрес либы майна.
- `Memory.getSymbol(String symbol);` - возвращает адрес символа `symbol` относительно либы.
- `Memory.getPlayer(int playerID);` - возвращает адрес игрока с айди `playerID` внутри игры.
- `Memory.getLocalPlayer();` - возвращает адрес локального игрока (твой).
- `Memory.getGameMode();` - возвращает адрес объекта `GameMode`.
- `Memory.getHitResult();` - возвращает адрес объекта `HitResult`.
- `Memory.getLevel();` - возвращает адрес объекта `Level`.
- `Memory.getDimension();` - возвращает адрес объекта `Dimension`.
- `Memory.getLevelRenderer();` - возвращает адрес объекта `LevelRenderer`.
- `Memory.getOptions();` - возвращает адрес объекта `Options`.
- `Memory.getTimer();` - возвращает адрес объекта `Timer`.

- `Memory.getString(int address, int offset);` - возвращает строку с адреса `address` и смещением `offset`.
- `Memory.getBoolean(int address, int offset);` - возвращает логическое значение с адреса `address` и смещением `offset`.
- `Memory.getInt(int address, int offset);` - возвращает число с адреса `address` и смещением `offset`.
- `Memory.getFloat(int address, int offset);` - возвращает число с плавающей точкой с адреса `address` и смещением `offset`.
- `Memory.getChar(int address, int offset);` - возвращает символ с адреса `address` и смещением `offset`.

- `Memory.setString(int address, int offset, String value);` - заменяет строку по адресу `address` и смещением `offset` на `value`.
- `Memory.setBoolean(int address, int offset, boolean value);` - заменяет логическое значение по адресу `address` и смещением `offset` на `value`.
- `Memory.setInt(int address, int offset, int value);` - заменяет число по адресу `address` и смещением `offset` на `value`.
- `Memory.setFloat(int address, int offset, float value);` - заменяет число с плавающей точкой по адресу `address` и смещением `offset` на `value`.
- `Memory.setChar(int address, int offset, char value);` - заменяет символ по адресу `address` и смещением `offset` на `value`.

***Внимание: этот класс очень опасная хуйня, если не шарите - не рекомендую вообще к нему прикасаться, иначе ни один метод не будет делать ничего, кроме как вызывать краш игры.***

**Примеры: скоро.**

## Константные классы
### ModuleCategory
*Категории для кастомных модулей*
```js
ModuleCategory.COMBAT
ModuleCategory.MOVEMENT
ModuleCategory.PLAYER
ModuleCategory.MISC
ModuleCategory.OTHER
```

### BlockSide
*В основном нужно для LocalPlayer.buildBlock(x, y, z, side) в качестве аргумента side*
```js
BlockSide.DOWN
BlockSide.UP
BlockSide.NORTH
BlockSide.SOUTH
BlockSide.WEST
BlockSide.EAST
```

### MoveButton
*Нужно для методов `LocalPlayer.isMoveButtonPressed` и `LocalPlayer.setMoveButtonStatus`*
```js
MoveButton.JUMP
MoveButton.SHIFT
MoveButton.JUMP_IN_FLIGHT
MoveButton.FORWARD
MoveButton.BACK
MoveButton.LEFT
MoveButton.RIGHT
MoveButton.LEFT_TOP
MoveButton.RIGHT_TOP
```

### PacketType
*Тип отправляемого пакета, нужно для onSendPacket (ЭТО ТОЖЕ ТОЛЬКО ДЛЯ ОСОБО УМНЫХ)*
```js
PacketType.ADD_BEHAVIOR_TREE_PACKET
PacketType.ADD_ENTITY_PACKET
PacketType.ADD_ITEM_ENTITY_PACKET
PacketType.ADD_ITEM_PACKET
PacketType.ADD_PAINTING_PACKET
PacketType.ADD_PLAYER_PACKET
PacketType.ADVENTURE_SETTINGS_PACKET
PacketType.ANIMATE_PACKET
PacketType.AVAILABLE_COMMANDS_PACKET
PacketType.BLOCK_ENTITY_DATA_PACKET
PacketType.BLOCK_EVENT_PACKET
PacketType.BLOCK_PICK_REQUEST_PACKET
PacketType.BOSS_EVENT_PACKET
PacketType.CAMERA_PACKET
PacketType.CHANGE_DIMENSION_PACKET
PacketType.CHUNK_RADIUS_UPDATED_PACKET
PacketType.CLIENT_TO_SERVER_HANDSHAKE_PACKET 
PacketType.COMMAND_BLOCK_UPDATE_PACKET
PacketType.CONTAINER_CLOSE_PACKET
PacketType.CONTAINER_OPEN_PACKET
PacketType.CONTAINER_SET_CONTENT_PACKET
PacketType.CONTAINER_SET_DATA_PACKET
PacketType.CONTAINER_SET_SLOT_PACKET
PacketType.CRAFTING_DATA_PACKET
PacketType.CRAFTING_EVENT_PACKET
PacketType.DISCONNECT_PACKET
PacketType.DROP_ITEM_PACKET
PacketType.ENTITY_EVENT_PACKET
PacketType.ENTITY_FALL_PACKET
PacketType.EXPLODE_PACKET
PacketType.FULL_CHUNK_DATA_PACKET
PacketType.GAME_RULES_CHANGED_PACKET
PacketType.HURT_ARMOR_PACKET
PacketType.INVENTORY_ACTION_PACKET
PacketType.INTERACT_PACKET
PacketType.ITEM_FRAME_DROP_ITEM_PACKET
PacketType.LEVEL_EVENT_PACKET
PacketType.LEVEL_SOUND_EVENT_PACKET
PacketType.LOGIN_PACKET
PacketType.MAP_INFO_REQUEST_PACKET
PacketType.MINING_FATIGUE_PARTICLE
PacketType.MOB_ARMOR_EQUIPMENT_PACKET
PacketType.MOB_EFFECT_PACKET
PacketType.MOB_EQUIPMENT_PACKET
PacketType.MOVE_ENTITY_PACKET
PacketType.MOVE_PLAYER_PACKET
PacketType.PLAYER_ACTION_PACKET
PacketType.PLAYER_INPUT_PACKET
PacketType.PLAYER_LIST_PACKET
PacketType.PLAY_SOUND_PACKET
PacketType.PLAY_STATUS_PACKET
PacketType.PURCHASE_RECEIPT_PACKET
PacketType.REMOVE_BLOCK_PACKET
PacketType.REMOVE_ENTITY_PACKET
PacketType.REPLACE_ITEM_IN_SLOT_PACKET
PacketType.REQUEST_CHUNK_RADIUS_PACKET
PacketType.RESPAWN_PACKET
PacketType.RESOURCE_PACK_CHUNK_DATA_PACKET
PacketType.RESOURCE_PACK_CHUNK_REQUEST_PACKET
PacketType.RESOURCE_PACK_DATA_INFO_PACKET
PacketType.RESOURCE_PACK_STACK_PACKET
PacketType.RESOURCE_PACKS_INFO_PACKET
PacketType.RIDER_JUMP_PACKET
PacketType.SERVER_TO_CLIENT_HANDSHAKE_PACKET 
PacketType.SET_COMMANDS_ENABLED_PACKET
PacketType.SET_DIFFICULTY_PACKET
PacketType.SET_ENTITY_DATA_PACKET
PacketType.SET_ENTITY_LINK_PACKET
PacketType.SET_ENTITY_MOTION_PACKET
PacketType.SET_HEALTH_PACKET
PacketType.SET_PLAYER_GAME_TYPE_PACKET
PacketType.SET_TIME_PACKET
PacketType.SET_TITLE_PACKET
PacketType.SHOW_CREDITS_PACKET
PacketType.SHOW_STORE_OFFER_PACKET
PacketType.SIMPLE_EVENT_PACKET
PacketType.SPAWN_EXPERIENCE_ORB_PACKET
PacketType.START_GAME_PACKET
PacketType.STOP_SOUND_PACKET
PacketType.STRUCTURE_BLOCK_UPDATE_PACKET
PacketType.TAKE_ITEM_ENTITY_PACKET
PacketType.TEXT_PACKET
PacketType.TRANSFER_PACKET
PacketType.UPDATE_ATTRIBUTES_PACKET
PacketType.UPDATE_BLOCK_PACKET
PacketType.UPDATE_EQUIP_PACKET
PacketType.UPDATE_TRADE_PACKET
PacketType.USE_ITEM_PACKET
```

### Enchantment
*Все виды зачарований. Нужно для `Item.enchant`*
```js
Enchantment.PROTECTION
Enchantment.FIRE_PROTECTION
Enchantment.FEATHER_FALLING
Enchantment.BLAST_PROTECTION
Enchantment.PROJECTILE_PROTECTION
Enchantment.THORNS
Enchantment.RESPIRATION
Enchantment.AQUA_AFFINITY
Enchantment.DEPTH_STRIDER
Enchantment.SHARPNESS
Enchantment.SMITE
Enchantment.BANE_OF_ARTHROPODS
Enchantment.KNOCKBACK
Enchantment.FIRE_ASPECT
Enchantment.LOOTING
Enchantment.EFFICIENCY
Enchantment.SILK_TOUCH
Enchantment.UNBREAKING
Enchantment.FORTUNE
Enchantment.POWER
Enchantment.PUNCH
Enchantment.FLAME
Enchantment.INFINITY
Enchantment.LUCK_OF_THE_SEA
Enchantment.LURE
Enchantment.FROST_WALKER
Enchantment.MENDING
```

### ParticleType
*Все (почти) виды партиклов. Нужно для `Level.addParticle`*
```js
ParticleType.BUBBLE
ParticleType.CRITICAL
ParticleType.EXPLODE
ParticleType.EVAPORATION
ParticleType.FLAME
ParticleType.LAVA
ParticleType.RED_DUST
ParticleType.SNOW_BALL_POOF
ParticleType.LARGE_EXPLODE
ParticleType.ULTRA_LARGE_EXPLODE
ParticleType.MOB_FLAME
ParticleType.HEART
ParticleType.TERRAIN
ParticleType.TOWN_AURA
ParticleType.PORTAL
ParticleType.WATER_SPLASH
ParticleType.WATER_WAKE
ParticleType.DRIP_WATER
ParticleType.DRIP_LAVA
ParticleType.FALLING_DUST
ParticleType.SLIME
ParticleType.RAIN_SPLASH
ParticleType.VILLAGE_ANGRY
ParticleType.VILLAGE_HAPPY
ParticleType.ENCHANTING_TABLE
ParticleType.NOTE
```

## Hooks
*Все игровые хуки на данный момент*

- `onScriptEnabled();` - вызывается после включения скрипта.
- `onScriptDisabled();` - вызывается до выключения скрипта.
- `onFastTick();` - вызывается 1000 раз за секунду. *Примечание: не игровой хук.*
- `onLevelTick();` - вызывается 20 раз за секунду.
- `onAttack(int playerID);` - вызывается когда ты бьешь игрока. `playerID` - айди игрока которого ты ударил. Для этого хука можно использовать `preventDefault();` (наебал, нельзя).
- `onPlayerAdded(int playerID);` - вызывается когда на сервер заходит игрока. `playerID` - айди игрока который зашел.
- `onUseItem(int x, int y, int z, int side, int itemId, int blockId);` - вызывается когда ты нажимаешь по блоку. `x`, `y`, `z` - координаты блока, `side` - сторона блока по которой ты нажал, `itemId` - айди предмета которым ты нажал, `blockId` - айди блока по которому ты нажал. Для этого хука можно использовать `preventDefault();`.
- `onTeleport(int playerID, int x, int y, int z);` - вызывается когда игрок телепортируется. `playerID` - айди игрока который телепортировался, `x`, `y`, `z` - координаты куда он телепортировался.
- `onChat(String text);` - вызывается когда ты пишешь сообщение в чат. `text` - текст твоего сообщения. Для этого хука можно использовать `preventDefault();`.
- `onReceiveServerMessage(String message);` - вызывается когда в чат отправляется сообщение от сервера/игрока. Для этого хука можно использовать `preventDefault();`.
- `onScreenChange(String screen);` - вызывается когда ты переходишь между игровыми экранами. `screen` - экран на который ты перешел.
- `onServerConnect(String address, int port);` - вызывается когда ты заходишь на сервер. `address` - адрес сервера, `port` - порт сервера.
- `onServerDisconnect();` - вызывается когда ты выходишь с сервера.
- `onSendPacket(String name, int address);` - вызывается при отправке пакета. `name` - название пакета, `address` - адрес пакета в памяти. Для этого хука можно использовать `preventDefault();`. (ТОЖЕ ТОЛЬКО ДЛЯ ОСОБО УМНЫХ)

За покупкой исходного кода писать сюда - t.me/lednevok1
