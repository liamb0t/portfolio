# Making an Evolutionary Game Theory Visualizer in JS

A while ago when I started learning Javascript for the first time I wanted to create a visualizer 
that simulated various games from game theory, where you could tweak and change parameters on the fly. 
I had already begun working on this sort of thing at university in my final year as a grad student for my thesis, 
but it was only a single game (the Utlimatum game) that simply printed out the output as images using heatmaps from Matplotlib in Python. Since it was written in Python, it also meant trying to run the simulation on grids with thousands and thousands of agents at a time, which became painstakingly slow in comparison to a faster language, even JS. 

The project did lead to some cool exporations of other games that I probably shouldn't have been spending time on since they weren't really relevant to my research but nevertheless I stumbled across during my readings. One such simulation was this implemenation of the Prisoners Dilemma on a 2D grid by Nowak-May that leads to some interesting fractal patterns under certain conditions. I have a repo on my github that implemenents and produces the same results as the paper written in Python. 

As mentioned before however, Python was probably not a great choice of language especially since I wanted to run the simulations on extremely large grids and be able to adjust parameters on the fly without having to stop and re-execute the program everytime an adjustment was made. Thus, JS! I had been meaning to learn web-dev at some point and as a person who is very much a learn by doing kind of person, it was the perfect project to start off my journey into JS. With no special frameworks or ChatGPT to help at the time, I jumped right in and started looking at how to implement Conways Game of Life in JS as a simple jumping off point.

The great thing about these sort of programs or "celluar automata" is that once you learn how to implement one and you know the formula of how to write a grid, retreive an agents neighbours, update the grid and so on is that implementing new games with different rules becomes a breeze because they all sort of follow the same pattern. So learning how to write Conways Game of Life, a very simple celluar automata, that was really all the knowledge I needed to start experimenting with other games. The only key difference you have to watch out for when implementing a celluar automata that threw me off the first few times I started working with them is how you update the agents at the end of a life cycle. Is it synchronous or asynchronous? That is, do all agents update their strategies simultanously at the end of a life cycle or do they adjust their strategy immediately after playing a game? Because an agents strategy affects a neighbours strategy which affects their neighbours strategy which affects theirs neighbours strategy and so on, the synchronosity of the update rule must be taken into consideration if the game is to be implemented as expected. In fact, this years Advent of Code had a grid problem where I wasn't getting the intended result until I realised I was stupidly updating the grid asynchrously. I really should have spotted my error earlier given how many of these type of problems I've done before.

One way in Python to update an array synchrously is simple to make a copy of the original array and update the copy during play while using the information from the original array for the game. Then at the beginning or end of each generation (it may depend on your specific case) is just to swap the new grid with the old grid. In Python though most of the time you will need to make a deepcopy of the original, otherwise the original will also get modified and the synchronous update won't work. 

```python

# Won't work because grid_copy just references grid
grid = [agent for agent in range(10)]
grid_copy = grid

# Create a copy by using slices[::]
grid = [agent for agent in range(10)]
grid_copy = grid[::]

# Create a copy of the original using deepcopy
import deepcopy from copy

grid = [agent for agent in range(10)]
grid_copy = deepcopy(grid)

```

Implementing Conways Game of Life in JS was simple enough, but I wanted to do more with my visualizer. There were two parts that I wanted to do differently from others: 1. Do something cool with the grid. 2. Add lots of parameters the user can adjust! Most visualizers of celluar automata simply just draw the agents and strategies as squares of different colors and call it a day, but this isn't very pleasing visually. First off, I wanted to add a transition effect when an agent changes their strategy so their colours would change smoothly and blend into their new strategies color. Since we can represent colors using RGB values that are just numbers from 0-255 for each red, green and blue value, I thought the simplest way was just to get the RGB values of the new strategy and increment (or decrement) the values of the old color until they equal the new colors RGB values. The faster or the bigger the value of the increment leads to a sharper or quicker transition, while a lower value, with 1 being the lowest, leads to a very slow and smooth transition. 

