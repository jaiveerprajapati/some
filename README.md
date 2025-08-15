Okay, let's build the Project Chimera game using the web technologies you specified: PHP for the backend, JavaScript for frontend interactivity, HTML for structure, and CSS for styling.

We'll focus on the **Curator's Hub** and the **first Data-Sphere challenge ("The Thinking Machine")**, demonstrating the core loop: Learn -> Challenge -> Create -> Reflect. This will be a more interactive version than the previous PHP example, using JavaScript for dynamic updates.

**Prerequisites:**

1.  A local web server environment that can run PHP (like XAMPP, WAMP, or MAMP).
2.  Basic understanding of HTML, CSS, JavaScript, and PHP.

**Project Structure:**

Create a folder named `project_chimera_web` in your web server's document root (e.g., `htdocs` for XAMPP). Inside this folder, create the following files and directories:

*   `index.php`: The main page.
*   `game_state.php`: PHP script to handle game logic and state (via sessions).
*   `styles.css`: CSS file for styling.
*   `script.js`: JavaScript file for frontend interactivity.
*   `assets/`: Folder for future images, sounds (currently empty).

---

**1. `index.php` (Main HTML Structure)**

This file sets up the HTML structure and includes the necessary CSS and JavaScript. It also makes the initial AJAX call to get the current game state (Hub or Challenge).

```html
<!-- index.php -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project Chimera: The AI Literacy Challenge</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="game-container">
        <div id="game-content">
            <!-- Content will be loaded dynamically by JavaScript -->
            <p>Loading game...</p>
        </div>
    </div>

    <!-- Hidden form for sending commands (used by JS) -->
    <form id="command-form" style="display:none;">
        <input type="hidden" id="command-input" name="command" value="">
    </form>

    <script src="script.js"></script>
    <script>
        // On page load, fetch the initial game state
        document.addEventListener('DOMContentLoaded', (event) => {
            fetchGameState();
        });
    </script>
</body>
</html>
```

**2. `game_state.php` (PHP Backend Logic & State Management)**

This PHP script handles all backend logic. It uses sessions to store the game state and processes actions sent via AJAX requests (like entering a sphere or submitting a command).

