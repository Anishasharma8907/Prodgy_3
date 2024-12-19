<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="tic-tac-toe">
        <h1>Tic-Tac-Toe</h1>
        <div id="game-board"></div>
        <p id="status"></p>
        <button id="reset">Reset Game</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
/* styles.css */
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #f4f4f9;
    margin: 0;
    padding: 20px;
}

.tic-tac-toe {
    max-width: 400px;
    margin: 50px auto;
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

#game-board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 5px;
    margin: 20px auto;
    width: 300px;
    height: 300px;
}

#game-board div {
    display: flex;
    align-items: center;
    justify-content: center;
    background: #e9ecef;
    font-size: 2rem;
    border-radius: 5px;
    cursor: pointer;
    user-select: none;
}

#game-board div:hover {
    background: #d6d8db;
}

#status {
    font-size: 1.2rem;
    margin: 10px 0;
}

button {
    padding: 10px 20px;
    font-size: 1rem;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

// script.js
const board = Array(9).fill(null);
let currentPlayer = "X";
let gameActive = true;

const gameBoard = document.getElementById("game-board");
const statusText = document.getElementById("status");
const resetButton = document.getElementById("reset");

const winningCombinations = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
    [0, 4, 8], [2, 4, 6]             // Diagonals
];

// Create game board cells
function createBoard() {
    gameBoard.innerHTML = "";
    board.forEach((_, index) => {
        const cell = document.createElement("div");
        cell.setAttribute("data-index", index);
        cell.addEventListener("click", handleCellClick);
        gameBoard.appendChild(cell);
    });
    updateStatus();
}

// Handle cell clicks
function handleCellClick(event) {
    const index = event.target.getAttribute("data-index");

    if (!gameActive || board[index]) return;

    board[index] = currentPlayer;
    event.target.textContent = currentPlayer;

    if (checkWin()) {
        statusText.textContent = `${currentPlayer} wins!`;
        gameActive = false;
    } else if (board.every(cell => cell)) {
        statusText.textContent = "It's a draw!";
        gameActive = false;
    } else {
        currentPlayer = currentPlayer === "X" ? "O" : "X";
        updateStatus();
    }
}

// Check for a winning combination
function checkWin() {
    return winningCombinations.some(combination =>
        combination.every(index => board[index] === currentPlayer)
    );
}

// Update status message
function updateStatus() {
    statusText.textContent = `${currentPlayer}'s turn`;
}

// Reset the game
function resetGame() {
    board.fill(null);
    currentPlayer = "X";
    gameActive = true;
    createBoard();
}

// Event listeners
resetButton.addEventListener("click", resetGame);

// Initialize the game
createBoard();
