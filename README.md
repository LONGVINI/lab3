<h2> Перехід між системами координа </h2>

<strong>1.1 Двовимірний простір: Декартова та полярна системи координат
- Задати координати декількох точок у полярній системі координат.
- Перевести ці координати в декартову систему координат.
- Здійснити зворотний перехід з декартової системи координат в полярну.
- Перевірити коректність розрахунків, упевнившись, що вихідні координати співпадають з отриманими після зворотного перетворення.</strong>

Для того, щоб перевести з одної системи координат у іншу, використовують формулу 
з полярної в декартову:
<p align="center">x = r * cos(ϴ)</p>
<p align="center">y = r * sin(ϴ)</p>
з декартової в полярну:
<p align="center">r = √(x^2 + y^2)</p>
<p align="center">ϴ = arctan(y/x)</p>

Нижче написаний код, який реалізує генерацію випадкових полярних координат, зокрема радіусу і кута. Ці координати перетворюються в декартові (x, y) і зберігаються у двох масивах: один для полярних, інший для декартових координат. Після цього код перетворює декартові координати назад у полярні, обчислюючи нові значення радіусу і кута. Він порівнює отримані полярні координати з вихідними, перевіряючи точність перетворень. В кінці код виводить на екран інформацію про вихідні полярні координати, обчислені полярні координати, декартові координати та результати перевірки відповідності.

``` csharp
using System;

class Program
{
    static void Main()
    {
        Random random = new Random();

        Console.WriteLine("Enter the number of points:");
        int n = int.Parse(Console.ReadLine());

        double[,] cartesianCoords = new double[n, 2]; // x и y
        double[,] polarCoords = new double[n, 2]; // радиус и угол

        //Генерируем случайные числа
        for (int i = 0; i < n; i++)
        {
            double radius = random.NextDouble() * 10; // Радіус від 0 до 10
            double angleInDegrees = random.NextDouble() * 360; // Кут від 0 до 360 градусів
            double angleInRadians = angleInDegrees * (Math.PI / 180); // Переведення в радіани

            //Перевод в декартовую
            double x = radius * Math.Cos(angleInRadians);
            double y = radius * Math.Sin(angleInRadians);

            //Сохранение в массиве
            polarCoords[i, 0] = radius;
            polarCoords[i, 1] = angleInDegrees;
            cartesianCoords[i, 0] = x;
            cartesianCoords[i, 1] = y;
        }

        // Цикл перевода из декартовой в полярную и обратно
        for (int i = 0; i < n; i++)
        {
            double radius = Math.Sqrt(cartesianCoords[i, 0] * cartesianCoords[i, 0] + cartesianCoords[i, 1] * cartesianCoords[i, 1]);
            double angle = Math.Atan(cartesianCoords[i, 1] / cartesianCoords[i, 0]) * (180 / Math.PI);

            if (cartesianCoords[i, 0] < 0)
            {
                angle += 180;
            }
            else if (cartesianCoords[i, 0] == 0 && cartesianCoords[i, 1] < 0)
            {
                angle -= 180;
            }

            if (angle < 0) {  angle += 360; }

                // Проверка на правильность перевода
                bool radiusMatch = Math.Abs(radius - polarCoords[i, 0]) < 1e-2;
            bool angleMatch = Math.Abs(angle - polarCoords[i, 1]) < 1e-2;

            Console.WriteLine($"Original polar coordinates: (r = {polarCoords[i, 0]:F2}, θ = {polarCoords[i, 1]:F2} degrees)");
            Console.WriteLine($"Calculated polar coordinates: (r = {radius:F2}, θ = {angle:F2} degrees)");
            Console.WriteLine($"Cartesian coordinates: (x = {cartesianCoords[i, 0]:F2}, y = {cartesianCoords[i, 1]:F2})");
            Console.WriteLine($"Radius match: {radiusMatch}");
            Console.WriteLine($"Angle match: {angleMatch}");
            Console.WriteLine();
        }
    }
}
```

<p align="center">
  <img src="Screenshots/1.jpg" alt="Описание изображения 1"/>
</p>
<p align="center">
    Результат задачі 1.1 
</p>

<br><br><br>

<strong>1.2. Тривимірний простір: Декартова та сферична системи координат

