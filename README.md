from datetime import datetime,timezone,time
import pytz

saldo= 5 
numero_saque=0
limite_saque=3
extrato= ""
limite_diario=0
lista={}
saldo_novo=0
 #Data de fora---------------------------------------------
codigo1=datetime.now(pytz.timezone("America/Sao_Paulo"))
data_inicial=codigo1.date()
#------------------------------------------------------
#FUNÇÃO DEPOSITAR---------------------
def depositar(extrato,saldo,limite_diario,data,lista):
     if lista =={ }:
        print("Realize um cadastro para efetuar operações !")
        return extrato,saldo,limite_diario
     solic_deposito=int(input("Selecione o valor que deseja depositar e insira as cedulas"))
     if solic_deposito >=5:
        extrato+= f"Deposito R$ +{solic_deposito:.2f} {data}\n"
        saldo+=solic_deposito
        limite_diario+=1
        print(f"Deposito de R${solic_deposito} feito com sucesso")
        print("Obrigado por utilizar os nossos serviços !!")
        return extrato,saldo,limite_diario
     else:
        print("Valor minimo de deposito é de R$ 5 reais")
        return extrato,saldo,limite_diario

# FUNÇÃO SACAR-------------------------------
def sacar(extrato,saldo,limite_diario,data,numero_saque,limite_saque,lista):
    if lista =={ }:
        print("Realize um cadastro para efetuar operações !")
        return extrato,saldo,limite_diario,numero_saque
    if numero_saque >=limite_saque:
            print("Limite de saques diarios atingidos")
            return extrato,saldo,limite_diario,numero_saque
            
            
    else:
        valor_saque=int(input("Qual o valor do saque ?"))
        
    if valor_saque >500:
        print("o limite é de R$ 500 reais por saque")
        return extrato,saldo,limite_diario,numero_saque
    elif valor_saque > saldo:
        print(f"saldo insuficiente")
        return extrato,saldo,limite_diario,numero_saque
    else:
        numero_saque+=1
        limite_diario+=1
        extrato+=f"Saque - R$ {valor_saque:.2f} {data} \n"
        saldo -=valor_saque
        print(f"saque de R${valor_saque} realizado com sucesso! ")
        print("Obrigado por utilizar os nossos serviços !!")
        return extrato,saldo,limite_diario,numero_saque

#FUNÇÃO EXTRATO---
def extrato_(extrato,saldo,lista):
     if lista =={ }:
        print("Realize um cadastro para consultar o historico !")
        return
     
     print(f"=======EXTRATO=======")
     
     if extrato == "":
        print(" Nenhuma operação realizada")
        return 
     else:
        print(f"Saldo: {saldo}")

        print(extrato)

        print("Obrigado por utilizar os nossos serviços !!")
        return

#FUNÇÃO CADASTRO USUARIO----
def criacao_conta(lista,saldo_novo,saldo):
    if lista !={ }:
        print("Usuario já cadastrado")
        return lista,saldo_novo,saldo

    print("Informe as seguintes informações para a realização do cadastro:")
    nome1=input("insira seu nome")
    adicionar1=lista.setdefault("NOME",nome1)
    nascimento=input("insira a sua data de nascimento")
    adicionar2=lista.setdefault("data de nascimento",nascimento)
    cpf=input("seu cpf (somente numeros)")
    adicionar3=lista.setdefault("CPF",cpf)
    print("insira respectivamente os seguintes dados")
    endereço1=input("logradouro / bairro/ cidade / estado")
    adicionar4=lista.setdefault("endereço",endereço1)

    print("CADASTROS PREENCHIDOS COM SUCESSO !")
    print("nome",lista["NOME"])
    print("nascimento:",lista["data de nascimento"])
    print("cpf:",lista["CPF"])
    print("Endereço:",lista["endereço"])
    print(""" seu numero de conta é 
          AGENCIA 001 --- CONTA 25154 ---- DIGITO 02""")
    
    while True:
        try:
            vinculacao=int(input("[1] CRIAR CONTA CORRENTE [2] EXCLUIR DADOS CADASTRAIS"))
            if vinculacao not in [1,2]: 
                print("selecione uma opção valida")
                continue

            break    
        except ValueError:
            print("digite um valor valido")
            continue

        
    if vinculacao==1:
        saldo=saldo_novo
        print("Conta Efetivada com Sucesso! ")
        print("deposite para começar autilizar nossos serviços ")
        return lista,saldo_novo,saldo
        
    if vinculacao==2:
        lista.clear()
        print("Dados cadastrais apagados com Sucesso !!")
        return lista,saldo_novo,saldo

#FUNÇÃO INFORMAÇÕES DO USUARIO
def informacoes(lista):
    if lista =={ }:
        print("Nenhuma informação encontrada ! Realize um cadastro")
        return
    print("DADOS CADASTRADOS:")
    print(""" CONTA: 
          AGENCIA 001 --- CONTA 25154 ---- DIGITO 02""")
    print("nome",lista["NOME"])
    print("nascimento:",lista["data de nascimento"])
    print("cpf:",lista["CPF"])
    print("Endereço:",lista["endereço"])
    return
#MENU -------------------
while True:
    #Data---------------------------------------------
    codigo=datetime.now(pytz.timezone("America/Sao_Paulo"))
    formato_data = "%d/%m/%Y %H:%M"
    data=codigo.strftime(formato_data)
    if codigo.date() != data_inicial:
        numero_saque=0
        limite_diario=0
        data_inicial=codigo

    #------------------------------------------------------
    opcoes= int( input("[1] DEPOSITO \\ [2] SAQUE\\ [3] EXTRATO\\[4] CADASTRO E EFETIVAÇÃO DE CONTA\\ [5] INFORMAÇÕES DA CONTA"))
    if opcoes not in [1,2,3,4,5]:
        print("insira um valor válido")
        continue
    if opcoes in [1,2] and limite_diario>=10:
        print("Limite de 10 operações esgotadas no dia")
        break
    #DEPOSITO
    if opcoes ==1:
        extrato,saldo,limite_diario=depositar(extrato,saldo,limite_diario,data,lista)
        
        saida=int(input(" [1] retornar as opções\\ [2] sair"))
        if saida ==1:
                print(opcoes)
        elif saida == 2:
            break
        
        #SAQUE
    if opcoes ==2:
        extrato,saldo,limite_diario,numero_saque= sacar(extrato,saldo,limite_diario,data,numero_saque,limite_saque,lista)
        
        saida=int(input(" [1] retornar as opções\\ [2] sair"))
        if saida ==1:
            print(opcoes)
        elif saida == 2:
            break
        #EXTRATO
    if opcoes ==3:
            extrato_(extrato,saldo,lista)
        
            saida=int(input(" [1] retornar as opções\\ [2] sair"))
            if saida ==1:
                print(opcoes)
            elif saida == 2:
                break    
    if opcoes==4:

        lista,saldo_novo,saldo=criacao_conta(lista,saldo_novo,saldo)
        print(opcoes)
    if opcoes==5:
        informacoes(lista)
        saida=int(input(" [1] retornar as opções\\ [2] sair"))
        if saida ==1:
            print(opcoes)
        elif saida == 2:
            break
