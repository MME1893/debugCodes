# GUI Frameworks for C and C++

This document provides an overview of popular frameworks and libraries for creating GUI applications and widgets using **C** and **C++**.

---

## **1. Qt**
- **Language**: C++
- **Overview**:
  - A robust, cross-platform GUI framework.
  - Used for applications on Windows, macOS, Linux, mobile (iOS, Android), and embedded devices.
  - Offers tools for desktop, mobile, and embedded GUI applications.
- **Key Features**:
  - High-level UI abstraction (widgets, layouts).
  - Support for modern graphics (OpenGL, Vulkan).
  - Event-driven programming with **signals and slots**.
  - Extensive tools for networking, multimedia, and database access.
- **Use Cases**:
  - Applications like KDE, Spotify, and Autodesk Maya.
- **License**: LGPL/GPL (open source) and commercial.
- **Example**:
```cpp
#include <QApplication>
#include <QPushButton>

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    QPushButton button("Hello, Qt!");
    button.resize(200, 100);
    button.show();
    return app.exec();
}
```

---

## **2. GTK**
- **Language**: C (bindings available for C++).
- **Overview**:
  - A cross-platform toolkit primarily used in Linux environments.
  - Backbone of the GNOME desktop environment.
- **Key Features**:
  - Rich set of widgets.
  - CSS-like theming.
  - Event-driven programming.
  - Supports both procedural and object-oriented programming.
- **Use Cases**:
  - Applications like GIMP, Inkscape, and GNOME desktop utilities.
- **License**: LGPL.
- **Example**:
```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Hello, GTK!");
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    gtk_widget_show(window);
    gtk_main();
    return 0;
}
```

---

## **3. X Window System (X11/Xlib/XCB)**
- **Language**: C.
- **Overview**:
  - The foundation for GUI on Unix and Linux systems.
  - **Xlib** and **XCB** are low-level libraries to interface with X Window System.
  - Requires significant manual handling of GUI components.
- **Key Features**:
  - Fine-grained control over rendering and input handling.
  - Lightweight but requires more code for complex UIs.
- **Use Cases**:
  - Custom GUI rendering or lightweight apps in Linux.
- **License**: MIT.
- **Example**:
```c
#include <X11/Xlib.h>

int main() {
    Display *display;
    Window window;
    XEvent event;

    display = XOpenDisplay(NULL);
    int screen = DefaultScreen(display);

    window = XCreateSimpleWindow(display, RootWindow(display, screen), 
                                 10, 10, 800, 600, 1,
                                 BlackPixel(display, screen), 
                                 WhitePixel(display, screen));

    XSelectInput(display, window, ExposureMask | KeyPressMask);
    XMapWindow(display, window);

    while (1) {
        XNextEvent(display, &event);
        if (event.type == Expose) {
            // Handle window expose
        } else if (event.type == KeyPress) {
            break;
        }
    }

    XCloseDisplay(display);
    return 0;
}
```

---

## **4. FLTK (Fast Light Toolkit)**
- **Language**: C++.
- **Overview**:
  - Lightweight and fast GUI library.
  - Primarily used for smaller applications with minimal dependencies.
- **Key Features**:
  - Simple API with built-in widgets.
  - Cross-platform: Windows, macOS, Linux.
  - Supports OpenGL for graphics.
- **Use Cases**:
  - Smaller tools and utilities.
- **License**: LGPL.
- **Example**:
```cpp
#include <FL/Fl.H>
#include <FL/Fl_Window.H>
#include <FL/Fl_Button.H>

int main() {
    Fl_Window *window = new Fl_Window(400, 300, "Hello, FLTK!");
    Fl_Button *button = new Fl_Button(160, 120, 80, 40, "Click Me!");
    window->end();
    window->show();
    return Fl::run();
}
```

---

## **5. Win32 API**
- **Language**: C/C++.
- **Overview**:
  - The native API for Windows GUI development.
  - Provides low-level access to the Windows GUI system.
- **Key Features**:
  - Fine control over the GUI elements.
  - High performance but verbose.
- **Use Cases**:
  - Windows-specific applications.
- **License**: Proprietary (part of Windows SDK).
- **Example**:
```cpp
#include <windows.h>

LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam) {
    switch (msg) {
    case WM_CLOSE:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hwnd, msg, wParam, lParam);
    }
    return 0;
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    const char CLASS_NAME[] = "Sample Window Class";
    WNDCLASS wc = {};
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = CLASS_NAME;

    RegisterClass(&wc);

    HWND hwnd = CreateWindowEx(0, CLASS_NAME, "Hello, Win32!",
                               WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT,
                               500, 400, NULL, NULL, hInstance, NULL);

    ShowWindow(hwnd, nCmdShow);

    MSG msg = {};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return 0;
}
```

---

## **6. ImGui (Dear ImGui)**
- **Language**: C++.
- **Overview**:
  - Immediate mode GUI library.
  - Designed for tools and debugging interfaces.
- **Key Features**:
  - Fast and easy-to-integrate GUI.
  - Requires a rendering backend (e.g., OpenGL, Vulkan).
- **Use Cases**:
  - Game development tools, real-time debugging UI.
- **License**: MIT.
- **Example**:
```cpp
#include "imgui.h"

void MyGuiFunction() {
    ImGui::Begin("Hello, ImGui!");
    ImGui::Text("This is a simple GUI example.");
    ImGui::End();
}
```

---

## **7. wxWidgets**
- **Language**: C++.
- **Overview**:
  - Cross-platform GUI library.
  - Provides a native look and feel on each platform.
- **Key Features**:
  - High-level abstraction.
  - Support for desktop and embedded systems.
- **Use Cases**:
  - Cross-platform desktop applications.
- **License**: LGPL.
- **Example**:
```cpp
#include <wx/wx.h>

class MyApp : public wxApp {
public:
    virtual bool OnInit() {
        wxFrame *frame = new wxFrame(NULL, wxID_ANY, "Hello, wxWidgets!");
        frame->Show(true);
        return true;
    }
};

wxIMPLEMENT_APP(MyApp);
```

---

## **Comparison Table**

| **Library**       | **Language**   | **Cross-Platform** | **Complexity** | **Use Cases**                   |
|--------------------|----------------|---------------------|----------------|----------------------------------|
| Qt                | C++            | Yes                 | Medium         | Modern UIs, desktop apps        |
| GTK               | C              | Yes                 | Medium         | GNOME apps, Linux tools         |
| X11 (Xlib/XCB)    | C              | No                  | High           | Low-level Linux GUIs            |
| FLTK              | C++            | Yes                 | Low            | Lightweight tools               |
| Win32 API         | C/C++          | No                  | High           | Windows-specific apps           |
| ImGui             | C++            | Yes                 | Low            | Game tools, debugging interfaces|
| wxWidgets         | C++            | Yes                 | Medium         | Native-look cross-platform GUIs |

---

These frameworks cover nearly every use case, from lightweight tools to feature-rich applications!
