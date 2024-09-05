# Desafio 1

#Este é o primeiro desafio do bootcamp da NTTDATA.

import textwrap

def menu():
    return input(textwrap.dedent("""\n
    ================ MENU ================
    [d]\tDepositar
    [s]\tSacar
    [e]\tExtrato
    [nc]\tNova conta
    [lc]\tListar contas
    [nu]\tNovo usuário
    [q]\tSair
    => """))

def depositar(saldo, valor, extrato):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito: R$ {valor:.2f}\n"
        print("\n=== Depósito realizado com sucesso! ===")
    else:
        print("\n@@@ Operação falhou! O valor informado é inválido. @@@")
    return saldo, extrato

def sacar(saldo, valor, extrato, limite, numero_saques):
    if valor > saldo or valor > limite or numero_saques >= 3 or valor <= 0:
        print("\n@@@ Operação falhou! Verifique o valor ou limite. @@@")
    else:
        saldo -= valor
        extrato += f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
        print("\n=== Saque realizado com sucesso! ===")
    return saldo, extrato, numero_saques

def exibir_extrato(saldo, extrato):
    print(f"\n==== EXTRATO ====\n{extrato if extrato else 'Não foram realizadas movimentações.'}\nSaldo: R$ {saldo:.2f}\n=================")

def criar_usuario(usuarios):
    cpf = input("Informe o CPF (somente número): ")
    if any(u["cpf"] == cpf for u in usuarios):
        print("\n@@@ Já existe usuário com esse CPF! @@@")
    else:
        usuarios.append({"cpf": cpf, "nome": input("Nome completo: "), "nascimento": input("Data de nascimento: "), "endereco": input("Endereço: ")})
        print("\n=== Usuário criado com sucesso! ===")

def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("Informe o CPF do usuário: ")
    if any(u["cpf"] == cpf for u in usuarios):
        print("\n=== Conta criada com sucesso! ===")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": cpf}
    print("\n@@@ Usuário não encontrado! @@@")

def listar_contas(contas):
    for c in contas: print(f"Agência: {c['agencia']} | C/C: {c['numero_conta']} | CPF: {c['usuario']}")

def main():
    saldo, limite, extrato, numero_saques, usuarios, contas = 0, 500, "", 0, [], []
    while (opcao := menu()) != "q":
        if opcao == "d":
            saldo, extrato = depositar(saldo, float(input("Informe o valor do depósito: ")), extrato)
        elif opcao == "s":
            saldo, extrato, numero_saques = sacar(saldo, float(input("Informe o valor do saque: ")), extrato, limite, numero_saques)
        elif opcao == "e":
            exibir_extrato(saldo, extrato)
        elif opcao == "nu":
            criar_usuario(usuarios)
        elif opcao == "nc":
            if conta := criar_conta("0001", len(contas) + 1, usuarios): contas.append(conta)
        elif opcao == "lc":
            listar_contas(contas)
        else:
            print("\n@@@ Operação inválida! @@@")

main()




