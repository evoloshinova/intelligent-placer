# intelligent-placer
## Требования
* Фигура
  * Фигура задается рисунком маркером на белом на листе формата А4.
  * Лист должен быть повернут горизонтально, стороны отклоняются не более чем на 5 градусов от границ кадра.
  * Предметы на фотографии не должны пересекаться с листом.
  * Фигура --- выпуклый многоугольник с не более чем 10-ю вершинами.
  * Фон светлый, не обязательно белый
* Предметы
  * Предметы на фотографии не должны пересекаться друг с другом и с листом, на котором задана фигура.
  * Предметы не должны выходить за границы кадра.
  * Предметы должны находиться не менее чем в двух сантиметрах от границы кадра (измерения в естественной плоскости).
  * Предеметы на фотографии не могут повторятся.
* Фотография
  * Фотография делается на высоте от 40 до 60 сантиметров над поверхностью.
  * Фотография делается под углом от 85 до 95 градусов.
  * Освещение равномерное.
  * Фотография делается вертикально.
  * Формат .jpg.
  * Разрешение от 5 Мп.
 ## Описание программы

Для работы используются бибилиотеки numpy, skimage, PyClipper, pandas, scipy, matplotlib.
 
 Все картинки для работы надо перевести в черно-белое представление.
 ### Подготовка шаблонов
 1. Рассматривается только центральный кусок изображения, чтобы царапина на изображении не мешала. Так как изображения сделаны в одном месте, это не не сложно сделать. Окно для всех одинакового размера, совпадает с окном многоугольников
 1. Выделяются контуры предметов алгоритмом Кэнни. 
 4. Строятся выпуклые оболочки каждого предмета по найденным границам, переносятся так, чтобы их левый угол был в точке (0, 0).
 6. Выделяются особые точки с помощью детектора Харриса в небинаризованном изображении, находятся их дескрипторы ORB (для последующего матчинга шаблонов с фотографиями). Зафиксировать для всех изображений один порог
 
 ### Выделение многоугольника
 1. Многоугольники в верху изображения обрезаются (размер окна одинаковый).
 1. Выделяются границы многоугольника алгоритмом Кэнни, замыкаются бинарным замыканием.
 2. Считается количество компонент связности, берется только первая (чтобы убрать шум внизу изображения).
 3. Находятся выпуклые оболочки по границам, многоугольник упрощается методом из pyClipper (чтобы было поменьше точек).
 
 ### Выделение объектов на фотографии
 1. Выделются границы детектором Кэнни, замыкаются бинарным замыканием. Считается количество компонент связности (почти всегда это количество предметов, кроме фотографии с книгой).
 1. Находятся в нижней половине фотографии особые точки предметов с помощью детектора Харриса, считаются их дескрипторы ORB.
 2. Для каждой фотографии считается количество сматченных точек с каждым шаблоном, дальше эти значения сортируются по убыванию количества сматченных точек. Берутся первые k предметв, k --- количество компонент связности.

### Проверка многоугольника
В этой части алгоритма работа ведется с выпуклыми оболочками шаблонов, подсчитанными ранее.

1. Перебираются обнаруженные предметы по очереди в порядке наибольшей вероятности предметов.
2. Считается разница Минковского текущего предмета и текущего многоугольника.
3. Считается разность (как множеств) текущего многоугольника и разницы Минковского
3. Если на пустая --- завершить работу с ответом "нет", 
4. Если не пустая --- переместить шаблон так, чтобы самый левый угол находился в самой левой точке получившейся разности в п. 3, взять разность многоугольника и шаблона за новый многоугольник. 
4. Продолжить с последующими предметами предыдущие четыре шага.
5. Вывести "да".

Была попытка добавить к этому всему поворот фотографий, но получилось что-то странное, что я никак не могу объяснить

## Результаты

Удалось достичь результата 87% верных ответов для белого фона (для серого данных нет), но, кажется, это довольно случайный ответ, потому что выборка маленькая и распознавание предметов работает довольно странно.

Что можно было бы доделать:
-- Сделать больше фотографий
-- Попробовать другие дескрипторы (у меня не получилось по техническим причинам)
-- Понять, что же делает функция can_fit
-- Поработать над границами в сером фоне
-- Вообще уйти от идеи с количеством компонент связности


 ## Предметы
1. Поверхность
 ![Поверхность](/images/photo5321203346188646679.jpg)
2. ![](/images/photo5321203346188646678.jpg)
3. ![](images/photo5321203346188646677.jpg)
4. ![](images/photo5321203346188646676.jpg)
5. ![](images/photo5321203346188646675.jpg)
6. ![](images/photo5321203346188646674.jpg)
7. ![](images/photo5321203346188646673.jpg)
8. ![](images/photo5321203346188646672.jpg)
9. ![](images/photo5321203346188646671.jpg)
10. ![](images/photo5321203346188646670.jpg)
11. ![](images/photo5321203346188646669.jpg)
