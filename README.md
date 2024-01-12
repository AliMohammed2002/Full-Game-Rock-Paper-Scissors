# Full-Game-Rock-Paper-Scissors , using C++
here i cearte a simple game and anyone can try it 
# here is the code 

#include <iostream>
#include <iostream>
#include <string>
#include <cmath>
#include <cstdlib>
#include <ctime>
#include<string.h>
using namespace std;
enum enGameChoice {
    stone = 1,
    paper = 2,
    scissors = 3
};

enum enWinner {
    computer = 1,
    player = 2,
    draw = 3
};


struct stRoundInfo {
    short roundNumber = 0;
    enGameChoice playerChoice;
    enGameChoice computerChoice;
    enWinner winnerOfTheRound;
    string nameOfWinner = "";

};

struct stGameInfo {
    short roundTimes = 0;
    short playerWonTimes = 0;
    short computerWonTimes = 0;
    short drawTimes = 0;
    enWinner finalWinner;
    string nameOfFinalWinner = "";
};

string taps(short numberOfTaps) {
    string tap = "";
    for (int i = 1;i <= numberOfTaps;i++) {
        tap += "\t";
    }
    return tap;
}



enGameChoice readPlayerChoice() {
    short choice = 0;
    do {
        cout << "your choice : 1-> stone , 2-> paper , 3-> scissors" << endl;
        cin >> choice;
    } while (choice < 1 || choice>3);
    return (enGameChoice)choice;
}



short readHowManyTimesToPlay() {
    short times = 0;
    do {
        cout << "please enter how many times to play 1->10 " << endl;
        cin >> times;
    } while (times < 1 || times>10);
    return times;
}

short getRandomeNumber(short from, short to) {
    return rand() % (from - to - 1) + from;
}

enGameChoice getComputerChoice() {
    return (enGameChoice)getRandomeNumber(1, 3);
}


string getWinnerName(enWinner winner) {
    string WinnerNames[3] = { "computer","player","no one" };
    return WinnerNames[winner - 1];
}

string getChoiceName(enGameChoice choice) {

    string ChoiceNames[3] = { "stone","paper","scissors" };
    return ChoiceNames[choice - 1];
}


void changeTheColorBasedOnWinner(enWinner winner) {
    if (winner == enWinner::computer) {

        cout << "\a" << endl;
        system("color 4F");
    }

    else if (winner == enWinner::player) {
        system("color 2F");
    }
    else {
        system("color 6F");
    }


}

string printSepartor(short number) {
    string str = "";
    for (int i = 1;i <= number;i++) {
        str += "_";
    }
    return str;
}

void printGameOver() {
    cout << printSepartor(60) << endl;
    cout << taps(3) << "+++++ Game over +++++" << endl;
    cout << printSepartor(60) << endl;
}

void printGameResult(stGameInfo info) {

    printGameOver();
    cout << printSepartor(60) << " [game result] " << printSepartor(60) << endl;
    cout << "game rounds         : " << info.roundTimes << endl;
    cout << "player won times    : " << info.playerWonTimes << endl;
    cout << "computer won times  : " << info.computerWonTimes << endl;
    cout << "winner              :" << info.nameOfFinalWinner << endl;
    cout << printSepartor(120) << "\n\n";
}



void printRoundResult(stRoundInfo result) {
    cout << "____________________________________" << "round[" << result.roundNumber << "]____________________________________" << endl;

    cout << "player choice : " << getChoiceName(result.playerChoice) << endl;
    cout << "computer choice : " << getChoiceName(result.computerChoice) << endl;
    cout << "winner : " << getWinnerName(result.winnerOfTheRound) << endl;

    cout << "____________________________________________________________________________________________________________________\n\n" << endl;

}


enWinner whoWonTheRound(stRoundInfo info) {
    if (info.computerChoice == info.playerChoice) {
        return enWinner::draw;
    }
    switch (info.playerChoice) {
    case enGameChoice::stone: {
        if (info.computerChoice == enGameChoice::paper) {
            return enWinner::computer;
        }
        break;
    }
    case enGameChoice::paper: {
        if (info.computerChoice == enGameChoice::scissors) {
            return enWinner::computer;
        }
        break;

    }
    case enGameChoice::scissors: {
        if (info.computerChoice == enGameChoice::stone) {
            return enWinner::computer;
        }
        break;
    }


    }
    return enWinner::player;

}
enWinner whoWonTheGame(short playerWonTimes, short computerWonTimes) {
    if (computerWonTimes == playerWonTimes) {
        return enWinner::draw;
    }
    else if (computerWonTimes > playerWonTimes) {
        return enWinner::computer;
    }
    return enWinner::player;
}


void resetScreen() {
    system("cls");
    system("color 0F");
}

stGameInfo fillGameResult(short numberOfRounds, short computerWonTimes, short playerWonTimes, short DrawTimes) {
    stGameInfo result;
    result.roundTimes = numberOfRounds;
    result.computerWonTimes = computerWonTimes;
    result.playerWonTimes = playerWonTimes;
    result.drawTimes = DrawTimes;
    result.finalWinner = whoWonTheGame(playerWonTimes, computerWonTimes);
    result.nameOfFinalWinner = getWinnerName(result.finalWinner);
    return result;

}


stGameInfo playAgame(short howManyRounds) {
    short playerTotalWinntimes = 0;
    short computerTotalWinTimes = 0;
    short drawTimes = 0;
    stRoundInfo roundInfo;

    for (int rounds = 1;rounds <= howManyRounds;rounds++) {
        roundInfo.roundNumber = rounds;
        roundInfo.computerChoice = getComputerChoice();
        roundInfo.playerChoice = readPlayerChoice();
        roundInfo.winnerOfTheRound = whoWonTheRound(roundInfo);
        roundInfo.nameOfWinner = getWinnerName(roundInfo.winnerOfTheRound);
        changeTheColorBasedOnWinner(roundInfo.winnerOfTheRound);
        printRoundResult(roundInfo);


        if (roundInfo.winnerOfTheRound == enWinner::computer) {
            computerTotalWinTimes++;
        }
        else if (roundInfo.winnerOfTheRound == enWinner::player) {
            playerTotalWinntimes++;
        }
        else {
            drawTimes++;
        }

    }
    return fillGameResult(howManyRounds, computerTotalWinTimes, playerTotalWinntimes, drawTimes);
}

void startGame() {
    short WantsToPlayAgain = 1;
    do {
        resetScreen();
        stGameInfo gameInfo = playAgame(readHowManyTimesToPlay());
        printGameResult(gameInfo);
        cout << "do you want to play again ? " << endl;
        cout << " if yes press 1 , no press 2 " << endl;
        cin >> WantsToPlayAgain;

    } while (WantsToPlayAgain == 1);
}






int main()
{
    srand((unsigned)time(NULL));
    startGame();

    return 0;
}
