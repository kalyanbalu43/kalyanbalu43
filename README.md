# LIFE AND DEATH
###############
import numpy as np
import matplotlib.pyplot as plt 
import matplotlib.animation as animation

N = 500
LIVE = 500
DEATH = 0
vals = [LIVE, DEATH]

# populate grid with random LIVE/DEATH - more off than LIVE
grid = np.random.choice(vals, N*N, p=[0.2, 0.8]).reshape(N, N)

def update(data):
  global grid
  # copy grid since we require 8 neighbors for calculation
  # and we go line by line 
  newGrid = grid.copy()
  for i in range(N):
    for j in range(N):
      # compute 8-neghbor sum 
      # using toroidal boundary conditions - x and y wrap around 
      # so that the simulaton takes place on a toroidal surface.
      total = (grid[i, (j-1)%N] + grid[i, (j+1)%N] + 
               grid[(i-1)%N, j] + grid[(i+1)%N, j] + 
               grid[(i-1)%N, (j-1)%N] + grid[(i-1)%N, (j+1)%N] + 
               grid[(i+1)%N, (j-1)%N] + grid[(i+1)%N, (j+1)%N])/500
      # apply rules
      if grid[i, j]  == LIVE:
        if (total < 2) or (total > 3):
          newGrid[i, j] = DEATH
      else:
        if total == 3:
          newGrid[i, j] = LIVE
  # update data
  mat.set_data(newGrid)
  grid = newGrid
  return [mat]

# set up animation
fig, ax = plt.subplots()
mat = ax.matshow(grid)
ani = animation.FuncAnimation(fig, update, interval=100,
                              save_count=100)
plt.show()
