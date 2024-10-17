# rebounding ball
from tkinter import *
import random
from math import sqrt

    
def animation():
    global dx; global dy
    global score; global AIscore
    x1, y1, x2, y2 = canvas.coords(ball)
    px1, py1, px2, py2 = canvas.coords(racket_id)
    aix1, aiy1, aix2, aiy2 = canvas.coords(racket_ai)



    #ball bounce off racket id
    if px1 < x2 < px2 and py1 < y2 < py2:
        dy = -dy
        
    #ball bounce off racket ai
    if aix1 < x1 < aix2 and aiy1 < y1 < aiy2:
        dy = -dy
    
    if x2 > width or x1 < 0: 
        dx = - dx
        
    if y2 > height:
        AIscore += 1
        dy = -dy
        canvas.itemconfig(text_ai, text=f'AI Score: {AIscore}')
        

    if y1 < 0:
        score += 1
        dy = - dy
        canvas.itemconfig(text_id, text=f'Your Score: {score}')

        

    AI_racket(x1, y1, x2, y2)
    canvas.move(ball, dx, dy)
    canvas.after(10, animation)


def player_racket(event):
    px0, py0, px1, py1 = canvas.coords(racket_id)
    
    if event.keysym == 'Left' and px0 > 0:
        canvas.move(racket_id, -20, 0)
    elif event.keysym == 'Right' and px1 < 800:
        canvas.move(racket_id, 20, 0)

# x y h w
def AI_racket(x1, y1, x2, y2):
    
    aix1, aiy1, aix2, aiy2 = canvas.coords(racket_ai)
    if aix1 > width:
        aix1 -= 5

    midpoint_ball = (x1 + x2) / 2
    midpoint_ai = (aix1 + aix2) / 2
    #if (random.randint(0, 5) == 1):
    
    # change the speed so the AI is not impossible to beat
    speed_rand = random.randint(1, 3)
    if speed_rand == 1:
        step = (midpoint_ball - midpoint_ai) * 0
    else:
        step = (midpoint_ball - midpoint_ai)
    canvas.move(racket_ai, step, 0)    

# main program
gui = Tk()
score = 0
AIscore = 0
global text_id
global text_ai
canvas = Canvas(gui, width = 800, height = 600, bg = '#BDFCC9')
ball = canvas.create_oval(100,100,125,125,fill="#FF3E96")
racket_id = canvas.create_rectangle(350, 550, 450, 560, fill='mediumpurple2')
racket_ai = canvas.create_rectangle(350, 35, 450, 45, fill='#00FFFF') #CHANGE
text_id = canvas.create_text(100, 120, text= f'Your Score: {score}', font=('Times', 20))
text_ai = canvas.create_text(100, 100, text= f'AI Score: {score}', font=('Times', 20))

racket_pos = canvas.coords(racket_id)

px, py = 5, 5
canvas.move(racket_id, px, py)
canvas.bind_all('<KeyPress-Left>', player_racket)
canvas.bind_all('<KeyPress-Right>', player_racket) 

canvas.pack()
width = int(canvas.cget('width'))
height = int(canvas.cget('height'))
dx, dy = 1, 1
animation()
mainloop() 
