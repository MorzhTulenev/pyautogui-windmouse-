# puautogui-windmouse-
An implementation of the well-known **WindMouse** mouse movement algorithm in **Python** using the `pyautogui` library.
[![Python Version](https://img.shields.io/badge/python-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)


## code:
 ```
  import pyautogui
  import time
  import math
  import random
  
  def wind_mouse(start_x, start_y, dest_x, dest_y, G_0=9, W_0=3, M_0=15, D_0=12, move_mouse=lambda x, y: None):
      current_x, current_y = start_x, start_y
      v_x = v_y = W_x = W_y = 0
      
      while (dist := math.hypot(dest_x - start_x, dest_y - start_y)) >= 1:
          W_m = min(W_0, dist)
          
          if dist >= D_0:
              W_x = W_x / math.sqrt(3) + (G_0 * (dest_x - start_x)) / dist
              W_y = W_y / math.sqrt(3) + (G_0 * (dest_y - start_y)) / dist
          else:
              W_x /= math.sqrt(3)
              W_y /= math.sqrt(3)
              
          v_x += W_x + random.uniform(-M_0/2, M_0/2)  
          v_y += W_y + random.uniform(-M_0/2, M_0/2)
          
          v_m = math.hypot(v_x, v_y)
          if v_m > M_0:
              v_x = (v_x / v_m) * M_0
              v_y = (v_y / v_m) * M_0
              
          start_x += v_x
          start_y += v_y
          
          move_x = int(round(start_x))
          move_y = int(round(start_y))
          
          if (move_x, move_y) != (current_x, current_y):
              move_mouse(move_x, move_y)
              current_x, current_y = move_x, move_y
  
  def draw_line(x1, y1, x2, y2):
      
      pyautogui.moveTo(x1, y1, duration=0.2)
      time.sleep(0.2)
      
      pyautogui.mouseDown()
      time.sleep(0.1)  
  
      wind_mouse(x1, y1, x2, y2, move_mouse=lambda x, y: pyautogui.moveTo(x, y, _pause=False))
      
      time.sleep(0.1)
      pyautogui.mouseUp()
      time.sleep(0.2)
  
  
  print("wait 5 seconds")
  time.sleep(5)
  
  draw_line(400, 300, 600, 300) 
  draw_line(600, 300, 600, 500) 
  draw_line(600, 500, 400, 500) 
  draw_line(400, 500, 400, 300)
  ```
