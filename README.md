Произвести рендеринг трехмерной модели. В процедуре рендеринге предусмотреть реализацию следующих требований:

![alt text](https://github.com/gerrrich/Rendering-3D-model/blob/master/man.jpeg?raw=true)

1. Матрицы перехода между системами координат:
	- Mo2w - матрица перехода между системой координат модели и мировой системой координат . Представить в виде
	произведения трех матриц RTS (вращения, переноса, масштабирования).
		Матрица вращения представлена в виде произведения 3 матриц поворота вокруг базисных осей. Построить матрицу
		таким образом, чтобы был осуществлен поворот вокруг оси Ox на 5 градусов, вокруг  оси Oy на 10 и вокруг оси OZ
		на 15 гардусов.
		Матрица переноса построена на основе векторе (-1, 0, -2)
		Матрица масштабирования осуществляет изменени масштаба в 0.8 раз.
	- Mw2c - матрица перехода между мировой системой координат и системой координат камеры.
		Камера располагается в точке (2,2,2) и смотрит в точку (-2,-2,0)
	- Mproj - матрица проекции (перспектиная/ортографическая)
		Выбрать параметры пирамиды отсечения таким образом, чтобы модель полностью попадала в неё
	- Mviewport - матрица перехода в систему координат области вывода (ViewPort)
		Предусмсотреть учет параметров области вывода (левая нижняя точка, размеры) при задании матрицы. По умолчанию
		область вывода занимает все графическое окно. Размер графического окна 1024x1024
	- При переходе от одной системы координат к другой, матрица перехода умножается на вектор из координат вершины.
	 Для нормалей при переходе используется обратная, транспонировання матрица,
	  которая умножается на вектор координат нормали.

2. Отсечение невидимых и пересекающихся фрагментов модели:
	- Проверка скалярного произведения вектора нормали к грани с вектором направленным от грани к камере
	- Проверка с использованием z-buffer для пересекающихся фрагментов модели

3. Наложение текстуры:
	- Расчет цвета пикселя (попавшего в спроецированную грань) на основе его барицентричесх координат (a,b,c)
	относительно спроецированных вершин граней P0, P1, P2. Барицентрические координаты пикселя используются для
	интерполяции текстурных координат пикселя на основе текстурных координат вершин грани:
	(u,v) = a*(u0,v0) + b*(u1,v1) + c*(u2,v2). 
	Цвет пикселя берется из текстуры (изобржения) по индексам, расчитаным на основе текстурных координат: 
	i = u*w
	j = v*h
	При этом необходимо учесть раположение точки начала отсчета в текстурных координатах.
	- Вычисление барицентрических координат можно осуществить с помощью отношения определителей, составленных из
	 координат вершин грани и координат рассматриваемой точки.
	
4. Освещение по модели Фонга:
	- Модель представлена в виде суммы фонового, диффузного и зеркального освещения. 
	- Фоновое: Ia = ia*ka. ia =(25,25,25); ka=(1,1,1)
	- Диффузное: Id = id*kd*cos(N,L). id =(180,150,190); kd=(0.8,1,0.9)
	- Зеркальное: Is = is*ks*cos(V,R) **alpha. is =(255,255,255); ks=(0.8,0.8,0.8); alpha = 10
	- Источник освещения является точечным и располагается в точка (-3, 3, 3)
	- Затухание освещения в зависимости от расстояния d: 1/ (a + b*d + c*d**2). В качестве коэффициентов
	взять a=1, b=0.02, c=0.004 .
	- Координаты всех составляющих модели освещения должны быть расчитаны в системе координат камеры.
	- Окончательный цвет в пикселе с учетом цвета текстуры и освещения по модели Фонга вычислить как поэлементное
	произведение вектора цвета текстуры на вектор интенсивности освещения поделенного на 255.
	- Нормаль для данного пикселя вычисляется на основе его барицентрических координат относительно вершин грани:
	  (nx,ny,nz) = a*(nx0,ny0,nz0) + b*(nx1,ny1,nz1) + c*(nx2,ny2,nz2). 
	- При превышении значений цвета пикселя величины 255, произвести замену значения на число 255.

5. Получаемые изображения:
	- Рендеринг проволочной модели. Отображаются только проекции ребер модели.
	- Рендеринг модели с гранями. Цвет грани задать в полутоновом виде (серый цвет). Интенсивность цвета определить
	 на основе проверки на видимость грани.
	- Рендеринг модели с текстурой.
	- Рендеринг модели с текстурой и учетом освещения.
