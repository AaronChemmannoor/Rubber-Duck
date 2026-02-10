import turtle as trtl
import math

# ----------------------------
# SCREEN SETUP
# ----------------------------
screen = trtl.Screen()
screen.setup(900, 600)
screen.title("Swimming Duck Animation")
# Use a sky blue background color
screen.bgcolor("#87CEEB")  

# Main turtle for drawing the duck
duck_drawer = trtl.Turtle()
duck_drawer.hideturtle()
duck_drawer.speed(0)

# Define a cohesive color palette
OUTLINE_COLOR = "#1b0038"
DUCK_YELLOW = "#FFD400"
BEAK_ORANGE = "#FF4500" 
EYE_WHITE = "#FFFFFF"
WATER_BLUE = "#00BFFF" 

# Disable animations for smoother frame-by-frame updates
screen.tracer(0)

# ----------------------------
# ENVIRONMENT: WATER
# ----------------------------
env_drawer = trtl.Turtle()
env_drawer.hideturtle()
env_drawer.speed(0)

def draw_water_background():
    """Draws the blue water rectangle at the bottom of the screen."""
    env_drawer.penup()
    env_drawer.goto(-450, -300)
    env_drawer.fillcolor(WATER_BLUE)
    env_drawer.begin_fill()
    for _ in range(2):
        env_drawer.forward(900)
        env_drawer.left(90)
        env_drawer.forward(120) # Total height of the water line
        env_drawer.left(90)
    env_drawer.end_fill()

# Draw the water once (it doesn't move)
draw_water_background()

# ----------------------------
# HELPER: ELLIPSE FUNCTION
# ----------------------------
def draw_duck_ellipse(turt, center_x, center_y, width, height, color):
    """Custom function to draw an ellipse since Turtle doesn't have one built-in."""
    turt.penup()
    turt.goto(center_x + width, center_y)
    turt.setheading(90)
    turt.pendown()
    turt.fillcolor(color)
    turt.begin_fill()
    
    # Calculate points along the ellipse using trigonometry
    for degree in range(0, 361, 5):
        radians = math.radians(degree)
        pos_x = center_x + width * math.cos(radians)
        pos_y = center_y + height * math.sin(radians)
        turt.goto(pos_x, pos_y)
    turt.end_fill()

# ----------------------------
# ANIMATION LOOP
# ----------------------------
frame_count = 0
starting_x_pos = 550 

# Using while True so the duck keeps swimming forever
while True: 
    duck_drawer.clear()
    
    # Calculate Movement: Moves the duck from right to left over time
    current_x = starting_x_pos - (frame_count * 2) # Slightly faster speed
    current_y = -135 

    duck_drawer.pensize(5)
    duck_drawer.pencolor(OUTLINE_COLOR)

    # 1. DRAW THE BODY
    draw_duck_ellipse(duck_drawer, current_x, current_y, 120, 75, DUCK_YELLOW)

    # 2. DRAW THE TAIL
    duck_drawer.penup()
    duck_drawer.goto(current_x + 100, current_y + 30) 
    duck_drawer.setheading(50)
    duck_drawer.pendown()
    duck_drawer.fillcolor(DUCK_YELLOW)
    duck_drawer.begin_fill()
    duck_drawer.circle(45, 95)
    duck_drawer.setheading(215)
    duck_drawer.circle(100, 45)
    duck_drawer.end_fill()

    # 3. DRAW THE NECK & HEAD
    head_x, head_y = current_x - 75, current_y + 115 
    
    # Neck 
    duck_drawer.penup()
    duck_drawer.goto(head_x - 30, head_y - 60)
    duck_drawer.begin_fill()
    duck_drawer.setheading(90)
    duck_drawer.circle(-35, 180) 
    duck_drawer.end_fill()
    
    # Head
    duck_drawer.penup()
    duck_drawer.goto(head_x, head_y - 55)
    duck_drawer.setheading(0)
    duck_drawer.pendown()
    duck_drawer.fillcolor(DUCK_YELLOW)
    duck_drawer.begin_fill()
    duck_drawer.circle(65) 
    duck_drawer.end_fill()

    # 4. DRAW THE BEAK
    duck_drawer.fillcolor(BEAK_ORANGE)
    duck_drawer.penup()
    duck_drawer.goto(head_x - 55, head_y + 5)
    duck_drawer.pendown()
    duck_drawer.begin_fill()
    duck_drawer.setheading(160)
    duck_drawer.circle(20, 190) 
    duck_drawer.setheading(0)
    duck_drawer.forward(45)
    duck_drawer.end_fill()

    # 5. DRAW THE EYE
    eye_center_x = head_x - 5 
    # White of the eye
    duck_drawer.penup()
    duck_drawer.goto(eye_center_x, head_y + 45)
    duck_drawer.fillcolor(EYE_WHITE)
    duck_drawer.begin_fill()
    duck_drawer.circle(15) 
    duck_drawer.end_fill()
    # Pupil
    duck_drawer.goto(eye_center_x, head_y + 50)
    duck_drawer.fillcolor("black")
    duck_drawer.begin_fill()
    duck_drawer.circle(8) 
    duck_drawer.end_fill()

    # 6. DRAW THE WING
    duck_drawer.penup()
    duck_drawer.goto(current_x + 25, current_y + 10)
    duck_drawer.fillcolor(DUCK_YELLOW)
    duck_drawer.setheading(15)
    duck_drawer.pendown()
    duck_drawer.begin_fill()
    duck_drawer.circle(-80, 65)
    duck_drawer.setheading(195)
    duck_drawer.circle(-110, 45)
    duck_drawer.end_fill()

    # 7. ADD FLOATING TEXT
    duck_drawer.penup()
    duck_drawer.goto(head_x - 140, head_y + 110)
    duck_drawer.pencolor("white")
    duck_drawer.write("Quack!", font=("Arial", 32, "bold"))

    # Refresh screen for the next frame
    screen.update() 
    frame_count += 1

    # --- THE IF STATEMENT ---
    # If the duck's X position goes past the left edge (-600),
    # reset frame_count to 0 to start him back at the right side.
    if current_x < -600:
        frame_count = 0

trtl.done()
