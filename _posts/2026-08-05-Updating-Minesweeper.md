---
title: "Updating Minesweeper"
tags: [projects, games]
read_time: "12 min read"
---

# Updating Minesweeper

I did not realize just how much motivation finishing my last post would give me to update and finalize my version of minesweeper, but it really did give me a boost.

On July 1st, I built minesweeper for my blog and posted it to it's own custom page. On July 28th, I posted about building the game and shared my entire initial process and code, **[Building Minesweeper](https://cdenton1.github.io/2026/07/28/Building-Minesweeper.html)**.

But I didn't want to stop there! I knew very early on that it was basically the bare bones of what minesweeper is and I could already spot all of these parts in it that I wanted to fix or improve.

Well here it is, and in this post I'm going to share my updated code and my experience building it from start to finish over the past month.

## Plan

In my last post, I think my wording on game improvements were poor with little to no explanation for them. So here is what I set out to improve initially:

1. Adding a proper end (win/lose) screen - as a placeholder in the beginning, for a win there wasn't anything and for a loss the grid was replaced with "GAME OVER YOU LOSE"
2. Ensure the correct amount of mines were placed - the game was solely relying on the randomness to get as close to the required amount as possible, which typically meant it was short
3. An initial click that clears multiple cells - often seen in minesweeper, the first click would clear a bunch of the board right away and I wanted something similar

For most of the above I was able to tweak functions I had already made, however, for some I chose to start fresh and eventually began building out an entire new JS file so I didn't feel stuck with my original code.

I didn't scrap any of the code I used, more so broke it down into even more functions, replaced portions, and adjusted the flow of everything. 

This especially made it easier once I learned that minesweeper boards are typically generated after the first click, to ensure that it's never a mine.

### End Screen

The following function is triggered once either a mine is clicked or if all the flags are placed, and it handles anything end game related such as the output and disabling any mouse activity on the grid.

```js
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

```js
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

It simply sets mines to grey with an 'M', and any flags that aren't on a mine are set to the default green with an 'x'.

And lastly a for loop that only cycles through the cells in the center of the grid and outputs the corresponding message based on whether the user won or lost.

```js
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

Like I mentioned above, I didn't learn till later that minesweeper boards aren't generated until after the first click. So when adjusting the mine placement to ensure it landed on the required amount, I made a massive change.

Initially, I started with the function `createGrid()` which handled building the grid and generating the mine placement, before calling the function that calculated each cell's value all immediately once the user selected a difficulty.

But for the improved version and fixing my issue of mine placement, it actually helped to split this function into two and call this second function after the first cell was clicked.

The function starts by deciding the number of mines based on the number of columns, which is decided by the difficulty.

```js
function setMines(firstR, firstC) {

    if (c == 8) {
        var mines = 10;
    } else if (c == 16) {
        var mines = 40;
    } else {
        var mines = 65;
    }
```

Then using a while loop that runs as long as there are less than the required of numbers placed, it loops through the grid placing mines.

However, two factors are tracked to determine if a mine can be placed. 
1. A "safe" zone is set around the initial click to ensure that it's not adjacent to any mines
2. It checks the ID of the cell to ensure it's not placing another mine in the same spot

```js
    while (mines != 0) {
        for (let i = 0; i < r; i++) {
            for (let j = 0; j < c; j++) {
                if (Math.abs(i - firstR) <= 1 && Math.abs(j - firstC) <= 1) {
                    grid[i][j].id = "safe"
                } else {
                    const m = Math.floor(Math.random() * 8);
                    if (m == 7 && mines != 0 && grid[i][j].id != "mine") {
                        grid[i][j].id = "mine"
                        mines--;
                        flagCount++;
                    }
                }
            }     
        }
    }

    for (let i = 0; i < r; i++) {
        for (let j = 0; j < c; j++) {
            if (grid[i][j].id == null) {
                grid[i][j].id = "safe"
            }
        }     
    }

    mineTotal = flagCount;

    document.getElementById("flag-count").textContent = `Flags: ${flagCount}`;
    calcValue(grid, r, c);

}
```

Then at the end of the function, a line visible to the user is updated with the number of flags they have and the function that handles calculating the value of each cell is called.

### Initial Click

If you play any other version of minesweeper, you will notice in most, that the initial click clears a lot of ground.

In my version, I tweaked that to just reveal the safe zone around the first click. But just like my last improvement, it required reworking a lot of my initial code.

The function that handles when a cell is clicked didn't change much besides now calling a new function that checks if it's the first click.

```js
function cellClick() {
    checkFirstClick(this);
    if (clickEnable == 0) {
        if (this.textContent == "F" && this.id != "mine") {
            flagCount++;
            document.getElementById("flag-count").textContent = `Flags: ${flagCount}`
        }
        if (this.id == "mine") {
            endGame()
        } else {
            this.style.backgroundColor = '#d8f3d8';
            this.textContent = this.dataset.mineCount;
        }
    }
}