- Задати координати декількох точок у сферичній системі координат.
- Перевести ці координати в декартову систему координат.
- Здійснити зворотний перехід з декартової системи координат в сферичну.
- Перевірити коректність розрахунків, упевнившись, що вихідні координати співпадають з отриманими після зворотного перетворення.</strong>

Для того, щоб переходити у тривимірному просторі переходити між декартовою та полярною системами координат, використовують наступні формули:
- зі сферичної в декартову:
<p align="center">x = r * sin(ф) * cos(ϴ)</p>
<p align="center">y = r * sin(ф) * sin(ϴ)</p>
<p align="center">z = r * cos(ф)</p>
- з декартової в сферичну
<p align="center">r = √(x^2 + y^2 + z^2)</p>
<p align="center">ϴ = arctan(y/x)</p>
<p align="center">ф = arctan(z/r)</p>

Нижче приведений код запитує у користувача кількість точок та створює масиви для сферичних і декартових координат. Він випадковим чином генерує сферичні координати (радіус, азимут і полярний кут), перетворює їх у декартові координати (x, y, z) і зберігає у масивах. Потім код перетворює декартові координати назад у сферичні, обчислює радіус, азимут і полярний кут, і порівнює ці значення з вихідними. Нарешті, він виводить на екран вихідні та обчислені сферичні координати, декартові координати і результати перевірки точності перетворень.

```csharp
using System;

class Program
{
    static void Main()
    {
        Random random = new Random();
        Console.WriteLine("Enter the number of points:");
        int n = int.Parse(Console.ReadLine());

        double[,] sphericalCoords = new double[n, 3]; // r, ϴ, ф
        double[,] cartesianCoords = new double[n, 3]; // x, y, z

        // Генерируем случайные числа
        for (int i = 0; i < n; i++)
        {
            double radius = random.NextDouble() * 10; //расстояние
            double thetaInDegrees = random.NextDouble() * 360; //азимут
            double phiInDegrees = random.NextDouble() * 180; //полярный круг

            //перевод в радианы
            double thetaInRadians = thetaInDegrees * (Math.PI / 180);
            double phiInRadians = phiInDegrees * (Math.PI / 180);

            //перевод в декартовую
            double x = radius * Math.Sin(phiInRadians) * Math.Cos(thetaInRadians);
            double y = radius * Math.Sin(phiInRadians) * Math.Sin(thetaInRadians);
            double z = radius * Math.Cos(phiInRadians);

            sphericalCoords[i, 0] = radius;
            sphericalCoords[i, 1] = thetaInDegrees;
            sphericalCoords[i, 2] = phiInDegrees;
            cartesianCoords[i, 0] = x;
            cartesianCoords[i, 1] = y;
            cartesianCoords[i, 2] = z;
        }

        //Из декартовой в сферическую и проверка
        for (int i = 0; i < n; i++)
        {
            double radius = Math.Sqrt(cartesianCoords[i, 0] * cartesianCoords[i, 0] + cartesianCoords[i, 1] * cartesianCoords[i, 1] + cartesianCoords[i, 2] * cartesianCoords[i, 2]);

            //азимут
            double theta = Math.Atan(cartesianCoords[i, 1] / cartesianCoords[i, 0]) * (180 / Math.PI);
            if (cartesianCoords[i, 0] < 0)
            {
                theta += 180;
            }
            else if (cartesianCoords[i, 0] == 0 && cartesianCoords[i, 1] < 0)
            {
                theta -= 180;
            }

            //полярный круг
            double phi = Math.Acos(cartesianCoords[i, 2] / radius) * (180 / Math.PI);

            //коррекция угла азимута
            if (theta < 0) theta += 360;

            // Проверка
            bool radiusMatch = Math.Abs(radius - sphericalCoords[i, 0]) < 1e-2;
            bool thetaMatch = Math.Abs(theta - sphericalCoords[i, 1]) < 1e-2;
            bool phiMatch = Math.Abs(phi - sphericalCoords[i, 2]) < 1e-2;

            Console.WriteLine($"Original spherical coordinates: (r = {sphericalCoords[i, 0]:F2}, O = {sphericalCoords[i, 1]:F2} degrees, ф = {sphericalCoords[i, 2]:F2} degrees)");
            Console.WriteLine($"Calculated spherical coordinates: (r = {radius:F2}, O = {theta:F2} degrees, ф = {phi:F2} degrees)");
            Console.WriteLine($"Cartesian coordinates: (x = {cartesianCoords[i, 0]:F2}, y = {cartesianCoords[i, 1]:F2}, z = {cartesianCoords[i, 2]:F2})");
            Console.WriteLine($"Radius match: {radiusMatch}");
            Console.WriteLine($"Theta match: {thetaMatch}");
            Console.WriteLine($"Phi match: {phiMatch}");
            Console.WriteLine();
        }
    }
}

```

