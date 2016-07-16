footris
=======

A Tetris clone for the [Android Hacked app coding game](http://www.hackedapp.com/).

The Hacked app is both awesome and a royal pain in the ass. The story mode is
very good and fun for all levels of programmers. The sandbox mode is a
different beast. The editor is terrible and with larger programs the
cut-n-paste/move functionality stops working and you're retyping code
constantly just to move it between code blocks. If the editor was more freeform
the experience would be a ton better. Also you can't save your own code to the
sdcard. It sure would have been nice to edit my Hacked code using Vim under Termux
and then reload in the app. I can't complain too much though... I spent many
hours in this app and had a great time. Check it out!

![footris](https://github.com/insanum/footris/raw/master/footris.gif)

Implementing this game was pretty easy. I challenged myself to make the program
as small as possible with respect to SLoC. I whittled it down to 175 lines. It
could be much smaller in other languages but there are some lame quirks to the
Hacked language that prevent clever hacks. Go figure.

You can load Footris after installing the Hacked app by going to this link:
[https://t.co/VygCy24Ci6](https://t.co/VygCy24Ci6)

**Footris variables and functions:**
```
var_o - initialization block
var_f - pre-rendered pieces
var_b - current state of the board
var_c - current falling piece
var_d - piece rotation state machine
var_e - game time/level/speed state
val_l - last button pressed state
f8()  - randomly generates a new piece
f1()  - draws the screen
f5()  - sticks a piece to the board, removes completed rows, checks if game over
f3()  - moves the falling piece down
f4()  - moves the falling piece left/right
f6()  - rotates the falling piece
```

**Footris code:**
```
function f8 var_b, var_f {
    var_c = var_b[5];
    var_b[5] = var_f[random(7)];
    return[var_c[0], var_c[1].copy(), var_c[2]];
}
if var_o == 0 {
    var_o = 1;
    var_f = new_list(7);
    var_f[0] = [0, [[-1, 5], [-2, 5], [-3, 5], [-4, 5]], 0, "bar"];
    var_f[1] = [1, [[-1, 5], [-1, 6], [-2, 5], [-2, 6]], 0, "square"];
    var_f[2] = [2, [[-1, 4], [-1, 5], [-1, 6], [-2, 5]], 0, "half plus"];
    var_f[3] = [3, [[-1, 4], [-1, 5], [-2, 5], [-2, 6]], 0, "mirror Z"];
    var_f[4] = [4, [[-1, 5], [-1, 6], [-2, 5], [-2, 4]], 0, "Z"];
    var_f[5] = [5, [[-1, 5], [-1, 6], [-2, 5], [-3, 5]], 0, "L"];
    var_f[6] = [6, [[-1, 5], [-1, 4], [-2, 5], [-3, 5]], 0, "mirror L"];
    var_b = [height() - 4, 10, new_list(height() - 4), 0, 0, var_f[random(7)]];
    var_b[2].map(function var_a -> new_list(10));
    var_c = f8(var_b, var_f);
    var_d = new_list(7);
    var_d[0] = new_list(2);
    var_d[0][0] = [[-2, 2], [-1, 1], [0, 0], [1, -1], 1];
    var_d[0][1] = [[2, -2], [1, -1], [0, 0], [-1, 1], 0];
    var_d[1] = new_list(1);
    var_d[1][0] = [[0, 0], [0, 0], [0, 0], [0, 0], 0];
    var_d[2] = new_list(4);
    var_d[2][0] = [[1, 1], [0, 0], [-1, -1], [1, -1], 1];
    var_d[2][1] = [[-1, 1], [0, 0], [1, -1], [1, 1], 2];
    var_d[2][2] = [[-1, -1], [0, 0], [1, 1], [-1, 1], 3];
    var_d[2][3] = [[1, -1], [0, 0], [-1, 1], [-1, -1], 0];
    var_d[3] = new_list(2);
    var_d[3][0] = [[1, 1], [0, 0], [1, -1], [0, -2], 1];
    var_d[3][1] = [[-1, -1], [0, 0], [-1, 1], [0, 2], 0];
    var_d[4] = new_list(2);
    var_d[4][0] = [[0, 0], [-1, -1], [1, -1], [2, 0], 1];
    var_d[4][1] = [[0, 0], [1, 1], [-1, 1], [-2, 0], 0];
    var_d[5] = new_list(4);
    var_d[5][0] = [[-1, 1], [-2, 0], [0, 0], [1, -1], 1];
    var_d[5][1] = [[-1, -1], [0, -2], [0, 0], [1, 1], 2];
    var_d[5][2] = [[1, -1], [2, 0], [0, 0], [-1, 1], 3];
    var_d[5][3] = [[1, 1], [0, 2], [0, 0], [-1, -1], 0];
    var_d[6] = new_list( 4 );
    var_d[6][0] = [[-1, 1], [0, 2], [0, 0], [1, -1], 1];
    var_d[6][1] = [[-1, -1], [-2, 0], [0, 0], [1, 1], 2];
    var_d[6][2] = [[1, -1], [0, -2], [0, 0], [-1, 1], 3];
    var_d[6][3] = [[1, 1], [2, 0], [0, 0], [-1, -1], 0];
    var_e = [0, 1, 0];
    var_l = new_list(3).fill(false);
}
function f1 var_b, var_c {
    var_i = (height() - (var_b[0] + 2)) / 2;
    var_j = (width() - (var_b[1] + 2)) / 2;
    var_m = var_i;
    while var_m < var_i + var_b[0] + 2 {
        var_n = var_j;
        while var_n < var_j + var_b[1] + 2 {
            if var_m == var_i || var_m == var_i + var_b[0] + 1 || var_n == var_j || var_n == var_j + var_b[1] + 1 {
                draw(var_n, var_m);
            }
            else {
                if var_b[2][var_m - (var_i + 1)][var_n - (var_j + 1)] == 1 {
                    draw(var_n, var_m);
                }
            }
            var_n ++;
        }
	var_m ++;
    }
    foreach var_k in var_c[1] {
        if var_k[0] > -1 {
            draw(var_k[1] + var_j + 1, var_k[0] + var_i + 1);
        }
    }
    foreach var_k in var_b[5][1] {
        draw(var_k[1] - var_j, var_k[0] + 8 - var_i);
    }
}
function f5 var_b, var_f, var_c {
    foreach var_k in var_c[1] {
        if var_k[0] < 0 {
            var_b[3] = 1;
            return var_c;
        }
        var_b[2][var_k[0]][var_k[1]] = 1;
    }
    var_l = [];
    foreach var_k in var_b[2] {
        var_m = var_k.copy();
        var_m.sort();
        if var_m[0] == 0 {
            var_l.push(var_k);
        }
    }
    while var_l.length() != var_b[0] {
        var_l.insert(0, new_list(var_b[1]));
        var_b[4]++;
    }
    var_b[2] = var_l;
    return f8(var_b, var_f);
}
function f3 var_b, var_f, var_c {
    foreach var_k in var_c[1] {
        var_m = var_k[0] + 1;
        if var_m == var_b[0] {
            return f5(var_b, var_f, var_c);
        }
        if var_m > -1 {
            if var_b[2][var_m][var_k[1]] == 1 {
                return f5(var_b, var_f, var_c);
            }
        }
    }
    var_c[1].map(function var_a -> [var_a[0] + 1, var_a[1]]);
    return var_c;
}
function f4 var_b, var_c, var_d {
    foreach var_k in var_c[1] {
        var_n = var_k[1] + var_d;
        if var_n < 0 || var_n == var_b[1] {
            return 0;
        }
        if var_k[0] > -1 {
            if var_b[2][var_k[0]][var_n] == 1 {
                return 0;
            }
        }
    }
    foreach var_k in var_c[1] {
        var_k[1] = var_k[1] + var_d;
    }
}
function f6 var_b, var_c, var_d {
    var_o = var_d[var_c[0]][var_c[2]];
    var_h = [var_c[1][0][0] + var_o[0][0], var_c[1][0][1] + var_o[0][1]];
    var_i = [var_c[1][1][0] + var_o[1][0], var_c[1][1][1] + var_o[1][1]];
    var_j = [var_c[1][2][0] + var_o[2][0], var_c[1][2][1] + var_o[2][1]];
    var_k = [var_c[1][3][0] + var_o[3][0], var_c[1][3][1] + var_o[3][1]];
    foreach var_a in[var_h, var_i, var_j, var_k] {
        if var_a[0] < 0 || var_a[0] > var_b[0] -1 || var_a[1] < 0 || var_a[1] > var_b[1] -1 {
            return 0;
        }
        if var_b[2][var_a[0]][var_h[1]] == 1 {
            return 0;
        }
    }
    var_c[1] = [var_h, var_i, var_j, var_k];
    var_c[2] = var_o[4];
}
var_e[0]++;
if var_b[3] == 0 {
    if var_e[2] < var_b[4] && mod(var_b[4], 3) == 0 {
        var_e[1]++;
        var_e[2] = var_b[4];
    }
    if down() || mod(var_e[0], 10 - var_e[1]) == 0 {
        var_c = f3(var_b, var_f, var_c);
    }
    if left() && var_l[0] == false {
        f4(var_b, var_c, -1);
    }
    if right() && var_l[1] == false {
        f4(var_b, var_c, 1);
    }
    if (a_btn() || b_btn()) && var_l[2] == false {
        f6(var_b, var_c, var_d);
    }
    var_l = [left(), right(), a_btn() || b_btn()];
}
else {
    draw_text(0, height() -1, "You suck!!!");
}
f1(var_b, var_c);
draw_text(0, height() - 7, "Level:");
draw_text(0, height() - 6, " " + var_e[1]);
draw_text(0, height() - 4, "Score:");
draw_text(0, height() - 3, " " + var_b[4]);
```

