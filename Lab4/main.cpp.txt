#include <iostream>
#include <tuple>
#include <cmath>

template<class T>
struct Pentagon {           //Структура Пятиугольник
    using type = T;
    std::pair<T, T> center; //Координаты центра пятиугольника
    double radius;          //Радиус пятиугольника
};

template<class T>
struct Hexagon {            //Структура Шестиугольник
    using type = T;
    std::pair<T, T> center; //Координаты центра шестиугольника
    double side;            //Сторона шестиугольника
};

template<class T>
struct Octagon {            //Структура Восьмиугольник
    using type = T;
    std::pair<T, T> center; //координаты центра
    std::pair<T, T> point;  //координаты одной точки
};

//Печать кортежа
template<class T, int index>
typename std::enable_if<index >= std::tuple_size<T>::value, void>::type print_tuple(T& tuple) {
    std::cout << std::endl;
}

template<class T, int index>
typename std::enable_if < index < std::tuple_size<T>::value, void>::type print_tuple(T& tuple) {
    auto figure = std::get<index>(tuple);
    print(figure);
    std::cout << "Area figure: " << area(figure) << std::endl;
    std::cout << std::endl;
    print_tuple<T, index + 1>(tuple);
}

//Печать Пятиугольника
template<class T>
typename std::enable_if<(sizeof(T::radius) > 0), void>::type print(T &p) {
    std::cout << "Pentagon:" << std::endl;
    std::cout << "Coordinates: ";
    double pi = acos(-1);
    for (int i = 0; i < 5; ++i) {
        double angle = 2 * pi * i / 5;
        std::cout.precision(2);
        std::cout << "(" << p.center.first + p.radius * cos(angle + pi / 10) << "; "
                  << p.center.second + p.radius * sin(angle + pi / 10) << ")";
        if (i != 4) {
            std::cout << ", ";
        }
    }
    std::cout << std::endl;
    std::cout << "Center: (" << p.center.first << ", " << p.center.second << ")" << std::endl;
}

//Печать Шестиугольника
template<class T>
typename std::enable_if<(sizeof(T::side) > 0), void>::type print(T &h) {
    std::cout << "Hexagon:" << std::endl;
    std::cout << "Coordinates: ";
    double pi = acos(-1);
    for (int i = 0; i < 6; ++i) {
        double angle = pi * i / 3;
        std::cout.precision(2);
        std::cout << "(" << h.center.first + h.side * cos(angle) << "; "
                  << h.center.second + h.side * sin(angle) << ")";
        if (i != 5) {
            std::cout << ", ";
        }
    }
    std::cout << std::endl;
    std::cout << "Center: (" << h.center.first << ", " << h.center.second << ")" << std::endl;
}

//Печать Восьмиугольника
template<class T>
typename std::enable_if<(sizeof(T::point) > 0), void>::type print(T &o) {
    double c = sqrt(2) / 2; //Косинус 45 градусов
    double s = sqrt(2) / 2; //Синус 45 градусов
    double x2 = o.center.first + (o.point.first - o.center.first) * c - (o.point.second - o.center.second) * s;
    double y2 = o.center.second + (o.point.first - o.center.first) * s + (o.point.second - o.center.second) * c;
    double x3 = o.center.first + (x2 - o.center.first) * c - (y2 - o.center.second) * s;
    double y3 = o.center.second + (x2 - o.center.first) * s + (y2 - o.center.second) * c;
    double x4 = o.center.first + (x3 - o.center.first) * c - (y3 - o.center.second) * s;
    double y4 = o.center.second + (x3 - o.center.first) * s + (y3 - o.center.second) * c;
    double x5 = o.center.first + (x4 - o.center.first) * c - (y4 - o.center.second) * s;
    double y5 = o.center.second + (x4 - o.center.first) * s + (y4 - o.center.second) * c;
    double x6 = o.center.first + (x5 - o.center.first) * c - (y5 - o.center.second) * s;
    double y6 = o.center.second + (x5 - o.center.first) * s + (y5 - o.center.second) * c;
    double x7 = o.center.first + (x6 - o.center.first) * c - (y6 - o.center.second) * s;
    double y7 = o.center.second + (x6 - o.center.first) * s + (y6 - o.center.second) * c;
    double x8 = o.center.first + (x7 - o.center.first) * c - (y7 - o.center.second) * s;
    double y8 = o.center.second + (x7 - o.center.first) * s + (y7 - o.center.second) * c;
    std::cout << "Octagon:" << std::endl;
    std::cout << "Coordinates: ";
    std::cout << "(" << o.point.first << "; " << o.point.second << "), (" 
              << x2 << "; " << y2 << "), (" << x3 << "; " << y3 << "), (" 
              << x4 << "; " << y4 << "), (" << x5 << "; " << y5 << "), ("
              << x6 << "; " << y6 << "), (" << x7 << "; " << y7 << "), ("  
              << x8 << "; " << y8 << ")" << std::endl;
    std::cout << "Center: (" << o.center.first << ", " << o.center.second << ")" << std::endl;
}

