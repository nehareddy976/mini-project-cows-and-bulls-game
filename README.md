# mini-project-cows-and-bulls-game
#include<stdio.h>
#include<conio.h>
#include<string.h>
#include<stdlib.h>

struct details{
    char name[100];
    char player1[100],player2[100];
}d;

void home();
void startgame();
void instructions();
void leaderboard();
void singleplayer();
void multiplayer();
int mode();
int playbullsandcows(char num[],int size,int pn);
int numberofbulls(char guess[],char num[],int size);
int numberofcows(char guess[],char num[],int size);

void main()
{
    FILE *f1,*f2;
    f1=fopen("SinglePlayer.txt","a");
    f2=fopen("MultiPlayer.txt","a");
    fclose(f1);
    fclose(f2);
    home();
}

void home() 
{
    system("CLS");
    printf("\n\t\t\t\t\t\tBULLS AND COWS");
    printf("\n\n\t1.Start Game");
    printf("\n\n\t2.Instructions");
    printf("\n\n\t3.View Past Results");
    printf("\n\n\t4.Quit");
    printf("\n\n\tPress the corresponding number to continue : ");
    char choice=getch();

    switch(choice) 
    {
        case '1':startgame();break;
        case '2':instructions();break;
        case '3':leaderboard();break;
        case '4':exit(0);break;
        default:home();  
    }
}

void startgame() 
{
    system("CLS");
    printf("\n\t1.Single Player");
    printf("\n\n\t2.Multi Player");
    printf("\n\n\t3.Back");
    printf("\n\n\n\t\tPress the corresponding number to continue : ");
    char option=getch();

    switch(option) 
    {
        case '1':singleplayer();break;
        case '2':multiplayer();break;
        case '3':home();break;
        default:startgame();
    }
}

void instructions() 
{
    system("CLS");
    printf("\n\t\tInstructions");
    printf("\n\n\t'Bulls and Cows' is an old Code-Breaking mind game in which");
    printf("\n\tThe player is engaged to figure out 3, 4 or a 5 digit code by the trail and error method");
    printf("\n\tThe number of cows represent the digits that were guessed and present in the actual code");
    printf("\n\tBut not in the exact position. Example:  3564   (Guessed Number)  Number of cows = 2");
    printf("\n\t                                         /  \\ ");
    printf("\n\t                                         5986    (Actual Code)");
    printf("\n\tThe number of bulls represent the digits that were guessed and present in the actual code");
    printf("\n\tAnd also the exact same position. Example:   4365    (Guessed Number)   Number of bulls =1");
    printf("\n\t                                              |");
    printf("\n\t                                             9312");
    printf("\n\tThus the code can be found by analyzing the number of bulls and cows");
    printf("\n\tThe game provides both Single Player and Multi Player mode");
    printf("\n\n\tHave Fun!!!!");
    printf("\n\n\tPress any key to continue : ");
    getch();
    home();
}

void leaderboard() 
{
    FILE *f;
    int numberofplayers=0;
    while(numberofplayers==0)
    {
        system("CLS");
        printf("\n\t 1 . Single Player Results");
        printf("\n\n\t 2 . Multi Player Results");
        printf("\n\n\t 3 . Back to Home Screen");
        printf("\n\n\t\t Press the corresponding number to continue : ");
        char ch=getch();
        switch(ch)
        {
        case '1':numberofplayers=1;break;
        case '2':numberofplayers=2;break;
        case '3':home();break;
        default:numberofplayers=0; 
        }
    }
    if(numberofplayers==1)
    {
        f=fopen("SinglePlayer.txt","r");
        system("CLS");
        printf("\n\tSingle Player Results:\n\n");
    }
    else
    {
        f=fopen("MultiPlayer.txt","r"); 
        system("CLS");
        printf("\n\tMulti Player Results:\n\n");
    }
    char c=fgetc(f);
    if(c==EOF)   
    {
        printf("\tNo Records Found");
    }
    while(c!=EOF) 
    {
        printf("%c",c);
        c=fgetc(f);
    }
    fclose(f);
    printf("\n\n\tPress any key to continue : ");
    getch();
    home();
}

