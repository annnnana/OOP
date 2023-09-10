#include "Stack.h"
#include <algorithm>

int main() {
    Stack<Rectangle<int>> st;
    int x, y, a,b;
    size_t n;
    std::cout << "Input count of rectangles" << std::endl;
    std::cin >> n;

    for (int i = 0; i < n; ++i){
        std::cout << "Input rectangles.\nTo end entering press the EOF button.\n"
                     "First, enter the X and Y - point of vertex\n"
                     "Second, enter the sides of rectangle - A and B" << std::endl;
        std::cin >> x >> y;
        std::cin >> a >> b;
        st.push(Rectangle<int>(std::pair<int,int> {x,y}, a, b));
    }
    auto printStack = [](const Node<Rectangle<int>>& obj){
        std::cout << obj << std::endl;
    };
    std::cout << "\nThis is all rectangles" << std::endl;
    std::for_each(st.begin(), st.end(), printStack);

    std::cout << "\nEnter your index to insert" << std::endl;
    std::cin >> n;
    try{
        if(st.getSize()+1 < n){
            throw std::invalid_argument("You entered wrong index");
        }
        else{
            auto it = st.begin();
            while(--n){
                ++it;
            }
            std::cout << "Input rectangle.\n"
                         "First, enter the X and Y - point of vertex\n"
                         "Second, enter the sides of rectangle - A and B" << std::endl;
            std::cin >> x >> y >> a >> b;
            st.insert(it, Rectangle<int>(std::pair<int,int>{x,y},a,b));
        }
    }
    catch (std::invalid_argument& e) {
        std::terminate();
    }

    std::cout << "\nThis is all rectangles" << std::endl;
    std::for_each(st.begin(), st.end(), printStack);

    std::cout << "\nEnter your index to erase" << std::endl;
    std::cin >> n;
    try{
        auto it = st.begin();
        while(--n){
            ++it;
        }
        st.erase(it);
    }
    catch (std::runtime_error& e) {
        std::cout << e.what() << std::endl;
    }

    std::cout << "\nThis is all rectangles" << std::endl;
    std::for_each(st.begin(), st.end(), printStack);

    int minArea;
    std::cout << "\nEnter your minimal area" << std::endl;
    std::cin >> minArea;
    auto checkMinArea = [&minArea](const Node<Rectangle<int>>& rec){
        return rec.data.area() >= minArea;
    };
    std::cout << "Number of rectangles with area greater or equal, than " << minArea << ": "
              << std::count_if(st.begin(), st.end(), checkMinArea) << std::endl;
    return 0;
}