```php
<?php
// game_state.php
session_start();

// --- Helper Functions ---
function initializeHubState() {
    $_SESSION['current_state'] = 'hub';
    $_SESSION['message'] = "Oracle: 'Welcome, Glitch. Your journey begins in The Thinking Machine.'";
}

function initializeDataSphere1State() {
    $_SESSION['current_state'] = 'data_sphere_1';
    $_SESSION['player_pos'] = [1, 1];
    $_SESSION['grid'] = [
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 1, 1, 1, 0], // Walls
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 2]  // Goal (2)
    ];
    $_SESSION['goal_pos'] = [4, 4];
    $_SESSION['feedback_message'] = "Enter command (go north/south/east/west):";
    $_SESSION['completed'] = false;
    $_SESSION['message'] = "Oracle: 'Navigate the digital maze using precise commands (go north/south/east/west).'";

    // For the "Create" phase simulation
    $_SESSION['construct_unlocked'] = false;
}

function processCommand($command) {
    if ($_SESSION['completed']) {
        $_SESSION['feedback_message'] = "Challenge already completed. Return to Hub.";
        return;
    }

    $playerPos = &$_SESSION['player_pos'];
    $grid = &$_SESSION['grid'];
    $goalPos = $_SESSION['goal_pos'];
    $gridSize = count($grid);

    $oldPos = $playerPos;

    switch (strtolower(trim($command))) {
        case 'go north':
            if ($playerPos[0] > 0) {
                $playerPos[0]--;
            } else {
                $_SESSION['feedback_message'] = "You cannot move further north.";
                return;
            }
            break;
        case 'go south':
            if ($playerPos[0] < $gridSize - 1) {
                $playerPos[0]++;
            } else {
                 $_SESSION['feedback_message'] = "You cannot move further south.";
                 return;
            }
            break;
        case 'go west':
             if ($playerPos[1] > 0) {
                 $playerPos[1]--;
             } else {
                  $_SESSION['feedback_message'] = "You cannot move further west.";
                  return;
             }
             break;
        case 'go east':
             if ($playerPos[1] < $gridSize - 1) {
                 $playerPos[1]++;
             } else {
                  $_SESSION['feedback_message'] = "You cannot move further east.";
                  return;
             }
             break;
        default:
            $_SESSION['feedback_message'] = "Invalid command: '$command'. Try 'go north', 'go south', 'go east', or 'go west'.";
            return;
    }

    // Check for collision after movement
    if ($grid[$playerPos[0]][$playerPos[1]] == 1) {
        $playerPos = $oldPos;
        $_SESSION['feedback_message'] = "You cannot move there! It's blocked by a wall.";
        return;
    }

    // Check for goal
    if ($playerPos[0] == $goalPos[0] && $playerPos[1] == $goalPos[1]) {
        $_SESSION['completed'] = true;
        $_SESSION['construct_unlocked'] = true; // Simulate "Create" phase
        $_SESSION['feedback_message'] = "SUCCESS! You reached the data core. You've demonstrated how an algorithm (your commands) guides action. Your 'Basic Algorithm Construct' is now available in the Hub!";
        // Simulate Oracle's Reflection message being sent back
        $_SESSION['message'] = "Oracle: 'Well done, Glitch. You've grasped the essence of an algorithm - a sequence of precise instructions. This knowledge is your first tool against the Chimera. Your Construct has been added to your inventory.'";
        return;
    }

    // Normal move feedback
    $_SESSION['feedback_message'] = "Moved to [" . $playerPos[0] . ", " . $playerPos[1] . "]. Enter next command:";
}

// --- Main Logic ---
header('Content-Type: application/json'); // Send JSON response

$response = [];

// Check the type of request/action
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $input = json_decode(file_get_contents('php://input'), true); // Get JSON data from JS
    $action = isset($input['action']) ? $input['action'] : '';

    // Ensure session is initialized
    if (!isset($_SESSION['current_state'])) {
        initializeHubState();
    }

    if ($action === 'enter_sphere_1') {
        initializeDataSphere1State();
        $response['status'] = 'success';
        $response['new_state'] = $_SESSION['current_state'];
        $response['message'] = $_SESSION['message'];
        $response['grid'] = $_SESSION['grid'];
        $response['player_pos'] = $_SESSION['player_pos'];
        $response['feedback_message'] = $_SESSION['feedback_message'];
        $response['completed'] = $_SESSION['completed'];
        $response['construct_unlocked'] = $_SESSION['construct_unlocked'];

    } elseif ($action === 'submit_command' && isset($input['command'])) {
        processCommand($input['command']);
        $response['status'] = 'success';
        $response['new_state'] = $_SESSION['current_state'];
        $response['message'] = $_SESSION['message']; // Oracle's reflection message if completed
        $response['grid'] = $_SESSION['grid'];
        $response['player_pos'] = $_SESSION['player_pos'];
        $response['feedback_message'] = $_SESSION['feedback_message'];
        $response['completed'] = $_SESSION['completed'];
        $response['construct_unlocked'] = $_SESSION['construct_unlocked'];

    } elseif ($action === 'return_to_hub') {
        initializeHubState(); // Reset to Hub
        // Check if Construct was unlocked in a previous session
        $construct_status = isset($_SESSION['construct_unlocked']) ? $_SESSION['construct_unlocked'] : false;
        $_SESSION['construct_unlocked'] = $construct_status; // Preserve construct status

        $response['status'] = 'success';
        $response['new_state'] = $_SESSION['current_state'];
        $response['message'] = $_SESSION['message'];
        $response['construct_unlocked'] = $_SESSION['construct_unlocked'];

    } else {
        $response['status'] = 'error';
        $response['message'] = 'Unknown action.';
    }

} else {
    // Initial state request (GET or first load)
    if (!isset($_SESSION['current_state'])) {
        initializeHubState();
    }
    $response['status'] = 'success';
    $response['new_state'] = $_SESSION['current_state'];
    $response['message'] = $_SESSION['message'];
    if ($_SESSION['current_state'] === 'data_sphere_1') {
        $response['grid'] = $_SESSION['grid'];
        $response['player_pos'] = $_SESSION['player_pos'];
        $response['feedback_message'] = $_SESSION['feedback_message'];
        $response['completed'] = $_SESSION['completed'];
        $response['construct_unlocked'] = $_SESSION['construct_unlocked'];
    } else {
         $response['construct_unlocked'] = isset($_SESSION['construct_unlocked']) ? $_SESSION['construct_unlocked'] : false;
    }
}

echo json_encode($response);
?>
```

