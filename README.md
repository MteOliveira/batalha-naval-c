#include <stdio.h>

#define TAM 10

// Inicializa o tabuleiro com 0 (água)
void inicializarTabuleiro(int tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            tabuleiro[i][j] = 0;
        }
    }
}

// Imprime o tabuleiro
void imprimirTabuleiro(int tabuleiro[TAM][TAM]) {
    printf("\n   ");
    for (int j = 0; j < TAM; j++) {
        printf("%2d ", j);
    }
    printf("\n");

    for (int i = 0; i < TAM; i++) {
        printf("%2d ", i);
        for (int j = 0; j < TAM; j++) {
            printf("%2d ", tabuleiro[i][j]);
        }
        printf("\n");
    }
}

// Valida se a posição é válida (dentro do tabuleiro e sem sobreposição)
int validarPosicao(int tabuleiro[TAM][TAM], int linha, int coluna, int tamanho, int direcao, int diagonal) {
    for (int k = 0; k < tamanho; k++) {
        int i = linha, j = coluna;

        if (diagonal) {
            if (direcao == 0) { // diagonal principal
                i += k;
                j += k;
            } else { // diagonal secundária
                i += k;
                j -= k;
            }
        } else {
            if (direcao == 0) { // horizontal
                j += k;
            } else { // vertical
                i += k;
            }
        }

        // Fora do tabuleiro
        if (i < 0 || i >= TAM || j < 0 || j >= TAM)
            return 0;

        // Já ocupado
        if (tabuleiro[i][j] != 0)
            return 0;
    }
    return 1;
}

// Posiciona navio no tabuleiro
void posicionarNavio(int tabuleiro[TAM][TAM], int linha, int coluna, int tamanho, int direcao, int diagonal) {
    for (int k = 0; k < tamanho; k++) {
        if (diagonal) {
            if (direcao == 0) {
                tabuleiro[linha + k][coluna + k] = 3; // diagonal principal
            } else {
                tabuleiro[linha + k][coluna - k] = 3; // diagonal secundária
            }
        } else {
            if (direcao == 0) {
                tabuleiro[linha][coluna + k] = 3; // horizontal
            } else {
                tabuleiro[linha + k][coluna] = 3; // vertical
            }
        }
    }
}

int main() {
    int tabuleiro[TAM][TAM];
    inicializarTabuleiro(tabuleiro);

    // Exemplo: 4 navios de tamanho 3
    // Dois horizontais/verticais
    if (validarPosicao(tabuleiro, 0, 0, 3, 0, 0)) // horizontal
        posicionarNavio(tabuleiro, 0, 0, 3, 0, 0);

    if (validarPosicao(tabuleiro, 2, 2, 3, 1, 0)) // vertical
        posicionarNavio(tabuleiro, 2, 2, 3, 1, 0);

    // Dois em diagonal
    if (validarPosicao(tabuleiro, 5, 0, 3, 0, 1)) // diagonal principal
        posicionarNavio(tabuleiro, 5, 0, 3, 0, 1);

    if (validarPosicao(tabuleiro, 5, 9, 3, 1, 1)) // diagonal secundária
        posicionarNavio(tabuleiro, 5, 9, 3, 1, 1);

    imprimirTabuleiro(tabuleiro);

    return 0;
}
