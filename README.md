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
  
Класс "Заказ блюда":
public class Kitchen{
    private String name;
    private double price;
    private int countfood;
    public Kitchen(String name,double price, int countfood,int countmenu,Kitchen[] menu){
        this.name=name;
        this.price=price;
        this.countfood=countfood;
    }
    public String getName(){
        return name;
    }
    public double getPrice(){
        return price;
    }
    public int getCountfood(){
        return countfood;
    }
    public void setName(String name){
        this.name=name;
    }
    public void setPrice(double price){
        this.price=price;
    }
    public void setCountfood(int countfood){
        this.countfood=countfood;
    }
    @Override;
    public String toString(){
        return String.format("%.s : %.2f руб., %d порций", name,price,countfood);
    }
}
