<h2> Перехід між системами координа </h2>

__1.1 Двовимірний простір: Декартова та полярна системи координат
- Задати координати декількох точок у полярній системі координат.
- Перевести ці координати в декартову систему координат.
- Здійснити зворотний перехід з декартової системи координат в полярну.
- Перевірити коректність розрахунків, упевнившись, що вихідні координати співпадають з отриманими після зворотного перетворення.__

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

__1.2. Тривимірний простір: Декартова та сферична системи координат

- Задати координати декількох точок у сферичній системі координат.
- Перевести ці координати в декартову систему координат.
- Здійснити зворотний перехід з декартової системи координат в сферичну.
- Перевірити коректність розрахунків, упевнившись, що вихідні координати співпадають з отриманими після зворотного перетворення.__



<p align="center">
  <img src="Screenshots/2.jpg" alt="Описание изображения 2"/>
</p>
<p align="center">
    Результат задачі 1.2
</p>

<p align="center">
  <img src="Screenshots/3.jpg" alt="Описание изображения 3"/>
</p>
<p align="center">
    Результат задачі 2
</p>

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
