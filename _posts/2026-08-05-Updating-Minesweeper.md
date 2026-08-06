---
title: "Updating Minesweeper"
tags: [project(s), hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# Updating Minesweeper

I did not realize just how much motivation finishing my last post would give me to update and finalize my version of minesweeper, but it really did give me a boost.

On July 1st, I built minesweeper for my blog and posted it to it's own custom page. On July 28th, I posted about building the game and shared my entire initial process and code, **[Building Minesweeper](https://cdenton1.github.io/2026/07/28/Building-Minesweeper.html)**.

But I didn't want to stop there! I knew very early on that it was basically the bare bones of what minesweeper is and I could already spot all of these parts in it that I wanted to fix or improve in my version.

Well here it is, and in this post I'm going to share my updated code and my experience building it from start to finish over the past month.

## Code

In my last post, I think my wording on game improvements were poor with little to no explanation for them. So here is what I set out to improve initially:

1. Adding a proper end (win/lose) screen - as a placeholder in the beginning, for a win there wasn't anything and for a loss the grid was replaced with "GAME OVER YOU LOSE"
2. Ensure the correct amount of mines were placed - the game was solely relying on the randomness to get as close to the required amount as possible, which typically meant it was short
3. An initial click that clears multiple cells - often seen in minesweeper, the first click would clear a bunch of the board right away and I wanted something similar

For most of the above I was able to tweak functions I had already made, however, for some I chose to start fresh and eventually began building out an entire new JS file so I didn't feel stuck with my original code.

I didn't scrap any of the code I used, more so broke it down into even more functions, replaced portions, and adjusted the flow of everything. 

This especially made it easier once I learned that minesweeper boards are typically generated after the first click, to ensure that it's never a mine.

### End Screen

The following function is triggered once either a mine is clicked or if all the flags are placed, and it handles anything end game related such as the output and disabling any mouse activity on the grid.

```html
function endGame() {
    score = 0;
    if (flagCount == 0) {
        for (let i = 0; i < r; i++) {
            for (let j = 0; j < c; j++) {
                if (grid[i][j].id == "mine") {
                    if (grid[i][j].textContent == "F") {
                        score++;
                    } else {
                        return;
                    }
                }
            }     
        }
        // re-loop through the grid to color each of the cells
        for (let i = 0; i < r; i++) {
            for (let j = 0; j < c; j++) {
                if (grid[i][j].id != "mine" && score == mineTotal) {
                    clickEnable = 1;
                    grid[i][j].style.backgroundColor = '#b3daff';
                    grid[i][j].textContent = grid[i][j].dataset.mineCount;
                }
            }     
        }
    }
```

This first large if statement shown above is to check if all the flags are in the correct spaces. 

This utilizes a for loop that steps through each cell in the grid and checks for the correct ID and text content, otherwise returning.

It keeps score to then determine in the following for loop if it's a win and then colors every non-mine cell to signify that.

Next in the function is another if statement that handles the cell colors incase of a loss:

```html
    if (score != mineTotal) {
        for (let i = 0; i < r; i++) {
            for (let j = 0; j < c; j++) {
                if (grid[i][j].id == "mine") {
                    grid[i][j].style.backgroundColor = '#bfbfbf';
                    grid[i][j].textContent = 'M';
                } else if (grid[i][j].textContent == "F") {
                    grid[i][j].textContent = "x"
                    grid[i][j].style.backgroundColor = '#76d576';
                }
            }     
        }
    }
```

It simply sets mines to a grey with an 'M', and any flags that aren't on a mine are set to the default green with an 'x'.

And lastly a for loop that only cycles through the cells in the center of the grid and outputs the corresponding message based on whether the user won or lost.

```
for (let i = (r/2) - 1; i < (r/2) + 1; i++) {
        for (let j = (c/2) - 2; j < (c/2) + 2; j++) {

            clickEnable = 1;
            grid[i][j].style.backgroundColor = '#ffffff';

            if (mineTotal == score) {
                if (i == (r/2)-1) {
                    if (j == (c/2) - 2) {
                        grid[i][j].textContent = "W"
                    } else if (j == (c/2) - 1) {
                        grid[i][j].textContent = "E"
                    } else if (j == (c/2)) {
                        grid[i][j].textContent = "L"
                    } else if (j == (c/2) + 1) {
                        grid[i][j].textContent = "L"
                    }
                } else if (i == (r/2)) {
                    if (j == (c/2) - 2) {
                        grid[i][j].textContent = "D"
                    } else if (j == (c/2) - 1) {
                        grid[i][j].textContent = "O"
                    } else if (j == (c/2)) {
                        grid[i][j].textContent = "N"
                    } else if (j == (c/2) + 1) {
                        grid[i][j].textContent = "E"
                    }
                }
            } else {

                if (grid[i][j].id == "mine") {
                    grid[i][j].style.backgroundColor = '#bfbfbf';
                } else {
                    grid[i][j].style.backgroundColor = '#db4d4d';
                }

                if (i == (r/2)-1) {
                    if (j == (c/2) - 2) {
                        grid[i][j].textContent = "G"
                    } else if (j == (c/2) - 1) {
                        grid[i][j].textContent = "A"
                    } else if (j == (c/2)) {
                        grid[i][j].textContent = "M"
                    } else if (j == (c/2) + 1) {
                        grid[i][j].textContent = "E"
                    }
                } else if (i == (r/2)) {
                    if (j == (c/2) - 2) {
                        grid[i][j].textContent = "O"
                    } else if (j == (c/2) - 1) {
                        grid[i][j].textContent = "V"
                    } else if (j == (c/2)) {
                        grid[i][j].textContent = "E"
                    } else if (j == (c/2) + 1) {
                        grid[i][j].textContent = "R"
                    }
                }
            }
        }     
    }
}
```

For the loss, any mines that overlap the message keep their grey color to stay identifiable for the user.

### Mine Placement

...

### Initial Click

...

## Challenges

Throughout building this improved version I ran into multiple problems; often fixing one and then creating or discovering another in the process.

For any that weren't mentioned above, I still wanted to share them as they taught me a lot and were pretty fun to solve. 

## Future Ideas

...

## Conclusion

...
