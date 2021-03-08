/*Universidade de Brasilia
Instituto de Ciencias Exatas
Departamento de Ciencia da Computacao
Algoritmos e Programação de Computadores –
2/2019
Aluno(a): Gustavo Rodrigues Gualberto
Matricula: 190108266
Turma: A
Versão do compilador: gcc (SUSE Linux) 7.3.1 20180323
Descricao: jogo semelhante ao classico dos anos 80, Bomberman, todavia
em ASCII e sem AI.*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
int escolha; /* tecla do menu */
typedef struct {
    char nomeJogador[20];
    char tab[15][28]; /*matriz onde o jogo ocorrera*/
    long int tempo_partida, tempo_final, tempo_bomba;/*variaveis para o timer*/
    int pos_x, pos_y, b1, b2; /* coordenadas do jogador */
    char tecla; /* tecla que o usuario ira teclar */
    unsigned int inimigos; /* contador de jogo.inimigos */
    int bomb_x, bomb_y; /* coordenadas da bomba */
} estadoPartida;
estadoPartida jogo;
#define LIN 15 /*linhas do tabuleiro*/
#define COL 27/*colunas do tabuleiro*/
#define RAND_MAXI 6
/*valor medio que a bomba levara para explodir (segundos)*/
#define MEDIA_TEMPO 7
/* erro da medida de tempo que a bomba levara para explodir (segundos)*/
#define ERRO_TEMPO 3
/* expressao que gera um numero aleatorio no intervalo [M - E, M + E]*/
#define RAND(M, E) (M + (rand()%(2*E + 1) - E))
void game_save(){ /* funcao para salvar o jogo */
    FILE* fd;
    char save[] = "save.bin";
    fd=fopen(save, "w+b");
    fwrite(&jogo, sizeof(estadoPartida), 1, fd);
    fclose(fd);
    getchar();
}
void game_load(){ /* funcao para carregar o jogo */
    FILE* fd;
    char save[] = "save.bin";
    fd=fopen(save, "rb");
    fread(&jogo, sizeof(estadoPartida), 1, fd);
    fclose(fd);
}
void teclas(){ /* teclas para jogar */
    printf("\nA-esquerda D-direita S-cima\nX-baixo B-bomba E-encerrar\n");
}
void tabuleiro_jogo(){
    int lin, col;
    for(lin=0; lin<LIN; lin++){
        for(col=0; col<COL; col++){
            if(lin==0){ /* paredes horizontais */
                jogo.tab[lin][col]='_';
                continue;}
            if(col==0 || col==COL-1){ /* paredes verticais */
               jogo.tab[lin][col]='|';
                continue;}
            if(lin%2==0 && lin!=0 && lin!=LIN-1){ /* paredes internas e espaços */
                if(col%2==0)
                jogo.tab[lin][col]='+';
                else
                jogo.tab[lin][col]=' ';   
                }
            else{
                if(lin==LIN-1)
                jogo.tab[lin][col]='_';
                else
                jogo.tab[lin][col]=' ';
            }
        }  
    }
    for(lin=0; lin<LIN; lin++){
        for(col=0; col<COL; col++){
            if(jogo.tab[lin][col]==' '){
                int a=RAND(0, RAND_MAXI); /* espectro pseudoaleatorio */
                switch(a){
                    case -1:
                        jogo.tab[lin][col]='#'; /* gera paredes destrutiveis */
                        break;
                    case 0:
                        jogo.tab[lin][col]='@'; /* gera inimigos */
                        jogo.inimigos++;
                        break;
                     case 1:
                        jogo.tab[lin][col]='#';
                        break;
                    default:
                        jogo.tab[lin][col]=' ';
                        break;
                }
            }
        }
    }
    jogo.tab[jogo.pos_x][jogo.pos_y]='&'; /* ponto inicial do jogador */
}
void menu(){ /* funcao para criar o menu inicial */
    printf("\n*****************************************");
    printf(" BOMBERMAN ");
    printf("*****************************************\n");
    printf("\n\n");
    printf("1. JOGAR\n2. CONTINUAR JOGO\n3. RANKING\n4. SAIR\nOpcao: ");
}