**3. `styles.css` (Styling)**

This file provides the visual design for the game elements.

```css
/* styles.css */
body {
    font-family: 'Courier New', Courier, monospace;
    background-color: #0a141e;
    color: #e0e0e0;
    margin: 0;
    padding: 20px;
}

.game-container {
    max-width: 900px;
    margin: 0 auto;
    text-align: center;
}

h1, h2 {
    color: #4fc3f7;
    text-shadow: 0 0 5px rgba(79, 195, 247, 0.5);
}

#game-content {
    padding: 20px;
    border: 1px solid #455a64;
    border-radius: 8px;
    background-color: rgba(30, 58, 95, 0.3);
    min-height: 500px; /* Ensure some space */
}

/* --- Hub Specific Styles --- */
.hub-title {
    font-size: 2.5em;
    margin-bottom: 20px;
}

.oracle-message {
    font-size: 1.2em;
    margin-bottom: 30px;
    color: #bbdefb;
}

.instruction {
    margin-bottom: 20px;
    font-size: 1.1em;
}

.enter-button {
    padding: 15px 25px;
    font-size: 1.2em;
    background-color: #4fc3f7;
    color: #0a141e;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin: 10px;
    transition: background-color 0.3s;
}

.enter-button:hover {
    background-color: #29b6f6;
    box-shadow: 0 0 8px rgba(79, 195, 247, 0.8);
}

.construct-info {
    margin-top: 30px;
    padding: 15px;
    border: 1px dashed #4fc3f7;
    border-radius: 5px;
    background-color: rgba(79, 195, 247, 0.1);
}

.construct-title {
    color: #81c784; /* Green for unlocked */
    font-weight: bold;
}

/* --- Data-Sphere 1 Specific Styles --- */
.sphere-title {
    font-size: 2em;
    margin-bottom: 15px;
}

.oracle-instruction {
    font-size: 1.1em;
    margin-bottom: 20px;
    color: #bbdefb;
}

.grid-container {
    display: flex;
    justify-content: center;
    margin: 20px 0;
}

.grid {
    display: grid;
    gap: 2px;
    /* Grid size will be set dynamically by JS */
    border: 2px solid #455a64;
}

.cell {
    width: 50px;
    height: 50px;
    background-color: #1e3a5f;
    border: 1px solid #26466d;
}

.wall {
    background-color: #455a64;
}

.goal {
    background-color: #4caf50;
    animation: pulse 1.5s infinite;
}

@keyframes pulse {
    0% { opacity: 0.7; box-shadow: 0 0 2px #4caf50; }
    50% { opacity: 1; box-shadow: 0 0 10px #4caf50; }
    100% { opacity: 0.7; box-shadow: 0 0 2px #4caf50; }
}

.player {
    background-color: #ffeb3b;
    box-shadow: 0 0 8px #ffeb3b;
    z-index: 10;
}

.feedback {
    margin: 15px 0;
    padding: 10px;
    background-color: rgba(30, 58, 95, 0.5);
    border: 1px solid #4fc3f7;
    border-radius: 4px;
    min-height: 2em; /* Prevent layout shift */
}

.command-form-container {
    margin-top: 20px;
}

.command-form-container label {
    display: block;
    margin-bottom: 5px;
    font-size: 1.1em;
}

#user-command {
    padding: 10px;
    font-size: 1em;
    width: 300px;
    max-width: 100%;
    background-color: #263238;
    color: #e0e0e0;
    border: 1px solid #4fc3f7;
    border-radius: 4px;
    margin-right: 10px;
}

.submit-button, .return-button {
    padding: 10px 15px;
    font-size: 1em;
    background-color: #4fc3f7;
    color: #0a141e;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin: 5px;
}

.submit-button:hover, .return-button:hover {
    background-color: #29b6f6;
}

/* General button style */
button {
    padding: 10px 15px;
    font-size: 1em;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin: 5px;
}
```