function checkFirstClick(cell) {
    if (firstCell == null) { 
        firstCell = cell; 
    } else {
        return;
    }
    setMines(firstCell.dataset.row, firstCell.dataset.column);
}
```

Simply by checking if the variable `firstCell` is null and then either returning or calling the function `setMines()`.

And I would like to note that while I was writing this out, I have already noticed multiple changes I could make to cut down on the amount of code while still keeping the functionality and logic.

Now lastly the function that handles the extra revealing of cells is finally called after the mines are set and values are calculated (this function as well could be also be improved).

```js
function checkForZero(clickedCell) {
    for (let i = -1; i <= 1; i++) {
        for (let j = -1; j <= 1; j++) {

            let iCheck;
            let jCheck;

            if (clickedCell.dataset.row != "0") {
                iCheck = Number(clickedCell.dataset.row) + i;
            } else {
                break;
            }

            if (clickedCell.dataset.column != "0") {
                jCheck = Number(clickedCell.dataset.column) + j;
            } else {
                break;
            }

            if (grid[iCheck][jCheck] != clickedCell && grid[iCheck][jCheck].id != "mine") {
                grid[iCheck][jCheck].style.backgroundColor = '#d8f3d8';
                grid[iCheck][jCheck].textContent = grid[iCheck][jCheck].dataset.mineCount;
            }
        }
    }
}
```

I will admit, the final product of this feature wasn't exactly what I imagined but I liked it and it works. 

## Challenges

Throughout building this improved version I ran into multiple problems; often fixing one and then creating or discovering another in the process.

I kept track of each one and the solutions, so that I could go back in the future if I didn't solve it immediately to see what I already tried. Some were quite simple, while others took me a while to figure out.

**1.** the win/lose screen is not properly determined so running out of flags triggered the win screen

Problem: Wasn't properly checking the mines when counting, so just hitting 0 flags was good enough

Solution: Adjusting the order of lines in function that handles the right click and end game

**2.** flags could be placed prior to the first click, and also for some reason triggered the win screen

Problem: Since the mines aren't generated until after the first click, placing a flag first meant the counter at the end matched the mine total, triggering the win

Solution: Disable the ability to right click until after the first click by checking if the `firstCell` variable is null

**3.** once you ran out of flags, you could not left-click mines or un-place flags

Problem: Once the function `endGame()` was called, all clicks were disabled even if the player didn't win or lose

Solution: Properly reset it when the grid is created and don't call it until after a win or loss is actually decided

**4.** only the squares after the last mine in the grid changed their color to blue while the rest didn't

Problem: One for loop handled the coloring of the grid for a win and checking if all the mines were flagged

Solution: Split them into separate for loops, the score is now completely counted before being checked

**5.** if I place all flags and then remove some, all of a sudden the flag count shoots up

Problem: Every click set the first cell again so if I placed any amount of flags and then removed them all, it would replace all the mines and recalculate all their values

Solution: Adjusted the variable `firstCell` to only be set when it was null so it couldn't be reset

**6.** if I have two cells left, and I flag one then unflag and flag the other - I lose, even if right

Problem: The score used for counting the flag placement and comparing to the mine total was only reset at the very beginning, meaning if the user placed and then unplaced, even if they won the numbers wouldn't match

Solution: Reset the score every time the `endGame()` function was called

**7.** the supposed safe space around the first click was actually smaller than it should have been

Problem: It only counted four of the eight cells around it, the non-corners

Solution: I tweaked the if statement to `if ((i - firstR0 <= 1 && (j - firstC) <= 1) {...}`

**8.** the supposed safe space is too big and if you click too far to the right it crashes

Problem: My fix to the last problem considered any computed number that was negative safe

Solution: Added `Math.abs` to each computed value, the **absolute** method returns negatives as positives so now it only catches 0s and 1s for the safe zone

**9.** the losing screens display is weird and doesn't always work properly

Problem: I was trying to color every mine cell in the grid grey while only looping through the center cells

Solution: Add a separate for loop to color the mines before the for loop that handles the message

## Future Ideas

Minesweeper seems pretty much complete, so unless I find more bugs or someone else does (please let me know), then this is the end for this project. 

However, it is very much the beginning for building games or little interactive stuff on my site.

For about a week now I have been thinking of a few different game ideas, with two I really want to do being: a simple matching game and [2048](https://www.2048.org/).

I think the matching game would be more simple compared to this and 2048 would be more complicated, but both being a good challenge for my CSS skills.

For interactive kind of stuff, I had some ideas but nothing that I have thought about too much.

That being said, as I slowly work on building and adding these in, some other stuff might slowly disappear or be adjusted. I've started to want to make some design choice changes to spice up this space a little more visually.

## Conclusion

This project really gave me the motivation and energy this past month. Work keeps me busy and when I get home I'm exhausted some days, so something like this to do a little here and there is refreshing.

It taught me a lot, was super fun to build and test, and genuinely felt like a decent small project to do in the evenings. 

No it's not cybersecurity focused, but it's coding practice and both a technical and creative hobby I really enjoy.