//Вычисление площади всех фигур в кортеже
template<class T, int index>
typename std::enable_if<index >= std::tuple_size<T>::value, double>::type TotalArea(T& tuple) {
    return 0;
}

template<class T, int index>
typename std::enable_if<index < std::tuple_size<T>::value, double>::type TotalArea(T &tuple) {
    auto figure = std::get<index>(tuple);
    double cur_area = area(figure);
    return cur_area + TotalArea<T, index + 1>(tuple);
}

//Вычисление площади пятиугольника
template<class T>
typename std::enable_if<(sizeof(T::radius) > 0), double>::type area(T &p) {
    double pi = acos(-1);
    double side = p.radius * cos(13 * pi / 10) - p.radius * cos(17 * pi / 10);
    return sqrt(25 + 10 * sqrt(5)) * pow(side, 2) * 0.25;
}

//Вычисление площади шестиугольника
template<class T>
typename std::enable_if<(sizeof(T::side) > 0), double>::type area(T &h) {
    return pow(h.side, 2) * 3 * sqrt(3) * 0.5;
}

//Вычисление площади восьмиугольника
template<class T>
typename std::enable_if<(sizeof(T::point) > 0), double>::type area(T& o) {
    return pow(sqrt(pow(o.point.first - o.center.first, 2) + pow(o.point.second - o.center.second, 2)), 2) * 2 * sqrt(2); //Площадь восьмиугольника через радиус описанной окружности
}

void Options() { 
    std::cout << "Options: " << std::endl;             //Параметры
    std::cout << "1. Enter figures." << std::endl;     //Ввести фигуры
    std::cout << "2. Preset figures." << std::endl;    //Использовать готовые фигуры
    std::cout << "3. Exit." << std::endl;              //Завешить программу
}

int main() {
    int option;
    Options();
    std::cout << "Select option: ";
    while (std::cin >> option) {
        if (option == 1) { //Ввод фигур пользователем
            std::cout << "Enter (coordinates of center) and radius of pentagon accordingly: ";  //Ввод (координат центра) и радиуса пятиугольника
            Pentagon<double> p3;
            std::cin >> p3.center.first >> p3.center.second >> p3.radius;
            std::cout << std::endl;

            std::cout << "Enter (coordinates of center) and side of hexagon accordingly: ";     //Ввод (координат центра) и стороны шестиугольника
            Hexagon<double> h3;
            std::cin >> h3.center.first >> h3.center.second >> h3.side;
            std::cout << std::endl;

            std::cout << "Enter coordinates of center and one point of octagon accordingly: ";  //Ввод координат центра и одной точки восьмиугольника
            Octagon<double> o3;
            std::cin >> o3.center.first >> o3.center.second >> o3.point.first >> o3.point.second;
            std::cout << std::endl;

            std::tuple<decltype(p3), decltype(h3), decltype(o3)>                                //Создание кортежа
                tuple{ p3, h3, o3 };

            print_tuple<decltype(tuple), 0>(tuple);                                             //Печать кортежа
            std::cout << std::fixed << "Total area: " << TotalArea<decltype(tuple), 0>(tuple);  //Вычисление площади всех фигур кортежа
        }
        else if (option == 2) { //Использование готовых фигур
            Pentagon<int> p1;
            p1.center = { 7, -7 };
            p1.radius = 7;

            Pentagon<double> p2;
            p2.center = { 0.25, 0.25 };
            p2.radius = 10.5;

            Hexagon<int> h1;
            h1.center = { -5, -10 };
            h1.side = 7.5;

            Hexagon<double> h2;
            h2.center = { 0.5, 0.87 };
            h2.side = 1;

            Octagon<int> o1;
            o1.center = { -2, -3 };
            o1.point = { 1, 1 };

            Octagon<double> o2;
            o2.center = { 12.34, -5.3 };
            o2.point = { 14, -3.12 };

            std::tuple<decltype(p1), decltype(h1), decltype(o1), decltype(p2), decltype(h2), decltype(o2)>  //Создание кортежа
                tuple{ p1, h1, o1, p2, h2, o2 };

            print_tuple<decltype(tuple), 0>(tuple);                                                         //Печать всех фигур кортежа
            std::cout << std::fixed << "Total area: " << TotalArea<decltype(tuple), 0>(tuple);              //Вычисление площади всех фигур кортежа


        }
        else if (option == 3) { //Выход из программы
            break; 
        }
        else //Неверный выбор параметра
        std::cout << "There is no such option, please try again!" << std::endl;
        std::cout << std::endl;
        std::cout << "Select option: "; //Повторный ввод
    }
    return 0;
}