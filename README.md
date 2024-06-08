Question 2: Snake game using python and turtle

Description: A simple, classic Snake Game built using Python's Turtle module, featuring score tracking and increasing difficulty as the snake grows

Code:



# Imports
import turtle
import time
import random




# Score and delay
current_score = 0
max_score = 0
game_speed = 0.1




# Set up the screen
game_screen = turtle.Screen()
game_screen.title('Snake Game')
game_screen.bgcolor("green")
game_screen.setup(width=700, height=700)
game_screen.tracer(0)  # Turns off screen updates


# Outline of the playing field
boundary_drawer = turtle.Turtle()
boundary_drawer.speed(0)
boundary_drawer.shape('circle')
boundary_drawer.color('black')
boundary_drawer.penup()
boundary_drawer.hideturtle()
boundary_drawer.goto(310, 310)
boundary_drawer.pendown()
boundary_drawer.goto(-310, 310)
boundary_drawer.goto(-310, -310)
boundary_drawer.goto(310, -310)
boundary_drawer.goto(310, 310)
boundary_drawer.penup()




# Snake head
snake_head = turtle.Turtle()
snake_head.speed(0)  # 0 is the maximum animation speed of turtle module
snake_head.shape("circle")
snake_head.color('black')
snake_head.penup()
snake_head.goto(0, 0)
snake_head.direction = 'stop'


# Snake Food
food_piece = turtle.Turtle()
food_piece.speed(0)  # 0 is the maximum animation speed of turtle module
food_piece.shape("square")
food_piece.color('red')
food_piece.penup()
food_piece.goto(0, 100)


# Contains information about snake body
body_segments = []


# Pen
score_display = turtle.Turtle()
score_display.speed(0)
score_display.shape('circle')
score_display.color('white')
score_display.penup()
score_display.hideturtle()
score_display.goto(0, 310)
score_display.write('Score: 0 High Score: 0', align='center', font=('Courier', 24, 'normal'))


### Functions
# Function to update the score
def refresh_score():
   score_display.clear()
   score_display.write('Score: {} High Score: {}'.format(current_score, max_score), align='center', font=('Courier', 24, 'normal'))


# Functions that move snake in response to keyboard keys
def move_up():
   if snake_head.direction != 'down':
       snake_head.direction = 'up'
def move_down():
   if snake_head.direction != 'up':
       snake_head.direction = 'down'
def move_left():
   if snake_head.direction != 'right':
       snake_head.direction = 'left'
def move_right():
   if snake_head.direction != 'left':
       snake_head.direction = 'right'


# Function that helps the snake move
def snake_move():
   # Move the end segments first in reverse order
   for index in range(len(body_segments) - 1, 0, -1):
       x = body_segments[index - 1].xcor()
       y = body_segments[index - 1].ycor()
       body_segments[index].goto(x, y)
   # Move segment 0 to the head
   if len(body_segments) > 0:
       x = snake_head.xcor()
       y = snake_head.ycor()
       body_segments[0].goto(x, y)
   # Keep the snake moving in the same direction
   if snake_head.direction == 'up':
       snake_head.sety(snake_head.ycor() + 10)
   if snake_head.direction == 'down':
       snake_head.sety(snake_head.ycor() - 10)
   if snake_head.direction == 'left':
       snake_head.setx(snake_head.xcor() - 10)
   if snake_head.direction == 'right':
       snake_head.setx(snake_head.xcor() + 10)


# Function that tells the game what to do when collision occurs
def handle_collision():
   time.sleep(0.5)
   snake_head.goto(0, 0)
   snake_head.direction = 'stop'
   # Hide the segments
   for segment in body_segments:
       segment.hideturtle()
   # Clear the segments list
   body_segments.clear()
   global current_score
   current_score = 0
   refresh_score()
   # Reset the delay
   global game_speed
   game_speed = 0.1


### Keyboard bindings
game_screen.listen()
game_screen.onkeypress(move_up, 'Up')
game_screen.onkeypress(move_down, 'Down')
game_screen.onkeypress(move_left, 'Left')
game_screen.onkeypress(move_right, 'Right')


### Loop that runs the game code
while True:
   # Updates the window repeatedly
   game_screen.update()


   # Check for collision with border
   if snake_head.xcor() > 290 or snake_head.xcor() < -290 or snake_head.ycor() > 290 or snake_head.ycor() < -290:
       handle_collision()


   # Check if the snake eats the food
   if snake_head.distance(food_piece) < 20:
       # Move the food to a random spot
       food_piece.goto(random.randint(-290, 290), random.randint(-290, 290))
       # Add a segment
       new_segment = turtle.Turtle()
       new_segment.speed(0)
       new_segment.shape('circle')
       new_segment.color("grey")
       new_segment.penup()
       body_segments.append(new_segment)
       # Shorten the delay - this increases speed of snake as it gets longer
       game_speed -= 0.001
       # Increase the score
       current_score += 10
       if current_score > max_score:
           max_score = current_score
       refresh_score()


   # Move the snake in the game
   snake_move()


   # Check for head collision with the body segments
   for segment in body_segments:
       if segment.distance(snake_head) < 10:
           handle_collision()
   # Delay so that we can see things move
   time.sleep(game_speed)


# Makes the window visible and runs everything
game_screen.mainloop()




Screenshot:

APIs Used
turtle: A standard Python library used for drawing graphics and creating simple games.
turtle.Screen(): Sets up the game window.
turtle.Turtle(): Creates turtle objects for the snake, food, and boundary.
Screen.tracer(), Screen.update(), Screen.listen(), Screen.onkeypress(): Controls the screen updates and keyboard bindings.
time: A standard Python library used for time-related functions.
time.sleep(): Introduces delay to control the game speed.
random: A standard Python library used for generating random numbers.
random.randint(): Places the food at a random position.
Game Objects Involved
Screen (game_screen): The game window where all game activities are displayed.
Boundary (boundary_drawer): The outline of the playing field.
Snake Head (snake_head): The main part of the snake controlled by the player.
Food (food_piece): The item that the snake eats to grow and score points.
Body Segments (body_segments): The parts of the snake that grow as it eats food.
Score Display (score_display): Shows the current and high score.
Game Over Display (game_over_display): Displays "Game Over" when the game ends.
Performance Measurement:
start_time = timeit.default_timer(): Marks the start time for performance measurement.
end_time = timeit.default_timer(): Marks the end time when the game loop ends (though it will not naturally end without intervention).
print("Game performance duration: {} seconds".format(end_time - start_time)): Outputs the duration the game ran.

