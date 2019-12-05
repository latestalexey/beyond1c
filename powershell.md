

## Комментарии

1C:
```bsl
// это комментарий
```
powershell:
```powershell
# это комментарий
```

## Объявление переменной

1C:
```bsl
Цена = 100;
```
powershell:
```powershell
$price = 100
```

## Объявление процедуры

1C:
```bsl
Процедура Тест()
    Цена = 100;
КонецПроцедуры
```

powershell:
```powershell
function test() {
    $price = 100
}
```

## Переменные уровня модуля

1C:
```bsl
Перем Количество;
Перем Сумма;

Процедура Тест()
    Цена = 100;
    Сумма = Цена * Количество;
КонецПроцедуры

Количество = 5;
Сумма = 0;
```

powershell:
```powershell
$quantity = 5
$amount = 0

function test() {
    $price = 100
    $script:amount = $price * $quantity
}
```

## Объявление функции

1C:
```bsl
Функция Сумма(Знач Цена, Знач Количество = 1)
    Возврат Цена * Количество;
КонецФункции
```

powershell:
```powershell
function amount($price, $quantity=1) {
    return $price * $quantity
}

# альтернативный вариант: параметр price можно передавать "через зад" (через конвейер)
function amount([Parameter(ValueFromPipeline)]$price, $quantity=1) {
    return $price * $quantity
}
```

## Вызов функции

1C:
```bsl
Сумма = Сумма(10)
```

powershell:
```powershell
$amount = amount 10

# альтернативный вариант: передача первого параметра "через зад"
$amount = 10 | amount

# альтернативный вариант: передача первого параметра "через зад" и второго по имени
$amount = 10 | amount -quantity 2
```

## Создание массива

1C:
```bsl
Имена = Новый Массив;
Имена.Добавить("Саша");
Имена.Добавить("Маша");
```

powershell:
```powershell
$names = @()
$names += "Саша" # очень медленное добавление элемента
$names += "Маша"

# альтернативный вариант:
$names = @("Саша", "Маша")

# альтернативный вариант:
$names = "Саша", "Маша"

# альтернативный вариант:
[Collections.Generic.List[Object]]$names = @() # эффективный динамический массив
$names.Add("Саша")
$names.Add("Маша")
```

## Цикл по массиву

1C:
```bsl
Для Каждого Имя Из Имена Цикл
    Сообщить(Имя);
КонецЦикла;
```

powershell:
```powershell
foreach ($name in $names) {
    Write-Host $name
}
```

## Проверка наличия значения в массиве

1C:
```bsl
Если Имена.Найти("Катя") <> Неопределено Тогда
    Сообщить("Катя присутствует!");
КонецЕсли;
```

powershell:
```powershell
if ($names.Contains("Катя")) {
    Write-Host "Катя присутствует!"
}
```

## Поиск значения в массиве

1C:
```bsl
Индекс = Имена.Найти("Катя");
Если Индекс <> Неопределено Тогда
    Сообщить(Индекс);
КонецЕсли;
```

powershell:
```powershell
$index = $names.IndexOf("Катя")
if ($index -ne -1) {
    Write-Host $index
}
```

## Создание соответствия

1C:
```bsl
Возраст = Новый Соответствие;
Возраст["Саша"] = 18;
Возраст["Маша"] = 19;
```

powershell:
```powershell
$age = @{}
$age["Саша"] = 18
$age["Маша"] = 19

-- альтернативный вариант:
$age = @{
    "Саша" = 18;
    "Маша" = 19;
}

-- альтернативный вариант:
$age = @{
    "Саша" = 18
    "Маша" = 19
}

-- альтернативный вариант:
$age = @{
    Саша = 18
    Маша = 19
}
```

## Цикл по соответствию

1C:
```bsl
Для Каждого Элемент Из Возраст Цикл
    Сообщить(Элемент.Ключ + ": " + Строка(Элемент.Значение));
КонецЦикла;
```

powershell:
```powershell
foreach ($key in $age.Keys) {
    Write-Host ($x + ": " + $age[$key])
}
```

## Обращение к соответствию по ключу

1C:
```bsl
ВозрастКати = Возвраст["Катя"];
Если ВозрастКати <> Неопределено Тогда
    Сообщить(ВозрастКати);
КонецЕсли;
```