void print_tab() {
    char aux;
    int lin, col;
    for(lin=0; lin<LIN; lin++){ /* imprime o tabuleiro */
        for(col=0; col<COL; col++){
            printf("%c", jogo.tab[lin][col]);
        }
        printf("\n");
    }
    printf("opcao: \n");
    switch(jogo.tecla){
        case 'a': /* tecla para ir para esquerda */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){ 
                jogo.tab[jogo.pos_x][jogo.pos_y-1]='&';
                jogo.pos_y--;
                break;
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y-1]=='@'){ 
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y-1]==' '){
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x][jogo.pos_y-1];
                jogo.tab[jogo.pos_x][jogo.pos_y-1]=aux;
                jogo.pos_y--;
            }
            
                break;
        case 's': /* tecla para ir para cima */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x-1][jogo.pos_y]='&';
                jogo.pos_x--;
                break;
            }
            if(jogo.tab[jogo.pos_x-1][jogo.pos_y]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x-1][jogo.pos_y]==' ')
                {
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x-1][jogo.pos_y];
                jogo.tab[jogo.pos_x-1][jogo.pos_y]=aux;
                jogo.pos_x--;
            }
            
                break;
        case 'd': /* tecla para ir para direita */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x][jogo.pos_y+1]='&';
                jogo.pos_y++;
                break;
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y+1]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y+1]==' ')
                {
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x][jogo.pos_y+1];
                jogo.tab[jogo.pos_x][jogo.pos_y+1]=aux;
                jogo.pos_y++;
            }
            
                break;
        case 'x': /* tecla para ir para baixo */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x+1][jogo.pos_y]='&';
                jogo.pos_x++;
                break;
            }
            if(jogo.tab[jogo.pos_x+1][jogo.pos_y]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x+1][jogo.pos_y]==' ')
                {
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x+1][jogo.pos_y];
                jogo.tab[jogo.pos_x+1][jogo.pos_y]=aux;
                jogo.pos_x++;
            }
            
                break;

        case 'A': /* tecla para ir para esquerda */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x][jogo.pos_y-1]='&';
                jogo.pos_y--;
                break;
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y-1]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y-1]==' '){
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x][jogo.pos_y-1];
                jogo.tab[jogo.pos_x][jogo.pos_y-1]=aux;
                jogo.pos_y--;
            }
            
                break;
        case 'S': /* jogo.tecla para ir para cima */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x-1][jogo.pos_y]='&';
                jogo.pos_x--;
                break;
            }
            if(jogo.tab[jogo.pos_x-1][jogo.pos_y]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x-1][jogo.pos_y]==' ')
                {
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x-1][jogo.pos_y];
                jogo.tab[jogo.pos_x-1][jogo.pos_y]=aux;
                jogo.pos_x--;
            }
            
                break;
        case 'D': /* tecla para ir para direita */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x][jogo.pos_y+1]='&';
                jogo.pos_y++;
                break;
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y+1]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x][jogo.pos_y+1]==' ')
                {
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x][jogo.pos_y+1];
                jogo.tab[jogo.pos_x][jogo.pos_y+1]=aux;
                jogo.pos_y++;
            }
                break;
        case 'X': /* tecla para ir para baixo */
            if (jogo.tab[jogo.pos_x][jogo.pos_y]=='*'){
                jogo.tab[jogo.pos_x+1][jogo.pos_y]='&';
                jogo.pos_x++;
                break;
            }
            if(jogo.tab[jogo.pos_x+1][jogo.pos_y]=='@'){
                jogo.tab[jogo.pos_x][jogo.pos_y]='@';
            }
            if(jogo.tab[jogo.pos_x+1][jogo.pos_y]==' ')
                {
                jogo.tab[jogo.pos_x][jogo.pos_y]='&';
                aux=jogo.tab[jogo.pos_x][jogo.pos_y];
                jogo.tab[jogo.pos_x][jogo.pos_y]=jogo.tab[jogo.pos_x+1][jogo.pos_y];
                jogo.tab[jogo.pos_x+1][jogo.pos_y]=aux;
                jogo.pos_x++;
            }
                break;
        

    }
    if(jogo.tempo_bomba<=time(0) && jogo.tempo_bomba>0){  /* loop para explodir a bomba */
        for(jogo.b1=jogo.bomb_y-1; jogo.b1<=jogo.bomb_y+1; jogo.b1++){
            for(jogo.b2=jogo.bomb_x-1; jogo.b2<=jogo.bomb_x+1; jogo.b2++){
                if(jogo.tab[jogo.b2][jogo.b1]=='*')
                    jogo.tab[jogo.b2][jogo.b1]=' ';
                if(jogo.tab[jogo.b2][jogo.b1]=='#')
                    jogo.tab[jogo.b2][jogo.b1]=' ';
                if(jogo.tab[jogo.b2][jogo.b1]=='@'){
                    jogo.tab[jogo.b2][jogo.b1]= ' ';
                    jogo.inimigos--;}
                if(jogo.tab[jogo.b2][jogo.b1]=='&')
                    jogo.tab[jogo.pos_x][jogo.pos_y]='?';
            }
        }
        jogo.tempo_bomba=-1;
    }
}
void tempo () { /* funcao timer */
    jogo.tempo_partida=time(0);
    printf("tempo: %ld", jogo.tempo_final-jogo.tempo_partida);
}
 void bomb() { /* funcao para colocar a bomba */
    jogo.tab[jogo.pos_x][jogo.pos_y]='*';
    jogo.bomb_x=jogo.pos_x;
    jogo.bomb_y=jogo.pos_y;
    jogo.tab[jogo.bomb_x][jogo.bomb_y]='*';
    int pavio=RAND(MEDIA_TEMPO,ERRO_TEMPO); /* tempo de explosao da bomba */
    jogo.tempo_bomba=time(0)+pavio;
}
int main (){
    srand(time(0));
    jogo.tempo_bomba=-1;
    jogo.pos_x=13;
    jogo.pos_y=1;
    jogo.inimigos=0;
    menu(); /* tela inicial */
    scanf("%d", &escolha);
    tabuleiro_jogo(); /* inicio da partida */
    while(1){ /* condicoes de tecla no menu */
        if(escolha==1){
            system("clear");
            printf("Digite seu nickname (max 20 caracteres): ");
            scanf("%s[^/n]", jogo.nomeJogador);
            break;
        }
        if(escolha==2){
            system("clear");
            game_load();
            break;
        }
        if(escolha==4){
            system("clear");
            printf("Ate mais!\n");
            getchar();
            getchar();
            return 0;
        }
    }
    system("clear");
    jogo.tempo_partida=time(0);
    jogo.tempo_final=time(0)+200;
    while(jogo.tempo_final-jogo.tempo_partida>=0){
        tempo();
        teclas();
        print_tab();
        scanf("%c", &jogo.tecla);
         if((jogo.tecla=='b' || jogo.tecla=='B') && jogo.tempo_bomba==-1){
            bomb();
            continue;
        } 
        if(jogo.tecla=='e' || jogo.tecla=='E'){ /* encerra o jogo */
            system("clear");
            game_save();
            printf("FIM DE JOGO\nSUA PARTIDA FOI SALVA.\n");
            break;
        }
        if(jogo.tempo_final-jogo.tempo_partida<=0){ /* caso acabar o tempo */
            printf("ACABOU O TEMPO\n");
            break;
        }
        if(jogo.tab[jogo.pos_x][jogo.pos_y]=='@'){ /* caso encoste em um inimigo */
            system("clear");
            printf("VOCE MORREU\n");
            break;
        }
        if(jogo.inimigos==0){
            system("clear");
            printf("VOCE GANHOU!\nSUA PONTUAÇÃO FOI: %ld\n", jogo.tempo_final-jogo.tempo_partida);
            break;
        }
        if(jogo.tab[jogo.pos_x][jogo.pos_y]=='?'){
            system("clear");
            printf("VOCE EXPLODIU.\n");
            break;
        }
        system("clear");
    }
    getchar();
    return 0;
}
