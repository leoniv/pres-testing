# Презентации. Тестирование.

Репозиторий содержит исходные тексты презентаций.

## Разработка

### Необходимое окружение

- Ruby >= 2.3 - `sudo apt-get install ruby`
- JRE - `sudo apt-get install default-jre`
- Gem bundler - `sudo gem install bundler`

### Получение и установка зависимостей

    $git clone https://github.com/leoniv/pres-testing.git && cd pres-testing
    $bundle install
    $mkdir -p tmp
    $git clone https://github.com/asciidoctor/asciidoctor-deck.js.git tmp/asciidoctor-deck.js

### Тест

    $rake test[index.adoc] && firefox testbuild/index.html

### Сборка

    $rake build

## Релиз

    $git push origin master
    $git push origin gh-pages
