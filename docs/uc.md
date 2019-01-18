## UC playbook

### Общее описание

Проект содержит набор книг(playbook) для управления голосовыми шлюзами cisco CUBE. (книга подготовлена под нужды определннго проекта, но может быть расширена при необходимости)
Основная задача книги - безопасно добавить набор настроек на шлюзы при добавлении нового региона в конфигурацию.

### Состав

 - save_CUBE_conf.yaml - книга сохраняет конфиг на шлюзах
 - update_CUBE_conf.yaml - книга обновляет конфиг на шлюзах на основании данных в директории vars
 - tasks - набор модулей используемых в ходе выполнения книг **save_CUBE** и **update_CUBE**
 
### Что делают книги

 Книга **update_CUBE_conf.yaml** делает следующиее:
 
 1. Делает резервную копию running_config шлюзов - task **backup running config of the {{ inventory_hostname }}**
 1. Загружает переменные из **yaml** файлов из директории **vars**:
    1. dialpeers.yaml
    1. translation_profiles.yaml
    1. translation_rules.yaml
 1. Проверяет есть ли **translation rule** из файлов **vars** в настройках шлюзов, если есть книга останавливает выполнение.
 1. Проверяет есть ли **translations profiles** из файлов **vars** в настройках шлюзов, если есть книга останавливает выполнение.
 1. Проверяет есть ли **dial-peers** из файлов **vars** в настройках шлюзов, если есть книга останавливает выполнение.
 1. Добавляет **translation rule** из файлов **vars** в **running config**
 1. Добавляет **translations profiles** из файлов **vars** в **running config**
 1. Добавляет **dial-peers** из файлов **vars** в **running config**
 1. Отображает **running config** на экране для ознакомления
 1. Переименовывает файлы в директории **vars**, меняя расширение с **.yaml** на **.yaml.bak**
 
 Книга не сохраняет **running config**, чтобы у администратора системы была возможность вернуть систему в исходное состояние в случае проблем.
 
 Книга **update_CUBE_conf.yaml** делает следующее:
 
 1. Сохраняет **running config** в **startup config**
 
 ### Переменные
 
 Переменные - названия, парметры dial-peer и прочее перечислены в файлах, расположенных в директории **vars**
 
 #### Фйал dialpeers.yaml
 
 В файле может быть перечилено необходимое количество dial-peer'ов. Каждый отдельный должен начинаться с дефиса, например
 
     - dialpeer_number: '3820000'
        dialpeer_description: 'Outgoing_to_CUCM_382'
        destination_pattern: '3..000....'
        translate_leg: 'incoming'
        translation_profile_name: '3000001'
        cucm_address: 'ipv4:CUCM_ADDRSS_CALLMN'
        codec_class_number: '1'

 Доступные переменные для dial-peer:
 
 - dialpeer_number: 'xxxx'
 - dialpeer_description: 'xxxx'
 - destination_pattern: 'xxxx'
 - answer_address: ''
 - translate_leg: ''
 - translation_profile_name: ''
 - bind_string: ''
 - session_protocol: ''
 - dtmf_relay_name: ''
 - uri_from: ''
 - codec_class_number: ''
 - codec_name: ''
 - cucm_address: ''
 - vad_mode: ''
 
 
  #### Фйал translation_profiles.yaml
  
  В файле может быть перечилено необходимое количество профилей. Каждый отдельный должен начинаться с дефиса, например
  
       - translation_profile_name: '3820002'
         translation_direction: 'called'
         translation_rule_number: '3820002'
         
  Доступные переменные для профилей:
  
 - translation_profile_name: 'xxxx'
 - translation_direction: 'callind\called'
 - translation_rule_number: 'xxxx'
 
 #### Фйал translation_rules.yaml
 
 В файле может быть перечилено необходимое количество правил. Каждый отдельный должен начинаться с дефиса, например
 
       - translation_rule_number: '3000001'
         rule_number: '1'
         rule_pattern: '/^\(...\)\(...\)\(....\)/ /\1\3/'
         
  Доступные переменные для правил:
  
 - translation_rule_number: 'xxxx'
 - rule_number: 'x'
 - rule_pattern: 'xxxx'