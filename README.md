import turtle as trtl

# ----------------------------
# SCREEN SETUP
# ----------------------------
window = trtl.Screen()
window.setup(900, 600)
window.bgcolor("#00D5FF")  # Bright blue water background

painter = trtl.Turtle()
painter.hideturtle()
painter.speed(0)

# Color Palette
dark_outline = "#1b0038"
ducky_yellow = "#FFD400"
beak_color = "#FF8C00"
feet_color = "#FF8C00"

# This turns off the "drawing" animation so the duck appears instantly each frame
window.tracer(0)

# ----------------------------
# DRAW WATER WAVES
# ----------------------------
wave_maker = trtl.Turtle()
wave_maker.hideturtle()
wave_maker.speed(0)
wave_maker.pensize(4)
painter.goto(-500,-500)
painter.pendown()
painter.forward(100)
horizontal_position = -450
while horizontal_position <= 450:
    # Use if/else to alternate between white and light blue waves
    if horizontal_position % 120 == 0:
        wave_maker.pencolor("white")
    else:
        wave_maker.pencolor("#b9f1ff")

    wave_maker.penup()
    wave_maker.goto(horizontal_position, -240)
    wave_maker.pendown()
    wave_maker.setheading(0)
    wave_maker.circle(30, 120)  # Draw a half-circle for the wave
    horizontal_position += 60

# ----------------------------
# ANIMATION LOOP
# ----------------------------
time_step = 0
while time_step < 1000: 
    painter.clear()

    # Calculate the "Bobbing" effect (up and down movement)
    if (time_step // 30) % 2 == 0:
        up_down_offset = time_step % 30
    else:
        up_down_offset = 30 - (time_step % 30)
    

    # Base position of the duck
    center_x = -50 
    center_y = -100 + up_down_offset

    

    # 2. DRAW THE BODY & TAIL
    painter.pensize(8)
    painter.pencolor(dark_outline)
    painter.fillcolor(ducky_yellow)
    painter.penup()
    painter.goto(center_x, center_y)
    painter.pendown()
    painter.begin_fill()
    painter.setheading(270)
    painter.circle(120)
    painter.end_fill() # Main round body
    painter.setheading(40)
    painter.penup()
    painter.goto(165,-5)
    painter.begin_fill()
    painter.setheading(0)
    painter.pendown()
    painter.forward(70)
    painter.right(270)
    painter.circle(200,-60)
    painter.end_fill()

    # 3. DRAW THE HEAD
    head_center_x = center_x + 80
    head_center_y = center_y + 160
    painter.penup()
    painter.goto(head_center_x, head_center_y - 85)
    painter.setheading(0)
    painter.pendown()
    painter.begin_fill()
    painter.circle(85,) # Round head
    painter.end_fill()

    # 4. DRAW THE BEAK
    painter.fillcolor(beak_color)
    painter.penup()
    painter.goto(head_center_x - 80, head_center_y + 10)
    painter.pendown()
    painter.begin_fill()
    painter.setheading(170)
    painter.circle(35, 90) # Top beak curve
    painter.setheading(280)
    painter.circle(35, 90) # Bottom beak curve
    painter.end_fill()

    # 5. DRAW THE EYE
    painter.fillcolor(dark_outline)
    painter.penup()
    painter.goto(head_center_x - 35, head_center_y + 25)
    painter.begin_fill()
    painter.circle(12)
    painter.end_fill()
    
    # 6. DRAW THE WING
    painter.fillcolor(ducky_yellow)
    painter.penup()
    painter.goto(center_x+100, center_y-25)
    painter.pendown()
    painter.begin_fill()
    painter.setheading(-40)
    painter.circle(80, 90) # Top of wing
    painter.left(90)
    painter.circle(80, 90) # Bottom of wing
    painter.end_fill()

    # Refresh the screen to show the new position
    window.update() 
    time_step += 1

# Keeps the window open
trtl.done()    
