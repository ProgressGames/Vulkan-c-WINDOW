

### Введение
Цель этого урока — познакомить вас с основами использования Vulkan для создания графического окна. Мы начнем с установки необходимых библиотек и инструментов, затем перейдем к написанию кода и наконец запустим наше приложение.

### Необходимые инструменты и библиотеки
Для выполнения этого урока вам понадобятся следующие инструменты и библиотеки:

- [Visual Studio](https://visualstudio.microsoft.com/) или другой редактор кода
- [GLFW](https://github.com/glfw/glfw)
- [Vulkan SDK](https://vulkan.lunarg.com/sdk/home)

Убедитесь, что все эти компоненты установлены и доступны в вашей системе.

### Шаг 1: Установка GLFW
GLFW — это кроссплатформенная библиотека для создания графических приложений. Она предоставляет интерфейс для управления окнами, ввода данных и других задач.

1. Скачайте последнюю версию GLFW с официального сайта или GitHub репозитория.
2. Распакуйте архив в удобную директорию.
3. Скопируйте файлы `glfw3.dll` (для Windows) или `libglfw3.so` (для Linux) в системную директорию.

### Шаг 2: Установка Vulkan SDK
Vulkan SDK содержит необходимые инструменты и библиотеки для разработки приложений на базе Vulkan.

1. Скачайте Vulkan SDK с официального сайта LunarG.
2. Установите Vulkan SDK, следуя инструкциям установщика.
3. Добавьте путь к Vulkan SDK в переменные среды PATH.

### Шаг 3: Написание кода
Теперь, когда у нас есть все необходимые инструменты, давайте напишем простую программу, которая создаст окно и позволит нам управлять им с помощью клавиатуры.

```cpp
#include <iostream>
#include <stdio.h>
#include <stdlib.h>

#define GLFW_INCLUDE_VULKAN
#include <GLFW/glfw3.h>

#define WINDOW_WIDTH 600
#define WINDOW_HEIGHT 400

GLFWwindow* window = NULL;

void GLFW_KeyCallback(GLFWwindow* window, int key, int scancode, int action, int mods)
{
	if ((key == GLFW_KEY_ESCAPE) && (action == GLFW_PRESS)) {
		glfwSetWindowShouldClose(window, GLFW_TRUE);
	}
}

int main(int argc, char* argv[])
{
	if (!glfwInit()) {
		std::cout << "Failed to initialize GLFW." << std::endl;
		return 1;
	}

	if (!glfwVulkanSupported()) {
		std::cout << "Vulkan is not supported." << std::endl;
		return 1;
	} else {
		std::cout << "Vulkan is supported." << std::endl;
	}

	glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
	glfwWindowHint(GLFW_RESIZABLE, GL_FALSE);

	window = glfwCreateWindow(WINDOW_WIDTH, WINDOW_HEIGHT, "My Window", NULL, NULL);

	if (!window) {
		std::cout << "Failed to create window." << std::endl;
		glfwTerminate();
		exit(EXIT_FAILURE);
	} else {
		std::cout << "Window created successfully." << std::endl;
	}

	glfwSetKeyCallback(window, GLFW_KeyCallback);

	while (!glfwWindowShouldClose(window)) {
		glfwPollEvents();
	}

	glfwTerminate();

	return 0;
}
```

### Код включает:

1. Необходимые заголовки и макросы:
   ```cpp
   #include <iostream>
   #include <stdio.h>
   #include <stdlib.h>

   #define GLFW_INCLUDE_VULKAN
   #include <GLFW/glfw3.h>
   ```
   Эти включения обеспечивают доступ к необходимым библиотекам, таким как `iostream` для вывода сообщений в консоль, `stdio.h` и `stdlib.h` для общих функций, и `GLFW/glfw3.h` для взаимодействия с графическим интерфейсом.

2. Определения и глобальные переменные:
   ```cpp
   #define WINDOW_WIDTH 600
   #define WINDOW_HEIGHT 400

   GLFWwindow* window = NULL;
   ```
   Здесь задаются размеры окна (`WINDOW_WIDTH` и `WINDOW_HEIGHT`) и определяется глобальная переменная `window`, которая будет хранить указатель на созданное окно.

3. Функция `GLFW_KeyCallback`:
   ```cpp
   void GLFW_KeyCallback(GLFWwindow* window, int key, int scancode, int action, int mods)
   {
       if ((key == GLFW_KEY_ESCAPE) && (action == GLFW_PRESS)) {
           glfwSetWindowShouldClose(window, GLFW_TRUE);
       }
   }
   ```
   Эта функция является обработчиком событий клавиатуры. Если пользователь нажмет клавишу Esc (GLFW_KEY_ESCAPE) и это событие произойдет как нажатие (GLFW_PRESS), то она отправит сигнал о закрытии окна.

4. Основная функция `main`:
   ```cpp
   int main(int argc, char* argv[])
   {
       if (!glfwInit()) {
           return 1;
       }

       if (!glfwVulkanSupported()) {
           std::cout << "Vulkan is not supported." << std::endl;
           return 1;
       } else {
           std::cout << "Vulkan is supported." << std::endl;
       }

       glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
       glfwWindowHint(GLFW_RESIZABLE, GL_FALSE);

       window = glfwCreateWindow(WINDOW_WIDTH, WINDOW_HEIGHT, "The window", NULL, NULL);

       if (!window) {
           std::cout << "Failed to create window." << std::endl;
           glfwTerminate();
           exit(EXIT_FAILURE);
       } else {
           std::cout << "Window created successfully." << std::endl;
       }

       glfwSetKeyCallback(window, GLFW_KeyCallback);

       while (!glfwWindowShouldClose(window)) {
           glfwPollEvents();
       }

       glfwTerminate();

       return 0;
   }
   ```
   Основная функция `main` выполняет следующие задачи:
    1. Инициализирует GLFW с помощью `glfwInit()`.
    2. Проверяет поддержку Vulkan с помощью `glfwVulkanSupported()`.
    3. Устанавливает настройки окна, включая использование GLFW_NO_API и запрет на изменение размеров окна.
    4. Создает окно с помощью `glfwCreateWindow()` и проверяет успешность создания.
    5. Настраивает обработчик событий клавиатуры с помощью `glfwSetKeyCallback()`.
    6. Запускает основной цикл программы, обрабатывая события с помощью `glfwPollEvents()` до тех пор, пока окно не будет закрыто.
    7. Освобождает ресурсы GLFW и завершает программу.

Таким образом, код создает простое окно, которое можно закрыть с помощью клавиши Esc, и отображает соответствующую информацию в консоли.


### Шаг 4: Запуск программы
Теперь, когда у нас есть готовый код, пришло время запустить его и проверить работу нашего окна.

1. Откройте ваш IDE (например, Visual Studio).
2. Импортируйте проект или создайте новый пустой проект.
3. Вставьте код из предыдущего шага в ваш проект.
4. Скомпилируйте и запустите программу.

### Заключение
Вы создали простое графическое окно с использованием Vulkan и GLFW. Вы можете расширить этот пример, добавляя различные элементы управления и функциональность. Надеюсь, этот урок помог вам начать ваше путешествие в мир графической разработки!