**4. `script.js` (Frontend Interactivity)**

This JavaScript file handles user interactions, makes AJAX requests to `game_state.php`, and dynamically updates the HTML content based on the responses.

```javascript
// script.js

const gameContentDiv = document.getElementById('game-content');

// --- Helper Functions ---
function getCsrfToken() {
    // In a real app, you'd get this from a meta tag or cookie for security
    // For this prototype, we'll just send an empty token or rely on session
    return ''; // Placeholder
}

// --- Main Game Logic Functions ---

function fetchGameState() {
    fetch('game_state.php', {
        method: 'GET', // Initial state fetch
        headers: {
            'Content-Type': 'application/json',
            // 'X-CSRF-TOKEN': getCsrfToken() // Add if using CSRF protection
        }
    })
    .then(response => response.json())
    .then(data => {
        if (data.status === 'success') {
            updateGameView(data);
        } else {
            console.error('Error fetching initial state:', data.message);
            gameContentDiv.innerHTML = `<p>Error loading game: ${data.message || 'Unknown error'}</p>`;
        }
    })
    .catch(error => {
        console.error('Fetch error:', error);
        gameContentDiv.innerHTML = `<p>Error connecting to server.</p>`;
    });
}

function sendAction(action, data = {}) {
    const payload = { action: action, ...data };

    fetch('game_state.php', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            // 'X-CSRF-TOKEN': getCsrfToken()
        },
        body: JSON.stringify(payload)
    })
    .then(response => response.json())
    .then(data => {
        if (data.status === 'success') {
            updateGameView(data);
        } else {
            console.error('Error with action:', action, data.message);
            alert(`Error: ${data.message || 'Action failed'}`);
        }
    })
    .catch(error => {
        console.error('Action fetch error:', error);
        alert('Error communicating with the server.');
    });
}

function updateGameView(data) {
    if (data.new_state === 'hub') {
        renderHub(data);
    } else if (data.new_state === 'data_sphere_1') {
        renderDataSphere1(data);
    } else {
        gameContentDiv.innerHTML = `<p>Unknown game state: ${data.new_state}</p>`;
    }
}

// --- Rendering Functions ---

function renderHub(data) {
    const constructStatus = data.construct_unlocked ? '<p class="construct-info"><span class="construct-title">Unlocked Construct:</span> Basic Algorithm Construct</p>' : '';

    gameContentDiv.innerHTML = `
        <h1 class="hub-title">Curator's Hub</h1>
        <p class="oracle-message">${data.message}</p>
        <p class="instruction">Select a Data-Sphere to enter:</p>
        <button class="enter-button" onclick="sendAction('enter_sphere_1')">1. The Thinking Machine</button>
        ${constructStatus}
    `;
}

function renderDataSphere1(data) {
    const gridSize = data.grid.length;
    let gridHtml = `<div class="grid" style="grid-template-columns: repeat(${gridSize}, 50px); grid-template-rows: repeat(${gridSize}, 50px);">`;

    for (let row = 0; row < gridSize; row++) {
        for (let col = 0; col < gridSize; col++) {
            let classes = "cell";
            if (data.grid[row][col] === 1) {
                classes += " wall";
            } else if (data.grid[row][col] === 2) {
                classes += " goal";
            }
            if (data.player_pos[0] === row && data.player_pos[1] === col) {
                classes += " player";
            }
            gridHtml += `<div class="${classes}"></div>`;
        }
    }
    gridHtml += `</div>`;

    const inputField = data.completed ? '' : `
        <div class="command-form-container">
            <label for="user-command">Enter Command:</label>
            <input type="text" id="user-command" placeholder="go north/south/east/west">
            <button class="submit-button" onclick="submitCommand()">Execute</button>
        </div>
        <p><em>(Press Enter after typing command)</em></p>
    `;

    const returnButton = data.completed ? `<button class="return-button" onclick="sendAction('return_to_hub')">Return to Hub</button>` : '';

    gameContentDiv.innerHTML = `
        <h2 class="sphere-title">Data-Sphere 1: The Thinking Machine</h2>
        <p class="oracle-instruction">${data.message}</p>
        <div class="grid-container">
            ${gridHtml}
        </div>
        <div class="feedback">
            <p>${data.feedback_message}</p>
        </div>
        ${inputField}
        ${returnButton}
    `;

    // Add event listener for Enter key in the input field
    if (!data.completed) {
        const commandInput = document.getElementById('user-command');
        if (commandInput) {
            commandInput.addEventListener('keypress', function(event) {
                if (event.key === 'Enter') {
                    submitCommand();
                }
            });
        }
    }
}

function submitCommand() {
    const commandInput = document.getElementById('user-command');
    if (commandInput) {
        const command = commandInput.value.trim();
        if (command) {
            sendAction('submit_command', { command: command });
            commandInput.value = ''; // Clear input after sending
        }
    }
}

// Make functions available globally for inline onclick handlers
window.sendAction = sendAction;
window.submitCommand = submitCommand;
```

