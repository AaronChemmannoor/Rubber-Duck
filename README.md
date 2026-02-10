import turtle as trtl
import math

# ----------------------------
# SCREEN SETUP
# ----------------------------
window = trtl.Screen()
window.setup(900, 600)
# Distinct Sky Color
window.bgcolor("#87CEEB")  

painter = trtl.Turtle()
painter.hideturtle()
painter.speed(0)

# Colors based on reference
dark_outline = "#1b0038"
ducky_yellow = "#FFD400"
beak_red_orange = "#FF4500" 
highlight_white = "#FFFFFF"
water_color = "#00BFFF" # Deep Deep Blue for water

window.tracer(0)

# ----------------------------
# DRAW WATER BACKGROUND & WAVES
# ----------------------------
wave_maker = trtl.Turtle()
wave_maker.hideturtle()
wave_maker.speed(0)

def draw_environment():
    # Draw the solid water block
    wave_maker.penup()
    wave_maker.goto(-450, -300)
    wave_maker.fillcolor(water_color)
    wave_maker.begin_fill()
    for _ in range(2):
        wave_maker.forward(900)
        wave_maker.left(90)
        wave_maker.forward(120) # Height of the water
        wave_maker.left(90)
    wave_maker.end_fill()

draw_environment()

# ----------------------------
# HELPER: DRAW ELLIPSE
# ----------------------------
def draw_duck_ellipse(t, x, y, width, height, color):
    t.penup()
    t.goto(x + width, y)
    t.setheading(90)
    t.pendown()
    t.fillcolor(color)
    t.begin_fill()
    for i in range(0, 361, 5):
        rad = math.radians(i)
        px = x + width * math.cos(rad)
        py = y + height * math.sin(rad)
        t.goto(px, py)
    t.end_fill()

# ----------------------------
# ANIMATION LOOP
# ----------------------------
time_step = 0
start_x = 550 

while time_step < 5000: 
    painter.clear()
    
    # Movement: Slow Right to Left
    cx = start_x - (time_step * 0.25)
    cy = -135 

    painter.pensize(5)
    painter.pencolor(dark_outline)

    # 1. THE BODY (More Compressed Ellipse)
    # Changed width to 120 and height to 75 for a flatter look
    draw_duck_ellipse(painter, cx, cy, 120, 75, ducky_yellow)

    # 2. THE TAIL (Flowing from the back)
    painter.penup()
    # Adjusted position for compressed body
    painter.goto(cx + 100, cy + 30) 
    painter.setheading(50)
    painter.pendown()
    painter.fillcolor(ducky_yellow)
    painter.begin_fill()
    painter.circle(45, 95)
    painter.setheading(215)
    painter.circle(100, 45)
    painter.end_fill()

    # 3. THE NECK & HEAD
    hx, hy = cx - 75, cy + 115 # Lowered head slightly for compression
    # Neck 
    painter.penup()
    painter.goto(hx - 30, hy - 60)
    painter.begin_fill()
    painter.setheading(90)
    painter.circle(-35, 180) 
    painter.end_fill()
    
    # Head
    painter.penup()
    painter.goto(hx, hy - 55)
    painter.setheading(0)
    painter.pendown()
    painter.fillcolor(ducky_yellow)
    painter.begin_fill()
    painter.circle(65) 
    painter.end_fill()

    # 4. THE BEAK
    painter.fillcolor(beak_red_orange)
    painter.penup()
    painter.goto(hx - 55, hy + 5)
    painter.pendown()
    painter.begin_fill()
    painter.setheading(160)
    painter.circle(20, 190) 
    painter.setheading(0)
    painter.forward(45)
    painter.end_fill()

    # 5. THE EYE
    eye_x = hx - 5 
    painter.penup()
    painter.goto(eye_x, hy + 45)
    painter.fillcolor(highlight_white)
    painter.begin_fill()
    painter.circle(15) 
    painter.end_fill()
    painter.goto(eye_x, hy + 50)
    painter.fillcolor("black")
    painter.begin_fill()
    painter.circle(8) 
    painter.end_fill()

    # 6. THE WING (Folded feather shape)
    painter.penup()
    # Adjusted position for compressed body
    painter.goto(cx + 25, cy + 10)
    painter.fillcolor(ducky_yellow)
    painter.setheading(15)
    painter.pendown()
    painter.begin_fill()
    painter.circle(-80, 65)
    painter.setheading(195)
    painter.circle(-110, 45)
    painter.end_fill()

    # 7. QUACK TEXT
    painter.penup()
    painter.goto(hx - 140, hy + 110)
    painter.pencolor("white")
    painter.write("Quack!", font=("Arial", 32, "bold"))

    window.update() 
    time_step += 1

trtl.done()
