# intelligent-placer
## Требования
* Фигура
  * Фигура задается рисунком маркером на белом на листе формата А4.
  * Лист должен быть повернут горизонтально, стороны отклоняются не более чем на 5 градусов от границ кадра.
  * Предметы на фотографии не должны пересекаться с листом.
  * Фигура --- выпуклый многоугольник с не более чем 10-ю вершинами.
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
  
 ## План решения задачи
 ### Подготовка шаблонов
 1. Выделить шаблоны предметов из образцовых фотографий с помощью метода бинаризации Оцу.
 2. Найти особые точки на границе бинаризованных объектов с помощью детектора Харриса. 
 3. Построить выпуклые оболочки каждого предмета по особым точкам (например, обходом Грэхема).
 4. Посчитать диаметр выпуклой оболочки каждого из предметов.
 5. Выделить особые точки с помощью детектора Харриса в небинаризованном изображении, найти их дескрипторы SIFT (для последующего матчинга шаблонов с фотографиями)
 
 ### Выделение многоугольника
 1. Выделить границы многоугольника в верхней половине фотографии методом бинаризации Оцу.
 2. Найти прямые в многоугольнике преобразованием Хафа.
 
 (Все еще остается вопрос --- как перевести многоугольник в координаты).
 
 ### Выделение объектов на фотографии
 1. Найти в нижней половине фотографии особые точки предметов с помощью детектора Харриса, посчитать их дескрипторы SIFT.
 2. Сматчить предметы на фотографии с шаблонами по дескрипторам точек и таким образом определить предметы.

### Проверка многоугольника
В этой части алгоритма работа ведется с выпуклыми оболочками шаблонов, подсчитанными ранее.

1. Отстортировать предметы по диаметру
2. Проверить, можно ли вписать наибольший предмет в многоугольник с помощью суммы по Минковскому
3. Если нет --- завершить работу, если можно --- вырезать дырку с предметом в каком-либо из мест, куда его можно вписать.
4. Продолжить с последующими предметами, но проводить триангуляцию многоугольника, потому что многоугольник может получится невыпуклым.

## Улучшения

Улучшить проверку многоугольника --- в таком виде она работает только без вращения предметов, а с большим количеством предметов скорее всего и довольно долго.
Придумать, как оптимальнее выбирать, где именно разместить наибольший предмет. Вообще эта часть алгоритма требует существенной доработки. Возможно, придется упростить этот алгоритм, если реализация будет слишком сложна.

В плане работы не написано, какие пороги выставлять и как их подбирать --- нужно будет в дальнейшем дополнить.

Попробовать использовать PCA-SIFT или другие дескрипторы вместо SIFT, если будут плохие результаты
Попробовать использовать дргие алгоритмы бинаризации, если будут плохие результаты.

Возможно, придется упростить постановку задачи и задавать многоугольник координатами, а не рисунком.

## Оценка качества

На начальном этапе --- количество правильно сматченных объектов.
Далее, когда распознование будет работать нормально --- количество правильных ответов при работе на размеченном датасете.
      

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
