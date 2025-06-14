# Sistema Bancário Simples em Python 💰🐍

## Sobre o Projeto

Este é um sistema bancário simples feito em Python para rodar no terminal, ideal para quem está começando em programação e quer praticar lógica, controle financeiro e manipulação de dados.  
Aqui você pode fazer depósitos, saques (com limite diário e máximo de saques), visualizar extratos e até simular empréstimos — tudo com mensagens claras e validações para evitar erros comuns.

## Funcionalidades 🚀

- 💵 Depósito fácil e rápido de valores positivos  
- 🏦 Saque com limite máximo por operação e número restrito de saques diários  
- 📜 Extrato detalhado mostrando todas as movimentações  
- 🤝 Simulação de empréstimo descontado do saldo e controlado em conta separada  
- ⚠️ Verificações inteligentes para evitar saques indevidos e saldo negativo  
- 📋 Menu interativo simples para navegar entre as opções  

## Por que usar este projeto? 🤔

Se você está aprendendo Python e quer um desafio prático, este sistema é uma ótima forma de entender condicionais, loops, manipulação de strings e entrada de dados do usuário. Também é um mini projeto para quem quer montar portfólio com algo funcional e didático.

## Explicação do Código

```python
"""
Este programa simula um sistema bancário simples com as seguintes funcionalidades:
- Depositar: Adiciona um valor ao saldo da conta.
- Sacar: Retira um valor do saldo da conta, respeitando limites de saque e saldo.
- Extrato: Exibe o histórico de transações e o saldo atual.
- Empréstimo: Permite ao cliente pegar um empréstimo, debitando do saldo do banco e creditando na conta do cliente.
- Sair: Encerra o programa.
"""

menu = """

[d] Depositar
[s] Sacar
[e] Extrato
[p] Empréstimo
[q] Sair

=> """

# Variáveis globais para armazenar o estado da conta
saldo = 0  # Saldo atual da conta do banco (ou do sistema)
conta_cliente = 0 # Saldo da conta do cliente (para empréstimos)
limite = 500  # Limite máximo por saque
extrato = ""  # String para armazenar o histórico de transações
numero_saques = 0  # Contador de saques realizados
LIMITE_SAQUES = 3  # Limite máximo de saques permitidos

while True:
    # Exibe o menu e obtém a opção do usuário
    opcao = input(menu)

    if opcao == "d":
        # Operação de depósito
        valor = float(input("Informe o valor do depósito: "))

        if valor > 0:
            saldo += valor
            extrato += f"Depósito: R$ {valor:.2f}\n"  # Registra a transação no extrato
            print(f"Depósito de R$ {valor:.2f} realizado com sucesso!")
        else:
            print("Operação falhou! O valor informado é inválido.")

    elif opcao == "s":
        # Operação de saque
        valor = float(input("Informe o valor do saque: "))

        # Verifica as condições para o saque
        excedeu_saldo = valor > saldo
        excedeu_limite = valor > limite
        excedeu_saques = numero_saques >= LIMITE_SAQUES

        if excedeu_saldo:
            print("Operação falhou! Você não tem saldo suficiente.")
        elif excedeu_limite:
            print("Operação falhou! O valor do saque excede o limite.")
        elif excedeu_saques:
            print("Operação falhou! Número máximo de saques excedido.")
        elif valor > 0:
            saldo -= valor
            extrato += f"Saque: R$ {valor:.2f}\n"  # Registra a transação no extrato
            numero_saques += 1  # Incrementa o contador de saques
            print(f"Saque de R$ {valor:.2f} realizado com sucesso!")
        else:
            print("Operação falhou! O valor informado é inválido.")

    elif opcao == "e":
        # Operação de extrato
        print("\n================ EXTRATO ================\n")
        if not extrato:  # Verifica se há transações registradas
            print("Não foram realizadas movimentações.")
        else:
            print(extrato)
        print(f"\nSaldo: R$ {saldo:.2f}")
        print("==========================================\n")

    elif opcao == "p":
        # Operação de empréstimo
        valor = float(input("Informe o valor do empréstimo: "))
        # O empréstimo é concedido se o saldo do "banco" for suficiente
        # O valor é debitado do saldo do "banco" e creditado na "conta_cliente"
        if saldo >= valor: # Verifica se o saldo do banco é suficiente para o empréstimo
            saldo -= valor # Deduz o valor do empréstimo do saldo do banco
            conta_cliente += valor # Adiciona o valor do empréstimo à conta do cliente
            extrato += f"Empréstimo concedido: R$ {valor:.2f}\n" # Registra a transação no extrato
            print(f"Empréstimo de R$ {valor:.2f} realizado com sucesso!")
            print(f"Saldo conta Cliente: R$ {conta_cliente:.2f}")
            print(f"Saldo Banco (após empréstimo): R$ {saldo:.2f}")
        else:
            print("Operação falhou! O banco não tem saldo suficiente para conceder este empréstimo.")

    elif opcao == "q":
        # Opção de sair do programa
        break
    else:
        # Opção inválida
        print("Operação inválida! Por favor, selecione uma opção válida.")