powershell:
```powershell
$kate_age = $age["Катя"]
if ($kate_age) {
    Write-Host $kate_age
}

-- альтернативный вариант
$kate_age = $age["Катя"]
if ($null -ne $kate_age) {
    Write-Host $kate_age
}
```

## Проверка наличия ключа в соответствии

1C:
```bsl
Если Возраст["Катя"] <> Неопределено Тогда
    Сообщить(Возраст["Катя"]);
КонецЕсли;
```

powershell:
```powershell
if ($age.ContainsKey("Катя")) {
    Write-Host $age["Катя"]
}
```

## Цикл по диапазону чисел

1C:
```bsl
Для Индекс = 1 По 10 Цикл
    Сообщить(Индекс);
КонецЦикла;
```

powershell:
```powershell
for ($i = 1; $i -le 10; $i++) {
    Write-Host $i
}
```

## Цикл с условием

1C:
```bsl
Индекс = 0;
Пока Индекс < 10 Цикл
    Сообщить(Индекс);
    Индекс = Индекс + 1;
КонецЦикла;
```

powershell:
```powershell
$i = 0
while ($i -le 10) {
    Write-Host $i
    $i++
}
```

## Создание структуры

1C:
```bsl
Сотрудник = Новый Структура("Имя, Возраст", "Катя", 19);
Сообщить(Сотрудник.Имя);
```

powershell:
```powershell
$employee = @{
    name = "Катя"
    age = 19
}
Write-Host employee.name

-- альтернативный вариант
$employee = @{}
$employee.name = "Катя"
$employee.age = 19
Write-Host $employee.name
```

## Добавление поля в структуру

1C:
```bsl
Сотрудник.Вставить("Пол", "Мужской");
Сообщить(Сотрудник.Пол);
```

powershell:
```powershell
$employee.gender = "male"
Write-Host $employee.gender

-- альтернативный вариант
$employee["gender"] = "male"
Write-Host $employee.gender
```

## Форматирование строк (шаблоны, интерполяция)

1C:
```bsl
Имя = "Катя";
Возраст = 19;
Сообщить(СтрШаблон("%1: %2", Имя, Возраст));
```

powershell:
```powershell
$name = "Kate"
$age = 19
Write-Host "${name}: $age" # без скобок {} будет ошибка синтаксиса из-за знака `:`
```

## Конкатенация массива строк

1C:
```bsl
Сообщить(СтрСоединить(Имена, Символы.ПС));
```

powershell:
```powershell
Write-Host ($names -join "`n")
```

## Тернарный оператор

1C:
```bsl
Сумма = Цена * ?(Количество > 0, Количество, 1);
```

powershell:
```powershell
$amount = $price * $(if ($quantity -gt 0) { $quantity } else { 1 })
```

## Условия

1C:
```bsl
Если Икс = 0 Или Икс = Неопределено Тогда
    Сообщить("пусто")
ИначеЕсли 0 < Икс И Икс < 10 Тогда
    Сообщить("от 1 до 9")
Иначе
    Сообщить("меньше 0 или больше 9")
КонецЕсли;
```

powershell:
```powershell
if ($x -eq 0 -or $null -eq $x) {
    Write-Host 'пусто'
} elseif (0 -lt $x -and $x -lt 10) {
    Write-Host 'от 1 до 9'
} else {
    Write-Host 'меньше 0 или больше 9'
}
```

## Чтение текстового файла в кодировке UTF-8

1C:
```bsl
ЧтениеТекста = Новый ЧтениеТекста;
ЧтениеТекста.Открыть("log.txt", "UTF-8");
Сообщить(ЧтениеТекста.Прочитать());
ЧтениеТекста.Закрыть();
```

powershell:
```powershell
Write-Host (Get-Content log.txt -Encoding UTF8) # вызов функции в выражении нужно полностью заключать в скобки

# альтернативный вариант
write (Get-Content log.txt -Encoding UTF8) # write - это синоним для Write-Host

# альтернативный вариант
Get-Content log.txt -Encoding UTF8 | Write-Host # передача первого аргумента для Write-Host "через зад"

# альтернативный вариант
Get-Content log.txt -Encoding UTF8 # результат функций по умолчанию выводится в консоль

# альтернативный вариант
cat log.txt -Encoding UTF8 # cat - это синоним для Get-Content
```
