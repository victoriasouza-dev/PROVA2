import sqlite3

n = input()
cod = input()
aux = input()
conexao = sqlite3.connect("estoque.db")
cursor = conexao.cursor()

cursor.execute("""
    CREATE TABLE IF NOT EXISTS Produtos(
    id INTEGER PRIMARY KEY AUTOINCREMENT,  
    nome TEXT NOT NULL,
    quantidade INTEGER NOT NULL,            
    preco REAL NOT NULL
)
""")

for i in range(0,4):
    nome = input("Digite o nome do produto: ")
    quantidade = int(input("Digite a quantidade: "))
    preco = float(input("Agora informe preço: "))
    cursor.execute("""
INSERT INTO Produtos (nome, quantidade, preco)
VALUES (?, ?, ?)                  
""", (nome, quantidade, preco))


cursor.execute("SELECT * FROM Produtos")
dados = cursor.fetchall()

for estoque in dados:
    print(estoque)


alteracao = int(input("Deseja alterar o preço ou quantidade do produto? DIgite 1 para preço e 2 para quantidade: "))
alterar = input("Informe o nome do produto que deseja alterar: ")
if alteracao == 1:
    cod = float(input("Digite o preço desejado:"))
    cursor.execute("""
    UPDATE Produtos
    SET preco = ?
    WHERE nome = ?
    """, (cod, alterar))

if alteracao == 2:
    cod = int(input("Digite a quantidade desejada: "))
    cursor.execute("""
    UPDATE Produtos
    SET quantidade = ?
    WHERE nome = ?
    """, (cod, alterar))

    cursor.execute("SELECT * FROM Produtos")
dados = cursor.fetchall()

for estoque in dados:
    print(estoque)


remocao = input("Deseja remover um produto do estoque? Responda com Sim ou não:  ")
if remocao == "Sim":
    aux = input("Digite o produto a ser deletado: ")
    cursor.execute("""
    DELETE FROM Produtos
    WHERE nome = ?
    """, (aux,))

    cursor.execute("SELECT * FROM Produtos")
dados = cursor.fetchall()

for estoque in dados:
    print(estoque)

n = input("Busque um produto especifico pelo id:")
cursor.execute("""
    SELECT * FROM Produtos
    WHERE id = ?
    """, (n,))
cursor.execute("SELECT * FROM Produtos")
dados = cursor.fetchmany()

for estoque in dados:
    print(estoque)

conexao.commit()

print("Organização do estoque realizada com sucesso. ")

conexao.close()
