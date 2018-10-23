# AudioKnigi.Club Siri Shortcut
Siri Shortcut for downloading books from [audioknigi.club](https://audioknigi.club) into iCloud

Итак, Apple анонсировала крутую фичу Команды для автоматизации рутины на iPhone и iPad.
А я издавна мучаюсь от неудобства быстро и бесплатно скачать аудиокниги на iPhone/iPad без использования компьютера.
Теперь есть решение: AudioKnigi.Club shortcut (Команда) для iOS 12+!

Собственно, можно скачать [.shortcut-файл](https://github.com/Akhrameev/AudioknigiClubSiriShortcut/raw/master/AudioKnigi.Club.shortcut) из этого репозитория и, следуя инструкции, получится скачивать аудиокниги прямо на iPhone/iPad (с сайта [audioknigi.club](https://audioknigi.club)).

Важно: нужно перед загрузкой включить переключатель `"По главам"` и создать папку `Shortcuts/AudioBooks` в iCloud (или включить "Спросить о месте сохранения" в последней команде).


# Инструкция по использованию:
1. Зайти в iCloud, создать папку `Shortcuts/Audiobooks` (можно создать папку для конкретной книги внутри неё и скопировать её название в буфер обмена - тогда название папки будет введено автоматически).
2. В Safari зайти на страницу конкретной книги и включить `"По главам"` (под списком треков), если выключено. Иначе загрузится только несколько секунд аудио.
3. Нажать поделиться, и выбрать `Команды->AudioBooks.Club`
4. Профит (не сворачивайте Safari до завершения загрузки - иначе работа Команды прервётся и нужно будет начинать с начала)

# Известные проблемы: 
1. "Файл не существует". Файл скачался - но не смог сохраниться в iCloud. Возможно, не создана папка и выбран несуществующий путь (для сохранения в `Shortcuts/AudioBooks` поле ввода названия оставляем пустым. но это не удобно для книг с множеством глав - в случае использования подпапки пишем не полный путь, а только название подпапки в AudioBooks).
Можно выбрать короткую книгу и установить галочку `"Спросить о месте сохранения"` в последней команде - обычно после успешного выбора одного пути при загрузке других книг проблема не проявляется (можно отключить `"Спросить о месте сохранения"`).
2. "Истекло время ожидания JavaScript"
Книга закрыта правообладателем; возможно мешает онлайн-радио или другая аудио-активность в другой вкладке Safari.
3. Воспроизведение из iCloud: стандартный плеер не переключается сам между главами и приходится переключать вручную. Решение: использовать Readdle Documents или VLC для воспроизведения аудиокниг из iCloud. Или сохранять через iCloud в Dropbox и воспроизводить через плеер, поддерживающий Dropbox.
# P.S. 
Можно плюсануть [мой пулл-реквест в VLC iOS](https://github.com/videolan/vlc-ios/pull/160), чтобы VLC открывал сразу всю папку с аудиокнигой. Сейчас VLC 3.1.3 позволяет только добавление файлов из iCloud по одному - грустно.
