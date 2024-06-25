saldo = 0
quantidade_de_saque = 0
sacado = []
deposite = []
usuarios = []
contas = []
agencia = "0001"

def deposito(depositar, /):
    global saldo
    if depositar > 0:
        saldo += depositar
        deposite.append(depositar)
        print(f'O seu depósito de R$ {saldo:.2f} foi efetuado com sucesso!')
    else:
        print('Erro no valor!')


def sacar(*, saque):
    global saldo
    global quantidade_de_saque
    if quantidade_de_saque < 3 and saque <= saldo and saque <= 500:
        saldo -= saque
        sacado.append(saque)
        print(f'O seu saque de R$ {saque:.2f} foi efetuado com sucesso! \nSeu saldo agora é de R$ {saldo:.2f}.')
        quantidade_de_saque += 1
        print(sacado)
    elif saque > saldo:
        print('\033[1;31mNão foi possível sacar o dinheiro por falta de saldo.\033[m')
    elif quantidade_de_saque >= 3:
        print('\033[1;31mQuantidade de saque diário excedido.\033[m')
    elif saque > 500:
        print('\033[1;31mSaque maior que R% 500,00. Valor excedido.\033[m')


def visualizar(saldo, /, *, extrato):
    if extratos == 0:
        print("Não foram realizadas movimentações.")
    else:
        print('-' * 40)
        print(f'{"EXTRATO":.^40}')
        print('-' * 40)
        print(f'\033[1;32m{"Depositos":.<30}\033[m', f'\033[1;32mR${"Valor":>7}\033[m')
        for c in range(0, len(deposite)):
            print(f'\033[1;32m{c + 1}º\033[m', f'\033[1;32m{"":.<27}\033[m', f'\033[1;32mR${deposite[c]:>7.2f}\033[m')
        print(f'\033[1;31m{"Saques":.<30}\033[m', f'\033[1;31mR${"Valor":>7}\033[m')
        for j in range(0, len(sacado)):
            print(f'\033[1;31m{j + 1}º\033[m', f'\033[1;31m{"":.<27}\033[m', f'\033[1;31mR${sacado[j]:>7.2f}\033[m')
        print(f'\033[1;34m{"SALDO":.<30}\033[m', f'\033[1;34mR${saldo:>7.2f}\033[m')
        print('-' * 40)


def criar_usuario(usuarios):
    cpf = int(input("Informe o CPF (somente número): "))
    for usuario in usuarios:
        if usuario["cpf"] == cpf:
            print('CPF já cadastrado.')
            return

    nome = input("Informe o nome completo: ")
    data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
    endereco = input("Informe o endereço (logradouro, nro - bairro - cidade/sigla estado): ")
    usuario = {"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco}
    usuarios.append(usuario)
    print("=== Usuário criado com sucesso! ===")


def criar_conta(usuarios):
    cpf = int(input("Informe o CPF do usuário: "))
    for usuario in usuarios:
        if usuario["cpf"] == cpf:
            print("=== Conta criada com sucesso! ===")
            numero_conta = len(contas) + 1
            conta = {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}
            contas.append(conta)
            print(conta)
            return
    print("Usuário não encontrado. Por favor, crie um usuário. \nFluxo de criação de conta encerrado!")


def listar_contas(contas):
    for conta in contas:
        linha = f"""
            Agência: {conta['agencia']}
            C/C: {conta['numero_conta']}
            Titular: {conta['usuario']['nome']}
        """
        print("=" * 40)
        print(linha)


menu = """
\033[1;34mBANCO AMERICANO
Opções:
[1] Depositar 
[2] Sacar
[3] Extrato 
[4] Nova conta
[5] Listar contas
[6] Novo usuário
[0] Sair\033[m
"""

opcao = 1
while opcao != 0:
    print(menu)
    opcao = int(input('Qual opção você quer? '))
    if opcao == 1:
        depositar = float(input('Quanto você quer depositar? R$ '))
        deposito(depositar)
    elif opcao == 2:
        saque = float(input('Quanto você quer sacar? R$ '))
        sacar(saque=saque)
    elif opcao == 3:
        extratos = len(sacado) + len(deposite)
        visualizar(saldo, extrato=extratos)
    elif opcao == 4:
        criar_conta(usuarios)
    elif opcao == 5:
        listar_contas(contas)
    elif opcao == 6:
        criar_usuario(usuarios)
    elif opcao == 0:
        print('Obrigado! E volte sempre.')
    else:
        print('\033[1;31mOpção inválida, tente novamente.\033[m')
