# EP2
import random
import string
from palavras import words

def filtra(lista_palavras, num_letras):
    palavras_filtradas = []

    for palavra in lista_palavras:
        palavra_processada = palavra.lower().strip(string.punctuation)
        
        if len(palavra_processada) == num_letras:
            palavras_filtradas.append(palavra_processada)

    palavras_filtradas = list(set(palavras_filtradas))  

    return palavras_filtradas

def inicializa(lista_palavras):
    n = len(lista_palavras[0])   

    dicionario = {
        'n': n,
        'tentativas': n + 1,
        'especuladas': [],
        'sorteada': random.choice(lista_palavras)
    }

    return dicionario

def indica_posicao(sorteada, especulada):
    if len(sorteada) != len(especulada):
        return []  

    resultado = []

    for i in range(len(sorteada)):
        if especulada[i] == sorteada[i]:
            resultado.append(0)  # Letra na posição correta
        elif especulada[i] in sorteada:
            resultado.append(1)  # Letra existe na palavra sorteada, mas em posição errada
        else:
            resultado.append(2)  # Letra não existe na palavra sorteada

    return resultado

def sortear_palavra(palavras):
    return random.choice(palavras)

def verificar_palavra(palavra_sorteada, tentativa):
    resultado = indica_posicao(palavra_sorteada, tentativa)
    cores = []
    for index, val in enumerate(resultado):
        if val == 0:
            cores.append('\033[34m' + tentativa[index])  # Azul para letra na posição correta
        elif val == 1:
            cores.append('\033[33m' + tentativa[index])  # Amarelo para letra na palavra, mas na posição errada
        else:
            cores.append('\033[37m' + tentativa[index])  # Cinza para letra ausente na palavra
    return ' '.join(cores)

def jogo():
    palavras = filtra(words, 5)
    estado_jogo = inicializa(palavras)
    palavra_sorteada = estado_jogo['sorteada']
    tentativas_restantes = estado_jogo['tentativas']

    continuar_jogando = True

    print("Bem-vindo ao jogo Termo! Tente adivinhar a palavra de 5 letras.")

    while continuar_jogando and tentativas_restantes > 0:
        print(f"\nTentativas restantes: {tentativas_restantes}")
        tentativa = input("Digite uma palavra de 5 letras ou 'desisto' para encerrar o jogo: ").lower()
        

        if len(tentativa) != estado_jogo['n']:
            print("Por favor, digite uma palavra de 5 letras.")
            continue
        
        if tentativa == 'desisto':
            print(f"A palavra correta era: {palavra_sorteada}")
            break

        if tentativa == palavra_sorteada:
            print("Parabéns! Você acertou a palavra!")
            break
        else:
            tentativas_restantes -= 1
            resultado = verificar_palavra(palavra_sorteada, tentativa)
            print(f"Palavra incorreta. Dicas: {resultado}")

    if tentativas_restantes == 0:
        print(f"Você perdeu! A palavra correta era: {palavra_sorteada}")

    continuar = input("Deseja jogar novamente? (sim/não): ")
    if continuar.lower() != 'sim':
        continuar_jogando = False

    if continuar_jogando:
        jogo()
    else:
        print("Obrigado por jogar!")

