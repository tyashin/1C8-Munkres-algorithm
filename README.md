## Конфигурация: «Упрощённый проектный менеджмент» для 1С 8.3+

**(использование [Венгерского алгоритма](https://ru.wikipedia.org/wiki/%D0%92%D0%B5%D0%BD%D0%B3%D0%B5%D1%80%D1%81%D0%BA%D0%B8%D0%B9_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC) (алгоритма Куна-Манкреса) для решения задачи распределения работ по исполнителям)**

#### Описание задачи.

Команда состоит из менеджера и программистов. Перед командой стоит некоторый набор задач. Нужно распределить задачи между программистами.

Критерии распределения
При распределении следует учитывать предпочтения двух сторон - менеджера, который предпочитает, чтобы задачу решал тот или иной программист - программиста, который предпочитает, считает для себя более интересной ту или иную задачу Например, менеджер считает, что задачу "Оптимизация регламентных операций закрытия месяца" Вася решит
эффективнее, чем Петя; а Васе интереснее задача "Учет подарочных сертификатов в автоматизированных розничных магазинах", чем другие задачи. В результате распределения не должно быть таких двух пар "задача-программист", в отношении которых менеджер и программисты могут сказать следующее: Менеджер: "Васе досталась задача про Корректировочные счета-фактуры. Но Петя решит ее лучше". Программист Петя: "Хорошо, мне задача про Корректировочные счета-фактуры интереснее, чем та, что досталась мне"

Ограничения
Количество задач и количество программистов равны.


#### Обоснование выбора алгоритма 
Венгерский алгоритм — классический способ решения задачи о назначениях, позволяющий находить решение за время O(n3), в то время как полный перебор  — за время O(n!).

#### Особенности реализации:  
За основу взята реализация на C#, описанная здесь: http://csclab.murraystate.edu/~bob.pilgrim/445/munkres.html В ходе портирования на платформу "1С 8" выполнены незначительные изменения в коде, связанные с переводом имён переменных на русский, а также с отличиями одного языка программирования от другого и c предметно-ориентированной архитектурой платформы 1С.

#### Описание объектов конфигурации

1. Справочник «Сотрудники» содержит список программистов и менеджеров, участвующих в проектах. 
2. Справочник «Проектные роли» выполняет вспомогательную функцию отнесения сотрудников к той или иной категории. Содержит два предопределённых элемента «Программист» и «Менеджер проекта». 
3. Справочник «Проекты и задачи» - иерархический справочник (глубина иерархии = 2). Группы справочника представляют проекты, элементы справочника — задачи в проектах. 
4. Документ «Распределение задач по программистам» (алгоритм распределения расположен в модуле объекта). При создании нового документа следует заполнить поля «проект» и «менеджер» в шапке. Затем следует нажать кнопку «Заполнить таблицу». В результате табличная часть будет заполнена задачами и программистами. Табличная часть на форме отображается в виде дерева, в узлах которого расположены задачи, а в ветвях списки программистов, которым эта задача может быть назначена. Поскольку в условии задачи сказано, что количество задач и программистов должно быть равно, при заполнении документа «лишние» задачи или программисты будут отброшены (т. е. будет вычислен минимум(количество задач, количество программистов) и это количество задач попадёт в ТЧ документа). После заполнения таблицы можно (но не обязательно) заполнить предпочтения программистов и менеджеров для каждой задачи. Используется следующая шкала приоритетов: от 0 до 9, где 0 — низший, а 9 — наивысший приоритет. Если приоритет не заполнен, он считается равным 0. Приоритет менеджера (т. е. менеджер желает, в той или иной степени, чтобы ту или иную задачу решил тот или иной программист)  и приоритет программиста имеют одинаковый вес при распределении задач (суммарный приоритет каждой задачи для каждого программиста находится в диапазоне 0..18). Также обратите внимание, что приоритеты можно заполнить вручную только для ветвей дерева, а для узлов дерева приоритеты будут заполнены автоматически при распределении задач. 
Поля дерева, доступные для редактирования, имеют зеленоватый фон.
Для выполнения распределения следует нажать кнопку «Распределить задачи на программистов». О том, что задачи распределены, можно судить по появлению флажков в одноимённой колонке, а также по заполненности полей «Программист», «Приоритет программиста» и «Приоритет менеджера» в каждом из узлов дерева.