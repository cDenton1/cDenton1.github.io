---
title: "Building Minesweeper"
tags: [projects]
read_time: "6 min read"
---

# Building Minesweeper

Happy mid-way through the year! I'm halfway through my summer break, and besides work I feel like I haven't done much.

To break out of that funk and work on my blog a little, I challenged myself to make **minesweeper** in my GitHub pages site (what you're currently viewing).

In this post, I'm going to discuss my entire process, everything I learnt, and some ideas this gave me for the future!

## Idea

Not really sure where it started, was lying in bed scrolling on my phone when the idea that I could make minesweeper popped into my head. 

Once sitting at my desk staring at my code editor, the idea of making it in HTML and JavaScript appeared as a challenge for myself. I have this page but I haven't done very many interesting things with it besides tweaking the template and adding a couple hidden pages.

So here was the big idea and end goal: making a working version of minesweeper in its own page on my GitHub pages site, not some typical post but an actual working game.

### Scope

On the evening of July 1st I began this project and gave myself the rest of the day as well as the upcoming weekend to complete the majority of the project. After that, any changes I wanted to make could be done any evening once I was off work but I wanted the majority of the functionality completed first.

It required (at first) three different files; this changed once I added it to my site as I combined all of the code into one file. It was made up of HTML for the base, JavaScript for the functionality and logic, and CSS for the appearance.

Once combined into my site, a lot of the HTML and CSS was adjusted to better match the rest of the site. However, during the initial build process, it was good practice as it has been a while since I've programmed in any of these languages.

## Outcome

After a few hours I got a simple working version. From there I combined all the code into one HTML file and created a new separate page for it on my site.

Since I've created multiple pages before, I had a default layout that I continued to use which handled a lot of the page layout without me needing to copy it over or redo in anyway.

Along with that I added a new button to my site header, next to my social media links I have now included a little bomb icon which links to my minesweeper game.

### Code

The first chunk included the typical GitHub pages header which held the layout, title, and permalink for the page. It also included the page head with the CSS which handled mainly the appearance of the grid.

```html
---
layout: default
title: Minesweeper
permalink: /minesweeper
---
<head>
    <style>
      .container div {
        border: 1px solid black;
        padding: 10px;
      }
      
      #board {
        display: grid;
        padding: 10px;
      }
    </style>
</head>
```

The second chunk includes the HTML. Handling the base for the page, like you would typically see.

```html
<h1>Minesweeper</h1>

<button onclick="createGrid(8, 8)" >Easy </button>
<button onclick="createGrid(16, 16)">Medium </button>
<button onclick="createGrid(22, 18)">Hard </button>

<div id="board" class="container"></div>

<p id="flag-count">Flags: 0</p>
```

The last chunk of code and the largest out of everything is the JavaScript section. This has all the functions for handling the cells, the grid, actions, and more.

```html
<script>
  let flagCount = 0;
  
  function createGrid(columns, rows) {
      const board = document.getElementById("board");
      let grid = []
  
      flagCount = 0;
  
      if (columns == 8) {
          var mines = 10;
      } else if (columns == 16) {
          var mines = 40;
      } else {
          var mines = 65;
      }
  
      board.innerHTML = "";
      for (let i = 0; i < rows; i++) {
          grid[i] = []
          for (let j = 0; j < columns; j++) {
              const cell = document.createElement("div");
              cell.className = "cell";
              grid[i][j] = cell;
  
              cell.textContent = "x"
              const m = Math.floor(Math.random() * 10);
              if (m == 9 && mines > 0) {
                  cell.id = "mine"
                  mines--;
                  flagCount++;
              } else {
                  cell.id = "safe"
              }
  
              cell.style.backgroundColor = '#76d576';
  
              cell.style.cursor = 'pointer';
              cell.onclick = cellClick;
              cell.oncontextmenu = rightClick;
  
              board.appendChild(cell);
          }     
      }
  
      board.style.gridTemplateColumns = `repeat(${columns}, 35px)`;
      board.style.gridTemplateRows = `repeat(${rows}, 35px)`
  
      document.getElementById("flag-count").textContent = `Flags: ${flagCount}`;
      calcValue(grid, columns, rows);
  
  }
```

The `createGrid()` function handles the initial build of the board. When the user selects their desired difficulty, it builds the grid, decides the mines via a calculated randomness, assigns click functionality to each cell, and handles some extra calculation requirements.

The following function calculates the value of each cell based on how many mines it is touching. When you left click a cell, if it's not a mine, its text display is replaced with its corresponding mine count.

```html
  function calcValue(grid, columns, rows) {
      for (let i = 0; i < rows; i++) {
          for (let j = 0; j < columns; j++) {
              var mineCount = 0;
  
              for (let iO = -1; iO <= 1; iO++) {
                  for (let jO = -1; jO <= 1; jO++) {
                      if (iO == 0 && jO == 0) {
                          continue
                      }
  
                      let iN = i + iO;
                      let jN = j + jO;
  
                      if (iN >= 0 && iN < rows && jN >= 0 && jN < columns) {
                          if (grid[iN][jN].id == "mine") {
                              mineCount++;
                          }
                      }
                  }
              }
              
              grid[i][j].dataset.mineCount = mineCount;
  
          }     
      }
  }

```

The last 3 functions are simply for click functionality and ending the game. In this original version, there isn't much to it besides adjusting flag related values, appearance of the cell, or simply ending the game.

```html
  function cellClick() {
      if (this.id == "mine") {
          endGame()
      } else {
          this.style.backgroundColor = '#d8f3d8';
          this.textContent = this.dataset.mineCount;
      }
  }
  
  function rightClick() {
      if (this.textContent != 'F' && flagCount > 0) {
          this.style.backgroundColor = '#db4d4d';
          this.textContent = 'F';
          flagCount--;
      } else if (this.textContent == 'F') {
          this.style.backgroundColor  = '#76d576';
          this.textContent = 'x';
          flagCount++;
      }
  
      console.log(flagCount);
      document.getElementById("flag-count").textContent = `Flags: ${flagCount}`;
  
  }
  
  function endGame() {
      board.innerHTML = ""
      board.innerHTML = "GAME OVER YOU LOSE"
  }
</script>
```

## Challenges

I haven't done much in JavaScript in general and haven't touched it in two years so all the functionality and logic required reading lots of resources and testing to get it where I wanted.

Even once it was completed I realized there are a mix of things I want to change or looking back I think I could make even better.

When it came to building and testing, I just repeated that cycle in-between trying new functions as to not miss when something might've broken the rest.

And simply just noticing and picking up on small mistakes such as when dealing with the grid and remembering not to mix up the `onclick` and `oncontextmenu` differences.

## Future Ideas

Since completing it, any downtime at work has had me brainstorming other possible games and improvements for this one.

For starters: 1) a proper win/lose screen, 2) higher occurrence of mine placement, and 3) a click that clears any spaces adjacent to 0 mines

During my downtime after work, I'm typically working on this. Even if it's only for a little, it feels more productive than scrolling on my phone.

## Conclusion

Build games to get out of funks! I get so caught up with work while I'm completing internships that I find towards the end of the summer I begin slacking elsewhere.

Building minesweeper, even if simple and not very cybersecurity focused, it helped challenge my brain a little while still having fun and being able to relax after a long day in the office.
