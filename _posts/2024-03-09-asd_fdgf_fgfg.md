---
published : true 
title : asd_fdgf_fgfg  
layout : single 
toc : true 
katex : True 
---
double sqr(double a)
{
    return a * a;
}


```c++
double a = 2.5;
double asqr = sqrt(a);
asqr
```




    1.5811388




```c++
#include <iostream>

class Foo11
{
public:
    
    Foo11() { std::cout << "Foo11 default constructor" << std::endl; }
    Foo11(const Foo11&) { std::cout << "Foo11 copy constructor" << std::endl; }
    Foo11(Foo11&&) { std::cout << "Foo11 move constructor" << std::endl; }
};
```


```c++
Foo11 f1;
Foo11 f2(f1);
Foo11 f3(std::move(f1));
```

    Foo11 default constructor
    Foo11 copy constructor
    Foo11 move constructor



```c++
#include <vector>

std::vector<int> v = { 1, 2, 3};
auto iter = ++v.begin();
v
```




    { 1, 2, 3 }




```c++
*iter
```




    2




```c++
?std::vector
```


<style>
            #pager-container {
                padding: 0;
                margin: 0;
                width: 100%;
                height: 100%;
            }
            .xcpp-iframe-pager {
                padding: 0;
                margin: 0;
                width: 100%;
                height: 100%;
                border: none;
            }
            </style>
            <iframe class="xcpp-iframe-pager" src="https://en.cppreference.com/w/cpp/container/vector?action=purge"></iframe>



```c++
#include <string>
#include <fstream>

#include "nlohmann/json.hpp"

#include "xtl/xbase64.hpp"

namespace nl = nlohmann;

namespace im
{
    struct image
    {   
        inline image(const std::string& filename)
        {
            std::ifstream fin(filename, std::ios::binary);   
            m_buffer << fin.rdbuf();
        }
        
        std::stringstream m_buffer;
    };
    
    nl::json mime_bundle_repr(const image& i)
    {
        auto bundle = nl::json::object();
        bundle["image/png"] = xtl::base64encode(i.m_buffer.str());
        return bundle;
    }
}
```


```c++
im::image marie("images/marie.png");
marie
```




    
![png](../assets/images/asd_fdgf_fgfg_8_0.png)
    




```c++
#include <string>
#include <fstream>

#include "nlohmann/json.hpp"

#include "xtl/xbase64.hpp"

namespace nl = nlohmann;

namespace au
{
    struct audio
    {   
        inline audio(const std::string& filename)
        {
            std::ifstream fin(filename, std::ios::binary);   
            m_buffer << fin.rdbuf();
        }
        
        std::stringstream m_buffer;
    };
    
    nl::json mime_bundle_repr(const audio& a)
    {
        auto bundle = nl::json::object();
        bundle["text/html"] =
           std::string("<audio controls=\"controls\"><source src=\"data:audio/wav;base64,")
           + xtl::base64encode(a.m_buffer.str()) +
            "\" type=\"audio/wav\" /></audio>";
        return bundle;
    }
}
```


```c++
au::audio drums("audio/audio.wav");
drums
```




<audio controls="controls"><source src="data:audio/wav;base64," type="audio/wav" /></audio>




```c++
#include "xcpp/xdisplay.hpp"
```


```c++
xcpp::display(drums);
```


<audio controls="controls"><source src="data:audio/wav;base64," type="audio/wav" /></audio>



```c++
#include <string>
#include "xcpp/xdisplay.hpp"

#include "nlohmann/json.hpp"

namespace nl = nlohmann;

namespace ht
{
    struct html
    {   
        inline html(const std::string& content)
        {
            m_content = content;
        }
        std::string m_content;
    };

    nl::json mime_bundle_repr(const html& a)
    {
        auto bundle = nl::json::object();
        bundle["text/html"] = a.m_content;
        return bundle;
    }
}

// A blue rectangle
ht::html rect(R"(
<div style='
    width: 90px;
    height: 50px;
    line-height: 50px;
    background-color: blue;
    color: white;
    text-align: center;'>
Original
</div>)");
```


```c++
xcpp::display(rect, "some_display_id");
```



<div style='
    width: 90px;
    height: 50px;
    line-height: 50px;
    background-color: red;
    color: white;
    text-align: center;'>
Updated
</div>



```c++
// Update the rectangle to be red
rect.m_content = R"(
<div style='
    width: 90px;
    height: 50px;
    line-height: 50px;
    background-color: red;
    color: white;
    text-align: center;'>
Updated
</div>)";

xcpp::display(rect, "some_display_id", true);
```


```c++
#include <chrono>
#include <iostream>
#include <thread>

#include "xcpp/xdisplay.hpp"
```


```c++
std::cout << "hello" << std::endl;
std::this_thread::sleep_for(std::chrono::seconds(1));
xcpp::clear_output();  // will flicker when replacing "hello" with "goodbye"
std::this_thread::sleep_for(std::chrono::seconds(1));
std::cout << "goodbye" << std::endl;
```

    goodbye



```c++
std::cout << "hello" << std::endl;
std::this_thread::sleep_for(std::chrono::seconds(1));
xcpp::clear_output(true);  // prevents flickering
std::this_thread::sleep_for(std::chrono::seconds(1));
std::cout << "goodbye" << std::endl;
```

    goodbye



```c++
#include <algorithm>
#include <vector>
```


```c++
std::vector<double> to_shuffle = {1, 2, 3, 4};
```


```c++
%timeit std::random_shuffle(to_shuffle.begin(), to_shuffle.end());
```

    175 ns +- 2.11 ns per loop (mean +- std. dev. of 7 runs 10000000 loops each)



```c++
#include <iostream>

#include "xtensor/xarray.hpp"
#include "xtensor/xio.hpp"
#include "xtensor/xview.hpp"

xt::xarray<double> arr1
  {{1.0, 2.0, 3.0},
   {2.0, 5.0, 7.0},
   {2.0, 5.0, 7.0}};

xt::xarray<double> arr2
  {5.0, 6.0, 7.0};

xt::view(arr1, 1) + arr2
```

    [1minput_line_55:2:10: [0m[0;1;31mfatal error: [0m[1m'xtensor/xarray.hpp' file not found[0m
    #include "xtensor/xarray.hpp"
    [0;1;32m         ^~~~~~~~~~~~~~~~~~~~
    [0m


    Interpreter Error: 



```c++

```
