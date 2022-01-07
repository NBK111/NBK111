#include <iostream>
#include <string.h>
#include <windows.h>
#include <Wincon.h>
#include <stdlib.h>
#include <time.h>
#include <iomanip>
#include <dir.h>
#include <dos.h>
#include <fstream>
#include <conio.h>
#include <time.h>
#include <MMSystem.h>

using namespace std;

char snow[30][50];


void setTextColor(WORD color)
{
    HANDLE hConsoleOutput;
    hConsoleOutput = GetStdHandle(STD_OUTPUT_HANDLE);
    CONSOLE_SCREEN_BUFFER_INFO screen_buffer_info;
    GetConsoleScreenBufferInfo(hConsoleOutput, &screen_buffer_info);
    WORD wAttributes = screen_buffer_info.wAttributes;
    color &= 0x000f;
    wAttributes &= 0xfff0; wAttributes |= color;
    SetConsoleTextAttribute(hConsoleOutput, wAttributes);
}

void gotoxy(int x, int y)
{
    COORD c = { x, y };
    SetConsoleCursorPosition(  GetStdHandle(STD_OUTPUT_HANDLE) , c);
}

int getRandomColor(){
	int colorIndex = rand() % 15+1;
	return colorIndex;
}

void readFile(){

	char str[150];
    FILE* fp;
    fp = fopen("1234.txt", "r");
    int height = 0;
    while (fgets(str, 150, fp)) {
    	for (int i = 0; i < strlen(str); i++) {
    		if(snow[height][i] == '*' && i < 50){
		    	setTextColor(8);//white
		    	cout << '*';
		    	if(i == (strlen(str)-1)){
	    			cout << '\n';
	    		}
		    	continue;
		    }else if(str[i] == '*'){
		    	setTextColor(0);//black
		    }else if(str[i] == '&'||
				str[i] == '*'||
				str[i] == '`'||
				str[i] == '~'||
				str[i] == 'o'||
				str[i] == '('||
				str[i] == ')'||
				str[i] == '_'||
				(str[i] == '+' && height >3 )||
				str[i] == ','){
				setTextColor(getRandomColor());
			}else if(height < 3){
    			setTextColor(14);//yellow	
    		}else if(height <= 22){
		    	setTextColor(10);// green
		    }else if(height <= 25){
    			setTextColor(6); // brown
    		}
		    cout << str[i];
    	}
        //printf("%s",str);
        height++;
    }
    fclose(fp);
}

void moveSnow(){
	// move down
	for (int i = 23; i >=0; i--){
		for (int j = 1; j< 50; j++){
			 if(snow[i][j] == '*'){
			 	if(snow[i][j] !=0){
					snow[i][j] = '*';
					snow[i+1][j] ='*';
					snow[i][j]--;
				}
			}
		}
	}
	int newSnow = rand() % 49;
	snow[0][newSnow] = '*';
}

int main(int argc, char *argv[])
{
	srand(time(NULL));
	while(true){
		readFile();
		Sleep(5);
		gotoxy(0, 0);
		moveSnow();
	}
	return 0;
}
