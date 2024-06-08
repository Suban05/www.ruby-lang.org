---
layout: news_post
title: "Выпуск Ruby 3.3.0-preview2"
author: "naruse"
translator: "suban05"
date: 2023-09-14 00:00:00 +0000
lang: en
---

{% assign release = site.data.releases | where: "version", "3.3.0-preview2" | first %}

Мы рады объявить о выпуске Ruby {{ release.version }}. Ruby 3.3 добавляет новый чистый JIT-компилятор на Ruby под названием RJIT, использует Lrama в качестве генератора парсеров, а также внесено множество улучшений производительности, особенно YJIT.

## RJIT

* Введен чисто-рубиновый JIT-компилятор RJIT и заменен MJIT.
  * RJIT поддерживает только архитектуру x86\_64 на платформах Unix.
  * В отличие от MJIT, он не требует компилятора C во время выполнения.
* RJIT существует только в экспериментальных целях.
  * В производственных условиях рекомендуется продолжать использовать YJIT.
* Если вас интересует разработка JIT для Ruby, ознакомьтесь с [презентацией k0kubun на 3-й день RubyKaigi](https://rubykaigi.org/2023/presentations/k0kubun.html#day3).

## Использование Lrama вместо Bison

* Заменить Bison на [Lrama LALR генератор парсеров](https://github.com/yui-knk/lrama) [Feature #19637](https://bugs.ruby-lang.org/issues/19637)
  * Если вас это интересует, ознакомьтесь с [будущим видением парсера Ruby](https://rubykaigi.org/2023/presentations/spikeolaf.html)

## YJIT

* Основные улучшения производительности по сравнению с 3.2
  * Улучшена поддержка аргументов splat и rest.
  * Выделяются регистры для операций со стеком виртуальной машины.
  * Компилируются больше вызовов с опциональными аргументами.
  * Также компилируются обработчики исключений.
  * Переменные экземпляра больше не возвращаются в интерпретатор с мегаморфными Object Shapes.
  * Неподдерживаемые типы вызовов больше не возвращаются в интерпретатор.
  * Особенно оптимизированы `Integer#!=`, `String#!=`, `Kernel#block_given?`, `Kernel#is_a?`, `Kernel#instance_of?`, `Module#===`.
  * Теперь более чем в 3 раза быстрее интерпретатора на тестах с optcarrot!
* Метаданные для скомпилированного кода используют гораздо меньше памяти.
* Генерация более компактного кода на ARM64.
* Возможность запуска YJIT в приостановленном режиме и позднее вручную его активировать
  * `--yjit-pause` и `RubyVM::YJIT.resume`
  * Это можно использовать для активации YJIT только после загрузки вашего приложения.
* Статистика `ratio_in_yjit`, получаемая с помощью `--yjit-stats`, теперь доступна в релизных сборках, не требуется специальная сборка для разработки или статистики.
* Опция трассировки выходов теперь поддерживает выборку
  * `--trace-exits-sample-rate=N`
* Более тщательное тестирование и множество исправлений ошибок.

## Другие значимые нововведения

### Язык



## Улучшения производительности

* Оптимизирован `defined?(@ivar)` с использованием Object Shapes.

## Другие значимые изменения с версии 3.2

### IRB

IRB получил несколько улучшений, включая, но не ограничиваясь:

- Расширенная интеграция `irb:rdbg`, предоставляющая опыт отладки, аналогичный `pry-byebug` ([документация](https://github.com/ruby/irb#debugging-with-irb)).
- Поддержка пейджера для команд типа `ls` и `show_cmds`.
- Более точная и полезная информация, предоставляемая командами `ls` и `show_source`.

Кроме того, IRB также претерпел обширную рефакторизацию и получил десятки исправлений ошибок для облегчения будущих улучшений.

## Проблемы совместимости

Примечание: Исключены исправления ошибок, касающиеся функциональности.

### Удаленные константы

Были удалены следующие устаревшие константы.



### Удаленные методы

Были удалены следующие устаревшие методы.



## Проблемы совместимости в стандартной библиотеке

### `ext/readline` уходит на пенсию

* У нас есть `reline`, чистая Ruby-реализация совместимая с API `ext/readline`. Мы полагаемся на `reline` в будущем. Если вам нужно использовать `ext/readline`, вы можете установить `ext/readline` через rubygems.org с помощью `gem install readline-ext`.
* Нам больше не нужно устанавливать библиотеки типа `libreadline` или `libedit`.

## Обновления C API

### Обновленные C API

Следующие API были обновлены.



### Удаленные C API

Были удалены следующие устаревшие API.



## Обновления стандартной библиотеки

RubyGems и Bundler предупреждают пользователей, если они требуют гем, который запланирован быть включенным в будущую версию Ruby.

Были обновлены следующие гемы по умолчанию.

* RubyGems 3.5.0.dev
* bigdecimal 3.1.4
* bundler 2.5.0.dev
* csv 3.2.8
* erb 4.0.3
* fiddle 1.1.2
* fileutils 1.7.1
* irb 1.7.4
* nkf 0.1.3
* optparse 0.4.0.pre.1
* psych 5.1.0
* reline 0.3.8
* stringio 3.0.9
* strscan 3.0.7
* syntax_suggest 1.1.0
* time 0.2.2
* timeout 0.4.0
* uri 0.12.2
* yarp 0.9.0

Были обновлены следующие включенные гемы.

* minitest 5.19.0
* test-unit 3.6.1
* rexml 3.2.6
* rss 0.3.0
* net-imap 0.3.7
* rbs 3.2.1
* typeprof 0.21.8
* debug 1.8.0

Был добавлен следующий гем по умолчанию.

* racc 1.7.1

Для получения дополнительной информации о стандартных и пакетных гемах обратитесь к GitHub-релизам, таким как [Logger](https://github.com/ruby/logger/releases) или к изменениям в журнале изменений.

Подробности о новостях смотрите в [NEWS](https://github.com/ruby/ruby/blob/{{ release.tag }}/NEWS.md)
или [логах коммитов](https://github.com/ruby/ruby/compare/v3_2_0...{{ release.tag }})
для получения дополнительной информации.

С учетом этих изменений, [{{ release.stats.files_changed }} файлов изменено, {{ release.stats.insertions }} добавлено(+), {{ release.stats.deletions }} удалено(-)](https://github.com/ruby/ruby/compare/v3_2_0...{{ release.tag }}#file_bucket)
с момента Ruby 3.2.0!

## Скачать

* <{{ release.url.gz }}>

      SIZE: {{ release.size.gz }}
      SHA1: {{ release.sha1.gz }}
      SHA256: {{ release.sha256.gz }}
      SHA512: {{ release.sha512.gz }}

* <{{ release.url.xz }}>

      SIZE: {{ release.size.xz }}
      SHA1: {{ release.sha1.xz }}
      SHA256: {{ release.sha256.xz }}
      SHA512: {{ release.sha512.xz }}

* <{{ release.url.zip }}>

      SIZE: {{ release.size.zip }}
      SHA1: {{ release.sha1.zip }}
      SHA256: {{ release.sha256.zip }}
      SHA512: {{ release.sha512.zip }}

## Что такое Ruby

Ruby был разработан Мацем (Юкихиро Мацумото) в 1993 году и сейчас развивается как открытое программное обеспечение. Он работает на множестве платформ и широко используется по всему миру, особенно в веб-разработке.