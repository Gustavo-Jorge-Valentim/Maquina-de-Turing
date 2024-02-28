#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TAPE_SIZE 1000

// Função para executar a Máquina de Turing
char* execute_turing_machine(const char* input_tape) {
    char* tape = (char*)malloc(TAPE_SIZE * sizeof(char));
     if (tape == NULL) {
      perror("Erro de alocação de memória para a fita");
       exit(EXIT_FAILURE);
    }

    strncpy(tape, input_tape, strlen(input_tape));

    // Estado inicial e posição da cabeça de leitura
    int current_state = 0;
     int head_position = 0;
      while (1) {

        // Condições para parada

        if (current_state == 3 || current_state == -1 || head_position >= TAPE_SIZE || head_position < 0) {
         break;
        }

        // Lógica de transição
        if (current_state == 0 && tape[head_position] == '0') {
         tape[head_position] = '1';
          head_position++;
           current_state = 1; }
            else if (current_state == 1 && tape[head_position] == '1') {
             tape[head_position] = '0';
              head_position--;
               current_state = 2;}
               else if (current_state == 2 && tape[head_position] == '0') {
              tape[head_position] = '1';
             head_position--;
            current_state = 1;}
          else if (current_state == 2 && tape[head_position] == '1') {
          tape[head_position] = '0';
            head_position++;
            current_state = 3;}
             else {

            // Se nenhuma transição aplicável for encontrada, pare

            current_state = -1;
        }
    }

    return tape;
}

int main() {
    // Solicitação da fita de entrada ao usuário

    char input_tape[TAPE_SIZE];
     printf("Digite a fita de entrada (0s e 1s): ");
      fgets(input_tape, TAPE_SIZE, stdin);
       input_tape[strcspn(input_tape, "\n")] = '\0';  // Remove a quebra de linha do final da string

    // Execução da Máquina de Turing

    char* output_tape = execute_turing_machine(input_tape);

    // Abre o arquivo de saída

    FILE* output_file = fopen("output_tape.txt", "w");
     if (output_file == NULL) {
      perror("Erro ao abrir o arquivo de saída");
       exit(EXIT_FAILURE);
    }

    // Grava a fita resultante no arquivo de saída
     fprintf(output_file, "%s\n", output_tape);
       fclose(output_file);

        // Exibição da fita resultante

          printf("Fita resultante: %s\n", output_tape);

           // Liberação da memória alocada para a fita

             free(output_tape);

               printf ("\nArquivo criado com sucesso !");

    return 0;
}
