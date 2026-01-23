## Cодержание

## Отчет по лабораторной работе № 3

#### № группы: `ПМ-2502`

#### Выполнил: `Янцеловский Виталий Станиславович`

#### Вариант: `25`

### Cодержание:
- [Постановка задачи](#1-постановка-задачи)
- [Алгоритм](#2-алгоритм)
- [Программа](#3-программа)
- [Анализ правильности решения](#4-анализ-правильности-решения)

### 1) Постановка задачи
> Разработать программу для управления меню столовой, включающего названия блюд,
их стоимость и количество порций. Реализовать функции добавления, изменения и удаления блюд, управления продажами и анализа ассортимента.
### 2) Алгоритм
Создаем класс блюда и указываем количество порций, цену и имя. Создаем класс меню, где указываем максимальное количество блюд в меню, количество сделанных заказов, массив для работы с классом блюд. 
1) Добавление блюда: возьмем параметры(цена, имя блюда, порция). Цена будет больше нуля, в противном случае исключаем из меню. Также смотрим на количество заказанных блюд, они не превышают количество максимальных и не могут быть меньше 1. Далее проверка на уникальность, берем названием и 
сверяем с остальными блюдами в меню. Если все удовлетворяет условиям, то добавляем в массив меню.
2) Распечатка меню: создаем цикл, в котором, благодаря сравнению и сортировке пузырьком, сортируется спсиок в алфавитном порядке, затем создаем второй цикл, в котором будут упорядоченное меню. Выводим результат.
3) Распечатка доступных блюд: создаем цикл, в котором главным условием будет то, что количество порций будет больше 0, если равен 0 или меньше, то не выводим в меню.
4) Приготовление порций блюда: Параметры(указ.номер, n порций). находим указанный номер блюда и добавляем к нему n порций по условию, проверяем также что порций не может быть меньше 0.
5) Изменение цены блюда: Параметры(указ.номер, цена). Цена должна быть только больше 0, указанный номер в диапозоне от 1 до максимального. Прибавляем цену к указанному блюду.
6) Покупка одного блюда: Параметры(указ.номер).Если порция или цена меньше 0, то возвращаем -1, в противном случае прибавляем цену к счетчику и выводим в меню измененную цену.
7) Покупка нескольких блюд: Параметры(массив номеров). Создаем цикл и прибаляем к счетчику те номера, которые удовлетворяются предыдущему условию.
8) Максимальное количество блюд по сумме: Параметры(цена).Создаем цикл и сортируем по возрастанию цены, берем самую минимальную цену, если меньше пармаетра, то из цены вычитаем цену минимального блюда, счетчик увеличиваем на 1, и так далее, пока цена не меньше 0.
9) Три самых дорогих блюда: создаем цикл и сортируем по возрастанию цены, берем другой цикл и узнаем у него последние максимальные 3 цены, если всего блюд меньше 3, то выводим доступные.
10) Три самых дешёвых блюда: создаем цикл и сортируем по возрастанию цены, берем другой цикл и узнаем у него первые минимальные 3 цены, если всего блюд меньше 3, то выводим доступные.
11) Удаление блюда из списка: Параметры(указ.номер). В двойном цикле переставляем блюдо в конец, а затем уменьшаем максимальное количество на 1.
12) Удаление блюда, если его нет в наличии: Параметры(указ.номер). Если порций блюда больше 0, то оставляем и выводим количество порций. Если количество порций равно нулю, то удаляем из системы по предыдущему случаю. Если указанный номер больше максимального или меньше 1, то такого блюда не существует.