void singleplayer() 
{
    system("CLS");
    printf("\n\t\tEnter Player Name : ");
   
    scanf("%[^\n]",d.name);
    int digits=mode();
    system("CLS");
    char number[5]="     ";
    int count=0;
    while(count<digits) 
    {
        int index,flag=0,r=rand()%9+49;
        for(index=0;index<count;index++) 
        {
            if((char)r==number[index])
            {
                flag=1;
            }
        }
        if(flag==0)
        {
            number[count]=(char)r;
            count++;
        }
    }
    FILE *f;
    f=fopen("SinglePlayer.txt","a");
    int completed=playbullsandcows(number,digits,0); 
    system("CLS");
    if(completed==0)
    {
        printf("\n\n\n\t\t\tYou Have Forfeited");
        printf("\n\n\n\n\tPress any key to Continue : ");
        fprintf(f,"\t\t%s forfeited trying to identify a %d-Digit Code\n",d.name,digits);
        fclose(f);
        getch();
        home();
    }
    else 
    {
        printf("\n\t\tCongratulations!!You Have Successfully Identified the Code");
        printf("\n\n\t\tNumber of Chances Taken : %d",completed);
        printf("\n\n\n\n\tPress any key to Continue : ");
        fprintf(f,"\t\t%s identified the %d-Digit Code in %d-Turn(s)\n",d.name,digits,completed);
        fclose(f);
        getch();
        home();
    }
}

void multiplayer()
{
    
    char pc1[5]="     ";
    char pc2[5]="     ";
    int p1,p2;
    system("CLS");
    printf("\n\t\tEnter Player 1 Name : ");
    scanf("%s",&d.player1);
    printf("\n\n\t\tEnter Player 2 Name : ");
    scanf("%s",&d.player2);
    int dig=mode(); 
    system("CLS");
    printf("\n\tPlayer 1 :");
    printf("\n\n\t\tEnter Your %d-Digit Secret Code  :  ",dig);
    int index=0;
    while(index<dig)
    {
        pc1[index]=getch();
        if(pc1[index]>'0'&&pc1[index]<='9')
        {
            int ind,flag=0;
            for(ind=0;ind<index;ind++)
            {
                if(pc1[ind]==pc1[index])
                {
                    flag=1;
                }
            }
            if(flag==0)
            {
                index++;
                printf("*");
            }
        }
    }
    printf("\n\n\tPress any key to continue : ");
    getch();
    system("CLS");
    printf("\n\tPlayer 2 :");
    printf("\n\n\t\tEnter Your %d-Digit Secret Code  :  ",dig);
    index=0;
    while(index<dig)
    {
        pc2[index]=getch();
        if(pc2[index]>'0'&&pc2[index]<='9')
        {
            int ind,flag=0;
            for(ind=0;ind<index;ind++)
            {
                if(pc2[ind]==pc2[index])
                {
                    flag=1;
                }
            }
            if(flag==0)
            {
                index++;
                printf("*");
            }
        }
    }
    printf("\n\n\tPress any key to continue : ");
    getch();
    system("CLS");
    printf("\n\tPlayer 1 must play the game first");
    printf("\n\n\tOnce Player 1 has completed the game,");
    printf("\n\n\tPlayer 2 Shall Continue...");
    printf("\n\n\t\tPress any key to continue : ");
    getch();
    p1=playbullsandcows(pc2,dig,1);
    system("CLS");
    FILE *f;
    f=fopen("MultiPlayer.txt","a");
    if(p1==0)
    {
        printf("\n\n\t\tCongrats!!! %s",d.player2);
        printf("\n\n\t\t%s has won the game as %s forfeited",d.player2,d.player1);
        fprintf(f,"\t\t%s Won the game as %s forfeited in finding %d-digit code\n",d.player2,d.player1,dig);
    }
    else 
    {
        system("CLS");
        printf("\n\t\tPlayer 1 has identified the code in %d turns",p1);
        printf("\n\n\t\tIts Player 2's Turn Now");
        printf("\n\n\t\tIdentify the code in fewer turns to win");
        printf("\n\n\t\tPress any key to continue : ");
        getch();
        p2=playbullsandcows(pc1,dig,2);
        system("CLS");
        if(p2==0)
        {
            printf("\n\n\t\tCongrats!!! %s",d.player1);
            printf("\n\n\t\t%s has won the game as %s forfeited",d.player1,d.player2);
            fprintf(f,"\t\t%s Won the game as %s forfeited in finding %d-digit code\n",d.player1,d.player2,dig);
        }
        else if(p1==p2) 
        {
            printf("\n\n\t\tCongrats Both the Players!!");
            printf("\n\n\t\tBoth of You have identified the code in %d turns",p1);
            printf("\n\n\t\tIts a Tie........");
            fprintf(f,"\t\tTie. %s and %s identified the code in %d-digit code in %d-Turn(s)\n",d.player1,d.player2,dig,p1);
        }
        else if(p1>p2) 
        {
            printf("\n\n\t\tCongrats!!! %s",d.player2);
            printf("\n\n\t\t%s(%d) has identified the code in %d turn(s)",d.player2,p2,p1-p2);
            printf("\n\n\t\tAhead of %s(%d)",d.player1,p1);
            printf("\n\n\t\t%s is the Winner!!!!.....",d.player2);
            fprintf(f,"\t\t%s(%d) identified %d-digit code in %d-turn(s) ahead of %s(%d)\n",d.player2,p2,dig,p1-p2,d.player1,p1);
        }
        else 
        {
            printf("\n\n\t\tCongrats!!! %s",d.player1);
            printf("\n\n\t\t%s(%d) has identified the code in %d turn(s)",d.player1,p1,p2-p1);
            printf("\n\n\t\tAhead of %s(%d)",d.player2,p2);
            printf("\n\n\t\t%s is the Winner!!!!.....",d.player1);
            fprintf(f,"\t\t%s(%d) identified %d-digit code in %d-turn(s) ahead of %s(%d)\n",d.player1,p1,dig,p2-p1,d.player2,p2);
        }
    }
    fclose(f);
    printf("\n\n\t\tPress any key to continue : ");
    getch();
    home();
}

