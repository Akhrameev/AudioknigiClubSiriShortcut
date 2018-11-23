# AudioKnigi.Club Siri Shortcut
Siri Shortcut for downloading books from [audioknigi.club](https://audioknigi.club) into iCloud

## Введение

Итак, Apple анонсировала крутую фичу Команды для автоматизации рутины на iPhone и iPad.
А я издавна мучаюсь от неудобства быстро и бесплатно скачать аудиокниги на iPhone/iPad без использования компьютера.
Теперь есть решение: AudioKnigi.Club shortcut (Команда) для iOS 12+!

Собственно, можно скачать [.shortcut-файл](https://github.com/Akhrameev/AudioknigiClubSiriShortcut/raw/master/AudioKnigi.Club.shortcut) из этого репозитория и, следуя инструкции, получится скачивать аудиокниги прямо на iPhone/iPad (с сайта [audioknigi.club](https://audioknigi.club)).

**Важно:** нужно перед загрузкой включить переключатель `"По главам"`, иначе скачается только несколько секунд аудио.
Если книга закрыта правообладателем, скачать её не получится (будет ошибка "Истекло время ожидания JavaScript").

**Перед использованием возникнет такое уведомление о потенциальной зловредности JavaScript-кода**

<img src="/javascriptAlert.jpg" width="50%" height="50%">

Бояться его не стоит - код я старался оставить там тривиальный. Он НЕ лезет в ваши персональные данные, НЕ крадёт пароли, cookies и т.п.

Код выглядит примерно так: 

```javascript
var result = [];

var player = document.getElementsByClassName("jp-jplayer")[0];  // нашли на странице плеер
var playlistItems = document.getElementsByClassName("jp-playlist-item"); // нашли на странице список треков

for (let item of playlistItems) {
  item.click(); // нажали на трек, чтобы его url появился в плеере 

  var audio = player.getElementsByTagName("audio200")[0]; // в этом элементе есть название трека и url для загрузки

  result.push({"title": audio.title, "url":audio.src});
}

document.getElementsByClassName("jp-pause")[0].click(); // после выполнения скрипта поставил плеер на паузу

// Call completion to finish
completion(JSON.stringify(result)); // собранный список названий и url-ов отпраивл в Siri Shortcut в формате JSON

var player = document.getElementsByClassName("jp-jplayer")[0];  // нашли на странице плеер
var playlistItems = document.getElementsByClassName("jp-playlist-item");  // нашли на странице список треков
var bookTitle = document.title; // взяли название страницы для того, чтобы использовать в качестве названия книги
bookTitle = bookTitle.replace(". Слушать аудиокнигу онлайн", ""); // в конце названия может быть вот такой лишний текст

var items = [];   // здесь будем накапливать данные глав (название файла и url для загрузки)
var min = 0;      // можно указать значение больше 0 для пропуска первых min глав
var i = 0;      
var max = 1000;   // можно указать другое значение для пропуска глав после max
for (let item of playlistItems) {
  i++;

  if (i < min){continue;}
  if (i > max){continue;}

  item.click(); // нажали на трек, чтобы его название и url появился в плеере 

  var audio = player.getElementsByTagName("audio")[0];  // в этом элементе есть название трека и url для загрузки
  items.push({"title": audio.title, "url":audio.src});
}

var result = {"bookTitle": bookTitle, "items": items};  // собрали в структуру название книги и данные глав
var json = JSON.stringify(result);

var playerBottom = document.getElementsByClassName("player-bottom")[0]; // взяли элемент под списком воспроизведения
playerBottom.innerHTML = "";  // убрали из него прежние данные (в том числе переключатель "По главам")

var el = document.createElement('textarea');  // создали элемент, из которого можно копировать длинный текст
el.value = json;
playerBottom.appendChild(el); // добавили его под список треков

document.getElementsByClassName("jp-pause")[0].click(); // нажали на кнопку "Пауза", чтобы звук последней галвы не сердил во время загрузки

// Call completion to finish
completion(json);

```
Его прекрасно видно в `plist-`файле в репозитории. И в самой команде перед запуском.

## Инструкция по использованию:
1. В Safari зайти на страницу конкретной книги и включить `"По главам"` (под списком треков), если выключено. Иначе загрузится только несколько секунд аудио.
2. Нажать поделиться, и выбрать `Команды->AudioBooks.Club`
3. Профит (не сворачивайте Safari до завершения загрузки - иначе работа Команды прервётся и нужно будет начинать с начала)

### Вот как это работает

<img src="/demonstration.gif">

## Известные проблемы: 
1. "Истекло время ожидания JavaScript"

<img src="/shantaram.gif">

Возможно, скрипт не успел выполнить все действия `click()` - хотя почти всю полезную работу успел сделать.
Это можно увидеть по изменившемуся блоку под списком треков. Если там появился текст `"{bookTitle:..."` - значит, повезло. Смело долго жмём на него и копируем в буфер обмена. Запускаем шорткат ещё раз - но уже из программы Команды. Она считает результат из буфера обмена и сможет скачать аудиокниги даже при свёрнутом Safari (не закрывайте приложение Команды смахиванием, свернуть его можно).
Если блок под списком воспроизведения мешает - его можно легко убрать обновлением страницы.

2. Папки в iCloud/Dropbox создаются - но сами файлы не скачиваются.
Возможно, в настройках Safari отключен Javascript (`Настройки->Safari->Дополнения->JavaScript`) или [Cookies](https://www.apple.com/ru/legal/privacy/ru/cookies/) (`Настройки->Safari->"Блокировка всех cookie"`). Эти технологии необходимы для корректной работы сайта, без них не удастся получить список файлов для загрузки - и, следовательно, загрузить аудиокнигу. Если на сайте не работает воспроизведение книги (и не появляется название главы в строке плеера), возможно, это ваш случай.

3. Воспроизведение из iCloud: стандартный плеер не переключается сам между главами и приходится переключать вручную. 
Решение: использовать [Readdle Documents](https://itunes.apple.com/ru/app/documents-by-readdle/id364901807?mt=8) или [VLC](https://itunes.apple.com/ru/app/vlc-for-mobile/id650377962?mt=8) для воспроизведения аудиокниг из iCloud. 
Или сохранять через iCloud в Dropbox (можно изменить это в настройках Команды - строка `"Служба"`) и воспроизводить через плеер, поддерживающий Dropbox.

### P.S. 
Начиная с версии [3.1.4 VLC](https://itunes.apple.com/ru/app/vlc-for-mobile/id650377962?mt=8) умеет добавлять файлы из iCloud всей папкой. Правда, сейчас текст в шапке пишется белым по белому (ждём исправления в следующей версии). 

### Если вы решили слушать аудиокнигу в VLC 3.1.4
1. Для добавления аудиокниги жмём значёк VLC в верхнем левом углу
2. `Облачные службы`
3. `Облачные службы` (да-да, второй раз)
4. Имеет смысл выбрать вкладку `"Обзор"` внизу - только так доступен выбор все папки
5. Ищем интересующую нас папку (`Shortcuts->AudioBooks->название книги`)

трудности могли настигнуть уже в этот момент из-за того, что стрелочка "Назад" в левом верхнем углу белая на белом. Тем не менее, она там есть. И помогает возвращаться назад до корневой папки. Также работает жест слева направо из-за экрана.

6. Сразу за чёрным названием папки следует кнопка `"Выбрать"` (её немного видно, если файлов в папке много и чуть-чуть проскроллить) и за ней крайняя правая `"Отменить"`. 
7. Благодаря этому мы переходим в выбор многих файлов или папок (можно воспользоваться кнопкой `"Выбрать все"` в левом верхнем углу, она тоже пока белым по белому) 
8. `"Открыть"` (на месте кнопки `"Выбрать"` сразу за словом `"Выбор"`).
9. Аудиокнига добавлена, можно наслаждаться.

### Сейчас также отображается сообщение об ошибке, если пробовать добавить файл повторно (так делала и версия 3.1.3 при одиночном выборе файлов) - надеюсь, и это исправится в версии 3.1.5.
