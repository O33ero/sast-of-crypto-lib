### Прогнать любую open source криптографическую библиотеку через статический анализатор. Пошаговое руководство для чайников. <img src="./img/чайник.jpg" alt="drawing" width="80"/>

1. Открываем [репозиторий](https://github.com/sobolevn/awesome-cryptography) со списоком криптографических библиотек на всех языках 
мира.
2. Выбираем любую. (Когда вы выбрали библиотеку, то теперь библиотека должна выбрать вас)
3. Выбираем из списка ниже SAST (статический анализатор), либо ищем другой, 
если по какой то причине никто из списка ниже не подходит. Будет хорошо, если поделитесь с остальными, 
что в итоге использовали. 
4. Запускаем анализатор (везде по разному)
5. Фоткаем вывод с ошибками безопасности
6. Смотрим как выглядит интерфейс библиотеки (точки вызова функций библиотеки), 
пробуем вызывать ошибки, описываем как выглядит обработка ошибок от библиотеки.
7. Собираем фотографии и свои рассуждения из п.6 в отчет и отправляем.

###### SAST, которые нашел и изучил

- `clang-tidy` - анализатор с [лабы Тимакова](https://github.com/O33ero/scan-coturn), **крайне рекомендую**
- `scan-build` - анализатор с [лабы Тимакова](https://github.com/O33ero/scan-coturn), **крайне рекомендую**
- `PVS-Studio` - дробовик среди рогаток, нужна лицензия, FREE trial хрен пойми как работает, **не рекомендую**, есть плагин для IDEA
- `JFrog Xray` - нужна регистрация и поднятия Cloud Environment (закалебешься настраивать), до конца не добрался, но должно работать, есть плагин для IDEA, **стоит попробовать**
- `SonarQube` - нужна лицензия, особо не разбирался
- `Snyk`([Шнюк](https://s27.ucoz.net/video/28/28438336.jpg)) - не нужна лицензия, есть плагин для IDEA, почему то тупит на некоторых проектах, **рекомендую**
- `Bearer` - легковесный анализатор, ставится в два клика, не очень мощный, но для наших задач подойдет, **рекомендую**
- `Semgrep` - _магия наяву_, установка в 3 клика, есть микро регистрация, сервис(в интернете) подключается к вашему локальному клиенту в консоли и отображает в сервисе(**_в интернете!!!_**) ваши локальные проекты и анализ по ним, очень красиво, _**КРАЙНЕ РЕКОМЕНДУЮ**_ 
- `PT Application Inspector` - отличный компактный анализатор, не нужны регистрации и смс, есть плагин в IDEA, почему то очень долго сильно тупит при сканировании, **рекомендую**
- `Brakeman` - поддерживает только Ruby, не тестировал

###### Если не хочется думать или времени мало

1. Берем какую то библиотеку на C (или даже C++) из репозитория выше.
2. Берем `scan-build` (или `clang-tidy`, но все же лучше первое).
3. Запустить `scan-build` дело пары минут, поскольку умеем это из лабы Тимакова.
4. Запускаем анализ на выбранной библиотеке, получаем готовый отчет, с которого можно сделать фотки.
5. Готово. Потрачено минимум времени на настройку анализатора, а значит сэкономлено много сил и времени на остальные дела.

###### Если не хочется думать или времени мало 2. Возвращение джедая

1. Берем какую то библиотеку из репозитория выше
2. Берем `Semgrep` в качестве анализатора, для этого нам понадобится:
   1. Регистрация на `Semgrep`
   2. Установка локального клиента, инструкция есть на сайте, не занимает больше 5 минут
   3. `git clone` выбранной библиотеки
3. Заходим в консоли в папку проекта с библиотекой
4. Делаем в консоли `semgrep login` и подверждаем авторизацию в браузере
5. Делаем в консоли `semgrep ci`
6. После завершения предыдущей операции заходим на сайтик и получаем отчет по библиотеки
7. Делаем фотки
8. Делаем отчет
9. Готово. Ничего кроме установки микро клиента и регистрации нам не понадобилось, а значит сэкономлено много сил и времени на остальные дела.


###### Матрица проверенных связок библиотека-анализатор

| Библиотека \ Анализатор | Bearer | Snyk | Semgrep | PT Application Inspector |
|-------------------------|--------|------|---------|--------------------------|
| tink-java               | ✅      | ❌    | ✅       |                          |
| tink-go                 | ✅      | ✅    | ✅       |                          |
| kyber                   | ❓      | ❓    | ✅       |                          |
| password4j              | ❓      | ❓    | ✅       |                          |
| shiro                   | ✅      | ❓    | ✅       |                          |
| themis                  | ✅      | ✅    | ✅       |                          |

- ✅ - все работает, есть что-то интересное
- ❌ - не работает по какой-то причине
- ❓ - работает, но анализатор ничего не нашел
- 🔵 - 
- ⛔ - анализатор не поддерживает язык библиотеки