int mode() 
{
    system("CLS");
    printf("\n\n\tChoose the difficulty level : ");
    printf("\n\n\t\t1.Easy ( 3-Digit Code )");
    printf("\n\n\t\t2.Medium ( 4-Digit Code )");
    printf("\n\n\t\t3.Hard ( 5-Digit Code )");
    printf("\n\n\t\t4.Back");
    printf("\n\n\tPress the corresponding number to continue : ");
    char ch=getch();

    switch(ch)
    {
        case '1':return 3;
        case '2':return 4;
        case '3':return 5;
        case '4':startgame();
        default:return mode();
    }
}

int playbullsandcows(char num[],int size,int pn)
{
    char allguesses[100][5];
    int allcows[100];
    int allbulls[100];
    int chance=1;
    char guess[5]="     ";
    while(1>0)
    {
        system("CLS");
        if(pn!=0)
        {
            printf("\n\tPlayer : %d\n",pn);
        }
        if(chance>1) 
        {
            int count;
            for(count=1;count<chance;count++)
            {
                printf("\n\t\tChance: %d  =>  ",count);
                int j;
                for(j=0;j<size;j++) printf("%c",allguesses[count-1][j]);
                printf("\tBulls => %d\tCows => %d",allbulls[count-1],allcows[count-1]);
            }
        }
        printf("\n\n\tChance : %d",chance);
        printf("\n\n\t\t\tEnter a %d-Digit number : ",size);
        int index=0;
        while(index<size)
        {
            guess[index]=getch();
            if(guess[index]>'0' && guess[index]<='9')
            {
                printf("%c",guess[index]);
                index++;
            }
        }
        strcpy(allguesses[chance-1],guess);
        int b=numberofbulls(guess,num,size);
        int c=numberofcows(guess,num,size);
        allbulls[chance-1]=b;
        allcows[chance-1]=c;
        printf("\n\n\n\tBulls : %d",b);
        printf("\n\n\tCows : %d",c);
        if(b==size) 
        {
            printf("\n\n\t\tPress any key to continue : ");
            getch();
            return chance;
        }
        printf("\n\n\t\tPress '0' to forfeit or any other key to continue : ");
        int forfeit=getch();
        if(forfeit=='0')
        {
            return 0;
        }
        chance++;
    }
}

int numberofbulls(char guess[],char num[],int size) 
{
    int index,bulls=0;
    for(index=0;index<size;index++)
    {
        if(guess[index]==num[index])
        {
            bulls++;
        }
    }
    return bulls;
}

int numberofcows(char guess[],char num[],int size) 
{
    int index1,index2,cows=0;
    for(index1=0;index1<size;index1++)
    {
        for(index2=0;index2<size;index2++)
        {
            if((guess[index1]==num[index2])&&(index1!=index2))
            {
                cows++;
            }
        }
    }
    return cows;
}
