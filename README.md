# Game-Of-The-Year
Проект по созданию игры года. Лучше чем у Хидэо Кодзимы точно. 


``` csharp
using System;

class Program
{
    static void Main()
    {
        Random random = new Random();

        
        // ДАННЫЕ ИГРОКА
        
        string playerName = "Инженер-изобретатель";

        int playerHp = 120;
        int maxPlayerHp = 120;

        int steam = 60;
        int maxSteam = 60;

        // Количество восстановлений пара за всю игру
        int repairKits = 3;

        Console.Title = "Steampunk: Последний автоматон";

        Console.WriteLine("==========================================");
        Console.WriteLine("       STEAMPUNK: ПОСЛЕДНИЙ АВТОМАТОН");
        Console.WriteLine("==========================================");
        Console.WriteLine();
        Console.WriteLine($"Вы играете за: {playerName}");
        Console.WriteLine("Ваш экзоскелет готов к бою.");
        Console.WriteLine();

        
        // ЦИКЛ FOR — ТРИ ПОСЛЕДОВАТЕЛЬНЫЕ ВОЛНЫ ПРОТИВНИКОВ
        
        for (int wave = 1; wave <= 3; wave++)
        {
            string enemyName;
            int enemyHp;
            int enemyDamage;

            // Характеристики противников увеличиваются с каждой волной
            if (wave == 1)
            {
                enemyName = "Паровой часовой";
                enemyHp = 70;
                enemyDamage = 10;
            }
            else if (wave == 2)
            {
                enemyName = "Заводской автоматон";
                enemyHp = 100;
                enemyDamage = 15;
            }
            else
            {
                enemyName = "Железный голиаф";
                enemyHp = 140;
                enemyDamage = 20;
            }

            Console.WriteLine();
            Console.WriteLine("==========================================");
            Console.WriteLine($"              ВОЛНА {wave}/3");
            Console.WriteLine("==========================================");
            Console.WriteLine($"Противник: {enemyName}");
            Console.WriteLine($"Прочность противника: {enemyHp}");
            Console.WriteLine();

            
            // WHILE — ОСНОВНОЙ ЦИКЛ БОЯ
            
            while (playerHp > 0 && enemyHp > 0)
            {
                bool defending = false;
                int action;
                bool validInput;

                
                // FOR — ОТРИСОВКА ШКАЛ СОСТОЯНИЯ
                
                Console.WriteLine("------------------------------------------");

                Console.Write("Экзоскелет [");

                int playerBars = playerHp / 10;

                for (int i = 0; i < playerBars; i++)
                {
                    Console.Write("#");
                }

                for (int i = playerBars; i < maxPlayerHp / 10; i++)
                {
                    Console.Write("-");
                }

                Console.WriteLine($"] {playerHp}/{maxPlayerHp}");

                Console.Write("Давление пара [");

                int steamBars = steam / 5;

                for (int i = 0; i < steamBars; i++)
                {
                    Console.Write("#");
                }

                for (int i = steamBars; i < maxSteam / 5; i++)
                {
                    Console.Write("-");
                }

                Console.WriteLine($"] {steam}/{maxSteam}");

                Console.Write($"{enemyName} [");

                int enemyBars = enemyHp / 10;

                for (int i = 0; i < enemyBars; i++)
                {
                    Console.Write("#");
                }

                for (int i = enemyBars; i < 14; i++)
                {
                    Console.Write("-");
                }

                Console.WriteLine($"] {enemyHp} HP");

                Console.WriteLine("------------------------------------------");

                
                // DO-WHILE — ПРОВЕРКА ВВОДА
                
                do
                {
                    Console.WriteLine();
                    Console.WriteLine("Выберите действие:");
                    Console.WriteLine("1 — Удар механической рукой");
                    Console.WriteLine("2 — Паровой импульс");
                    Console.WriteLine("3 — Защитная стойка");
                    Console.WriteLine($"4 — Восстановить давление пара (осталось: {repairKits})");
                    Console.Write("Ваш выбор: ");

                    validInput = int.TryParse(Console.ReadLine(), out action)
                                 && action >= 1
                                 && action <= 4;

                    if (!validInput)
                    {
                        Console.WriteLine("Ошибка! Введите число от 1 до 4.");
                    }

                } while (!validInput);

                
                // SWITCH — ОБРАБОТКА ДЕЙСТВИЯ ИГРОКА
                
                switch (action)
                {
                    case 1:
                        // Базовая атака
                        int basicDamage = random.Next(12, 19);

                        enemyHp -= basicDamage;

                        Console.WriteLine();
                        Console.WriteLine(
                            $"Вы ударили механической рукой и нанесли {basicDamage} урона!"
                        );

                        break;

                    case 2:
                        // Специальная способность
                        if (steam >= 20)
                        {
                            int steamDamage = random.Next(25, 36);

                            enemyHp -= steamDamage;
                            steam -= 20;

                            Console.WriteLine();
                            Console.WriteLine(
                                $"Вы выпустили мощный паровой импульс!"
                            );
                            Console.WriteLine(
                                $"Нанесено {steamDamage} урона."
                            );
                            Console.WriteLine(
                                $"Потрачено 20 единиц давления."
                            );
                        }
                        else
                        {
                            Console.WriteLine();
                            Console.WriteLine(
                                "Недостаточно давления пара!"
                            );

                            // continue — пропускаем ход игрока
                            continue;
                        }

                        break;

                    case 3:
                        // Защита
                        defending = true;

                        Console.WriteLine();
                        Console.WriteLine(
                            "Вы включили защитную стойку."
                        );
                        Console.WriteLine(
                            "Получаемый урон будет уменьшен вдвое."
                        );

                        break;

                    case 4:
                        // Восстановление ресурса
                        if (repairKits > 0)
                        {
                            steam += 25;

                            if (steam > maxSteam)
                            {
                                steam = maxSteam;
                            }

                            repairKits--;

                            Console.WriteLine();
                            Console.WriteLine(
                                "Вы восстановили давление в котле."
                            );
                            Console.WriteLine(
                                "Восстановлено 25 единиц давления."
                            );
                        }
                        else
                        {
                            Console.WriteLine();
                            Console.WriteLine(
                                "У вас больше нет запасных клапанов!"
                            );

                            // continue — игрок получает возможность
                            // выбрать другое действие
                            continue;
                        }

                        break;
                }

                
                // ПРОВЕРКА ПОБЕДЫ ПОСЛЕ АТАКИ
                
                if (enemyHp <= 0)
                {
                    enemyHp = 0;

                    Console.WriteLine();
                    Console.WriteLine($"*** {enemyName} уничтожен! ***");

                    break;
                }

                
                // ХОД ПРОТИВНИКА
               
                int enemyAttack = random.Next(
                    enemyDamage - 3,
                    enemyDamage + 4
                );

                if (defending)
                {
                    enemyAttack /= 2;

                    if (enemyAttack < 1)
                    {
                        enemyAttack = 1;
                    }

                    Console.WriteLine();
                    Console.WriteLine(
                        $"{enemyName} атакует, но ваша броня поглощает часть удара."
                    );
                }
                else
                {
                    Console.WriteLine();
                    Console.WriteLine(
                        $"{enemyName} наносит удар!"
                    );
                }

                playerHp -= enemyAttack;

                Console.WriteLine(
                    $"Получено урона: {enemyAttack}"
                );

                if (playerHp < 0)
                {
                    playerHp = 0;
                }

                Console.WriteLine(
                    $"Прочность экзоскелета: {playerHp}/{maxPlayerHp}"
                );
            }

            // ПРОВЕРКА ПОРАЖЕНИЯ
            
            if (playerHp <= 0)
            {
                Console.WriteLine();
                Console.WriteLine("==========================================");
                Console.WriteLine("             ЭКЗОСКЕЛЕТ РАЗРУШЕН");
                Console.WriteLine("==========================================");
                Console.WriteLine("Инженер-изобретатель потерпел поражение.");
                Console.WriteLine();
                Console.WriteLine("Игра окончена.");

                break;
            }

            
            // ПОБЕДА НАД ВОЛНОЙ
            
            Console.WriteLine();
            Console.WriteLine("==========================================");
            Console.WriteLine($"          ВОЛНА {wave} ПРОЙДЕНА!");
            Console.WriteLine("==========================================");

            // Небольшое восстановление после каждой волны
            playerHp += 15;

            if (playerHp > maxPlayerHp)
            {
                playerHp = maxPlayerHp;
            }

            steam += 10;

            if (steam > maxSteam)
            {
                steam = maxSteam;
            }

            Console.WriteLine(
                "После боя механик немного восстановил экзоскелет."
            );

            Console.WriteLine(
                $"Прочность: {playerHp}/{maxPlayerHp}"
            );

            Console.WriteLine(
                $"Давление пара: {steam}/{maxSteam}"
            );

            // Если это не последняя волна — продолжаем
            if (wave < 3)
            {
                Console.WriteLine();
                Console.WriteLine(
                    "Следующая волна противников приближается..."
                );
            }
        }

        
        // ФИНАЛЬНАЯ ПРОВЕРКА
        
        if (playerHp > 0)
        {
            Console.WriteLine();
            Console.WriteLine("==========================================");
            Console.WriteLine("             ПОБЕДА!");
            Console.WriteLine("==========================================");
            Console.WriteLine();
            Console.WriteLine(
                "Все механические противники уничтожены!"
            );
            Console.WriteLine(
                "Инженер-изобретатель спас город."
            );
            Console.WriteLine();
            Console.WriteLine("Паровые машины снова работают!");
        }

        Console.WriteLine();
        Console.WriteLine("Нажмите любую клавишу для выхода...");
        Console.ReadKey();
    }
}

```


<img width="979" height="512" alt="изображение" src="https://github.com/user-attachments/assets/f93f08a0-78e4-48ba-8196-49d99abd3066" />