<p align="center">
  <img src="Screenshots/2.jpg" alt="Описание изображения 2"/>
</p>
<p align="center">
    Результат задачі 1.2
</p>

<br><br><br>

<strong>2. Розрахунок відстаней у сферичній системі координат:

- Виконати обчислення відстані між точками у сферичній системі координат двома способами:
- Декартова система координат: Використати стандартну формулу для обчислення прямої відстані у двовимірному та тривимірному просторі.
- Полярна система координат: Використати формулу для обчислення відстані між точками у двовимірному просторі.
- Сферична система координат: Виконати обчислення відстані між точками двома способами:
- Через об'єм сфери: використати формулу для прямої відстані у тривимірному просторі.
- По поверхні сфери: використати формулу для великої колової відстані.</strong>

Для розрахунку відстаней використовують наступні формули:
Пряма відстань між двома точками у декартовій системі координат:
- двовимірний простір: 
<p align="center"">
 d = √((x<sub>2</sub> - x<sub>1</sub>)<sup>2</sup> + 
       (y<sub>2</sub> - y<sub>1</sub>)<sup>2</sup>)
</p>
- тривимірний простір: 
<p align="center"">
  d = √((x<sub>2</sub> - x<sub>1</sub>)<sup>2</sup> +
       (y<sub>2</sub> - y<sub>1</sub>)<sup>2</sup> +
       (z<sub>2</sub> - z<sub>1</sub>)<sup>2</sup>)
</p>

```csharp
using System;

class Program
{
    static void Main()
    {
        Random random = new Random();
        Console.WriteLine("Enter the number of points:");
        int n = int.Parse(Console.ReadLine());

        double[,] cartesianCoords = new double[n, 3]; // X, Y, Z
        double[,] sphericalCoords = new double[n, 3]; // r, theta, phi

        for (int i = 0; i < n; i++)
        {
            double x = random.NextDouble() * 20 - 10; // Від -10 до 10
            double y = random.NextDouble() * 20 - 10; // Від -10 до 10
            double z = random.NextDouble() * 20 - 10; // Від -10 до 10

            //в сферичную
            double radius = Math.Sqrt(x * x + y * y + z * z);
            double theta = Math.Atan2(y, x); // Азимут
            double phi = Math.Acos(z / radius); // Полярний кут

            cartesianCoords[i, 0] = x;
            cartesianCoords[i, 1] = y;
            cartesianCoords[i, 2] = z;
            sphericalCoords[i, 0] = radius;
            sphericalCoords[i, 1] = theta;
            sphericalCoords[i, 2] = phi;
        }

        //расчёт расстояния
        for (int i = 0; i < n; i++)
        {
            int j = (i + 1) % n; // следующая точка для цикла

            // декартовая
            double distance3D = Math.Sqrt(
                Math.Pow(cartesianCoords[j, 0] - cartesianCoords[i, 0], 2) +
                Math.Pow(cartesianCoords[j, 1] - cartesianCoords[i, 1], 2) +
                Math.Pow(cartesianCoords[j, 2] - cartesianCoords[i, 2], 2));

            //перевод в полярную
            double x1 = cartesianCoords[i, 0];
            double y1 = cartesianCoords[i, 1];
            double x2 = cartesianCoords[j, 0];
            double y2 = cartesianCoords[j, 1];
            double radius1 = Math.Sqrt(x1 * x1 + y1 * y1);
            double radius2 = Math.Sqrt(x2 * x2 + y2 * y2);
            double angle1 = Math.Atan2(y1, x1);
            double angle2 = Math.Atan2(y2, x2);
            double distancePolar = Math.Sqrt(Math.Pow(radius2 * Math.Cos(angle2) - radius1 * Math.Cos(angle1), 2) +
                                             Math.Pow(radius2 * Math.Sin(angle2) - radius1 * Math.Sin(angle1), 2));

            //перевод из декартовой в сферическую
            double r1 = sphericalCoords[i, 0];
            double theta1 = sphericalCoords[i, 1];
            double phi1 = sphericalCoords[i, 2];
            double r2 = sphericalCoords[j, 0];
            double theta2 = sphericalCoords[j, 1];
            double phi2 = sphericalCoords[j, 2];

            //рассчёт по объёму сферы
            double distance3D_sphere = Math.Sqrt(
                Math.Pow(r1, 2) + Math.Pow(r2, 2) -
                2 * r1 * r2 * (Math.Sin(phi1) * Math.Sin(phi2) * Math.Cos(theta2 - theta1) +
                                Math.Cos(phi1) * Math.Cos(phi2)));

            //расчёт по поверхности сферы
            double radiusSphere = 10;
            double surfaceDistance = radiusSphere * Math.Acos(
                Math.Sin(phi1) * Math.Sin(phi2) * Math.Cos(theta2 - theta1) +
                Math.Cos(phi1) * Math.Cos(phi2));

            Console.WriteLine($"Point 1: (x = {cartesianCoords[i, 0]:F2}, y = {cartesianCoords[i, 1]:F2}, z = {cartesianCoords[i, 2]:F2})");
            Console.WriteLine($"Point 2: (x = {cartesianCoords[j, 0]:F2}, y = {cartesianCoords[j, 1]:F2}, z = {cartesianCoords[j, 2]:F2})");
            Console.WriteLine($"Distance in Cartesian coordinates: {distance3D:F2}");
            Console.WriteLine($"Distance in Polar coordinates: {distancePolar:F2}");
            Console.WriteLine($"Distance in spherical coordinates (volume): {distance3D_sphere:F2}");
            Console.WriteLine($"Distance in spherical coordinates (surface): {surfaceDistance:F2}");
            Console.WriteLine();
        }
    }
}
```

