# AudioKnigi.Club Siri Shortcut
Siri Shortcut for downloading books from [audioknigi.club](https://audioknigi.club) into iCloud

## Введение

Итак, Apple анонсировала крутую фичу Команды для автоматизации рутины на iPhone и iPad.
А я издавна мучаюсь от неудобства быстро и бесплатно скачать аудиокниги на iPhone/iPad без использования компьютера.
Теперь есть решение: AudioKnigi.Club shortcut (Команда) для iOS 12+!

Собственно, можно скачать [.shortcut-файл](https://github.com/Akhrameev/AudioknigiClubSiriShortcut/raw/master/AudioKnigi.Club.shortcut) из этого репозитория и, следуя инструкции, получится скачивать аудиокниги прямо на iPhone/iPad (с сайта [audioknigi.club](https://audioknigi.club)).

**Важно:** нужно перед загрузкой включить переключатель `"По главам"`, иначе скачается только несколько секунд аудио.

**Перед использованием возникнет такое уведомление о потенциальной зловредности JavaScript-кода**

![javascript alert image](/javascriptAlert.jpg)

Бояться его не стоит - код я старался оставить там тривиальный. Он НЕ лезет в ваши персональные данные, НЕ крадёт пароли, cookies и т.п.

Код выглядит примерно так: 

```javascript
var result = [];

var player = document.getElementsByClassName("jp-jplayer")[0];  // нашли на странице плеер
var playlistItems = document.getElementsByClassName("jp-playlist-item"); // нашли на странице список треков

for (let item of playlistItems) {
  item.click(); // нажали на трек, чтобы его url появился в плеере 

  var audio = player.getElementsByTagName("audio")[0]; // в этом элементе есть название трека и url для загрузки

  result.push({"title": audio.title, "url":audio.src});
}

document.getElementsByClassName("jp-pause")[0].click(); // после выполнения скрипта поставил плеер на паузу

// Call completion to finish
completion(JSON.stringify(result)); // собранный список названий и url-ов отпраивл в Siri Shortcut в формате JSON
```
Его прекрасно видно в `plist-`файле в репозитории. И в самой команде перед запуском.

## Инструкция по использованию:
1. Рекомендую зайти в iCloud, создать папку для книги (при сохранении можно использовать только ранее созданные папки) 
 Например, создать папку `Shortcuts/Audiobooks`, в которой потом накапливать аудиокниги
 
![create AudioBooks folder Shortcuts folder in Files](/createAudioBooksFolder.gif)
* можно создать папку для конкретной книги внутри неё - это удобно, если у книг много глав: главы разных книг не смешиваются

![create BookName folder in Shortcuts/AudioBooks](/createBookNameFolder.gif)

2. В Safari зайти на страницу конкретной книги и включить `"По главам"` (под списком треков), если выключено. Иначе загрузится только несколько секунд аудио.
3. Нажать поделиться, и выбрать `Команды->AudioBooks.Club`
4. Профит (не сворачивайте Safari до завершения загрузки - иначе работа Команды прервётся и нужно будет начинать с начала)

### Вот как это работает после создания папок в программе Файлы
![download audiobook demonstration into Shortcuts/AudioBooks/BookName](/demonstration.gif)

## Известные проблемы: 
1. "Истекло время ожидания JavaScript"
Книга закрыта правообладателем; возможно мешает онлайн-радио или другая аудио-активность в другой вкладке Safari.
2. Воспроизведение из iCloud: стандартный плеер не переключается сам между главами и приходится переключать вручную. 
Решение: использовать Readdle Documents или VLC для воспроизведения аудиокниг из iCloud. 
Или сохранять через iCloud в Dropbox и воспроизводить через плеер, поддерживающий Dropbox.
### P.S. 
Можно плюсануть [мой пулл-реквест в VLC iOS](https://github.com/videolan/vlc-ios/pull/160), чтобы VLC открывал сразу всю папку с аудиокнигой. Сейчас VLC 3.1.3 позволяет только добавление файлов из iCloud по одному - грустно.
