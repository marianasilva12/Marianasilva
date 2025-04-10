#ifndef LOGICA_H
#define LOGICA_H

int desafio_logica();

#endif#include <stdio.h>

int desafio_logica() {
    int resposta;
    printf("\n🧠 DESAFIO DE LÓGICA:\n");
    printf("Qual número completa a sequência?\n");
    printf("2, 4, 8, 16, ?\n");
    printf("Sua resposta: ");
    scanf("%d", &resposta);

    if (resposta == 32) {
        printf("✅ Correto! Você pode ver a carta do computador nesta rodada.\n");
        return 1;
    } else {
        printf("❌ Errado! Nada de vantagem dessa vez.\n");
        return 0;
    }
}#ifndef UTILS_H
#define UTILS_H

void embaralhar(int *vetor, int tamanho);

#endif#include <stdlib.h>
#include <time.h>
#include "utils.h"

void embaralhar(int *vetor, int tamanho) {
    srand(time(NULL));
    for (int i = tamanho - 1; i > 0; i--) {
        int j = rand() % (i + 1);
        int temp = vetor[i];
        vetor[i] = vetor[j];
        vetor[j] = temp;
    }
}#include <stdio.h>
#include "cartas.h"
#include "logica.h"
#include "utils.h"

int escolher_atributo() {
    int escolha;
    printf("\nEscolha um atributo para competir:\n");
    printf("1. Inteligência\n2. Força\n3. Velocidade\n4. Estratégia\n5. Lógica\n");
    printf("Digite o número do atributo: ");
    scanf("%d", &escolha);
    return escolha;
}

int obter_valor_atributo(Carta carta, int atributo) {
    switch (atributo) {
        case 1: return carta.inteligencia;
        case 2: return carta.forca;
        case 3: return carta.velocidade;
        case 4: return carta.estrategia;
        case 5: return carta.logica;
        default: return -1;
    }
}

int main() {
    Carta baralho[TOTAL_CARTAS];
    inicializar_baralho(baralho);

    int ordem[TOTAL_CARTAS];
    for (int i = 0; i < TOTAL_CARTAS; i++) ordem[i] = i;
    embaralhar(ordem, TOTAL_CARTAS);

    Carta jogador_carta = baralho[ordem[0]];
    Carta computador_carta = baralho[ordem[1]];

    printf("=== Sua carta ===\n");
    imprimir_carta(jogador_carta);

    int vantagem = desafio_logica();
    if (vantagem) {
        printf("\n👀 Carta do computador:\n");
        imprimir_carta(computador_carta);
    }

    int atributo = escolher_atributo();
    int val_jog = obter_valor_atributo(jogador_carta, atributo);
    int val_comp = obter_valor_atributo(computador_carta, atributo);

    printf("\nVocê: %d vs Computador: %d\n", val_jog, val_comp);

    if (jogador_carta.super_trunfo) {
        printf("🌟 Você jogou o SUPER TRUNFO! Vitória garantida!\n");
    } else if (computador_carta.super_trunfo) {
        printf("🌟 Computador usou o SUPER TRUNFO! Vitória dele!\n");
    } else if (val_jog > val_comp) {
        printf("✅ Você venceu a rodada!\n");
    } else if (val_jog < val_comp) {
        printf("❌ Computador venceu a rodada.\n");
    } else {
        printf("🤝 Empate!\n");
    }

    return 0;
}CC = gcc
CFLAGS = -Wall

OBJS = main.o cartas.o logica.o utils.o

supertrunfo: $(OBJS)
	$(CC) $(CFLAGS) -o supertrunfo $(OBJS)

main.o: main.c cartas.h logica.h utils.h
cartas.o: cartas.c cartas.h
logica.o: logica.c logica.h
utils.o: utils.c utils.h

clean:
	rm -f *.o supertrunfo