Here is the function I wrote in Javascript to do this:

```javascript
this.calcRGBcolor = transitionSpeed => {
        if (!this.strategyNew) {
            return;
        }

        const [r, g, b] = this.colorRGB;
        const newColor = colorDictRGB[this.strategyNew];

        this.colorRGB = [r + (newColor[0] - r > 0 ? 1 : -1) * transitionSpeed,
                            g + (newColor[1] - g > 0 ? 1 : -1) * transitionSpeed,
                            b + (newColor[2] - b > 0 ? 1 : -1) * transitionSpeed];

        return `rgb(${this.colorRGB[0]}, ${this.colorRGB[1]}, ${this.colorRGB[2]})`;
    };
```

Each agent has a `strategy` attribute that represents their current strategy and a `strategyNew` attribute that represents their new strategy if they have one. If they don't have one, we can simply exit the function since their color will be the same, so we don't have to draw them. If they do have a new color, we get the new color from the colors store and we simply just increment or decrement the old value mulitplied by the `transitionSpeed` argument. Then we return the new value as a string which we will use in our draw function to draw the agent on the canvas. Whether this is the best way or not to do this, probably not, but it works and was simple enough to implement. One could probably store the RGB values as three seperate attributes instead of an array and then can avoid using indexes for some cleaner code, but performance wise it doesn't matter.  

The next thing I wanted to do to make the grid more interesting was add some kind of interaction beyond being able to to draw agents to the grid. This feature is purely aesthetic and at the time educational for me but neverthless provides a pretty cool user experience. When a user hovers over the grid with their mouse the agents around the mouse cursor will grow and when the user moves away from those agents they will shrink back to normal size, thus creating a cool trail effect. Basically to acheive this we just add an arbitrarily sized square boundary around our current mouse position and then agents check via their update function whether they are inside that boundary. If they are, they increase in size, and if not they, they shrink back to normal. In our update function we check this by checking the distance between the mouses `x, y` coords and the agents `x, y` coords and see if it falls within the range of our boundary:

```javascript
this.update = function() {
    if (drawMode === false) {
        //If the agent is inside the boundary, make the cell larger
        if (mouse.x - this.x < 50 && mouse.x - this.x > -50 
            && mouse.y - this.y < 50 && mouse.y - this.y > -50) {
            if (this.height < height && this.width < width) {
                this.height += 1;
                this.width += 1;
            }
        }
        else {
            //If the agent is outside the boundary, shrink cell size back to original
            if (this.height > height - 5 && this.width > width - 5) {
                this.height -= 0.3;
                this.width -= 0.3;
            }
        }
    }
```

So now agents colors will change smoothly and we've got some cool hover effects that don't really do anything but look cool nonetheless. But what I really wanted was to make the agents move around the grid as if they were really interacting as after all, that is what they are doing! To do this I did it as simply as I could think with some very basic random walking. Each cell would have an arbitrarily defined boundary around them and then move in a random direction each step as long as they are within that boundary. So we just get two random values `dx, dy` as the direction the cell will walk in and mutliply it by its step length. Then we check if the new direction is within the bounds of the cells boundaries which is created just by adding a new value to the existing `x, y` coordinates of the cell. If it is, then we walk in that direction: 

```javascript
            let dx = (Math.random() - 0.5) * step_length;
            let dy = (Math.random() - 0.5) * step_length;
            
            if (this.x + dx < this.originalX + wanderX && this.x + dx > this.originalX - wanderX) {
                this.x += dx;
            }
            if (this.y + dy < this.originalY + wanderY && this.y + dy > this.originalY - wanderY) {
                this.y += dy;
            }
```

Very simple solution but it makes our grid appear much more dynamic and gives the illusion that the agents are actually moving about and interacting with each other!

That pretty much takes care of the grid side of things, there's alot more I could have done here but I was satisfied with it how it looked as is. So now lets move on to the where the real fun begins: adding some parameters the user can adjust while running a simulation! Again there are infinite things one could do here but the main things I
