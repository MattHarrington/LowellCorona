Notes from J.A. Whye’s **Mastering CoronaSDK Crash Course**

# Introduction #

Corona is a cross-platform game framework which can build games for iOS, Android, and soon for Windows.  Games are built using a language called Lua and a text editor.  No fancy drag-and-drop utilities are used.  I prefer this, since it allows students to learn real programming and problem solving rather than just how to use as tool.  Lua is easy to learn, very similar to JavaScript, and popular in the game industry.  Read this blog post for more: http://blogs.msdn.com/b/matt-harrington/archive/2014/04/09/five-reasons-schools-should-teach-corona-sdk.aspx.

**Prerequisites**
1. Register on http://coronalabs.com
2. Download and install Corona.  There’s a free version.
3. Download and install Notepad++ or TextWrangler
4. If you're on Windows, download and install *Lua for Windows* from http://lua.org.  This isn’t strictly necessary, but it’s nice for testing Lua snippets.
5. Skim this introduction to Lua: http://docs.coronalabs.com/guide/start/introLua/index.html. For a good introduction to Lua, watch the Lua course on http://www.pluralsight.com by Jon Sonmez.
6. Read this Hello World tutorial: http://docs.coronalabs.com/guide/start/helloWorld/index.html
5. Register for J.A. Whyte’s free Game Development Crash Course: http://masteringcoronasdk.com/game-development-crash-course/
 	
# Notes from each section of the Game Development Crash Course #

**Space Oddity**
1. Download zip archive.  Show the fully built game.
2. Rename `main-begin.lua` to `main.lua`
3. Walk through parts of `main.lua`
4. `display.newImage("background.png")` -- don’t put in function
5. `display.newImage("planet.png")`
6. Note that `planet` is at (0,0)
7. `local planet = display.newImage("planet.png")`
8. `planet.x = centerX`
9. `planet.y = centerY`
10. Do same for `background`

**Move on Down the Road**
1. `transition.to(planet, { time = 2000, y = 300 })` -- put in create playscreen region

**Tweaking the Display Objects**
1. `planet.alpha = 0.2` -- explain what alpha is
2. `planet.alpha = 0`
3. add `alpha = 1` to `transition.to(planet…)`
4. Mention that display objects have lots of other properties, like `xScale`, `rotation`, etc.

**Are You Through Yet?**
1. Add this callback to `transition.to(planet, …)`: `onComplete = showTitle`
2. Above `transition.to(planet,…)` create a `showTitle()` function
3. `local gameTitle = display.newImage("gametitle.png")` – in `showTitle()`
4. Run it
5. `gameTitle.alpha = 0; gameTitle:scale(4,4)` -- in `showTitle()`
6. `transition.to(gameTitle, { time = 500, alpha = 1, xScale = 1, yScale = 1})` -- in `showTitle()`

**Touch-a Touch-a, Touch-a, Touch Me**
1. Touch vs. tap: touch events have phases.  We’ll use tap

**Tap**
1. Put all the code we’ve written so far into a `createPlayScreen()` function.
2. In `spawnEnemy()` put:
```lua
local enemy = display.newImage(“beetleship.png”)`
enemy.x = math.random(20, display.contentWidth – 20)
enemy.y = math.random(20, display.contentHeight – 20)
```
3. Call `spawnEnemy()` after `startGame()`.  Run it a few times to see the random enemy position.
4. Move `spawnEnemy()` to `showTitle()`.  You’ll get an error if you run.  Put `local spawnEnemy` into forward declaration region.  Make `spawnEnemy()` not local.
5. `enemy:addEventListener(“tap”, shipSmash)` -- add to `spawnEnemy()`
6. `local obj = event.target ; display.remove(obj)` -- add to `shipSmash()`
7. To stop processing an event listener, put `return true` in the callback

**Bleep, Bloop! Pew, Pew!**
1. Put this in the `preload audio` region:
```lua
local sndKill = audio.loadSound(“boing-1.wav”) – in preload audio
local sndblast = audio.loadSound(“blast.mp3”) – in preload audio
local sndLose = audio.loadSound(“wahwahwah.mp3”) – in preload audio
```
2. In `shipSmash()`, put `audio.play(sndKill)`

**Text**
1. Remove call to `spawnEnemy()`
2. Remove call at bottom to `startGame()`
3. Put this in `startGame()`:
```lua
local text = display.newText("Tap here to start. Protect the planet!", 0, 0, "Helvetica", 24)
text.x = centerX
text.y = display.contentHeight - 30
```
4. Call `startGame()` at end of `showTitle()`
5. Add an event listener to dismiss the text: `text:addEventListener("tap", goAway)`
6. Create `goAway()`:
```lua
	local function goAway(event)
		display.remove(event.target)
		text = nil
		spawnEnemy()
	end
```
7. 