<p align="center">
  <img src="Screenshots/3.jpg" alt="Описание изображения 3"/>
</p>
<p align="center">
    Результат задачі 2
</p>



<br><br><br>

<strong>3. Бенчмарки продуктивності:

- Згенерувати масив координат пар точок у кожній системі координат (декартова, полярна, сферична).
- Виконати розрахунок відстаней між цими точками для кожної системи координат.
- Виміряти тривалість обчислень для кожної системи координат.
- Обрати такий розмір масиву, за якого результат бенчмаркінгу матиме незначну варіативність від запуску до запуску (рекомендовано розмір масиву 10,000 - 100,000 точок).</strong>


```csharp

using System;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        int numPoints = 10000; //количество точек для теста

        Random random = new Random();

        double[,] cartesianCoords = new double[numPoints, 3]; // X, Y, Z

        for (int i = 0; i < numPoints; i++)
        {
            cartesianCoords[i, 0] = random.NextDouble() * 20 - 10; // X
            cartesianCoords[i, 1] = random.NextDouble() * 20 - 10; // Y
            cartesianCoords[i, 2] = random.NextDouble() * 20 - 10; // Z
        }

        //измерение времени для расчета расстояний в декартовой системе координат
        Stopwatch stopwatch = Stopwatch.StartNew();
        double[,] distancesCartesian = new double[numPoints, numPoints];

        for (int i = 0; i < numPoints; i++)
        {
            for (int j = 0; j < numPoints; j++)
            {
                if (i != j)
                {
                    distancesCartesian[i, j] = Math.Sqrt(
                        Math.Pow(cartesianCoords[j, 0] - cartesianCoords[i, 0], 2) +
                        Math.Pow(cartesianCoords[j, 1] - cartesianCoords[i, 1], 2) +
                        Math.Pow(cartesianCoords[j, 2] - cartesianCoords[i, 2], 2));
                }
            }
        }

        stopwatch.Stop();
        Console.WriteLine($"Time for Cartesian coordinates: {stopwatch.ElapsedMilliseconds} ms");

        //перевод из декартовой в полярную
        double[,] polarCoords = new double[numPoints, 2];

        for (int i = 0; i < numPoints; i++)
        {
            double radius = Math.Sqrt(Math.Pow(cartesianCoords[i, 0], 2) + Math.Pow(cartesianCoords[i, 1], 2));
            double angle = Math.Atan2(cartesianCoords[i, 1], cartesianCoords[i, 0]) * (180 / Math.PI);
            polarCoords[i, 0] = radius;
            polarCoords[i, 1] = angle;
        }

        //измерение времени для расчета расстояний в полярной системе координат
        stopwatch.Restart();
        double[,] distancesPolar = new double[numPoints, numPoints];

        for (int i = 0; i < numPoints; i++)
        {
            for (int j = 0; j < numPoints; j++)
            {
                if (i != j)
                {
                    double radius1 = polarCoords[i, 0];
                    double radius2 = polarCoords[j, 0];
                    double angle1 = polarCoords[i, 1] * (Math.PI / 180);
                    double angle2 = polarCoords[j, 1] * (Math.PI / 180);

                    distancesPolar[i, j] = Math.Sqrt(
                        Math.Pow(radius2 * Math.Cos(angle2) - radius1 * Math.Cos(angle1), 2) +
                        Math.Pow(radius2 * Math.Sin(angle2) - radius1 * Math.Sin(angle1), 2));
                }
            }
        }

        stopwatch.Stop();
        Console.WriteLine($"Time for Polar coordinates: {stopwatch.ElapsedMilliseconds} ms");

        //перевод декартововой в сферическую
        double[,] sphericalCoords = new double[numPoints, 3];

        for (int i = 0; i < numPoints; i++)
        {
            double radius = Math.Sqrt(cartesianCoords[i, 0] * cartesianCoords[i, 0] +
                                      cartesianCoords[i, 1] * cartesianCoords[i, 1] +
                                      cartesianCoords[i, 2] * cartesianCoords[i, 2]);
            double theta = Math.Atan2(cartesianCoords[i, 1], cartesianCoords[i, 0]);
            double phi = Math.Acos(cartesianCoords[i, 2] / radius);
            sphericalCoords[i, 0] = radius;
            sphericalCoords[i, 1] = theta;
            sphericalCoords[i, 2] = phi;
        }

        // Измерение времени для расчета расстояний по объему сферы
        stopwatch.Restart();
        double[,] distancesVolume = new double[numPoints, numPoints];

        for (int i = 0; i < numPoints; i++)
        {
            for (int j = 0; j < numPoints; j++)
            {
                if (i != j)
                {
                    double r1 = sphericalCoords[i, 0];
                    double theta1 = sphericalCoords[i, 1];
                    double phi1 = sphericalCoords[i, 2];
                    double r2 = sphericalCoords[j, 0];
                    double theta2 = sphericalCoords[j, 1];
                    double phi2 = sphericalCoords[j, 2];

                    double distanceVolume = Math.Sqrt(
                        Math.Pow(r1, 2) + Math.Pow(r2, 2) -
                        2 * r1 * r2 * (Math.Sin(phi1) * Math.Sin(phi2) * Math.Cos(theta2 - theta1) +
                                        Math.Cos(phi1) * Math.Cos(phi2)));

                    distancesVolume[i, j] = distanceVolume;
                }
            }
        }

        stopwatch.Stop();
        Console.WriteLine($"Time for volume distance calculation: {stopwatch.ElapsedMilliseconds} ms");

        //измерение времени для расчета расстояний по поверхности сферы
        stopwatch.Restart();
        double[,] distancesSurface = new double[numPoints, numPoints];

        for (int i = 0; i < numPoints; i++)
        {
            for (int j = 0; j < numPoints; j++)
            {
                if (i != j)
                {
                    double r1 = sphericalCoords[i, 0];
                    double theta1 = sphericalCoords[i, 1];
                    double phi1 = sphericalCoords[i, 2];
                    double r2 = sphericalCoords[j, 0];
                    double theta2 = sphericalCoords[j, 1];
                    double phi2 = sphericalCoords[j, 2];

                    double surfaceDistance = r1 * Math.Acos(
                        Math.Sin(phi1) * Math.Sin(phi2) * Math.Cos(theta2 - theta1) +
                        Math.Cos(phi1) * Math.Cos(phi2));

                    distancesSurface[i, j] = surfaceDistance;
                }
            }
        }

        stopwatch.Stop();
        Console.WriteLine($"Time for surface distance calculation: {stopwatch.ElapsedMilliseconds} ms");
    }
}

```

<p align="center">
  <img src="Screenshots/4.jpg" alt="Описание изображения 4"/>
</p>
<p align="center">
  <img src="Screenshots/5.jpg" alt="Описание изображения 5"/>
</p>
<p align="center">
  <img src="Screenshots/6.jpg" alt="Описание изображения 6"/>
</p>
<p align="center">
    Результати задачі 3 з різним часов виконання 
</p>
