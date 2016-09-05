import simplegui
import random

click1 = 0
click2 = 0
 

# helper function to initialize globals
def new_game():
    global cards, exposed, turns, mode
    mode = 0
    turns = 0
    cards = [i%8 for i in range(16)]
    exposed = [False for i in range(16)]
    
    random.shuffle(cards)
    label.set_text("Turns = " + str(turns))
    pass

# define event handlers
def mouseclick(pos):
    # add game state logic here
    global mode, exposed, click1, click2, turns, cards
    choice = int(pos[0] / 50)
    if mode == 0:
        mode= 1
        click1 = choice
        exposed[click1] = True
    elif mode == 1:
        if not exposed[choice]:
            mode = 2
            click2 = choice
            exposed[click2] = True
            turns += 1
    elif mode== 2:
        if not exposed[choice]:
            if cards[click1] == cards[click2]:
                pass
            else:
                exposed[click1] = False
                exposed[click2] = False
            click1 = choice
            exposed[click1] = True
            mode = 1       
    label.set_text("Turns = " + str(turns))
    pass
    
                        
# cards are logically 50x100 pixels in size    
def draw(canvas):
    for i in range(16):
        if exposed[i]:
            canvas.draw_text(str(cards[i]), (50*i+10, 70), 60, "Red")
        else:
            canvas.draw_polygon([(50*i, 0), (50*i, 100), (50*i + 50, 0), (50*i + 50, 100)], 3, "Orange", "Yellow")
    pass

    

# create frame and add a button and labels
frame = simplegui.create_frame("Memory", 800, 100)

frame.add_button("Start", new_game, 150)
frame.add_button("Reset", new_game, 150)
label = frame.add_label("Turns = 0")

# register event handlers
frame.set_mouseclick_handler(mouseclick)
frame.set_draw_handler(draw)

# get things rolling
new_game()
frame.start()


