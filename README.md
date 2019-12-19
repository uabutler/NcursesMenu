# NcursesMenu
The menu system that is built to accompany ncurses didn't quite meet my needs, so I developed this instead.
It's a class that allows the programmer to create a menu and provides functionality for interacting with said menu.
Each menu on the item has a string associated with it that will be displayed to the user, and an object that might
be useful to the programmer. Depending on the user's actions, the programmer can move the selector around the menu,
and retrieve the accociated object at any time. For example, one might move the selector when the user uses the arrow
keys, and retrieve the object when they hit enter.

## Usage
The menu is presented to the programmer as an object. `T` represents the type of object returned to the programmer that
represents each item.

First, the constructor can optionally take a string, boolean, or attributes. The string, if provided, will act as a
header for the menu. The boolean determines whether or not the list will be printed vertically. For example, we could have

```C++
Menu<T> menu;
Menu<T> menu("HEADER");
Menu<T> menu(false);
Menu<T> menu("HEADER", true);
```

Finally, we can always specify the attributes for the header, items, and selected item. By default, they `A_BOLD | A_UNDERLINE`,
`A_NORMAL`, and `A_STANDOUT`, respectively. That means these two are equivalent.

```C++
Menu<T> menu("HEADER");
Menu<T> menu("HEADER", A_BOLD | A_UNDERLINE, A_NORMAL, A_STANDOUT);
```

Next, items are added using the `addItem()` method. This method takes two paramenters. The first is the string, the second is
the corresponding data. For example, we might have small, medium and large and options that represent sizes. These sizes might
be integers 1, 3, and 10.

```C++
Menu<int> menu("Size");

menu.addItem("Small", 1);
menu.addItem("Medium", 3);
menu.addItem("Large", 10);
```

We can get the size of the menu using the `getHeight()`, `getWidth()`, and `getyx()` methods. Finally, we can put the menu on the screen
the `print()` method. To place the menu at the center of the screen, we might use

```C++
int height, width;
menu.getxy(height, width);

int maxrow, maxcol;
getmaxyx(maxrow, maxcol);

int row = (maxrow - height) / 2;
int col = (maxcol - width) / 2;

menu.print(row, col);
```

We can move the cursor using `prev()` and `next()`. These methods return a boolean indicating whether or not the operations was
successful. (For exmample, it might fail if the user tries to go above the first option). We can then see where the cursor is
using the `getCurrent()` method. This returns the associated object.

```C++
int input;
while((input = getch()) != '\n')
{
  if(input == KEY_UP)
    menu.prev();
  if(input == KEY_DOWN)
    menu.next();
}
return menu.getCurrent();
```
