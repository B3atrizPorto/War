/*
    Trabalho de Programação - War
    Nome: Beatriz Porto
    Disciplina: Estrutura de Dados
   
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

/* Estrutura dos territórios */
typedef struct {
    char nome[30];
    char cor[10];
    int tropas;
} Territorio;

/* Protótipos das funções */
void atribuirMissao(char* destino, char* missoes[], int totalMissoes);
int verificarMissao(char* missao, Territorio* mapa, int tamanho);
void exibirMissao(char* missao);
void atacar(Territorio* atacante, Territorio* defensor);
void exibirMapa(Territorio* mapa, int tamanho);
void liberarMemoria(char* missaoJogador, Territorio* mapa);

/* Sorteia uma missão e copia para o jogador */
void atribuirMissao(char* destino, char* missoes[], int totalMissoes) {
    int indice = rand() % totalMissoes;
    strcpy(destino, missoes[indice]);
}

/* Verifica se a missão foi cumprida (lógica simples inicial) */
int verificarMissao(char* missao, Territorio* mapa, int tamanho) {
    // Exemplo básico: se missão contém "Conquistar", verifica se há território azul
    if (strstr(missao, "Conquistar") != NULL) {
        for (int i = 0; i < tamanho; i++) {
            if (strcmp(mapa[i].cor, "azul") == 0) {
                return 1;
            }
        }
    }
    return 0;
}

/* Exibe a missão do jogador */
void exibirMissao(char* missao) {
    printf("\n==============================\n");
    printf("        MISSÃO DO JOGADOR\n");
    printf("==============================\n");
    printf("%s\n\n", missao);
}

/* Simula um ataque entre dois territórios */
void atacar(Territorio* atacante, Territorio* defensor) {
    int dadoAtacante = (rand() % 6) + 1;
    int dadoDefensor = (rand() % 6) + 1;

    printf("\nAtaque de %s contra %s\n", atacante->nome, defensor->nome);
    printf("Dado atacante: %d | Dado defensor: %d\n", dadoAtacante, dadoDefensor);

    if (dadoAtacante > dadoDefensor) {
        printf("O atacante venceu o combate!\n");
        strcpy(defensor->cor, atacante->cor);
        defensor->tropas = atacante->tropas / 2; // transfere metade das tropas
    } else {
        printf("O ataque falhou. Atacante perde 1 tropa.\n");
        atacante->tropas -= 1;
    }
}

/* Exibe o estado atual do mapa */
void exibirMapa(Territorio* mapa, int tamanho) {
    printf("\n=== MAPA ATUAL ===\n");
    for (int i = 0; i < tamanho; i++) {
        printf("%s | Cor: %s | Tropas: %d\n", mapa[i].nome, mapa[i].cor, mapa[i].tropas);
    }
}

/* Libera toda memória alocada dinamicamente */
void liberarMemoria(char* missaoJogador, Territorio* mapa) {
    free(missaoJogador);
    free(mapa);
}

/* ---------------------- MAIN ---------------------- */
int main() {
    srand(time(NULL));

    /* Missões pré-definidas */
    char* missoes[] = {
        "Conquistar 1 territorio azul",
        "Eliminar tropas vermelhas",
        "Dominar 3 territorios seguidos",
        "Conquistar um territorio amarelo",
        "Acumular 10 tropas"
    };
    int totalMissoes = 5;

    /* Aloca memória para a missão do jogador */
    char* missaoJogador = (char*) malloc(100 * sizeof(char));
    atribuirMissao(missaoJogador, missoes, totalMissoes);

    /* Exibe missão apenas uma vez no início do jogo */
    exibirMissao(missaoJogador);

    /* Criação do mapa com territórios */
    int tamanhoMapa = 3;
    Territorio* mapa = (Territorio*) calloc(tamanhoMapa, sizeof(Territorio));

    strcpy(mapa[0].nome, "Territorio A");
    strcpy(mapa[0].cor, "vermelho");
    mapa[0].tropas = 5;

    strcpy(mapa[1].nome, "Territorio B");
    strcpy(mapa[1].cor, "verde");
    mapa[1].tropas = 3;

    strcpy(mapa[2].nome, "Territorio C");
    strcpy(mapa[2].cor, "amarelo");
    mapa[2].tropas = 2;

    exibirMapa(mapa, tamanhoMapa);

    /* Loop de turnos */
    int turno = 1;
    int venceu = 0;

    while (!venceu) {
        printf("\n----- TURNO %d -----\n", turno);

        /* Exemplo: atacante sempre ataca B usando A */
        atacar(&mapa[0], &mapa[1]);
        exibirMapa(mapa, tamanhoMapa);

        /* Verificação da missão do jogador */
        if (verificarMissao(missaoJogador, mapa, tamanhoMapa)) {
            printf("\n=====================================\n");
            printf("       JOGADOR COMPLETOU A MISSÃO!\n");
            printf("=====================================\n");
            venceu = 1;
        }

        turno++;
        if (turno > 10) { // limite de turnos apenas para teste
            printf("\nLimite de turnos atingido.\n");
            break;
        }
    }

    /* Libera memória */
    liberarMemoria(missaoJogador, mapa);

    return 0;
}
# War