### 3) Программа
### 3.1) "Заказ блюда"
```java
class MenuSystem {
       class Dish {
        private String name;
        private double price;
        private int porcia;

        public Dish(String name, double price, int porcia) {
            this.name = name;
            this.price = price;
            this.porcia = porcia;
        }

        public double getPrice() {
            return price;
        }

        public int getPorcia() {
            return porcia;
        }

        public String getName() {
            return name;
        }

        @Override
        public String toString() {
            return String.format("%s (%dx порций): %.2f руб.", name, porcia, price);
        }

        public void setPrice(double price) {
            if (price > 0)
                this.price = price;
        }

        public void setPorcia(int porcia) {
            if (porcia > 0) {
                this.porcia = porcia;
                System.out.print("Заказ блюда недоступно");
            }
        }

        class Menu {
            private int count;//количество заказанных блюд
            private Dish[] dish;
            private int maxDish;//макс.количество блюд

            public Menu(int count, Dish[] dish, int maxDish) {
                this.count = count;
                this.dish = new Dish[maxDish];
            }

            //Добавление блюда(1)
            public boolean addDish(double price, String name, int porcia) {
                if (price <= 0) {
                    System.out.print("ошибка системы");
                    return false;
                }
                if (porcia < 0) {
                    System.out.print("ошибка системы");
                    return false;
                }
                if (count >= maxDish) {
                    System.out.print("Меню уже заполнено");
                    return false;
                }
                for (int i = 0; i < count; i++) {
                    if (dish[i].getName().equals(name))
                        return false;
                }
                dish[count] = new Dish(name, price, porcia);
                count++;
                return true;
            }

            //Печать нашего меню(2)
            public void listMenu() {
                if (maxDish <= 0)
                    System.out.print("ошибка системы");
                for (int i = 0; i < maxDish - 1; i++) {
                    for (int j = 0; j < maxDish - i - 1; j++) {
                        if (dish[j].getName().compareTo(dish[j + 1].getName()) > 0) {
                            Dish z = dish[j + 1];
                            dish[j + 1] = dish[j];
                            dish[j] = z;
                        }
                    }
                }
                for (int i = 0; i < maxDish; i++) {
                    System.out.printf("%s (%dx порций): %.2f руб.", dish[i].getName(), dish[i].getPorcia(), dish[i].getPrice());
                }
            }

            //Печать доступных блюд(3)
            public void dostypDish() {
                for (int i = 0; i < maxDish; i++) {
                    if (dish[i].getPorcia() > 0)
                        System.out.printf("%s (%dx порций)", dish[i].getName(), dish[i].getPorcia());
                }
            }

            //Приготовление порций блюда(4)
            public void uppPorcia(int n, int ukazonnoeblydo) {
                if (ukazonnoeblydo > maxDish || ukazonnoeblydo < 1) {
                    System.out.print("Такого блюда не существует");
                }
                if (n <= 0) {
                    System.out.print("Невозможно добавить порцию к блюду");
                }
                int k = (int) (dish[ukazonnoeblydo].getPorcia() + n);
                System.out.printf("Было:%s (%d1x порций) Стало:%s (%d2x порций)", dish[ukazonnoeblydo].getName(), dish[ukazonnoeblydo].getPorcia(), dish[ukazonnoeblydo].getName(), k);
            }

            //Изменение цены блюда(5)
            public void uppPrice(int n, int ukazonnoeblydo) {
                if (n <= 0) {
                    System.out.print("Увеличение не происходит");
                }
                if (ukazonnoeblydo > maxDish || ukazonnoeblydo < 1) {
                    System.out.print("Такого блюда не существует");
                }
                int k = (int) (dish[ukazonnoeblydo].getPrice() + n);
                System.out.printf("Было:%s : %.2f1 руб. Стало:%s : %.2f2 руб.", dish[ukazonnoeblydo].getName(), dish[ukazonnoeblydo].getPrice(), dish[ukazonnoeblydo].getName(), k);
            }

            //Покупка одного блюда(6)
            public double buyDish(int ukazonnoeblydo) {
                if (ukazonnoeblydo > maxDish || ukazonnoeblydo < 1) {
                    System.out.print("Такого блюда не существует");
                }
                if (dish[ukazonnoeblydo].getPrice() < 0 || dish[ukazonnoeblydo].getPorcia() < 0)
                    return -1;
                return dish[ukazonnoeblydo].getPrice();
            }

            //Покупка нескольких блюд(7)
            public double somebuyDish(int[] ukazonnoeblydo) {
                double k = 0;
                for (int i = 0; i < ukazonnoeblydo.length; i++) {
                    if (buyDish( ukazonnoeblydo[i]) == -1) {
                        System.out.print("Такого блюда не существует");
                        return -1;
                    } else
                        k += buyDish(ukazonnoeblydo[i]);
                }
                return k;
            }

            //Максимальное количество блюд по сумме(8)
            public int maxDish(double price) {
                int k = 0;//счетчик макс количество блюд
                if (maxDish <= 0)
                    System.out.print("ошибка системы");
                for (int i = 0; i < maxDish - 1; i++) {
                    for (int j = 0; j < maxDish - i - 1; j++) {
                        if (dish[j].getName().compareTo(dish[j + 1].getName()) > 0) {
                            Dish z = dish[j + 1];
                            dish[j + 1] = dish[j];
                            dish[j] = z;
                        }
                    }
                }
                for (int i = 0; i < maxDish; i++) {
                    while (price > 0) {
                        if (dish[i].getPrice() < price) {
                            k++;
                            price = price - dish[i].getPrice();
                        } else
                            return k = 0;
                    }
                }
                return k;
            }

            //Три самых дорогих блюд(9)
            public void expensiveDish() {
                if (maxDish >= 3) {
                    for (int i = 0; i < maxDish - 1; i++) {
                        for (int j = 0; j < maxDish - i - 1; j++) {
                            if (dish[j].getPrice() < dish[j + 1].getPrice()) {
                                Dish z = dish[j + 1];
                                dish[j + 1] = dish[j];
                                dish[j] = z;
                            }
                        }
                    }
                    for (int i = maxDish - 1; i > maxDish - 4; i--) {
                        System.out.printf("%s : %.2f руб.", dish[i].getName(), dish[i].getPrice());
                    }
                } else
                    for (int i = maxDish - 1; i >= 0; i--) {
                        System.out.printf("%s : %.2f руб.", dish[i].getName(), dish[i].getPrice());
                    }
            }

            //Три самых дешёвых блюд(10)
            public void lessDish() {
                if (maxDish >= 3) {
                    for (int i = 0; i < maxDish - 1; i++) {
                        for (int j = 0; j < maxDish - i - 1; j++) {
                            if (dish[j].getPrice() < dish[j + 1].getPrice()) {
                                Dish z = dish[j + 1];
                                dish[j + 1] = dish[j];
                                dish[j] = z;
                            }
                        }
                    }
                    for (int i = 0; i < 3; i++) {
                        System.out.printf("%s : %.2f руб.", dish[i].getName(), dish[i].getPrice());
                    }
                } else
                    for (int i = 0; i < maxDish; i++) {
                        System.out.printf("%s : %.2f руб.", dish[i].getName(), dish[i].getPrice());
                    }
            }

            //Удаление блюда из списка(11)
            public void udalenieDish(int ukazonnoeblydo) {
                if (ukazonnoeblydo > maxDish || ukazonnoeblydo < 1) {
                    System.out.print("Такого блюда не существует");
                }
                for (int i = 0; i < maxDish - 1; i++) {
                    if (dish[ukazonnoeblydo].getName().compareTo(dish[i].getName()) == 0) ;
                    for (int j = i; j < maxDish - 1; j++) {
                        if (dish[j].getName().compareTo(dish[j + 1].getName()) > 0) {
                            Dish z = dish[j + 1];
                            dish[j + 1] = dish[j];
                            dish[j] = z;
                        }
                    }
                    dish[maxDish] = null;
                    maxDish--;
                    System.out.print("Блюдо удалено из списка");
                }
            }

            //Удаление блюда, если его нет в наличии(12)
            public void nalicieDish(int ukazonnoeblydo) {
                if (ukazonnoeblydo > maxDish || ukazonnoeblydo < 1) {
                    System.out.print("Такого блюда не существует");
                }
                if (dish[ukazonnoeblydo].getPorcia() < 0) {
                    System.out.print("ошибка системы");
                }
                if (dish[ukazonnoeblydo].getPorcia() == 0) {
                    udalenieDish(ukazonnoeblydo);
                } else {
                    System.out.printf("Блюдо еще есть в наличии, осталось %d порций", dish[ukazonnoeblydo].getPorcia());
                }
            }
            public class Test {
                public class Main {
                    public void main(String[] args) {

                        // Создаем меню на 6 блюд
                        Menu menu = new Menu(6,new Dish[maxDish],6);
                        //1
                        System.out.println(menu.addDish(450.0, "Пицца", 5));
                        System.out.println("Салат: " + menu.addDish(320.0, "Салат", 3));
                        System.out.println("Суп: " + menu.addDish(280.0, "Суп", 6));
                        System.out.println("Стейк: " + menu.addDish(890.0, "Стейк", 2));
                        System.out.println("Паста: " + menu.addDish(380.0, "Паста", 4));
                        System.out.println("Повторно Пицца: " + menu.addDish(500.0, "Пицца", 3));

                        //2
                        menu.listMenu();

                        //3
                        menu.dostypDish();

                        //4
                        menu.uppPorcia(2, 3);

                        //5
                        menu.uppPrice(300, 3);
                        //6
                        System.out.println("Стоимость: " + menu.buyDish(1));

                        //7
                        int[] order = {1, 2, 3};
                        menu.somebuyDish(order);
                        //8
                        System.out.println("За 1000 руб можно купить: " + menu.maxDish(1000.0) + " блюд");

                        //9
                        menu.expensiveDish();

                        //10
                        menu.lessDish();

                        //11
                        System.out.println("Удаляем блюдо 5:");
                        menu.udalenieDish(5);

                        //12
                        menu.nalicieDish(2);
                    }
                }
            }
        }
    }
}

```
### 4) Анализ правильности решения
1. Тест на `Добавление блюда`:
    - **Input**:
        ```
        Создаем меню на 6 блюд
      Добавляем: Пицца 450р 5порций 
      Добавляем: Салат 320р 0 порции   
      Добавляем: Суп -280р 6порций 
      Добавляем: Стейк 890р 2порции 
      Добавляем: Паста 0р 4порции 
      Добавляем: Бургер 340р 7порций 
        ```

    - **Output**:
        ```
        True
        False
        False
        True
        False
        True
        ```
   