**How to Run the Web-Based Game:**

1.  **Set Up Server:** Start your local web server (Apache, MySQL if needed - though not used here).
2.  **Create Files:** Save the code snippets into their respective files (`index.php`, `game_state.php`, `styles.css`, `script.js`) within the `project_chimera_web` folder in your server's document root.
3.  **Access in Browser:** Open your web browser and navigate to `http://localhost/project_chimera_web/index.php`.
4.  **Play:**
    *   You'll start in the styled "Curator's Hub".
    *   Click the "1. The Thinking Machine" button.
    *   You'll see the styled grid challenge.
    *   Type commands like `go north`, `go south`, etc., into the input field and press Enter (or click "Execute").
    *   Navigate the yellow square (player) to the green square (goal) while avoiding gray squares (walls).
    *   Upon reaching the goal, you'll see a success message, the grid will stop responding to commands, and a "Return to Hub" button will appear. An "Oracle Reflection" message will also be displayed.
    *   Click "Return to Hub". You should now see the Hub again, and if you completed the challenge, it will indicate that a "Construct" was unlocked.
    *   You can re-enter the Data-Sphere 1 to see the challenge again (state is reset upon re-entry in this prototype).

**Extending the Web-Based Version (Following Best Practices):**

1.  **Implement Other Data-Spheres:** Create new logic branches in `game_state.php` and corresponding rendering functions in `script.js` and styles in `styles.css` for challenges like "The Data Dojo" (sorting sim), "The Algorithm Audit" (bias sim), etc.
2.  **Enhance Narrative & UI:** Load dialogue and story elements dynamically. Improve the visual design with sprites/images for characters and environments.
3.  **Persistent State/Database:** Use a database (like MySQL) instead of just `$_SESSION` to store user progress, unlocked constructs, and potentially user accounts.
4.  **Security:** Implement proper input sanitization, validation, and CSRF protection in `game_state.php`.
5.  **Accessibility:** Ensure the game is usable with screen readers and keyboard navigation.
6.  **Age-Appropriate Design:** While the narrative targets teens, ensure the UI complexity and instructions are suitable for Grade 6 students (11-12 years old).
7.  **User Research & Playtesting:** As emphasized in the articles, conduct playtesting sessions with your target age group (Grades 6-12). Gather feedback on usability, difficulty, engagement, and narrative clarity.
8.  **Iterate Design:** Use feedback to refine game mechanics, adjust difficulty curves, improve the UI/UX, and enhance the story. Iterate based on assessment results (e.g., do players understand the AI concepts?).

This implementation provides a functional, interactive web-based prototype of the core loop and the first challenge using PHP, JavaScript, HTML, and CSS, aligning with the Project Chimera blueprint.
