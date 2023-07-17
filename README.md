# Tic-Tac-Toe
HTML Code!
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Tic Tac Toe Game</title>
</head>

<body>
    <div class="container">
        <h1>let's play Tic-Tac-Toe Game</h1>
        <div class="game-board">
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
        </div>

        <div class="game-players">
            <div class="player player1">Player 1 :</div>
            <div class="player player2">Player 2 :</div>
        </div>

        <button class="restartBtn">Restart</button>
        <div class="alertBox">Alert</div>
    </div>
    <script src="script.js"></script>
</body>

</html>

CSS Code!
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: sans-serif;
}

body{
    background: #334155;
    color: #fff;
}

.container {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.container h1{
    margin: 20px 0 40px 0;
    text-decoration: underline;
}

.game-board{
    display: grid;
    grid-template-columns: repeat(3, minmax(120px, 1fr));
}

.cell{
    border: 2px solid #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    font-size: 60px;
    font-weight: 600;
    height: 120px;
    cursor: pointer;
}

.cell:first-child,.cell:nth-child(2),.cell:nth-child(3){
    border-top: none;
}
.cell:nth-child(3),.cell:nth-child(6),.cell:nth-child(9){
    border-right: none;
}
.cell:nth-child(3n+1) {
    border-left: none;
}
.cell:nth-child(n+7) {
    border-bottom: none;
}
.game-players{
    display: flex;
    justify-content: space-between;
    margin-top: 30px;
}
.player{
    margin-inline: 18px;
    font-size: 24px;
    font-weight: 600;
}
.restartBtn{
    border: none;
    background: #5f5fc4;
    color: #fff;
    font-size: 22px;
    font-weight: 700;
    margin-block: 18px;
    padding: 10px 30px;
    border-radius: 4px;
    cursor: pointer;
}
.restartBtn:hover{
    background: #7272e1;
}
.cell.disabled{
    background: #8c8c8c;
}

.alertBox{
    position: absolute;
    top: 0;
    background: #dfdfdf;
    color: #4b4b51;
    width: 100%;
    padding: 8px, 12px;
    font-size: 20px;
    font-weight: 600;
    height: 40px;
    display: none;
}

@media screen and (max-width:550px){
    .container h1{
        font-size: 24px;
    }
    .game-board{
        grid-template-columns: repeat(3, minmax(90px, 1fr));
    }
    .cell{
        height: 90px;
        font-size: 50px;
    }    
}

JavaScript Code!
const gameCells = document.querySelectorAll('.cell');
const player1 = document.querySelector('.player1');
const player2 = document.querySelector('.player2');
const restartBtn = document.querySelector('.restartBtn');
const alertBox = document.querySelector('.alertBox');

// Making Variables
let currentPlayer = 'X';
let nextPlayer = 'O';
let playerTurn = currentPlayer;

player1.textContent = `Player 1: ${currentPlayer}`;
player2.textContent = `Player 2: ${nextPlayer}`;

// Function to start your game
const startGame = () => {
    gameCells.forEach(cell => {
        cell.addEventListener('click', handleClick);
    });
}
const handleClick = (e) => {
    if (e.target.textContent === '') {
        e.target.textContent = playerTurn;
        if(checkWin()) {
            //console.log('${playerTurn} is a Winner!');
            showAlert(`${playerTurn} is a Winner!`);
            disableCells();
        }
        else if(checkTie()){
            //console.log('It is a Tie!');
            showAlert(`It's is a Tie!`);
            disableCells();
        }else{
            changePlayerTurn();
            showAlert(`Turn for player: ${playerTurn}`);
        }
    }

}

// Function to change player's turn
const changePlayerTurn = () => {
    if (playerTurn === currentPlayer) {
        playerTurn = nextPlayer;
    }
    else {
        playerTurn = currentPlayer;
    }
}
// Function to check win
const checkWin = () => {
    const winningConditions =
        [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6],
        ];
    for (let i = 0; i < winningConditions.length; i++) {
        const [pos1, pos2, pos3] = winningConditions[i];
        if (gameCells[pos1].textContent !== '' &&
            gameCells[pos1].textContent === gameCells[pos2].textContent &&
            gameCells[pos2].textContent === gameCells[pos3].textContent) {
            return true;
        }
    }
    return false;
}
// function to check for a tie
const checkTie = () => {
    let emptyCellsCount = 0;
    gameCells.forEach(cell => {
        if (cell.textContent === ''){
            emptyCellsCount++;
        }
    });
    return emptyCellsCount === 0 && !checkWin();
}
// function to disable game-board cells after a win or tie
const disableCells = () => {
    gameCells.forEach(cell => {
        cell.removeEventListener('click', handleClick);
        cell.classList.add('disabled');
    });
}
// Function to restart game
const restartGame = () => {
    gameCells.forEach(cell => {
        cell.textContent = '';
        cell.classList.remove('disabled');
    });
    startGame();
}
// Function to show alert
const showAlert = (msg) => {
    alertBox.style.display = "block";
    alertBox.textContent = msg;
    setTimeout(()=> {
        alertBox.style.display = "none";
    },3000);
}

// Adding Event Listener to restart button
restartBtn.addEventListener('click', restartGame);

// Calling start Game function
startGame();
