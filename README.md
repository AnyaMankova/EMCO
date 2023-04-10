# emco
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white) ![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white) ![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=for-the-badge&logo=scipy&logoColor=%white) ![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black)

Представлен датасет, отражающий работу самосвалов за 11 месяцев 2022 года со следующими данными: 
- дата смены
- самосвал
- средний вес в кузове автомобиля БелаЗ 75306 (г.п. 220т) за смену
- пройденное расстояние суммарно за смену
- средняя скорость за смену
- стандартное отклонение перепада высот за смену
- потраченный объем топлива за смену

Поставленные задачи: 
- выявить взаимосвязь технологических параметров (взаимное влияние и влияние на расход топлива);
- дать предложение по диапазонам параметров, при которых расход топлива минимальный (при этом нет ухудшения скорости (снижения), вес в кузове не менее 220 т., ср. перепад высот и пройденное расстояние - без существенных изменений, поскольку параметры нерегулируемые).


<p>Проведен первичный анализ данных (EDA).

Команда df.describe() генерирует описательную статистику для всех числовых столбцов: число непропущенных значений, среднее, стандартное отклонение, минимум, 25 и 75 процентили, медиану и максимум. 
![describe](https://github.com/AnyaMankova/emco/blob/main/images/describe.png)
 Результат: 
- необычные/аномальные данные не обнаружены;
- имеются пропуски данных.

 Проанализируем пропуски данных. Общее количество пропусков равно 5 881.
![пропуски в датасете](https://github.com/AnyaMankova/emco/blob/main/images/nullst.png)

На рисунке по оси x указаны строки датасета, белые линии на нем отражают пропущенные значения.
Общее количество пропусков - примерно 17% от общего количества данных в датасете.
! принято решение на текущем этапе удалить пропуски.
   
Для каждого атрибута данных проведем анализ.
Базовым методом проверки данных на нормальность распределения является гистограмма плотности распределения:
![гистограмма](https://github.com/AnyaMankova/emco/blob/main/images/all_hist.png)
Полученные гистограммы визуально соответсвуют признаку нормального распределения, кроме того, во всех наблюдаемых выборках значения среднего, медианы и моды близки что позволяет предположить нормальное распределение выборок.

Построим boxplot для наших данных:    
![all_boxplot](https://github.com/AnyaMankova/emco/blob/main/images/all_boxplot.png)
Видно, что за пределами +- 1,5 межквартильных размаха у данных есть выбросы, т.е. слишком маленькие или слишком большие значения данных, которые сильно выбиваются из генеральной совокупности данных.   

Посмотрим на корреляцию данных:
![corr](https://github.com/AnyaMankova/emco/blob/main/images/corr.png)
Очевидна взаимосвязь между расходом топлива и пройденным расстоянием, также довольно значительно на расход топлива влияет перепад высот. Другие признаки взаимосвязи выражены очень слабо, что может указывать на нелинейную связь между показателями.

Проверим некоторые гипотезы относительно средних значений: 
    1) Н0 - не существует различий в расходе топлива в зависимости от смены 
       Н1 - существуют различия в расходе топлива в зависимости от смены 
    2) Н0 - не существует различий в расходе топлива в зависимости от времени года 
       Н1 - существуют различия в расходе топлива в зависимости от времени года 
    3) Н0 - не существует различий в расходе топлива в зависимости от самосвала 
       Н1 - существуют различия в расходе топлива в зависимости от самосвала
    Проверим данные гипотезы с помощью построения графиков, проверки распределений на нормальность, а также используя t-критерий Стьюдента. Принимаем уровень значимости (p-значение равным 0,05 или 5%).
    
Гипотеза 1 - смены.
    Проверим как распределены средние наших выборок:
    ![shift_box](https://github.com/AnyaMankova/emco/blob/main/images/shift_box.png)
    Средние двух выборок примерно равны и находятся в доверительных интервалах друг друга. Таким образом, мы не можем отвергнуть нулевую гипотезу о том, что не существует различий в расходе топлива в зависимости от смены.
    Кроме того, распределение данных соответствует нормальному:
    ![shift_qq](https://github.com/AnyaMankova/emco/blob/main/images/shift_qq.png)
Итак, принимаем гипотезу о том, что разницы в расходе топлива посменно не существует.    
    
Гипотеза 2 - месяцы.
    Рандомно выбраны месяц 3 и 4. Распределение средних выборок:
    ![month_box](https://github.com/AnyaMankova/emco/blob/main/images/month_box.png)
    Средние двух выборок примерно равны и находятся в доверительных интервалах друг друга. Тем не менее, средний расход топлива в марте выше, чем в апреле. Можно предположить, что в связи с более низкими температурами расход топлива может возрастать, однако разница не критична. Не можем отвергнуть нулевую гипотезу о том, что не существует различий в расходе топлива в зависимости от месяца.
    Распределение данных:
    ![month_qq](https://github.com/AnyaMankova/emco/blob/main/images/month_qq.png)
    Для апреля месяца распределение не похоже на нормальное, что связано с выбросами в данных - несколько точек показывают небольшой расход топлива (менее 1000 л), что сильно отклоняется от среднего этой выборки (1572 л). Тем не менее, данные распределены похожим образом, поэтому принимаем гипотезу о том, что не существует разницы в расходе топлива в зависимости от месяца. 

Гипотеза 3 - самосвалы.
    Рандомно выбраны самосвалы с номерами 1036 и 1499. Распределение средних выборок:
    ![trucks_box](https://github.com/AnyaMankova/emco/blob/main/images/trucks_box.png)
    Средние двух выборок примерно равны и находятся в доверительных интервалах друг друга. Cредний расход топлива для самосвала 1036 выше, чем для 1499. Данные выборок отличаются на 50 л. Не можем отвергнуть нулевую гипотезу о том, что не существует различий в расходе топлива в зависимости от самосвала.
    Распределение данных:
    ![trucks_qq](https://github.com/AnyaMankova/emco/blob/main/images/trucks_qq.png)
    Для обоих выборок данные распределяются похожим образом и соответствует нормальному.Таким образом, мы не отвергаем нулевую гипотезу о том, что не существует разницы в расходе топлива в зависимости от самосвала. Тем не менее, можно провести более подробный анализ данных по всем самосвалам и провести дополнительное ТО для тех, у которых среднее значение превышает среднее по общей выборке (т.е. 1551 л.).

    Для всех сформулированных гипотез p-значение превышает 5%, таким образом, принимаем утверждения о том, что различия в расходе топлива не зависят от смены, конкретного самосвала и месяца работы.
    
    
Сформулируем и проверим гипотезы о различиях в расходе топлива в зависимости от веса груза и скорости движения.
    разброс скоростей - от 15 до 24,98 км/ч, среднее = 19,401 
    разброс весов - от 190 до 249,56 т, среднее = 221,681 
    нам нужно понять, при каких параметрах значение расхода топлива меньше. при этом, нужно, чтобы снижение скорости не было фатальным, а груз был не менее чем 220 т. рассмотрим наши три параметра более подробно:
    ![avg_par](https://github.com/AnyaMankova/emco/blob/main/images/avg_par.png)
    На выборке в 100 значений посмотрим на графики рассеивания: 
    ![avg_par1](https://github.com/AnyaMankova/emco/blob/main/images/avg_par1.png)]
    Не вижу четкой линейной связи между параметрами, тем не менее, попробуем провести регрессионный анализ: показатели скорости и веса в качестве переменных-предикторов (независимые переменные) и расход топлива в качестве переменной ответа.
    Коэффициент детерминации (R-squared) равен 0.001, что означает, что около 0,1% изменчивости расхода топлива могут быть объяснены изменениями в рассматриваемых параметрах. F-статистика имеет значение 0,66, а вероятность (Prob (F-statistic)) больше 0.05, что указывает на НЕ значимость модели.
    В целом модель указывает, на наличие сильной мультиколлинеарности параметров или других численных проблем.
    При этом, при проведении аналогичного анализа данных для взаимодействия между параметрами - расход топлива-расстояние-перепад высот мы видим наличие прямой линейной связи. изменение перепада высот и пройденного расстояния объясняют более 80% изменений параметра Расход топлива.
    
Дальнейшие цели исследования: 
    - выяснить какая связь между параметрами расход топлива-вес, расход топлива-скорость;
    - стандартизировать данные для анализа (сейчас выборки нерелевантны, поскольку в каждой из выборок большой разброс значений по параметрам дистанция и перепад высот);
    - провести анализ взаимосвязи;
    - сделать выводы и дать рекомендации;
    - обработать пропуски данных - найти подходящий способ заполнения.
  </p>
